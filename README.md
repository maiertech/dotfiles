# dotfiles

My dotfiles shared between machines. Use [Strap](https://github.com/MikeMcQuaid/strap) to bootstrap a new machine. If you want to create your own dotfiles repo, start with [Mike McQuaid's dotfiles](https://github.com/MikeMcQuaid/dotfiles).

## Installation

To bootstratp a new machine you need to got to https://macos-strap.herokuapp.com/ and download a customized `strap.sh`. To download the customized `strap.sh` you need give Strap access to your GitHub account and authorize it as OAuth app. Strap will then customize the download file with your GitHub username and pull your dotfiles from `https://github.com/<username>/dotfiles`.

For the GitHub authorizaton you need your GitHub password. If you keep it in a password manager like [LastPass](https://www.lastpass.com/), your password might be difficult or impossible to type in manually. With Apple's universal clipboard, you can copy your GitHub password from a password manager on another device and paste in you new machine. [This support article](https://support.apple.com/en-us/HT209460) explains how setup universal clipboard.

## Documentation

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

For any dependencies installed with Homebre Casks use

- `brew cask list`
- `brew cask outdated`
- `brew cask upgrade`

For any dependencies installed with mas-cli use

- `mas list`
- `mas outdated`
- `mas upgrade`
