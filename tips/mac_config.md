

prettyping 
diff-so-fancy

```bash
# install Command Line Tools
xcode-select --install

# install software manager homebrew(maybe very slowly - you can use cellular)
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# change mirror to tuna
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

# install zsh & change bash -> zsh
brew install zsh git autojump
chsh -s /bin/zsh

# install oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-completions $ZSH_CUSTOM/plugins/zsh-completions
git clone https://github.com/zsh-users/zsh-history-substring-search $ZSH_CUSTOM/plugins/zsh-history-substring-search
# configure system set in ~/.zshrc
vim ~/.zshrc

# plugins=(
#     git
#     docker
#     zsh-syntax-highlighting
#     zsh-autosuggestions
#     zsh-completions
#     history-substring-search
# )

# no update when use brew
export HOMEBREW_NO_AUTO_UPDATE=true
```

https://wyydsb.xin/other/terminal.html
https://escapelife.github.io/posts/1c151aac.html


