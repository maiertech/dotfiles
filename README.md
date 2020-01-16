# dotfiles

My dotfiles shared between machines. Use [Strap](https://github.com/MikeMcQuaid/strap) to bootstrap a new machine. If you want to create your own dotfiles repo, start with [Mike McQuaid's dotfiles](https://github.com/MikeMcQuaid/dotfiles).

## How to bootstrap a new MacBook

### Setup assistant

On new MacBooks or after a factory reset the first thing you see it the seup assistant:

- Do not link your local user account to iCloud and prevent local user account password reset with your Apple ID via iCloud.
- Turn on FileVault and write down the recovery key.
- Do not turn on iCloud Keychain.

### Automated installation with Strap

Go to https://macos-strap.herokuapp.com/ and download a customized `strap.sh`. To download the customized `strap.sh` you need give Strap access to your GitHub account and authorize it as OAuth app. Strap will then customize the download file with your GitHub username and pull your dotfiles from `https://github.com/<username>/dotfiles`.

For the GitHub authorizaton you need your GitHub password. If you keep it in a password manager like [LastPass](https://www.lastpass.com/), your password might be difficult or impossible to type in manually. With Apple's universal clipboard, you can copy your GitHub password from a password manager on another device and paste in you new machine. [This support article](https://support.apple.com/en-us/HT209460) explains how setup universal clipboard.

### Rerunning the automated installation

If the automated installation fails or if you you make changes in this `dotfiles` repository, you can run Strap again to install any new dependencies and to copy any new dotfiles. Homebrew will skip anything that has been installed already. However, Strap does not remove obsolete symlinks in `~/`.

### Setting system preferences manually

Most system preferences that you would normally have to configure manually can be scripted. This is what script `macos` is for. In macOS Catalina there are settings cannot be scripted and need to be configured manually:

#### Language & Region

| Setting                         | Value  | Comment                                                             |
| :------------------------------ | :----- | :------------------------------------------------------------------ |
| General → First day of the week | Monday | Scripted configuration of this setting is broken in macOS Catalina. |

#### Security & Privacy

| Setting                                                        | Value       | Comment                                                             |
| :------------------------------------------------------------- | :---------- | :------------------------------------------------------------------ |
| General → Require password X after sleep or screensaver begins | immediately | Scripted configuration of this setting is broken in macOS Catalina. |
| Privacy                                                        |             | Review all privacy settings.                                        |

#### Users & Groups

| Setting | Comment                 |
| :------ | :---------------------- |
| Image   | Choose preferred image. |

#### Sharing

Set computer name.

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

For any dependencies installed with Homebrew Casks use

- `brew cask list`
- `brew cask outdated`
- `brew cask upgrade`
- `brew cask uninstall`

Note that many apps installed with `brew casks` update themselves and the actual formula is just a shell to install it. Therefore, `brew cask outdated` will almost never show anything to update.

For any dependencies installed with mas-cli use

- `mas list`
- `mas outdated`
- `mas upgrade`
- `sudo mas uninstall <identifier>`

Note that `mas outdated` and `mas upgrade` are broken on macOS Catalina. You can follow [this issue](https://github.com/mas-cli/mas/issues/111) for an update.

### Managing SSH keys with LastPass

You can store all your SSH keys in LastPass as [secure notes](https://helpdesk.lastpass.com/secure-notes/) using note type "SSH keys". The `extract-lastpass-secrets` scripts shows how to retrieve stored keys while running Strap.

### Visual Studio Code

Visual Studio Code (VSCode) on Mac uses two locations to store settings:

- `~/Library/Application Support/Code` with user settings located at `~/Library/Application Support/Code/User/settings.json`.
- `~/.vscode` with extensions installed into the `extensions` folder.

When Strap is running, it symlinks `vscode-settings.json` and you can later on commit any configuration changes you make in VSCode. Strap also installs all VSCode extensions listed in `vscode-extensions`. You can install additional VSCode extensions, but they are not synchronized back to `vscode-extensions`. If you want to add a new VSCode extension for future Strap installs, add it to `vscode-extensions` and commit the file together with any corresponding VSCode settings changes in `vscode-settings.json`.

### Configuring macOS

On a new macOS system you normally have to make a number of manual configurations using the System Preferences app. Many of these manual configurations can be scripted. This is what script `macos` is for.

macOS comes with the `defaults` command-line utility with which you can change many system preferences. The challenge with `defaults` is that you need to figure out the domain and key arguments for each setting. Unfortunately, these are not documented well. Here are some pointers:

- A good starting point is [Mathias Bynen's `.macos` configuration file](https://github.com/mathiasbynens/dotfiles/blob/master/.macos).
- For any security and privacy related confiugrations this [macOS Security and Privacy Guide](https://github.com/drduh/macOS-Security-and-Privacy-Guide) is a great read.
- The [defaults-write.com website](https://www.defaults-write.com/) contains a collection of command-line configurations.
- And last, but not least, if above docs do not have what you were looking for, you can try to reverse engineer the domains and keys required for the `defaults` command as described in [this article](https://pawelgrzybek.com/change-macos-user-preferences-via-command-line/).
