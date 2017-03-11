adding a readme...

## dotfiles via bare git repo and $HOME working dirctory

Here's the guidance for using a bare git repo for dotfile managment: [https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/](https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/)
Here's my config repo: [https://github.com/dgdosen/cfg](https://github.com/dgdosen/cfg)

### Using zsh to init:

```
git init --bare $HOME/.cfg
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config config --local status.showUntrackedFiles no
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.bashrc
```
[https://gist.github.com/dgdosen/e77ebea754166cb7c384ec794f2ae37d#file-cfg-init-sh](https://gist.github.com/dgdosen/e77ebea754166cb7c384ec794f2ae37d#file-cfg-init-sh)

### Using my .cfg repo for setup

```
git clone --bare git@github.com:dgdosen/cfg.git $HOME/.cfg
function config {
   /usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME $@
}
mkdir -p .config-backup
config checkout
if [ $? = 0 ]; then
  echo "Checked out config.";
  else
    echo "Backing up pre-existing dot files.";
    config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | xargs -I{} mv {} .config-backup/{}
fi;
config checkout
config config status.showUntrackedFiles no
```
[https://gist.github.com/dgdosen/e77ebea754166cb7c384ec794f2ae37d#file-cfg-install-sh](https://gist.github.com/dgdosen/e77ebea754166cb7c384ec794f2ae37d#file-cfg-install-sh)
