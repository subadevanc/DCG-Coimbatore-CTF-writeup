# Misc
## Git Necromancy

![](assets/image%2023.png)

it was zip file i unzip get all history

```
tar -xzf repo.tar.gz
cd project-archive
git log --oneline --decorate --all

e5b6912 (HEAD -> master) Initial project structure
```

Use `git fsck` to find disconnected objects that are not referenced by any branch or tag:

```
git fsck --full --no-reflogs --unreachable --dangling

Checking ref database: 100% (1/1), done.
Checking object directories: 100% (256/256), done.
Checking objects: 100% (8/8), done.
unreachable tree 800016dd831794f8a3a98ebf1ccb7f20d8a56770
unreachable commit a93585887246672d81679c548f11e1a33fc17788
unreachable commit 31d90cfa58c5b991c365daca287aeeaf55709924
unreachable blob b5e6590a8c945f16248d8350421f83d3be231c89
Verifying commits in commit graph: 100% (1/1), done.
```

there are few unreachable thing just vist all the nodes

```
git cat-file -p a93585887246672d81679c548f11e1a33fc17788
git cat-file -p 31d90cfa58c5b991c365daca287aeeaf55709924

git cat-file -p 31d90cfa58c5b991c365daca287aeeaf55709924
tree 800016dd831794f8a3a98ebf1ccb7f20d8a56770
parent e5b691283c9597c6edc78de416a47f98657b7aa7
author NerdsLab Dev <dev@nerdslab.in> 1774771814 +0000
committer NerdsLab Dev <dev@nerdslab.in> 1774771814 +0000
```

one node contains credentials…. let get into tree now

```
git cat-file -p 800016dd831794f8a3a98ebf1ccb7f20d8a56770

100644 blob 1784f2c901f7334140a310cebc9fd49863586739	README.md
100644 blob 74290a4fbf1d58a7d70ec2f5fe7598635f371116	config.yaml
100644 blob 100644 blob 1784f2c901f7334140a310cebc9fd49863586739	README.md
100644 blob 74290a4fbf1d58a7d70ec2f5fe7598635f371116	config.yaml
100644 blob b5e6590a8c945f16248d8350421f83d3be231c89	credentials.bak
	credentials.bak
```

now get into b5e6590a8c945f16248d8350421f83d3be231c89

```
git cat-file -p b5e6590a8c945f16248d8350421f83d3be231c89

# Emergency backup credentials
# DO NOT COMMIT
flag=defcon{d3l3t3d_c0mm1t_r3c0v3r3d_fr0m_p4ck}
```

flag

```
defcon{d3l3t3d_c0mm1t_r3c0v3r3d_fr0m_p4ck}
```

git fsck is savior for all my git challenges… 😊
