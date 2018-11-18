### How to open Sublime Text with zsh

1. Open a terminal
2. go to ~ directory
3. `vi ./.zshrc`
4. Find "plugins=(...)"
5. Type i to edit
6. Change to "plugins=(git autojump sublime)"
btw. autojump is a powerful tool
7. Type esc to quit editing mode and :wq to save & quit
8. `source ./.zshrc`
9. Restart the terminal (IMPORTANT)
10. `st` to open a Sublime Text
