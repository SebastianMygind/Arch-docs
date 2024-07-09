# Git 

Using git with ssh requires:

    sudo pacman -S git openssh

## Integration with GitHub

For using git with GitHub

### 1. Configure git

Set email and name

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

### 2. Configure SSH keys

1. Generate key

If you have a new install you can safely generate a ssh key using:

    ssh-keygen -t ed25519 -C "johndoe@example.com"

2. Start ssh agent   

Use the following command

    eval "$(ssh-agent -s)"


3. Add private SSH key to ssh-agent

Command:

    ssh-add ~/.ssh/id_ed25519

4. Add public key to GitHub

Navigate to GitHubs SSH settings and add the output of the following command as a new key:

    cat ~/.ssh/id_ed25519.pub

5. Test the connection to GitHub

An easy way to test that all the above is done correctly is using:

    ssh -T git@github.com

If every thing is as it should be github will respond with the message:

    Hi <GitHub Username>! You've successfully authenticated, but GitHub does not provide shell access.

