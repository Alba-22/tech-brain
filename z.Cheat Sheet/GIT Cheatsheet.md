#cheatsheet
### Remover branchs locais que não existem mais na origin
```shell
git remote prune origin
```

### Pretty git log
Adicionar um alias pro comando
```zsh
...
git log -10 --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
...
```

### Limitar PAGER
Dentro de .zshrc, setar variável PAGER
```shell
export PAGER="less -F"
```
