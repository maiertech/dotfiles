# dotfiles

My dotfiles shared between machines. Use [Strap](https://github.com/MikeMcQuaid/strap) to bootstrap a new machine. If you want to create your own dotfiles repo, start with [Mike McQuaid's dotfiles](https://github.com/MikeMcQuaid/dotfiles).

## How to bootstrap a new MacBook

### Apple onboarding checklist

For new MacBooks or after factory reset:

- Make user account local account and do not allow account recovery via iCloud.
- Turn off telemetry.
- Turn off Siri.

### Manual configurations

#### General

| Setting                                 | Value |
| :-------------------------------------- | :---- |
| Automatically hide an show the menu bar | on    |

#### Siri

| Setting         | Value |
| :-------------- | :---- |
| Enable Ask Siri | off   |

#### Language & Region

| Setting                     | Value        |
| :-------------------------- | :----------- |
| General → First day of week | Monday       |
| General → Time format       | 24-Hour Time |
| General → Temperature       | Celsius      |

#### Security & Privacy

| Setting                                                          | Value                               |
| :--------------------------------------------------------------- | :---------------------------------- |
| General → Require password                                       | immediately                         |
| General → Set lock message...                                    | Set your lock message               |
| General → Allow apps downloaded from                             | App store and identified developers |
| FileVault → Turn on FileVault...                                 | on                                  |
| Firewall → Firewall Options ... → Block all incoming connections | on                                  |
| Privacy                                                          | Review all privacy settings         |

#### Sharing

Set computer name.

### Automated installation with Strap

Go to https://macos-strap.herokuapp.com/ and download a customized `strap.sh`. To download the customized `strap.sh` you need give Strap access to your GitHub account and authorize it as OAuth app. Strap will then customize the download file with your GitHub username and pull your dotfiles from `https://github.com/<username>/dotfiles`.

For the GitHub authorizaton you need your GitHub password. If you keep it in a password manager like [LastPass](https://www.lastpass.com/), your password might be difficult or impossible to type in manually. With Apple's universal clipboard, you can copy your GitHub password from a password manager on another device and paste in you new machine. [This support article](https://support.apple.com/en-us/HT209460) explains how setup universal clipboard.

### Rerunning the automated installation

If the automated installation fails or if you you make changes in this `dotfiles` repository, you can run Strap again to install any new dependencies and to copy any new dotfiles. Homebrew will skip anything that has been installed already. However, Strap does not remove obsolete symlinks in `/~`.

### Known problems

#### Visual Studio Code is not signed

As of Visual Studio Code (VSCode) 1.41, when you install VSCode with `brew cask` on macOS Catalina and try to run it, you get this error message:

<img width="532" alt="Error message when starting Visual Studio Code on macOS Catalina" src="https://user-images.githubusercontent.com/1482402/70838000-39e80280-1dd4-11ea-837e-934bd5690ba7.png">

This will fail the subsequent installation of VSCode extensions.

VSCode still needs to be notarized and signed by Microsoft to prevent this warning. Meanwhile you can manually approve running VSCode in macOS Catalina in Security & Privacy settings:

<img width="780" alt="Approving Visual Studio Code in macOS Catalina Security & Privacy settings" src="https://user-images.githubusercontent.com/1482402/70838052-6ac83780-1dd4-11ea-83f7-5b7588075cb9.png">

When doen rerun Strap.

## Documentation

### Local GitHub repository

Strap clones this repository to `~/.dotfiles` and symlinks any dotfile located in `~/` to a file in `~/.dotfiles`. If you change any configs, e.g. settings in VSCode, you can diff the changes with Git in `~/.dotfiles` and commit them.

### Scripts

All scripts in `script` need to be owner executable. Note that the owner executable flag is part of a GitHub commit and will be set when you clone this repository.

### Brewfile

You can think of the `Brewfile` as your `Gemfile` or `package.json` for system dependencies ([this post](https://thoughtbot.com/blog/brewfile-a-gemfile-but-for-homebrew) came up with this analogy). Strap uses the [`brew bundle`](https://github.com/Homebrew/homebrew-bundle) command under the hood to install dependencies from 3 sources:

1. [Homebrew](https://brew.sh/),
1. [Homebrew Cask](https://github.com/Homebrew/homebrew-cask) and
1. [macOS App Store](http://www.apple.com/mac/app-store) (via [mas-cli](https://github.com/mas-cli/mas)).

### Managing installed dependencies

Once your system dependencies are installed, you should manage them only via the follwoing command-line tools. For any dependencies installed with Homebrew use

- `brew list`
- `brew update`
- `brew outdated`
- `brew update`
- `brew uninstall`

For any dependencies installed with Homebre Casks use

- `brew cask list`
- `brew cask outdated`
- `brew cask upgrade`
- `brew cask uninstall`

For any dependencies installed with mas-cli use

- `mas list`
- `mas outdated`
- `mas upgrade`
- `sudo mas uninstall <identifier>`

### Managing SSH keys with LastPass

You can store all your SSH keys in LastPass as [secure notes](https://helpdesk.lastpass.com/secure-notes/) using note type "SSH keys. The `extract-lastpass-secrets` scripts shows how to retrieve stored keys during Strap automated installation.

### Visual Studio Code

Visual Studio Code (VSCode) on Mac uses two locations to store settings:

- `~/Library/Application Support/Code` with user settings located at `~/Library/Application Support/Code/User/settings.json`.
- `~/.vscode` with extensions installed into the `extensions` folder.

When Strap runs it symlinks `vscode-settings.json` and you can later on commit any changes you make. Then Strap installs all VSCode extensions listed in `vscode-extensions`. You can install additional VSCode extensions, but they are not synchronized back to `vscode-extensions`. If you want to add a new VSCode extension for future Strap installs, add it to `vscode-extensions` and commit the file together with corresponding VSCode settings changes.
