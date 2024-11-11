# Git Commands

- `git rm --cached *.extension` : Removes all the files with specified extension.
- `git remote set-url origin <new-remote-url>` : Updates the existing origin link of repo

```bash


...................Setups below........................




```
# Step-by-Step Guide: Push Local Repository to Two Different GitHub Accounts
### Let’s take an example where we have two GitHub accounts, `ik` and `ff`, and we want to configure both of them so that we can push our files to both using a single computer and a single local directory.

## Step 1: Create SSH Keys for Both Accounts
If you haven't already created SSH keys for both accounts, follow these steps:

### Generate SSH key for `ik`:

```bash
ssh-keygen -t rsa -b 4096 -C "ik@gmail.com"
```
When prompted, save the key as `id_rsa_ik`:

```bash
Enter file in which to save the key: ~/.ssh/id_rsa_ik
```
Leave the passphrase empty or enter a passphrase.

### Generate SSH key for `ff`:

```bash
ssh-keygen -t rsa -b 4096 -C "ff@gmail.com"
```
When prompted, save the key as `id_rsa_ff`:

```bash
Enter file in which to save the key: ~/.ssh/id_rsa_ff
```
Again, leave the passphrase empty or enter a passphrase.

## Step 2: Add SSH Keys to the SSH Agent
Start the SSH agent and add the keys:

### Start the SSH agent:
```bash
eval $(ssh-agent -s)
```

### Add the keys:
```bash
ssh-add ~/.ssh/id_rsa_ik
ssh-add ~/.ssh/id_rsa_ff
```

## Step 3: Configure the SSH Config File
Configure the SSH client to recognize both accounts:

### Open the SSH config file:
```bash
nano ~/.ssh/config
```

### Add the following configuration:

```bash
# GitHub account ik
Host github-ik
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_ik

# GitHub account ff
Host github-ff
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_ff
```

Save and exit the file:
- Press `CTRL + O` to save.
- Press `Enter` to confirm the filename.
- Press `CTRL + X` to exit.

## Step 4: Create Repositories on GitHub
1. Log in to your `ik` GitHub account and create a new repository, e.g., `testik10`.
2. Log in to your `ff` GitHub account and create a new repository, e.g., `testff10`.

## Step 5: Set Up Your Local Repository
Create a local repository:

```bash
mkdir bothtest
cd bothtest
git init
```

### Create a sample file:
```bash
echo "# Test Repository" > README.md
git add .
git commit -m "Initial commit"
```

## Step 6: Add Remotes for Both Accounts
Add the remote for the `ik` account:

```bash
git remote add origin-ik git@github-ik:ik/testik10.git
```

Add the remote for the `ff` account:

```bash
git remote add origin-ff git@github-ff:ff/testff10.git
```

## Step 7: Push to Both Remotes
### Push to the `ik` account:

```bash
git push origin-ik main
```

### Push to the `ff` account:

```bash
git push origin-ff main
```

## Step 8: Verify on GitHub
Go to your GitHub accounts (`ik` and `ff`) and verify that the repositories (`testik10` and `testff10`) have been updated with the content from your local `bothtest` repository.

## Optional: Push to Both Remotes Simultaneously
You can set up a Git alias to push to both remotes at once:

### Create a Git alias:

```bash
git config --global alias.pushboth '!git push origin-ik main && git push origin-ff main'
```

### Use the alias to push:

```bash
git pushboth
```

This will push the changes to both `ik` and `ff` repositories simultaneously.
