## Task 1 – SSH signed commits

Signing commits with SSH keys lets GitHub verify that a commit really came from me.  
This protects the repository history from attackers pushing code under my name and makes it easier for a team to trust who made which change.

To avoid touching my existing work SSH keys, I would:

- keep current keys in `~/.ssh` unchanged;
- generate a separate key pair for this course with a different file name:

```sh
ssh-keygen -t ed25519 -C "devops-labs" -f ~/.ssh/id_ed25519_devops
```

- add `~/.ssh/id_ed25519_devops.pub` to my GitHub account;
- configure Git **only in this repo** (without `--global`) to use SSH signing:

```sh
git config gpg.format ssh
git config user.signingkey ~/.ssh/id_ed25519_devops.pub
git config commit.gpgSign true
```

After that I can create a signed commit:

```sh
git commit -S -m "Signed commit for lab1"
```

