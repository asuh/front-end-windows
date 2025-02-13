# Front-End Development Setup on a PC

This document assumes you're running a fresh copy of the latest version of **Windows 10**, >=version 22H2. Most of this also applies to Windows 11.

The following workflow assumes a clean installation of Windows or from a full manual reinstall. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a manually installed Windows system.

- [Command Line Interface](#command-line-interface)
- [Windows Installations](#windows-installations)
- [Windows Prepartion](#system-update-and-disk-encryption)
- [Projects Directory](#projects-directory)
- [Chocolatey and Boxstarter](#chocolatey-and-boxstarter)
- [Ninite](#ninite)
- [Privacy](#privacy)
- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
- [AI](#ai)
- [Sublime Text and VSCode](#sublime-text-and-vscode)
- [Vim](#vim)
- [ZSH](#zsh)
- [SSH](#ssh)
- [Git](#git)
- [Node.js](#nodejs)
- [Python](#python)
- [Composer](#composer)
- [VirtualBox](#virtualbox)
- [Docker](#docker)

## Command Line Interface

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

```bash
sudo command -flag --flag directory file.extention # Comments are behind pound signs
```

Anytime you see the above, it is referring to your CLI of choice, whether it's the built-in Command Prompt or Powershell as well as Bash Shell or Z Shell [see below](#zsh) in [Windows Subsystem for Linux](#windows-subsystem-for-linux). You might also use a third-party application like [Cygwin](https://www.cygwin.com/) or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/). Setting up a command-line shell to your liking is a good idea.

## Windows Installations

If you're looking to just wipe the system complete and start from scratch, there's a project called Tiny10 (and Tiny11 for Win11) that provides full ISOs tweaked to be minimal.

[https://archive.org/details/@ntdev](https://archive.org/details/@ntdev)

I've never used these myself, but this could be a seriously convenient install if it takes care of a lot of removing telemetry and enhancing privacy. YMMV, so you probably want to test this out before using it.

## System update and Disk Encryption

Step One - Update the system! (Prepare for lots of reboots)

**Windows Key > Settings > Update & Security**

Step Two - Turn on BitLocker for full disk encryption (read 2018/11 Addendum below before continuing)

(If you can't install Bitlocker, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded by the security community.)

**Control Panel\System and Security\BitLocker Drive Encryption**

> [!NOTE]
> As of November 2018, it is recommended to disable hardware encryption and force Bitlocker to use Software Encryption because of [research that reveals vulnerabilities](https://msrc.microsoft.com/update-guide/en-us/advisory/ADV180028).

```bash
    # If encryption was previously enabled, check the type of drive encryption being used
    manage-bde.exe -status
```

To enable software encryption with a group policy, [follow the instructions found on lifehacker.com](https://lifehacker.com/how-to-switch-to-software-encryption-on-your-vulnerable-1830289471)

In Bitlocker manage page, click on the text "Turn on BitLocker" to turn on and enable BitLocker and follow the recommendations for drive encryption.

### Full Disk Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your encryption software password. You'll have multiple options to retain a recovery key so choose the best option for you. Losing this recovery key means you cannot log in and everything on your computer is 100% inaccessible.
- After encryption completes, any corruption that makes the Windows 10 partition unaccessible has no recovery. Make sure you're both backing up using a local backup device such as [Windows 10 Backup](https://support.microsoft.com/en-us/windows/back-up-and-restore-with-windows-backup-87a81f8a-78fa-456e-b521-ac0560e32338) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Sync](https://www.sync.com), or [iDrive](https://www.idrive.com).

### Clean up Windows

Whether you're using a company's provided Windows with third-party software or you installed Windows from scratch yourself, it's always a good idea to have a minimal and clean base installation with as few potential issues as possible.

There are a ton of community driven starter packages and files to help clean up and uninstall unnecessary software and files. [Tron](https://github.com/bmrf/tron) is an all-inclusive from start to finish that touches everything from uninstall software to cleaning up temp files and checking for malware. It can easily take a minimum of 2-3 hours because of how thorough it is, but I often just run specific sections that I think are necssary to clean up Windows, such as removing default apps or blocking telemetry.

I've also included additional scripts and information on cleaning up and updating Windows in the [Boxstarter](#chocolatey-and-boxstarter) and [Privacy](#privacy) sections below.

### Windows reinstall tasks

Refreshing Windows often means taking an existing machine, wiping the drive and starting again from scratch. This also means you have to back up and reload what you had. LifeHacker has a [guide for backing up and restoring a previous Windows installation](https://lifehacker.com/the-ultimate-guide-to-reinstalling-windows-from-scratch-1832897572).

### .NET

An important dependency for application installation and IIS is **.NET Framework**. If you don't install it now, you'll likely be prompted to during other software installation in the future.

**Control Panel > Turn Windows features on or off**

## Projects Directory

If you don't already have one, create a directory for your digital projects. I like to use `C:\Users\<winusername>\Sites\<project-name>`, and the name *Sites* can be anything you want. I prefer my *Sites* folder to exist along side the rest of my user profile folders.

    cd c:\Users\<winusername>
    mkdir -p Sites

Depending on the type of projects you work on, this might not be necessary or preferable.

## Chocolatey and Boxstarter

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for Windows is [Chocolatey](https://communitychocolatey.org/).

### Install

([Ensure Get-ExecutionPolicy is at least RemoteSigned](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.5&viewFallbackFrom=powershell-7.1)). 

PowerShell.exe (Right-click, Run as Administrator)

Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass -Scope Process`.

    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

### Usage

Chocolatey can be used to install applications with either `choco install`.

```bash
choco install <package>
choco install <package> --version=x.x.x # specific version (replace x with version number)
```

Replace `<package>` with the name of the package you want to install.

To view Chocolatey's directory of packages:

(https://community.chocolatey.org/packages)[https://community.chocolatey.org/packages]

Other useful commands:

```bash
choco upgrade <package>
choco list --localonly # what's installed on your local machine, including version numbers
choco outdated # which installs are not current
```

> [!NOTE]
> `choco outdated` does not stay in sync with evergreen software using the free version of Chocolatey. For example, if you use Chocolatey to install Firefox version 135, and Firefox upgrades itself to version 136, Chocolatey doesn't know that Firefox updated itself and the `outdated` command only remembers the version of Firefox you installed. I use `outdated` to see what the newest versions are available rather than what I have installed. In order to track local installation versions, [Chocolatey provides this feature in the paid upgrade](https://docs.chocolatey.org/en-us/features/package-synchronization/) of Chocolatey.

### Installing multiple applications

Chocolatey is awesome because now that you understand what it does, you can install all your favorite apps in one command. Here's a list of my favorite apps, including Firefox, that I need for development on a regular basis.

    choco install firefox firefox-dev brave ungoogled-chromium tor-browser powertoys thunderbird slack skype zoom git nvm sublimetext4 vscodium filezilla mullvad-app vlc virtualbox vnc-viewer docker-desktop malwarebytes qbittorrent quicklook libreoffice-fresh wireguard freetube --pre -y


> [!NOTE]
> if you use Boxstarter, you can include the above line inside your Powershell script and run everything together.

Using Chocolatey to install software will sometimes require you to restart Windows to complete its installation. Several packages will auto-start the software and ask you to set them up as well.

### Boxstarter

The folks at Chcolatey provide new tool called [Boxstarter](http://www.boxstarter.org/) to automate a Windows setup process. The important reasons to use this tools are for resilient reboot and Windows customization.

```bash
# Install boxstarter
choco install boxstarter
```

Using boxstarter requires some step by step instructions to create an executable file all found on the website above. To get you started, I recommend starting with [this already created .ps1 file](https://gist.github.com/asuh/ba46d8c534b13ae4db89b4d3323eec97).

Note: you will have to restart Windows to complete some of the installations.

## Ninite

If you have more software you need installed not included in Chocolatey or you wish to use something that has a GUI, [Ninite](https://ninite.com/) is an awesome addition.

## Privacy

I think now is the time to briefly let you know that Windows 10 communicates with Microsoft by default. Microsoft collects data on how you use the operating system through a process called telemetry. There's not a lot of transparency about what's going on but there are many free and open source applications that help us shut down and block as many as we know about.

First, I recommend you look through [https://www.privacyguides.org/en/](https://www.privacyguides.org/en/). There's a ton of valuable software and links to consume.
Second, [consult this evergreen, updated list of privacy tools for Windows 10](https://www.ghacks.net/2015/08/14/comparison-of-windows-10-privacy-tools/). As of June 2018, most of the content is updated and relevant. Run any scripts you feel will help you.

## Windows Subsystem for Linux

Web development is not a sole operating system or browser process. WSL gives us more tools such as using native Unix commands within Windows. This is a big deal if you come from a Unix based OS and want to feel more comfortable on Windows.

Let's [install WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install), the latest version of WSL. You must be on the latest version of Windows 10 or Windows 11.

Powershell.exe (Run as Administrator)

    wsl --install
    
Now it's time to choose your Linux distro, which I recommend the [lastest version of Ubuntu](https://apps.microsoft.com/detail/9NBLGGH4MSV6?hl=en-us&gl=us)

### Windows Terminal

Once WSL is installed, as you'll see below, we're going to use this container to harbor the various languages and packages needed for web development. Before we continue, let's use one more highly recommended program called *Windows Terminal*. As of Windows 11, this is included by default. Windows 10 22H2 and newer should also now have it installed by default.

    choco install microsoft-windows-terminal

This app gives us one place to use all Windows command line apps: Command Prompt, Powershell, and WSL default shell. Any time I use this, I search for it and right-click to open as Administrator.

Next, I'd recommend installing cURL in your distro for installing other Linux packages too. So for this next command, let's open Windows Terminal and go to the shell for your distro (Ubuntu in my case).

    sudo apt-get install curl

For more detailed instructions for installing WSL on Windows 10, [here's detailed guide with more front-end minded advice](https://www.smashingmagazine.com/2019/09/moving-javascript-development-bash-windows/).

### WSL performance can suffer

WSL has two caveats that I can't overcome yet both decreasing performance.

I have my environment set up, using Windows Terminal or another terminal in VSCodium. I run `npm` script or just trying to `git status` my project and it TAKES FOREVER. It's one of the worst issues I've had with WSL. There are fixes slowly being added to WSL or Windows but it's a slow rollout. In the meantime, if you have issues with `npm`, `webpack`, `gulp`, `git`, or one of many other utilities, try using Powershell or Git Bash (if you installed git for Windows).

Another issue that can cause a headache is the virtual machine memory task called `Vmmem`. After running WSL and Docker for more than a week, I saw the memory usage went up to more almost 10GB. At first I thought it was just Docker, but after shutting that down, I still saw about 8GB or memory usage. Only after using powershell to `wsl --shutdown` did I see the memory usage drop to a decent level below 1GB.

### Projects Directory

One of the first things we need to figure out is how to access our projects within Windows.

Here's the windows folder structure for the Home directory:

    C:\Users\<winusername>\

WSL maps that to this:

    /mnt/c/Users/<winusername>

Using the Projects directory I created above, if I store all my projects in `C:\Users\micah\Sites`, in bash I'll get there using this command:

    cd /mnt/c/Users/micah/Sites

Once you create a new folder inside of your Projects directory, this will become your home for that project. It's where you'll install any package managers you might need such as NPM and Composer.

### Security

Two of the most common commands to set up new projects and tools are `apt-get` and `sudo`. Knowing this, it's imperative to keep the system and software up-to-date and patched.

Once WSL is installed, update the `apt-get` tool as well as packages so that it updates the package lists for app updates.

```bash
sudo apt update && sudo apt upgrade # apt and apt-get are interchangable
```

`apt update` synchronizes your list of available packages with the servers in source repositories. 
`apt upgrade` then downloads & installs any newer versions of your installed programs.

It's a good habit to get into that every time you use `apt-get`, run this update first. You can combine the two using the following:

    sudo apt update && apt-get <package>

### Environment confusion

One last thing to mention about WSL inside of Windows, using these environments means you're effectively working with one environment in Windows and another in Linux. In this respect, it's the same for the various terminals. Powershell works in the Windows environment and Ubuntu shell works in the Ubuntu environment.

I've had to install packages on both Windows and Ubuntu as well. So I'll have git installed in Ubuntu and then also in Windows and they're not related. Same with Node.js. And so on.

When using your favorite editor, it's a similar situation. Opening the project in your Windows folder means you're working with it in the Windows file system. It could be faster to put the project folder/files in the Ubuntu folder. Mount the Ubuntu file system as a mapped drive which will make things a bit easier to work with from Windows.

Long story short, this is a sad reality that will take some maturing to understand. You'll have to fiddle around with Window to make it work well sometimes.

## AI

As the 2020s move forward, it is clear that AI is neessary for many tasks. Used properly, it can give feedback that could be expected from senior developers and management. It can give immediate feedback and help that can increase productivity.

Using many code editors, you can integrate many different services like [Github CoPilot](https://github.com/features/copilot) using models from [OpenAI](https://openai.com/), [Google](https://gemini.google.com), or [Anthropic](https://www.anthropic.com/).

I can recommend [T3 Chat](https://t3.chat) as the most affordable service to use any of the above AI and more as standalone chat services. You can have extensive conversations and get code samples as feedback from the conversations.

## Sublime Text and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of two editors: [Sublime Text](https://www.sublimetext.com/) or [VSCode](https://code.visualstudio.com/).

### VSCode

As of 2025, most of my time is spent in a fork of Visual Studio Code. It found popularity at the end of the 2010s and is a staple open-source code editor for most front-end developers. I use it both personally and professionally because of various built-in features like git support, terminal integration, live sharing your code with another developer (using an extension), and a large repository of great extensions. 

    choco install vscode

#### VSC Forks

I recommend using [VSCodium](https://vscodium.com/) because it strips away the telemetry and tracking that Microsoft/Github integrates into VSCode. However, [VSCodium has issues with extensions](https://github.com/VSCodium/vscodium/wiki/Extensions-Compatibility) like Live Share, so keep that in mind.

    choco install vscodium

There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics).

Because of the rise of AI since CoPilot and the release of ChatGPT, there are multiple projects that take an approach treating AI as a primary integration. I've tried multiple of these editors and they're interesting and helpful if you are good at your craft and can request the right approaches.

[Chris Coyier wrote about various code editors in 2025](https://frontendmasters.com/blog/notes-on-the-code-editors-with-ai-landscape/).

### Sublime Text

My preferred editor is Sublime Text.

    choco install sublimetext4

I prefer using the [beta version of Sublime Text 4](https://www.sublimetext.com/dev) which is usually just as stable as version 3.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](https://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:

- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

My favorite theme is [Material Design Darker](https://github.com/material-theme/vsc-material-theme) as well as [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for your preferred theme, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines (using Seti UI) and restart Sublime:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

#### Settings

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code (and edit it if you want) from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

[I also recommend creating shortcuts so you can launch Sublime Text from the command-line](https://www.sublimetext.com/docs/command_line.html)
    
Now, we can open a file with `subl myfile.html` or start a new project in the current directory with `subl .`. Pretty cool!

## Vim

It is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular open-source editor accessed using command-line shells and is usually pre-installed on Unix systems.

For example, when you run a `git commit`, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. After that it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you're like me and just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

First, install [pathogen.vim](https://github.com/tpope/vim-pathogen) by running:

    mkdir -p ~/.vim/autoload ~/.vim/bundle && \
    curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim

If you're brand new to Vim and lacking a vimrc, vim ~/.vimrc and paste in the following super-minimal example:

    execute pathogen#infect()
    syntax on
    filetype plugin indent on

And finally, install the Vim "sensible defaults" by running:

    cd ~/.vim/bundle && \
    git clone git://github.com/tpope/vim-sensible.git

With that, Vim will look a lot better next time you open it!

## ZSH

[Z Shell](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH), or ZSH, was written to extend Bash and make improvements to how Bash works. Install ZSH on WSL:

    apt install zsh

Don't forget to customize ZSH!

[Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) are available.
[Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) and [Syntax Highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) will improve ZSH user experience too.

[Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful. Or a [free 80 minute video on YouTube by Karl Hadwen](https://www.youtube.com/watch?v=MSPu-lYF-A8).

## SSH

SSH is imperative, just like git and node as you'll see.

[Github has excellent instructions for setting up git and connecting it to a Github account](https://docs.github.com/en/get-started/quickstart/set-up-git). This will help you to install the repos to your computer from Github as well as set up keys that you'll need to connect git and github.

Now you can add a little shortcut to make SSHing into other boxes easier. Paste the following block of code into your SSH config file at `~/.ssh/config`, changing the variables for any hosts that you connect to.

```ssh
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

Below the above, you can add other sites as needed.

```ssh
Host myssh
  HostName example.com
  User user
  IdentityFile ~/.ssh/key.pem
```

With the above code, you can now run the alias `myssh` to connect.

    ssh myssh

## Git

What's a developer without [Git](http://git-scm.com/)? To install, from **WSL command line** run:

    sudo apt install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"
    git config --global core.autocrlf input # Set to change how Git handles line endings

If you have [2FA enabled on Github](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) (you should), you’ll also need to follow the [Add SSH Key to Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=linux) and be sure you’re using the Linux instructions.

### Git aliases (optional)

Less keystrokes can be better, so here are optional and ensible shortcuts to a global Git config file.

    touch ~/.gitconfig
    
Pick and choose any of these aliases to help you.

    [user]
      name   = Firstname Lastname
      email  = you@example.com
    [github]
      user   = username
    [alias]
      a      = add
      ca     = commit -a
      cam    = commit -am
      cm     = commit -m
      s      = status
      pom    = push origin master
      pog    = push origin gh-pages
      puom   = pull origin master
      puog   = pull origin gh-pages
      cob    = checkout -b
      co     = checkout
      fp     = fetch --prune --all
      l      = log --oneline --decorate --graph
      lall   = log --oneline --decorate --graph --all
      ls     = log --oneline --decorate --graph --stat
      lt     = log --graph --decorate --pretty=format:'%C(yellow)%h%Creset%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)%an%Creset'

With the above aliases, I can run `git s` instead of `git status` or `git ca` instead of `git commit -a` when I have a bunch of file updates.

If you used Chocolatey above to install applications, you'll notice this is the 2nd time we're installing Git; it is required for Visual Studio Code and is available for other local Windows apps.

## Node.js

For modern Javascript programming, Node.js is required. Using [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm/) to install Node allows you to easily switch between Node versions and is useful for projects on different versions of Node.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

### nvm usage

When you enter a project, you can install Node using NVM.

    nvm install node

Restart terminal and run the final command.

    nvm use node

Set a default version of Node.

    nvm alias default xx.xx

Confirm that you are using the default version of Node and npm.

    node -v && npm -v

You can switch to another version of Node and use it by changing to the directory where you want to use Node and run the following.

    nvm install xx.xx
    nvm use xx.xx

Node modules are defined in a local `package.json` file inside your project. `npm install` will download external libraries and frameworks into each project's own `node_modules` folder by default. You'll never need to edit files in this folder, only reference them.

Update NVM

    nvm install node --reinstall-packages-from=node

## Python

Even if you don't use Python in your day to day, it's likely you'll encounter something that requires it. Older WSL distros probably include Python 2.x by default. Version 2 is years outdated and almost all python can and should run with a minimum of version 3. Let's update python!

Even if you already installed it with `choco`, let's install the latest python using Windows Terminal on your **WSL command line**. (Remember to update `apt` if  you're doing this at a later date)

    sudo apt update python3

After installation, open a new terminal tab or window to make sure it installed correctly.

    python --version

### Pip

With `python` now running the latest version, it's a good idea to install Pip, which is a package manager for Python.

    sudo apt install python3-pip
    python get-pip.py --user

## Composer

PHP is still one of the most used programming languages on the web, thanks in part to the amount of sites still using WordPress. We need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer. It would be preferable to install this inside WSL instead of Windows.

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) to install the environments above.

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    choco install virtualbox

## Docker

Similar to Vagrant, I increasingly use Docker for professional projects. It comes with similar benefits to Vagrant, such as portability, encapsulation for the environment within the OS, and consistent environments. Docker goes a little further because it's a container manager it's lighter in resources and file size than Vagrant.

    choco install docker-desktop
    
Docker can be quite powerful but complicated to set up. For this reason, I'm a fan of another project which is a wrapper around Docker called [Lando](https://lando.dev/). Originally designed for Drupal, it's increased support for many other environments including WordPress, Node.js, and Laravel. You can find the [latest executable file on Github](https://github.com/lando/lando/releases).
    
For privacy, I recommend disabling tracking. Inside of your `.lando.yml` file, add the following:

    stats:
      - report: false
        url: https://metrics.lando.dev

## TODO
- Allow apps downloaded from anywhere system preferences

## Credits

- [Front-End OS X](https://github.com/asuh/front-end-osx)
- [Webdev on Windows with WSL and VS Code](https://daverupert.com/2018/04/developing-on-windows-with-wsl-and-visual-studio-code/)
- [How to Install WSL 2 on Windows 10 (Updated)](https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10)
- [Nodejs and npm setup on WSL](https://evancalz.medium.com/nodejs-and-npm-setup-on-wsl-a4fa06ad7c0)
- [Windows-as-a-nuisance: How I clean up a “clean install” of Windows 11 and Edge](https://arstechnica.com/gadgets/2024/02/what-i-do-to-clean-up-a-clean-install-of-windows-11-23h2-and-edge/)
