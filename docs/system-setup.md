# Setting up your system

Here are things that you must, and things that you should, install on your system.

## IDE
An integrated development environment is a software application that provides comprehensive facilities to computer programmers for software development.

Recommended: [Visual Studio Code](https://code.visualstudio.com/)

???+ tip "Want to encourage me?"

    Check out my free themes for VSCode!

    * [Great White](https://marketplace.visualstudio.com/items?itemName=thehedgefrog.greatwhite)

    * [Hammerhead](https://marketplace.visualstudio.com/items?itemName=thehedgefrog.hammerhead)

    * [Blue Whale](https://marketplace.visualstudio.com/items?itemName=thehedgefrog.blue-whale)

## Terminal Emulator
A terminal emulator, terminal application, or term, is a computer program that emulates a video terminal within some other display architecture. Though typically synonymous with a shell or text terminal, the term terminal covers all remote terminals, including graphical interfaces.

!!! info "This is optional, but highly recommended!"

=== ":fontawesome-brands-windows: Windows"
    * [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab)
    * [PowerShell](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab)
    * [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install)
    * [Ubuntu](https://www.microsoft.com/en-ca/p/ubuntu-20044-lts/9mttcl66cpxj#activetab=pivot:overviewtab)

=== ":material-apple: macOS"
    * [iTerm2](https://iterm2.com/)

## Git
Git needs to be installed on your system.  This can be done by command line, on the terminal emulator we just installed.

=== ":fontawesome-brands-windows: Windows"
    [Install Git for Windows](https://git-scm.com/download/win)

=== ":material-apple: macOS"
    ``` sh
    git --version
    ```

=== ":material-ubuntu: WSL - Ubuntu"
    ``` sh
    sudo apt install git-all
    ```

## GPG and Signing Commits
!!! question "Why should I sign my commits?"
    [This article](https://withblue.ink/2020/05/17/how-and-why-to-sign-git-commits.html) explains why you should be signing every commit.

=== ":fontawesome-brands-windows: Windows"
    [Install GPG4Win](https://gnupg.org/download/)

=== ":material-apple: macOS"
    ``` sh
    brew install gpg
    ```
    On macOS, you might also want to install a graphical pinentry application with brew install pinentry-mac, then add this line to ~/.gnupg/gpg-agent.conf (if the file doesn’t exist, create it):
    ```
    pinentry-program /usr/local/bin/pinentry-mac
    ```

=== ":material-ubuntu: WSL - Ubuntu"
    Most Linux distributions come with GPG pre-installed; if not, you can always find it on their official repositories.

    *Note that in some Linux distributions, the application is called gpg2, so you might need to replace gpg with gpg2 in the commands below. In this case, you might also need to run `git config --global gpg.program $(which gpg2).`*

### Additional configuration for Linux and macOS
On Linux and macOS, you can enable the GPG agent to avoid having to type the secret key’s password every time. To do that, add this line to ~/.gnupg/gpg.conf (if the file doesn’t exist, create it):
``` sh
# Enable gpg to use the gpg-agent
use-agent
```

You will also need to add these two lines to your profile file (~/.bashrc, ~/.bash_profile, ~/.zprofile, or wherever appropriate), then re-launch your shell (or run source ~/.bashrc or similar):
``` sh
export GPG_TTY=$(tty)
gpgconf --launch gpg-agent
```