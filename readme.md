# Front-End Development Setup on a PC

This document assumes you're running a fresh copy of the latest version of **Windows 10**, >=version 1507.

If you have any comments or suggestions, feel free to give me a shout [on Twitter](https://twitter.com/asuh)!

- [Windows Prepartion](#system-update-and-disk-encryption)
- [.NET Framework](#net-framework)
- [Chocolatey](#chocolatey)
- [Git](#git)
- [Node.js](#nodejs)
- [Ninite](#ninite)
- [Projects Directory](#projects-directory)
- [ZSH](#zsh-optional)
- [Sass](#sass)
- [Sublime Text and Atom](#sublime-text-and-atom)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [Yeoman](#yeoman)
- [ES6](#es6)
- [MongoDB](#mongodb)
- [PHP and Composer](#php-and-composer)
- [Apps](#apps)

## System update and Disk Encryption

Step One - Update the system!
**Windows Key > Settings > Update & Security**

Step Two - Turn on BitLocker
**Control Panel\System and Security\BitLocker Drive Encryption**

Click on the text "Turn on BitLocker" to turn on and enable BitLocker. On a brand new machine or Windows 10 installation, it will take some time to fully encrypt the disk.

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your BitLocker password. You'll have four options to retain a recovery key so choose the best option for you. Losing this recovery key means you cannot log in and everything on your computer is 100% inaccessible.
- After encryption completes, if Windows gets corrupted and you need to download files from the drive after accessing the drive from an external case, it's not possible. Make sure you're both backing up using [Windows 10 Backup](http://windows.microsoft.com/en-us/windows-10/getstarted-back-up-your-files) and a cloud backup provider like [iDrive](https://www.idrive.com/) or [Crashplan](https://www.code42.com/crashplan/)

## .NET

An important dependency before services like IIS can work is **.NET Framework**.

**Control Panel\All Control Panel Items\Programs and Features > Turn Windows features on or off**

Choose to install both .NET Framework 3.5 and .NET Framework 4.6 Advanced Services. .NET Framework is a large installation and is useful for the back-end development but is not necessary unless you're developing within Visual Studio on Microsoft languages. 

For older OSes, go to [https://www.microsoft.com/net/download](https://www.microsoft.com/net/download) or just use Windows Update to get the compatible version for the OS.

## Chocolatey

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for Windows is [Chocolatey](https://chocolatey.org/).

### Install

Three ways (Run as Administrator):

Cmd.exe

    @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

PowerShell.exe 

    iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

PowerShell v3+
    
    iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex

For PowerShell, ([Ensure Get-ExecutionPolicy is at least RemoteSigned](http://windowsitpro.com/powershell/ps-101-checking-and-setting-powershell-execution-policy)).


### Usage

To [install a package](https://chocolatey.org/docs/commands-install) simply type:

    C:\ choco install <package>

Replace `<package>` with the name of the package you want to install.

To view Chocolatey's directory of packages:

[https://chocolatey.org/packages](https://chocolatey.org/packages)

To update a package:

    C:\ choco upgrade <package>

To install a specific version of a package (replace x with version number):

    C:\ choco install <package> --version=x.x.x

To see if any of your packages need to be updated:

    C:\ choco outdated

To see what you have installed (with their version numbers):

    C:\ choco list --a

### Installing multiple applications

Chocolatey is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis.

    C:\ choco install nodejs git googlechrome firefox opera atom 7zip ccleaner openvpn cmder notepadplusplus ruby github sourcetree vlc filezilla skype virtualbox vagrant

Note: It is unlikely you need to install Adobe Flash or Java anymore. Google Chrome and Microsoft Edge contain both packages by default. Running standalone versions of each is a security risk without regular maintenance and updates.

## Git

What's a developer without [Git](http://git-scm.com/)? To install, simply run:

    C:\ choco install git

Once installation is complete, restart your command line application so that you can verify that Node and NPM are correctly installed:

    C:\ git --version

And using cmd.exe, `C:\ where git` should output `C:\Program Files\Git\cmd\git.exe`.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

    C:\ git config --global user.name "Your Name Here"
    C:\ git config --global user.email "your_email@youremail.com"

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, use the recommended HTTPS method (versus SSH). So you don't have to type your username and password everytime, enable Git password caching as described [here](https://help.github.com/articles/set-up-git):

    C:\ git config --global credential.helper wincred

## Node.js

Install [Node.js](http://nodejs.org/) with Chocolatey:

    C:\ choco install nodejs

This also installs the [npm](https://npmjs.org/) package manager. 

Once installation is complete, restart your command line application so that you can verify that Node and NPM are correctly installed:

    C:\ node --version
    C:\ npm --version

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least four that are worth installing globally. Those are [TypeScript](http://www.typescriptlang.org/), [Grunt](http://gruntjs.com/), [Gulp](http://gulpjs.com/), and [Bower](http://bower.io):

    C:\ npm install typescript
    C:\ npm install -g grunt-cli
    C:\ npm install --global gulp
    C:\ npm install -g bower

### Npm usage

To install a package:

    C:\ npm install <package> # Install locally
    C:\ npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    C:\ npm install <package> --save

To see what's installed:

    C:\ npm list # Local
    C:\ npm list -g # Global

To find outdated packages (locally or globally):

    C:\ npm outdated [-g]

To upgrade all or a particular package:

    C:\ npm update [<package>]

To uninstall a package:

    C:\ npm uninstall <package>

## Ninite

If you have more software you need installed not included in Chocolatey or you wish to use something that has a GUI, [Ninite](https://ninite.com/) is an awesome addition. Go to the site and choose all the software you prefer in the first step. It'll then let you download a global installer for everything in step 2.

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

### Install

Installing it is easy using Chocolatey:

    C:\ choco install mongodb

### Usage

In a command line or Powershell window, start the MongoDB server:

    C:\ mongod

In another window, connect to the database with the Mongo shell using:

    C:\ mongo

I'll let you refer to MongoDB's [Getting Started](https://docs.mongodb.com/getting-started/shell/) guide for more!

## Projects Directory

Create a project directory somewhere on your machine. I like to use `C:\Users\{yourusername}\Sites\project-name`. I prefer my Sites folder to exist along side the rest of my user profile folders.

    $ mkdir -p Sites

Depending on the type of project you work on, this might not be necessary or preferable.

## ZSH (optional)

By default, Windows comes with three command line tools: Command Prompt, Bash for Windows, and Powershell. Powershell is a task automation and configuration management framework that includes command-line shell.

Because many tools recommended can be used with another shell called Bash, which contains the command language used to interact with Unix functions, such as `pwd`, `ls`, and `cd`, it is recommended to either use [Bash on Unbuntu on Windows](https://msdn.microsoft.com/en-us/commandline/wsl/about) or use something like [Cmder](http://cmder.net/). Cmder is a powerful console emulator that provides a shell for Bash.

[Z Shell](https://en.wikipedia.org/wiki/Z_shell), or ZSH, was written to extend Bash and make improvements to how Bash works. One of the most popular frameworks written around ZSH is called [Oh My Zsh!](http://ohmyz.sh/).

Install it with the following command:

    $ curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

Now you have ZSH installed. [Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful.

## Sass

Install your preprocessor of choice, but I highly recommend using Sass. They all do the same thing but Sass has the most momentum behind it right now.

    C:\ sudo gem install sass

Sass requires you already installed Ruby, which we took care of in the Chocolatey section above.

## Sublime Text and Atom

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

    C:\ choco install sublimetext3

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

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Parallels](https://www.parallels.com/products/desktop/) or [Virtual PC](https://www.microsoft.com/en-us/download/details.aspx?id=3702).

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    C:\ choco install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Modern.ie site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).

## Vagrant

I have personally tried to move away from packaged development environments like MAMP. The alternative I've been enjoying a lot lately is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built in support talk to your local OS so that you develop in Windows but the environment runs in the VM.

    C:\ choco install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, by reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    C:\ vagrant up

My favorite box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## NPM packages

### Yeoman

If you intend to use a modern Javascript MVC framework and need a quick way to scaffold out a base framework, [Yeoman](http://yeoman.io/) was built specifically for this purpose. It contains generators for all the most popular frameworks and tools.

    C:\ npm install yo

### ES6

The time has come for you to start learning the newest version of Javascript, ECMAScript 6 (ES6). Where we needed jQuery previously, it is not the golden standard it once was. Browser quirks are decreasing because web standards are regularly implemented and iterated. Thus, the golden age of Javascript is upon us.

As of June 2015, ES6 is ratified as the newest JS version. Until browsers catch up implementing the newest features, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/).

    C:\ npm install -g babel

Pairing this with a build system will give you the ability to write today's scripting language without worrying about incompatibilities. Check out [Babel's tools](https://babeljs.io/docs/setup/) to build your custom stack.

## PHP and Composer

Because so many people use PHP in their day to day work, we need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM and Bower.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer.

## Apps

Here is a quick list of some apps I use, and that you might find useful as well:

- [Mou](http://markedapp.com/): As a developer, some of the stuff you write ends up being in [Markdown](http://daringfireball.net/projects/markdown/). In fact, this `README.md` file (possibly the most important file of a GitHub repo) is indeed in Markdown, written in Sublime Text, and I use Marked to preview the results everytime I save. **(FREE)**
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. Another alternative is [Simplenote](http://simplenote.com/) because I only take text notes, and I got tired of Evernote putting extra spaces between paragraphs when I copy & pasted into other applications. Simplenote is so much better for text notes (and it supports Markdown!). **(Both are free)**

## TODO
- Allow apps downloaded from anywhere system preferences

## Credits

- [Front-End OS X](https://github.com/asuh/front-end-osx)
