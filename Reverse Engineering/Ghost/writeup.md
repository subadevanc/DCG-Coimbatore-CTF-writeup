# Reverse Engineering
## Ghost

![](assets/image%2015.png)

it just a exe that contains encrypted thing with vbscript… i got

![](assets/image%2016.png)

I use ilspycmd decomple to extract source

```
dotnet tool install -g ilspycmd 
```

no time decoplie

```
ilspycmd "ghost_main (1).exe" > decompiled.cs
```

```
using System.Diagnostics;
using System.IO;
using System.Reflection;
using System.Reflection.Emit;
using System.Runtime.CompilerServices;
using System.Security.Cryptography;

[assembly: CompilationRelaxations(8)]
[assembly: RuntimeCompatibility(WrapNonExceptionThrows = true)]
[assembly: AssemblyVersion("0.0.0.0")]
namespace System.Runtime.Ghost;

internal class Kernel
{
	private static readonly string _s = "BQkV68RUc8qGPuOONQ8Lxg==";

	private static readonly string _b = "KGkRKhhfxFH6lVH0ZOmK9BjA0apWspLEfmYaIWDB7QDUAhJ/E1PLLjarfIyBs5e8PvNkU9qhU038IsUQ9bEjOf411Tm5GFazzEHO251/uMA=";

	private static void Main(string[] args)
	{
		Console.Title = "System Integrity Check";
		Console.Write("Enter Access Key: ");
		string k = Console.ReadLine();
		try
		{
			InitializeSubsystem(k);
		}
		catch (Exception)
		{
			Console.WriteLine("Error: Memory Segment Fault (0xC0000005)");
		}
		Console.ReadKey();
	}

	private static void InitializeSubsystem(string k)
	{
		byte[] array = Convert.FromBase64String(_s);
		byte[] d = Convert.FromBase64String(_b);
		if (Debugger.IsAttached)
		{
			array[^1] ^= byte.MaxValue;
		}
		byte[] k2 = Derive(k, array);
		byte[] il_stream = Decrypt(d, k2);
		ExecuteGhost(il_stream);
	}

	private static void ExecuteGhost(byte[] il_stream)
	{
		DynamicMethod dynamicMethod = new DynamicMethod("SysCall", null, null, typeof(Kernel).Module);
		ILGenerator iLGenerator = dynamicMethod.GetILGenerator();
		using (MemoryStream memoryStream = new MemoryStream(il_stream))
		{
			using BinaryReader binaryReader = new BinaryReader(memoryStream);
			while (memoryStream.Position < memoryStream.Length)
			{
				try
				{
					short v = binaryReader.ReadInt16();
					OpCode opcode = FindOp(v);
					switch (binaryReader.ReadByte())
					{
					case 0:
						iLGenerator.Emit(opcode);
						break;
					case 1:
					{
						string str = binaryReader.ReadString();
						iLGenerator.Emit(opcode, str);
						break;
					}
					case 2:
					{
						string text = binaryReader.ReadString();
						if (text == "WriteLine")
						{
							MethodInfo method = typeof(Console).GetMethod("WriteLine", new Type[1] { typeof(string) });
							iLGenerator.Emit(opcode, method);
						}
						break;
					}
					default:
						throw new Exception();
					}
				}
				catch
				{
					break;
				}
			}
		}
		Action action = (Action)dynamicMethod.CreateDelegate(typeof(Action));
		action();
	}

	private static OpCode FindOp(short v)
	{
		FieldInfo[] fields = typeof(OpCodes).GetFields();
		for (int i = 0; i < fields.Length; i++)
		{
			if (fields[i].FieldType == typeof(OpCode))
			{
				OpCode result = (OpCode)fields[i].GetValue(null);
				if (result.Value == v)
				{
					return result;
				}
			}
		}
		return OpCodes.Nop;
	}

	private static byte[] Derive(string p, byte[] s)
	{
		using Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(p, s, 10000);
		return rfc2898DeriveBytes.GetBytes(32);
	}

	private static byte[] Decrypt(byte[] d, byte[] k)
	{
		using Aes aes = Aes.Create();
		aes.Key = k;
		byte[] array = new byte[16];
		Buffer.BlockCopy(d, 0, array, 0, 16);
		aes.IV = array;
		using ICryptoTransform transform = aes.CreateDecryptor();
		using MemoryStream stream = new MemoryStream(d, 16, d.Length - 16);
		using CryptoStream cryptoStream = new CryptoStream(stream, transform, CryptoStreamMode.Read);
		using MemoryStream memoryStream = new MemoryStream();
		try
		{
			cryptoStream.CopyTo(memoryStream);
		}
		catch
		{
			return new byte[0];
		}
		return memoryStream.ToArray();
	}
}
```

what i understand:  the user input is the AES password. Correct password decrypts valid IL that prints the flag.

```
private static readonly string _s = "BQkV68RUc8qGPuOONQ8Lxg==";

private static readonly string _b = "KGkRKhhfxFH6lVH0ZOmK9BjA0apWspLEfmYaIWDB7QDUAhJ/E1PLLjarfIyBs5e8PvNkU9qhU038IsUQ9bEjOf411Tm5GFazzEHO251/uMA=";
```

- `_s` → Base64 → **salt**

- `_b` → Base64 → **encrypted data**

- Wrong password → random garbage after decryption

- Correct password → meaningful IL code

- IL contains strings like:`"WriteLine"``"flag"``"ctf{...}"`

So we detect **valid structure instead of exact match**

```
import base64, hashlib
from Crypto.Cipher import AES

salt = base64.b64decode("BQkV68RUc8qGPuOONQ8Lxg==")
data = base64.b64decode("KGkRKhhfxFH6lVH0ZOmK9BjA0apWspLEfmYaIWDB7QDUAhJ/E1PLLjarfIyBs5e8PvNkU9qhU038IsUQ9bEjOf411Tm5GFazzEHO251/uMA=")

iv = data[:16]
ct = data[16:]

with open("/usr/share/wordlists/rockyou.txt", "r", errors="ignore") as f:
    for pwd in f:
        pwd = pwd.strip()

        key = hashlib.pbkdf2_hmac("sha1", pwd.encode(), salt, 10000, 32)
        pt = AES.new(key, AES.MODE_CBC, iv).decrypt(ct)

        # ✅ minimal but strong check
        if b"WriteLine" in pt or b"flag" in pt or b"ctf" in pt:
            print("[+] PASSWORD:", pwd)
            break
```

now time burteforce the pass with rockyou.txt

![](assets/image%2017.png)

then cracked the password and get the flag

![](assets/image%2018.png)

```
defcon{C#_r3v3r51ng_1s_c00l_4int_1t_xD}
```
