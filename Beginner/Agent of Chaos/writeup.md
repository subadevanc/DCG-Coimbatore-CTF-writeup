# Beginner
## Agent of Chaos

![](assets/image%208.png)

they clearly mentioned that User-Agent is key to get the flag….

```
Error Message: "Authentication mismatch detected. Browser signature does not correspond with authorized Aether administrative protocols."
Blocking Reason: "INCOMPATIBLE_AGENT_SIGNATURE" - Standard consumer browsers are strictly prohibited
```

```
curl -s -A 'SecureBrowser/1.0' 'https://beginner.labs.nerdslab.in/agentofchaos/'
```

- `curl` with `-A` flag for User-Agent spoofing

```
DEFCON{H3AD3R_HUNT3R_S_D3LIGHT}
```
