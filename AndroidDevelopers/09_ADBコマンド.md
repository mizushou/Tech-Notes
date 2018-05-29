### インストールディレクトリ
- 以下のディレクトリがインストールディレクトリ
```
$ /Users/shouhei/Library/Android/sdk/platform-tools/
```

### terminalのパス設定
- パスを通す
```bash
PATH=$HOME/.nodebrew/current/bin:$PATH:/Users/shouhei/Library/Android/sdk/platform-tools
PS1="[\u@\h \t \W]\$ "
export PATH
alias ll='ls -hl'
```
