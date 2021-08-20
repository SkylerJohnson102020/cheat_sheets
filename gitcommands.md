# Git Commands

### SSH Keys

1. First things first, type in:

        ssh-keygen -t rsa -b 4096 -C "bob@loblaw.com"

2. Type in file to save the key 

        (/Users/myname/.ssh/id_rsa):

3. Enter passphrase, leave empty and press enter if you do not want a passphrase.
4. You will see something like this:

            blah@machine ~ $ ssh-keygen -t rsa -b 4096 -C "blah@examplecom"
            blah@machine
            Generating public/private rsa key pair.
            Enter file in which to save the key (/Users/myname/.ssh/id_rsa):
            Enter passphrase (empty for no passphrase):
            Enter same passphrase again:
            Your identification has been saved in /Users/myname/.ssh/id_rsa.
            Your public key has been saved in /Users/myname/.ssh/id_rsa.pub.
            The key fingerprint is:
            SHA256:ki0TNHm8qIvpH7/c0qQmdv2xxhYHCwlpn3+rVhKVeDo USEFUL-COMMENT
            The key's randomart image is:
            +---[RSA 4096]----+
            |  =   o+@ !!     |
            |     .=.o . +    |
            |     ..= + +     |
            |   *  .+* E      |
            | ** +++= So =    |
            | .. .  +. = +  oo|
            |   o.. = ..* .   |
            |  o ++=.o =o.    |
            | ..o.++o.=+. S   |
            +----[SHA256]-----+

5. Pass key to github, enter this command: 

        cat ~/.ssh/id_rsa.pub

6. You will get a bunch of random characters in return, you can copy and paste this the typical way, or type in this command:

        pbcopy < ~/.ssh/id_rsa.pub

        Windows:
            sudo apt-get install xclip
            xclip -selection clipboard < ~/.ssh/id_rsa.pub 

7. Go into GitHub and go into Settings. Go to SSH and GPG keys.
8. Select "New SSH key".
9. Give it a Title (ex. "bobloblawskey")
10. Paste the key (random bunch of characters from step 6).
11. Go back to the terminal and enter this command:

        eval "$(ssh-agent -s)"

12. You will get this in return:

        Agent pid 12345

13. Next, enter this command:

        Mac:
            ssh-add -K ~/.ssh/id_rsa

        Windows:
            ssh-add ~/.ssh/id_rsa

14. You will see this:

        Identity added: /Users/bobloblaw/.ssh/id_rsa (my@email.com)

15. Enter this command:

        nano ~/.ssh/config

16. Copy and paste the following:

        Host *
          AddKeysToAgent yes
          UseKeychain yes
          IdentityFile ~/.ssh/id_ed25519

17. Once you've pasted the above:

        press ctrl-x, then y, then enter. 

18. Give it a little test, enter: 

        ssh -T git@github.com

19. You should see:

        Hi Bob Loblaw! You've successfully authenticated, but GitHub does not provide shell access.



20. Whenever you want to clone down a repo, make sure you select the SSH option. Each repo will need the key added otherwise you may see this error:

        git@github.com: Permission denied (publickey).
        fatal: Could not read from remote repository.

        Please make sure you have the correct access rights
        and the repository exists.

        TO ADD THE NEW KEY, TYPE IN:

                ssh-add -K ~/.ssh/id_YOURIDNUMBER

            Mac:
                ssh-add -K

