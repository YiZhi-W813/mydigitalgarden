---
{"dg-publish":true,"tags":["tools","Linux"],"permalink":"/knowledge point/Linux/tmux常用快捷键汇总/","dgPassFrontmatter":true}
---


### Session
* **new session**:`tmux new -s <session-name>` 
* **exit session**:`tmux detach` or *Cb+d*
* **ls sessions**:`tmux ls` or *Cb+s*
* **attach session**:`tmux a -t <session-name>`
* **kill session**:`tmux kill-session -t <session-name>` or *C+d*
* **switch session**:`texttmux switch -t <session-name>`
* **rename session**:`tmux rename-session -t <old-session-name> <new-session-name>` or *Cb+$

### Pane
* **split window**:`tmux split-window (-h)` or *Cb+%(")*
* **select pane**:`tmux select-pane  -U/-D/-L/-R` or *Cb+arrowkey*
* **move pane**:`tmux swap pane -U/-D`
* **close pane**:*Cb+x*

### Window
* **create window**:`tmux new-window -n <window-name>` or *Cb+c*
* **select window**:`tmux select-window -t <window-name>` or *Cb+w*
* **rename window**:`tmux rename-window <new-window-name>` or *Cb+,*
* **kill window**:`tmux kill-window -t <window-name>` or *Cb+&*
