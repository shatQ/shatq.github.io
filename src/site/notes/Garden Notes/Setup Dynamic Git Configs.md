---
{"dg-publish":true,"permalink":"/garden-notes/setup-dynamic-git-configs/","tags":["note","seedling"],"created":"2023-02-16","updated":"2024-11-29T14:52"}
---

# Setup Dynamic Git Configs

To setup git config that depends on the work folder, firstly edit  `~/.gitconfig` and replace it's content with:

```
[includeIf "gitdir:~/Documents/git/personal/**/.git"]
  path = .gitconfig-personal

[includeIf "gitdir:~/Documents/git/work/**/.git"]
  path = .gitconfig-work
```

Each section represents _"profile"_  that will be used for defined workdir.

Defined configuration for each  _"profile"_ as it would be reqular git config file, e.g.

```
cat ~/.gitconfig-personal
[user]
  name = Me
  email = me@example.com

[core]
  sshCommand = "ssh -i ~/.ssh/me"
```

```
~/.gitconfig-work
[user]
  name = Me at Work
  email = me-at-work@work.com

[core]
  sshCommand = "ssh -i ~/.ssh/work"
```

---
- https://gist.github.com/bgauduch/06a8c4ec2fec8fef6354afe94358c89e
