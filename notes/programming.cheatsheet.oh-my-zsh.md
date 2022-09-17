---
id: xcei1r5x6qeh2fpqhqenw27
title: Oh My Zsh
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 10.8142
longitude: 106.6438
altitude: 0
title_imported: oh-my-zsh
updated_imported: '2021-01-31T15:12:20.000Z'
created_imported: '2020-03-26T02:34:17.000Z'
---

[toc]








# [How to Install Oh My Zsh! on Windows 10 Home Edition](https://dev.to/vsalbuq/how-to-install-oh-my-zsh-on-windows-10-home-edition-49g2)
**.zshrc**

```bash
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Get Yarn keyword working
export PATH=/home/bastian/.fnm:$PATH
eval "`fnm env`"

# Path to your oh-my-zsh installation.
export ZSH="/home/bastian/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="random"


# Uncomment the following line to automatically update without prompting.
 DISABLE_UPDATE_PROMPT="true"


# Uncomment the following line if pasting URLs and other text is messed up.
 DISABLE_MAGIC_FUNCTIONS="true"


# Uncomment the following line to disable auto-setting terminal title.
 DISABLE_AUTO_TITLE="true"



# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)

source $ZSH/oh-my-zsh.sh

# User configuration



# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

#expand aliases when typing Tab
zstyle ':completion:*' completer _expand_alias _complete _ignored

# Customize the Oh-my-zsh tab title to the current folder
case $TERM in
    xterm*)
     precmd ()  {
        print -Pn "\e]0; %24<..<%/\a"
     }
     ;;
esac

### Added by Zinit's installer
if [[ ! -f $HOME/.zinit/bin/zinit.zsh ]]; then
    print -P "%F{33}▓▒░ %F{220}Installing %F{33}DHARMA%F{220} Initiative Plugin Manager (%F{33}zdharma/zinit%F{220})…%f"
    command mkdir -p "$HOME/.zinit" && command chmod g-rwX "$HOME/.zinit"
    command git clone https://github.com/zdharma/zinit "$HOME/.zinit/bin" && \
        print -P "%F{33}▓▒░ %F{34}Installation successful.%f%b" || \
        print -P "%F{160}▓▒░ The clone has failed.%f%b"
fi

source "$HOME/.zinit/bin/zinit.zsh"
autoload -Uz _zinit
(( ${+_comps} )) && _comps[zinit]=_zinit

# Load a few important annexes, without Turbo
# (this is currently required for annexes)
zinit light-mode for \
    zinit-zsh/z-a-rust \
    zinit-zsh/z-a-as-monitor \
    zinit-zsh/z-a-patch-dl \
    zinit-zsh/z-a-bin-gem-node

zinit light zdharma/fast-syntax-highlighting
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions

### End of Zinit's installer chunk

# export PATH=/opt/tmux/bin:$PATH
# export LD_LIBRARY_PATH=/opt/libevent/lib:/opt/ncurses/lib:$LD_LIBRARY_PATH
# export MANPATH=/opt/tmux/share/man:$MANPATH
```

# [Oh-my-zsh Show aliases when typing Tab](https://stackoverflow.com/questions/24045044/oh-my-zsh-aliases-do-not-autocomplete)

```bash
#expand aliases when typing Tab
zstyle ':completion:*' completer _expand_alias _complete _ignored
```

# [Customize the Oh-my-zsh tab title to the current folder](https://www.rogerpence.com/posts/how-to-customize-ubuntu's-terminal-tab-titles)

Add this code to the ~/.zshrc file.
```bash
# Customize the Oh-my-zsh tab title to the current folder
case $TERM in
    xterm*)
     precmd ()  {
        print -Pn "\e]0; %24<..<%/\a"
     }
     ;;
esac
```

If you want more or less than 24 characters showing, change the 24 embedded in the code above to whatever value works for you.

It was also necessary to set the `DISABLE_AUTO_TITLE` value in .zshrc file to true.
Don't forget this! The technique doesn't work without it.

```bash
DISABLE_AUTO_TITLE="true"
```

# Update oh my zsh
```bash
cd .oh-my-zsh/ #to change to its root directory

git status # now you should be in your oh-my-zsh root with master on it and do git status

git stash/git add -A #to stash the changes and head back to master/add the changes

git commit -a #if you decided to add the changes

upgrade_oh_my_zsh #you can now upgrade
```

# [fix ... parse error near ...](https://github.com/ohmyzsh/ohmyzsh/issues/4870)
```bash
cd $ZSH
git config core.autocrlf input
git rm --cached -r .
git reset --hard
```
# xung đột màu nền giữa Solarized dark với Zsh-autosuggestions
zsh-autosuggestions: Hiển thị gợi ý, auto suggest Note: Màu của chữ mờ hiển thị bởi zsh-autosuggestions mặc định sẽ trùng với màu của background color trong Terminal nếu như bạn chọn Solarized dark.

Vậy nên bạn sẽ không thấy chữ mờ hiện ra mặc dù bạn đã install thành công 😃)))

**Giải pháp:**

+ Hoặc là thay đổi màu nền của Terminal, không chọn Solarized dark nữa.

+ Hoặc là đổi màu nền hiển thị chữ mờ: Cấu hình trong file `~/.oh-my-zsh/custom/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh` với biến `ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=blue'`