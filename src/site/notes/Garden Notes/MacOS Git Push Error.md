---
{"dg-publish":true,"permalink":"/garden-notes/mac-os-git-push-error/","tags":["note","seedling"],"created":"2023-01-30T09:48:00","updated":"2024-11-29T14:51"}
---

# MacOS Git Push Error

```bash
fatal: Could not read from remote repository.
```
 
This error informs us we have an authentication issue. If you encounter an SSH authentication issue, your first port of call is to add your key to the SSH keychain:

```bash
ssh-add -K ~/.ssh/id_rsa
```

This will add our id_rsa key to the keychain.

---
- https://careerkarma.com/blog/git-fatal-could-not-read-from-remote-repository/