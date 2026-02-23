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

After that I can create a signed commit

## Task 2 – Merge strategies in Git

### Standard merge
- creates a separate merge commit;
- keeps the full branch history as it was;
- makes it easy to see who did what and when.

Cons: history can become noisy with many branches and merge commits.

### Squash and merge
- combines all commits from the feature branch into a single commit;
- `main` receives one clean commit with the final result;
- useful when the feature branch has many small or messy commits.

Cons: you lose detailed history inside the branch, it is harder to find exactly which change introduced a bug.

### Rebase and merge
- replays the branch commits on top of the base branch;
- produces a straight, linear history without merge commits.

Cons: it rewrites history, which is dangerous for shared branches; teammates can get confused if they already pulled the old history.

### Why standard merge is often preferred
- it does not rewrite history, only adds to it;
- easier to understand what actually happened with branches;
- lower risk of breaking other people’s work in collaborative projects.

