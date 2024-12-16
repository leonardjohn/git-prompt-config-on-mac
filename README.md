# git-prompt-config-on-mac
1. How to enable git completion when press "tab" key to complete the full command?
   add below config to ~/.zshrc
   ```
   zstyle ':completion:*:*:git:*' script ~/git-completion.bash
   autoload -Uz compinit && compinit
   ```
   After that, execute command:
   ```
   source ~/.zshrc
   ```

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


