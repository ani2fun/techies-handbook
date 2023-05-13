# Homebrew


## Cheatsheet

#### [Upgrade all the casks installed via Homebrew Cask](https://stackoverflow.com/questions/31968664/upgrade-all-the-casks-installed-via-homebrew-cask)

There is now finally an official upgrade mechanism for Homebrew Cask:

`brew upgrade --cask`

However this will not update casks that do not have versioning information (`version :latest`) or applications that have a built-in upgrade mechanism (`auto_updates true`). To reinstall these casks (and consequently upgrade them if upgrades are available), run the upgrade command with the `--greedy` flag like this:

`brew upgrade --cask --greedy`

To get outdated:

`brew outdated --cask --greedy --verbose`
