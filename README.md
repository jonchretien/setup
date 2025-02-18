# Setting Up a New Mac 

_Last Updated: 01/03/2025_

A running list of software and preferences for my development machine. 

## Table of Contents

- [Setting Up a New Mac ](#setting-up-a-new-mac-)
  - [Table of Contents](#table-of-contents)
  - [Step 1: macOS Settings](#step-1-macos-settings)
    - [System Update](#system-update)
    - [Security](#security)
    - [System Settings](#system-settings)
  - [Step 2: Command Line](#step-2-command-line)
    - [Python 3](#python-3)
  - [Step 3: Folder Structure](#step-3-folder-structure)
    - [Prevent Spotlight from indexing files in `code`.](#prevent-spotlight-from-indexing-files-in-code)
    - [Conditional Includes For Git Config](#conditional-includes-for-git-config)
  - [Step 4: Applications](#step-4-applications)
    - [Browsers](#browsers)
    - [Visual Studio Code](#visual-studio-code)
    - [Misc.](#misc)
  - [Appendix: Inspiration and Shout Outs](#appendix-inspiration-and-shout-outs)

## Step 1: macOS Settings

### System Update

First thing you need to do, on any OS actually, is update the system! For that: **Apple Icon > About This Mac** then **Software Update...**.

### Security

I recommend checking that basic security settings are enabled. You will be happy to have done so if ever your Mac is lost or stolen.

In **Apple Icon > System Settings**:

- Users & Groups: If you haven't already set a password for your user during the initial set up, you should do so now
- Security & Privacy > General: Require password immediately after sleep or screen saver begins (you can keep a grace period of a couple minutes if you prefer, but I like to know that my computer locks as soon as I close it)
- Security & Privacy > FileVault: Make sure FileVault disk encryption is enabled
- iCloud: If you haven't already done so during set up, enable Find My Mac
- Enable Firewall settings

### System Settings

If this is a new computer, there are a couple of tweaks I like to make to the System Settings. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Settings**:

- Trackpad > Tap to click
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)
- Dock > Automatically hide and show the Dock

**Screen Saver**

Start after 5 minutes. _Hot Corners_ set to lower left hand side ("Start Screen Saver"). Also set up [modifier keys](https://www.macrumors.com/how-to/macos-set-up-hot-corners-modifier-keys/)

**Dock**
- Automatically hide and show Dock
- Show indicators for open applications

**Sharing**
- Change computer name
- Make sure all file sharing is disabled

**Defaults**

```
# Show Library folder
chflags nohidden ~/Library

# Show hidden files
defaults write com.apple.finder AppleShowAllFiles YES

# Show path bar
defaults write com.apple.finder ShowPathbar -bool true

# Show status bar
defaults write com.apple.finder ShowStatusBar -bool true
```

## Step 2: Command Line

Boot up Terminal and run this command:

```
xcode-select --install
```

Transfer any SSH keys from the old machine and then install the following:
- [Homebrew](https://brew.sh)
- [Oh My Zsh](https://ohmyz.sh/#install)
- [Node (through NVM)](https://github.com/nvm-sh/nvm)
- `brew install ack`
- `brew install ffmpeg`
- `brew install jq`
- `brew install wget`

### Python 3

Note: this instructions are tailored for Python 3.

```sh
brew install python3
brew postinstall python3
pip3 install --upgrade pip setuptools wheel
sudo pip3 install virtualenv virtualenvwrapper
```

Add this to your .bash_profile:

```sh
# python 3
export VIRTUALENV_PYTHON=/usr/local/bin/python3
export VIRTUALENVWRAPPER_PYTHON=$VIRTUALENV_PYTHON
```

In your terminal, type:

```sh
source ~/.bash_profile
```

## Step 3: Folder Structure

Mostly taken from [@sindresorhus](https://github.com/sindresorhus/ama/issues/557):

```
.
├── Applications
├── Desktop
├── Documents
├── Downloads
├── Movies
├── Music
├── Pictures
├── code
│   ├── deprecated
│   ├── forks
│   ├── oss
│   │   ├── project1
│   │   ├── project2
│   │   └── …
│   ├── private
│   ├── todo
│   ├── work
│   └── scratchpad.md
```

- **deprecated:** Unmaintained projects are moved here to reduce noise in other directories.
- **forks:** Repos I fork are placed here.
- **oss:** My open source projects.
- **private:** Private projects like my macOS apps.
- **todo:** Projects I started but never finished enough to publish.
- **work:** Projects for my day job.
- **scratchpad.md:** Random ideas and todos.

### Prevent Spotlight from indexing files in `code`.

Tip from [@roelvangils](https://twitter.com/roelvangils/status/1113074439976075264):

> Does your Mac becomes slow/unresponsive (fans kicking in etc.) when you `npm install` a huge project with a million tiny dependencies? I learned that adding an empty `.metadata_never_index` in /node_modules _beforehand_ will prevent Spotlight from indexing all that crap.

As an alternate, there's a command line fix from this [yarn issue](https://github.com/yarnpkg/yarn/issues/6453#issuecomment-476048618):

```sh
find . -type d -name "node_modules" -exec touch "{}/.metadata_never_index" \;
```

And as a bash alias:

```sh
alias fix-spotlight='find . -type d -name "node_modules" -exec touch "{}/.metadata_never_index" \;'
```

### Conditional Includes For Git Config

_Mainly taken from [Eric William's blog](https://www.motowilliams.com/conditional-includes-for-git-config)._

Removing the following section in my .gitconfig

```shsh
[user]
name = Firstname Lastname
email = user@example.com
```

with this conditional

```sh
[includeIf "gitdir:~/code/oss/"]
	path = .gitconfig-oss
[includeIf "gitdir:~/code/private/"]
	path = .gitconfig-private
[includeIf "gitdir:~/code/todo/"]
	path = .gitconfig-todo
[includeIf "gitdir:~/code/work/"]
	path = .gitconfig-work
```

Then I add two additional sibling files to my profile root directory, next to my .gitconfig.

For my private & open source work I have my non-work values in a .gitconfig-private file.

```sh
[user]
name = Jon Doe
email = jon@doe.com
```

For my source code work related to my.gitconfig-work

```sh
[user]
name = Jon Doe
email = jon@work.com
```

So that I end up with these files in my profile root.

```sh
C:\users\<USERNAME>\
├───.gitconfig
├───.gitconfig-deprecated
├───.gitconfig-private
├───.gitconfig-oss
├───.gitconfig-todo
└───.gitconfig-work
```

This approach could be replicated to any number of patterns you choose. If you have specific accounts you use for client work you just add more entries to the conditional block. There have been scenarios where I would need to use an identity given by the client so that would require another conditional entry and .gitconfig-work-acme-archery config file.

I did find that with the nested directory approach the last entry wins so you have to put more specific values at the end.

This effectly translates an if or switch statement in my profile to explicit .gitconfig entries and config files. I'm not sure if I'll stick with it but I'll test drive it for now.

## Step 4: Applications

### Browsers
- [Chrome](http://www.google.com/chrome)
- [Chrome Canary](https://www.google.com/chrome/browser/canary.html)
- [Duck Duck Go](https://duckduckgo.com/mac)
- [Firefox](http://www.firefox.com/)
- [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
- [Firefox Nightly](https://download.mozilla.org/?product=firefox-nightly-latest-ssl&os=osx&lang=en-US)
- [Internet Explorer VMs](https://dev.windows.com/en-us/microsoft-edge/tools/vms/mac/)

### [Visual Studio Code](https://code.visualstudio.com)

**Settings**

- [Install shell command](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line)
- [User Settings](https://gist.github.com/jonchretien/b50044d4adf151edfebe8232897f9de5)

**Packages**

```sh
code --list-extensions

alefragnani.project-manager
christian-kohler.path-intellisense
dbaeumer.vscode-eslint
editorconfig.editorconfig
equinusocio.vsc-material-theme
equinusocio.vsc-material-theme-icons
esbenp.prettier-vscode
formulahendry.auto-rename-tag
github.copilot
github.copilot-chat
mechatroner.rainbow-csv
mgmcdermott.vscode-language-babel
ms-vscode.sublime-keybindings
ms-vscode.theme-materialkit
nicoespeon.abracadabra
orta.vscode-jest
serge.vsc-material-theme-italicize
visualstudioexptteam.intellicode-api-usage-examples
visualstudioexptteam.vscodeintellicode
yzhang.markdown-all-in-one
```

### Misc.
- [Dropbox](https://www.dropbox.com)
- [Spotify](https://www.spotify.com/us/download/)
- [Transmit](http://panic.com/transmit/)


## Appendix: Inspiration and Shout Outs

- [How to Setup Your Mac to Develop News Applications Like We Do](http://blog.apps.npr.org/2013/06/06/how-to-setup-a-developers-environment.html)
- Nicolas Hery's [Mac OS X Dev Setup](https://github.com/nicolashery/mac-dev-setup)
- Sourabh Bajaj's [Mac Setup Guide](https://github.com/sb2nov/mac-setup).
- [Set Up Your Mac Like an Interactive News Developer](https://open.nytimes.com/set-up-your-mac-like-an-interactive-news-developer-bb8d2c4097e5)
- [Setting up a Brand New Mac for Development](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/)
