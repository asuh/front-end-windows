# Front-End Development Setup on a PC

This document assumes you're running a fresh copy of the latest version of **Windows 10**, >=version 1909.

The following workflow assumes a clean installation of Windows 10, whether from Signature PC or a full manual reinstall from a vendor laptop. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a bare Windows 10 system.

- [Command Line Interface](#command-line-interface)
- [Windows Prepartion](#system-update-and-disk-encryption)
- [Projects Directory](#projects-directory)
- [.NET Framework](#net)
- [Chocolatey and Boxstarter](#chocolatey-and-boxstarter)
- [Ninite](#ninite)
- [Privacy](#privacy)
- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
- [Sublime Text, Atom, and VSCode](#sublime-text-atom-and-vscode)
- [Vim](#vim)
- [ZSH](#zsh)
- [SSH](#ssh)
- [Git](#git)
- [Node.js](#nodejs)
- [SSH](#ssh)
- [ES6](#es6)
- [Sass](#sass)
- [PHP and Composer](#php-and-composer)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [Docker](#docker)

## Command Line Interface

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

```bash
sudo command -flag --flag directory file.extention # Comments are behind pound signs
```

Anytime you see the above, it is referring to your CLI of choice, whether it's the built-in Command Prompt or Powershell as well as Bash Shell or Z Shell [see below](#zsh) in your [Windows Subsystem for Linux](#windows-subsystem-for-linux). You might also use a third-party application like [Cmder](https://cmder.net/), [Cygwin](https://www.cygwin.com/) or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/).

Front-end development has increasingly moved towards an open-source driven, command-line interface (CLI) dependent workflow. Whether we access modules, packages or simply useful commands, setting up a command-line shell to your liking is a good idea.

## System update and Disk Encryption

Step One - Update the system!

**Windows Key > Settings > Update & Security**

Step Two - Turn on BitLocker for full disk encryption (read 2018/11 Addendum below before continuing)

(If you can't install Bitlocker on Windows 10 Home, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded by the security community.)

**Control Panel\System and Security\BitLocker Drive Encryption**

Note: As of November 2018, it is recommended to disable hardware encryption and force Bitlocker to use Software Encryption because of [research that reveals vulnerabilities](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180028).

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
- Make sure you do not forget your BitLocker or encryption software password. You'll have multiple options to retain a recovery key so choose the best option for you. Losing this recovery key means you cannot log in and everything on your computer is 100% inaccessible.
- After encryption completes, any corruption that makes the Windows 10 partition unaccessible has no recovery. Make sure you're both backing up using a local backup device such as [Windows 10 Backup](https://support.microsoft.com/en-us/help/17143/windows-10-back-up-your-files) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Sync](https://www.sync.com), or [iDrive](https://www.idrive.com).

### Clean up Windows

Whether you're using a [https://www.microsoft.com/en-gd/store/b/signaturepcs](Signature PC from Microsoft), a manufacturer's Windows installation with third-party software or you installed Windows from scratch yourself, it's always a good idea to have a minimal and clean base installation with as few potential issues as possible.

There are a ton of community driven starter packages and files to help clean up and uninstall unnecessary software and files. [Tron](https://github.com/bmrf/tron) is an all-inclusive from start to finish that touches everything from uninstall software to cleaning up temp files and checking for malware. It can easily take a minimum of 2-3 hours because of how thorough it is.

I've also included additional scripts and information on cleaning up and updating Windows in the [Boxstarter](#chocolatey-and-boxstarter) and [Privacy](#privacy) sections below.

### Windows reinstall tasks

Refreshing Windows often means taking an existing machine, wiping the drive and starting again from scratch. This also means you have to back up and reload what you had. LifeHacker has a [guide for backing up and restoring a previous Windows installation](https://lifehacker.com/the-ultimate-guide-to-reinstalling-windows-from-scratch-1832897572).

## Projects Directory

If you don't already have one, create a projects directory. I like to use `C:\Users\<winusername>\Sites\<project-name>`. I prefer my Sites folder to exist along side the rest of my user profile folders.

    cd c:\Users\<winusername>
    mkdir -p Sites

Depending on the type of projects you work on, this might not be necessary or preferable.

## .NET

An important dependency for application installation and IIS is **.NET Framework**.

**Control Panel > Turn Windows features on or off**

If not already installed, choose to install both .NET Framework 3.5 and .NET Framework 4.7.x.

## Chocolatey and Boxstarter

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for Windows is [Chocolatey](https://chocolatey.org/).

### Install

([Ensure Get-ExecutionPolicy is at least RemoteSigned](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6&viewFallbackFrom=powershell-Microsoft.PowerShell.Core)). 

PowerShell.exe (Right-click, Run as Administrator)

Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass -Scope Process`.

    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

### Usage

Chocolatey can be used to install applications with either `choco install` or `cinst`. They are interchangable.

```bash
choco install <package>
choco install <package> --version=x.x.x # specific version (replace x with version number)
```

Replace `<package>` with the name of the package you want to install.

To view Chocolatey's directory of packages:

[https://chocolatey.org/packages](https://chocolatey.org/packages)

Other useful commands:

```bash
choco upgrade <package>
choco list --localonly # what's installed on your local machine, including version numbers
```

Note: `choco outdated` does not stay in sync with evergreen software using the free version of Chocolatey. For example, if you use Chocolatey to install Firefox version 90, and Firefox upgrades itself to version 91, Chocolatey doesn't know that Firefox updated itself and the `outdated` command only remembers the version of Firefox you installed. In order to sync this up, [Chocolatey provides this feature in the paid upgrade](https://chocolatey.org/docs/features-synchronize) of Chocolatey.

### Installing multiple applications

Chocolatey is awesome because now that you understand what it does, you can install all your favorite apps in one command. Here's a list of my favorite apps, including Firefox, that I need for development on a regular basis.

Note: if you use Boxstarter, you can include the following line inside your Powershell script and run everything together.

    choco install firefox microsoft-edge-insider-dev brave tor-browser microsoft-windows-terminal powertoys rainmeter thunderbird slack skype zoom git nvm sublimetext3 atom vscodium filezilla notepadplusplus vlc virtualbox vagrant docker-desktop malwarebytes qbittorrent quicklook authy-desktop libreoffice-fresh wireguard --pre -y

Note: Most modern browsers contain PDF readers like Adobe Reader and sometimes Flash and Java by default. Running standalone versions of each is not recommended because they are a security risk without regular maintenance and updates.

Using Chocolatey to install software like Vagrant will require you to restart Windows for Vagrant to complete its installation. Several packages will auto-start the software and ask you to set them up as well.

### Boxstarter

The folks at Chcolatey provide new tool called [Boxstarter](http://www.boxstarter.org/) to automate a Windows setup process. The important reasons to use this tools are for resilient reboot and Windows customization.

```bash
# Install boxstarter
. { iwr -useb https://boxstarter.org/bootstrapper.ps1 } | iex; get-boxstarter -Force
# Run your boxstarter script
Install-BoxstarterPackage -PackageName https://gist.githubusercontent.com/asuh/ba46d8c534b13ae4db89b4d3323eec97/raw/1a4c9d55dce2909218919b16ed567cf16d6c85a6/boxstarter.ps1 -DisableReboots
```

Using boxstarter requires some step by step instructions to create an executable file all found on the website above. To get you started, I recommend starting with [this already created .ps1 file](https://gist.github.com/asuh/ba46d8c534b13ae4db89b4d3323eec97).

Note: you will have to restart Windows to complete some of the installations.

## Ninite

If you have more software you need installed not included in Chocolatey or you wish to use something that has a GUI, [Ninite](https://ninite.com/) is an awesome addition.

## Privacy

I think now is the time to briefly let you know that Windows 10 communicates with Microsoft by default. Microsoft collects data on how you use the operating system through a process called telemetry. There's not a lot of transparency about what's going on but there are many free and open source applications that help us shut down and block as many as we know about.

First, I recommend you look through [PrivacyTools.io](https://www.privacytools.io/). There's a ton of valuable software and links to consume.
Second, [consult this evergreen, updated list of privacy tools for Windows 10](https://www.ghacks.net/2015/08/14/comparison-of-windows-10-privacy-tools/). As of June 2018, most of the content is updated and relevant. Run any scripts you feel will help you.

## Windows Subsystem for Linux

Web development is not a sole operating system or browser process. WSL gives us more tools such as using native Unix commands within Windows. This is a big deal if you come from a Unix based OS and want to feel more comfortable on Windows.

Let's [install WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install-win10), the latest version of WSL. You must be on the latest version of Windows 10, as far back as Version 1903.

Powershell.exe (Run as Administrator)

    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

Next, enable the Virtual Machine Platform.

    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

(or if you're on version 1903 or 1909)

    Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
    
Once you restart, set WSL 2 as the default.

    wsl --set-default-version 2
    
Now it's time to choose your Linux distro, which I recommend the [lastest version of Ubuntu](https://www.microsoft.com/store/apps/9n6svws3rx71)

Once installed, as you'll see below, we're going to use this container to harbor the various languages and packages needed for web development. I'd recommend install cURL for installing other packages too.

    sudo apt-get install curl

For more detailed instructions for installing WSL on Windows 10, [here's detailed guide with more front-end minded advice](https://www.smashingmagazine.com/2019/09/moving-javascript-development-bash-windows/).

### Projects Directory

One of the first things we need to figure out is how to access our projects within Windows.

Here's the windows folder structure for the Home directory:

    C:\Users\<winusername>\

WSL maps that to this:

    /mnt/c/Users/<winusername>

Using the Projects directory I created above, if I store all my projects in `C:\Users\Micah\Sites`, in bash I'll get there using this command:

    cd /mnt/c/Users/Micah/Sites

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

## Sublime Text, Atom, and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of three editors: [Sublime Text](https://www.sublimetext.com/), [VSCode](https://code.visualstudio.com/) or [Atom](https://atom.io/).

### Sublime Text

My preferred editor is Sublime Text 3.

    choco install sublimetext3

I prefer using the [beta version of Sublime Text 3](https://sublimetext.com/3) which is usually just as stable as version 2.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](https://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:

- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

My favorite theme is [Material Design Darker](https://github.com/equinusocio/material-theme) and a close second to [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for your preferred theme, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines (using Seti UI) and restart Sublime:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

#### Settings

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your preference. When done, save the file and close it.

[Let's create a shortcut so we can launch Sublime Text from the command-line](https://scotch.io/tutorials/open-sublime-text-from-the-command-line-using-subl-exe-windows) 
    
    1. Open Powershell as Administrator
    2. Type `sysdm.cpl` and press enter and go to System Properties > Advanced System Settings > Advanced > Environment Variables
    3. Create a New System Variable Create a new system variable called SUBLIME that will point to the folder of your Sublime installation.
    4. Add the System Variable to Your PATH Add the following to the end of your PATH variable: `;%SUBLIME%`

![Add System variable](https://i.imgur.com/pXTqfL8.jpg)

Now I can open a file with `subl myfile.html` or start a new project in the current directory with `subl .`. Pretty cool!

### Atom

If you like Sublime Text but can't afford or don't want to pay for it, [Atom](https://atom.io/) is an open-source editor in the spirit of Sublime Text that has a healthy community and regular updates.

The main problem for Atom as of autumn 2018 is that large files and projects noticeably slow down Atom's performance.

### VSCode

Visual Studio Code found popularity at the end of the 2010s and has become a staple open-source code editor for many front-end developers. I use it both personally and professionally because of various built-in features like git support, terminal integration and a similar-to-Sublime-Text repository of great plugins. I recommend using VSCodium, as it strips away the tracking that Github integrates into VSCode.

    choco install vscodium

There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

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

[Github has excellent instructions for setting up git and connecting it to a Github account](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/set-up-git). This will help you to install the repos to your computer from Github as well as set up keys that you'll need to connect git and github.

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
    git config --global core.autocrlf input

The `core.autocrlf=input` is used to change how Git handles line endings. 

If you have [2FA enabled on Github](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/configuring-two-factor-authentication) (you should), you’ll also need to follow the [Add SSH Key to Github](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account#platform-linux) and be sure you’re using the Linux instructions.

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
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
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

You can switch to another version and use it by changing to the directory where you want to use Node and run the following.

    nvm install xx.xx
    nvm use xx.xx

Node modules are defined in a local `package.json` file inside your project. `npm install` will download external libraries and frameworks into each project's own `node_modules` folder by default. You'll never need to edit files in this folder, only reference them.

Update NVM

    nvm install node --reinstall-packages-from=node

## ES6

Javascript libraries such as React and Vue use modern versions of Javascript starting with ECMAScript 2015 (ES6). Browser quirks that gave rise to jQuery are less problematic because web standards are regularly implemented and iterated. Thus, a golden age of Javascript is upon us.

Until browsers catch up implementing the newest features of ES6, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/). Only install this locally into an already created project if you need ES5 support. Check your project, it might already use Babel and you can skip this step.

    npm install --save-dev babel-cli babel-preset-env

Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`

## Sass

Install your preprocessor of choice, but I recommend using either Sass (or maybe PostCSS). They all do the same thing but Sass has the most momentum since the end of the 2010s.

With **WSL command line**:

    npm install -g sass

Keep in mind that the utility that Sass offers is slowly being complimented and deprecated with the rise of [CSS variables](https://www.freecodecamp.org/news/everything-you-need-to-know-about-css-variables-c74d922ea855/).

## PHP and Composer

PHP is still one of the most used programming languages on the web, thanks in part to the amount of sites still using WordPress. We need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer. It would be preferable to install this inside WSL instead of Windows.

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Virtual PC](https://www.microsoft.com/en-us/download/details.aspx?id=3702) to install the environments above.

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    choco install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Microsoft's VM site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).

## Vagrant

I have personally tried to move away from packaged development environments like MAMP. The alternative I like is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built-in connection to your local OS so that you develop in Windows but the environment runs in the VM.

    choco install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, by reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    vagrant up

A great box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## Docker

Similar to Vagrant, I increasingly use Docker for professional projects. It comes with similar benefits to Vagrant, such as portability, encapsulation for the environment within the OS, and consistent environments. Docker goes a little further because it's a container manager it's lighter in resources and file size than Vagrant.

    brew cask install docker
    
Docker can be quite powerful but complicated to set up. For this reason, I'm a fan of another project which is a wrapper around Docker called [Lando](https://lando.dev/). Originally designed for Drupal, it's increased support for many other environments including WordPress, Node.js, and Laravel.

    brew cask install lando
    
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
