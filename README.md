# Install homebrew
Follow the guide here: [Homebrew](https://brew.sh/) to install homebrew
The problem I experienced during installation:
```
fatal: unable to access 'https://github.com/Homebrew/brew/': Failed to connect to github.com port 443 after 29 ms: Couldn't connect to server
Failed during: /usr/bin/git fetch --quiet --progress --force origin
```
This is caused by network issue as I used VPN tool(ClashX) to access github

How to solve:
Set proxy as following: click Clash/Settings:
Refer to https://blog.csdn.net/zpf1813763637/article/details/128340109
```
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890
```
Remove proxy:
```
$ git config --global --unset http.proxy 
$ git config --global --unset https.proxy
```
<img width="505" alt="image" src="https://github.com/user-attachments/assets/c20a2762-391a-4959-966c-896ca20e8729" />

# How to change user name/computer name
https://askubuntu.com/questions/1470880/how-to-change-my-user-or-computer-name-which-appeares-before-each-command-in-the
https://askubuntu.com/questions/159558/change-the-device-name-in-the-details-window-of-system-settings

# git-prompt-config-on-mac
1. How to enable git completion when press "tab" key to complete the full command?
   Download the corresponding tag according to the git version you installed on your local:
   [Git Tags/](https://github.com/git/git/tags)
   
   Unzip the zip file based on the doc here: https://git-scm.com/book/en/v2/Appendix-A:-Git-in-Other-Environments-Git-in-Bash.

   please refer to the solution mentioned here: https://stackoverflow.com/questions/28028740/git-tab-completion-in-zsh-throwing-errors in case you ran into exception:
   ```
   command not found __git_aliased_command
   ```

   add below config to ~/.zshrc
   ```
   zstyle ':completion:*:*:git:*' script ~/git-completion.bash
   fpath=(~/.zsh $fpath)
   autoload -Uz compinit && compinit
   ```
   After that, execute command:
   ```
   source ~/.zshrc
   ```
   if the error mentioned above was still there, try to relaunch Terminal.

2. How to config the git-prompt on Mac ZSH terminate?
   ```
   ...
   . ~/git-prompt.sh
   autoload -Uz compinit && compinit

   function parse_git_branch() {
     local stat=""
     if [ -e .git/MERGE_HEAD ]; then
       stat="|MERGING"
     fi
     git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/(\1'"${stat}"')/p'
   }
   setopt PROMPT_SUBST
   # new line
   export PROMPT='%F{grey}%n%f %F{cyan}%~%f %F{green}$(parse_git_branch)%f'$'\n''%F{normal}$%f '
   ```
   the new style should look like:
   
   <img width="253" alt="image" src="https://github.com/user-attachments/assets/6d1e98d9-3c58-4aa7-b10a-5bb1c25cce80" />

  **The complete config in ~/.zshrc should be like**:
   ```
   zstyle ':completion:*:*:git:*' script ~/.zsh/git-completion.bash
   fpath=(~/.zsh $fpath)
   #. ~/.zsh/git-prompt.sh
   
   autoload -Uz compinit && compinit
   
   function parse_git_branch() {
     local stat=""
     if [ -e .git/MERGE_HEAD ]; then
       stat="|MERGING"
     fi
     git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/(\1'"${stat}"')/p'
   }
   setopt PROMPT_SUBST
   # new line
   export PROMPT='%F{grey}%n%f %F{cyan}%~%f %F{green}$(parse_git_branch)%f'$'\n''%F{normal}$%f '
   ```
