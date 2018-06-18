# Front-End Development Setup on a PC

This document assumes you're running a fresh copy of the latest version of **Windows 10**, >=version 1803.

The following workflow assumes a clean installation of Windows 10, whether from Signature PC or a full manual reinstall from a vendor laptop. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a bare Windows 10 system.

- [Windows Prepartion](#system-update-and-disk-encryption)
- [Projects Directory](#projects-directory)
- [.NET Framework](#net)
- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
- [Chocolatey and Boxstarter](#chocolatey-and-boxstarter)
- [Git](#git)
- [Node.js](#nodejs)
- [Ninite](#ninite)
- [ZSH](#zsh-optional)
- [Sass](#sass)
- [Sublime Text, Atom, and VSCode](#sublime-text-atom-and-vscode)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [ES6](#es6)
- [PHP and Composer](#php-and-composer)
- [Apps](#apps)

## System update and Disk Encryption

Step One - Update the system!
**Windows Key > Settings > Update & Security**

Step Two - Turn on BitLocker (Windows 10 Pro and above)
**Control Panel\System and Security\BitLocker Drive Encryption**

Click on the text "Turn on BitLocker" to turn on and enable BitLocker. On a brand new machine or Windows 10 installation, it will take some time to fully encrypt the disk.

### Full Disk Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your BitLocker password. You'll have four options to retain a recovery key so choose the best option for you. Losing this recovery key means you cannot log in and everything on your computer is 100% inaccessible.
- After encryption completes, a corruption that makes the Windows 10 partition unaccessible has no recovery because of the level of encryption. Make sure you're both backing up using a local backup device such as [Windows 10 Backup](https://support.microsoft.com/en-us/help/17143/windows-10-back-up-your-files) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Carbonite](https://carbonite.com/), or [iDrive](https://www.idrive.com).

## Projects Directory

If you don't already have one, create a projects directory. I like to use `C:\Users\{yourusername}\Sites\[project-name]`. I prefer my Sites folder to exist along side the rest of my user profile folders.

    mkdir -p Sites

Depending on the type of projects you work on, this might not be necessary or preferable.

## .NET

An important dependency for application installation and IIS is **.NET Framework**.

**Control Panel > Turn Windows features on or off**

Choose to install, if not already installed, both .NET Framework 3.5 and .NET Framework 4.7.x.

## Windows Subsystem for Linux

Web development is not a sole operating system or browser process. WSL gives us more tools such as using native Unix commands within Windows. This is a big deal if you come from a Unix based OS and want to feel more comfortable on Windows.

Powershell.exe (Run as Administrator)

    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

After a restart, install your preferred version of Linux Distribution, Ubuntu being the most popular choice. Here's a [complete set of instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10) from Microsoft to install WSL.

## Chocolatey and Boxstarter

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for Windows is [Chocolatey](https://chocolatey.org/).

### Install

Two ways (Run as Administrator):

Cmd.exe

    C:\ @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
    
For PowerShell, ([Ensure Get-ExecutionPolicy is at least RemoteSigned](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6&viewFallbackFrom=powershell-Microsoft.PowerShell.Core)). 

PowerShell.exe 

    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))


### Usage

Chocolatey can be used with either `choco install` or `cinst`. They are interchangable.

To [install a package](https://chocolatey.org/docs/commands-install) simply type:

    choco install <package>

Replace `<package>` with the name of the package you want to install.

To view Chocolatey's directory of packages:

[https://chocolatey.org/packages](https://chocolatey.org/packages)

To update a package:

    choco upgrade <package>

To install a specific version of a package (replace x with version number):

    choco install <package> --version=x.x.x

To see if any of your packages need to be updated:

    choco outdated

To see what you have installed (with their version numbers):

    choco list --a

### Installing multiple applications

Chocolatey is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis.

Note: if you use Boxstarter, you can include the following line inside your Powershell script and run everything together.

    choco install nodejs git googlechrome chromium firefox opera brave vivaldi tor-browser thunderbird slack atom vscode openvpn cmder notepadplusplus github sourcetree vlc filezilla virtualbox vagrant malwarebytes superantispyware qbittorrent authy-desktop boxstarter -y

Note: Google Chrome and Microsoft Edge contain Adobe Reader, Flash and Java by default. Running standalone versions of each is not recommended because they are a security risk without regular maintenance and updates.

### Boxstarter

The folks at Chcolatey provide new tool called [Boxstarter](http://www.boxstarter.org/) to automate a Windows setup process. The important reasons to use this tools are for resilient reboot and Windows customization.

    cinst boxstarter

Using boxstarter requires some step by step instructions to create an executable file all found on the website above. To get you started, I recommend starting with [this already created .ps1 file](https://gist.github.com/jessfraz/7c319b046daa101a4aaef937a20ff41f).

## Git

What's a developer without [Git](http://git-scm.com/)? To install, from your WSL command line run:

    sudo apt install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"
    git config --global core.autocrlf input

The core.autocrlf=input setting is pretty crucial. It can break things you install over git (like rbenv). If you have 2FA enabled on Github (you should), you’ll also need to follow the [Add SSH Key to Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-linux) and be sure you’re using the Linux instructions.

## Node.js

Install [Node.js](http://nodejs.org/) with WSL command line:

    sudo apt install nodejs

This also installs the [npm](https://npmjs.org/) package manager. 

Once installation is complete, restart your command line application so that you can verify that Node and NPM are correctly installed:

    node --version
    npm --version

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least two that are worth installing globally if you know you'll use them. Those are [TypeScript](http://www.typescriptlang.org/), [Gulp](http://gulpjs.com/):

    npm install typescript
    npm install --global gulp

### Npm usage

To install a package:

    npm install <package> # Install locally
    npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    npm install <package> --save

To see what's installed:

    npm list # Local
    npm list -g # Global

To find outdated packages (locally or globally):

    npm outdated [-g]

To upgrade all or a particular package:

    npm update [<package>]

To uninstall a package:

    npm uninstall <package>

## Ninite

If you have more software you need installed not included in Chocolatey or you wish to use something that has a GUI, [Ninite](https://ninite.com/) is an awesome addition.

## ZSH (optional)

By default, Windows comes with three command line tools: Command Prompt, Bash for Windows, and Powershell. Powershell is a task automation and configuration management framework that includes command-line shell.

Because many tools recommended can be used with Bash, which contains the command language used to interact with Unix functions, such as `pwd`, `ls`, and `cd`, it is recommended to either use [Bash on Unbuntu on Windows](https://msdn.microsoft.com/en-us/commandline/wsl/about) or use something like [Cmder](http://cmder.net/). Cmder is a powerful console emulator that provides a shell for Bash.

[Z Shell](https://en.wikipedia.org/wiki/Z_shell), or ZSH, was written to extend Bash and make improvements to how Bash works. One of the most popular frameworks written around ZSH is called [Oh My Zsh!](http://ohmyz.sh/).

Install it with the following command:

    curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

Now you have ZSH installed. [Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful.

## Sass

Install your preprocessor of choice, but I highly recommend using Sass. They all do the same thing but Sass has the most momentum behind it right now.

With WSL command line:

    npm install -g sass

## Sublime Text, Atom, and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

    choco install sublimetext3

I prefer using the [beta version of Sublime Text 3](https://sublimetext.com/3) which is usually just as stable as version 2.

Sublime Text is not free, but I think it has an unlimited "evaluation period". Anyhow, we're going to be using it so much that even the seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome tool. :)

The first thing you do after installing Sublime is to [install Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and everything in between.

Now for the color. I'm going to change two things:
- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

A popular Theme is the [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for Seti_UI, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

Restart Sublime Text for all changes to take affect.

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your preference. When done, save the file and close it.

[Let's create a shortcut so we can launch Sublime Text from the command-line](https://scotch.io/tutorials/open-sublime-text-from-the-command-line-using-subl-exe-windows) 
    
    1. Open Powershell as Administrator
    2. Type `sysdm.cpl` and press enter and go to System Properties > Advanced System Settings > Advanced > Environment Variables
    3. Create a New System Variable Create a new system variable called SUBLIME that will point to the folder of your Sublime installation.
    4. Add the System Variable to Your PATH Add the following to the end of your PATH variable: `;%SUBLIME%`

![Add System variable](https://i.imgur.com/pXTqfL8.jpg)


Now I can open a file with `C:\ subl myfile.html` or start a new project in the current directory with `C:\ subl .`. Pretty cool.

Sublime Text is very extensible. For now we'll leave it like that, we already have a solid installation.

### Atom

If you like Sublime Text but can't afford or don't want to pay for it, [Atom](https://atom.io/) is an open-source version of Sublime Text that has a healthy community and regular updates.

The main problem for Atom as of autumn 2016 is that large files and projects slow down noticeably Atom's performance.

### VSCode

Visual Studio Code is 2018's "it" open-source code editor. There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Parallels](https://www.parallels.com/products/desktop/) or [Virtual PC](https://www.microsoft.com/en-us/download/details.aspx?id=3702).

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    choco install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Modern.ie site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).

## Vagrant

I have personally tried to move away from packaged development environments like MAMP. The alternative I've been enjoying a lot lately is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built in support talk to your local OS so that you develop in Windows but the environment runs in the VM.

   choco install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, by reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    vagrant up

My favorite box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## ES6

The time has come for you to start learning the newest version of Javascript, ECMAScript 6 (ES6). Where we needed jQuery previously, it is not the golden standard it once was. Browser quirks are decreasing because web standards are regularly implemented and iterated. Thus, the golden age of Javascript is upon us.

As of June 2015, ES6 is ratified as the newest JS version. Until browsers catch up implementing the newest features, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/).

    npm install --save-dev babel-cli

Only install this locally into an already created project.
    
Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`

Pairing this with a build system will give you the ability to write today's scripting language without worrying about incompatibilities. Check out [Babel's tools](https://babeljs.io/docs/setup/) to build your custom stack.

## PHP and Composer

Because so many people use PHP in their day to day work, we need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM and Bower.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer.


## TODO
- Allow apps downloaded from anywhere system preferences

## Credits

- [Front-End OS X](https://github.com/asuh/front-end-osx)
