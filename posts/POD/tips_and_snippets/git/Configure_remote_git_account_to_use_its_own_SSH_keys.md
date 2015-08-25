## Configure remote git account to use its own SSH keys

**Next is useful when multiple git accounts would be used on same machine
with multiple SSH keys**.

_based on: https://gist.github.com/jexchan/2351996_

### Scenario

Let's play next scenario: on my machine I want to have two github accounts:

    - GitDefault                 # this will be the default git account
    - GitRecycleBin

each using its own set of SSH keys:

    - id_rsa                     # for the GitDefault account
    - id_rsa_2048_recycle_bin    # for the GitRecycleBin account

And of course, I want to have flawless SSH access to different repositories
using these different accounts on same machine.

### Create and configure SSH keys for the **GitDefault** account

Follow: [https://help.github.com/articles/generating-ssh-keys/](https://help.github.com/articles/generating-ssh-keys/)
or shortly: ["Create and configure SSH for another github account"](#create-and-configure-ssh-for-another-github-account)

### Create and configure SSH for another github account

Follow: [https://help.github.com/articles/generating-ssh-keys/](https://help.github.com/articles/generating-ssh-keys/)
or shortly:

- Generate a new SSH key for separate git account

        $ ssh-keygen -t rsa -b 2048 -f id_rsa_2048_recycle_bin

    \* you may comment the key by the option:

        -C "your_email@example.com" # or whatever is meaningful for you

- Add the key to the ssh-agent

        $ ssh-add id_rsa_2048_recycle_bin

- It is good to delete all cached keys before

        $ ssh-add -D

- Check your active keys

        $ ssh-add -l

### Tell git which key to use with the given account

    $ sudo subl ~/.ssh/config

And add next block

    #GitRecycleBin account
    Host github.com-GitRecycleBin
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa_2048_recycle_bin

### Configure git repo with GitRecycleBin account

- Go to your repo root folder

        $ cd ~/projects/RecycleBin/

        If you have not associated the local repository with a remote, do

        $ git remote add origin git@github.com-GitRecycleBin:GitRecycleBin/RecycleBin.git

- Edit git config file

        $ subl .git/config

- Modify the `[remote "origin"]` section as:

        [remote "origin"]
            url = git@github.com-GitRecycleBin:GitRecycleBin/RecycleBin.git

### Test

    $ ssh -T git@github.com-GitRecycleBin

#### Debug (if in trouble)

    $ ssh -vT git@github.com-GitRecycleBin

### **NB!**

You have to perform the above steps for each account you've created,
if you want it to use own SSH key!
