# neovim

## installation
```bash
sudo apt install neovim
```

## neovim config

includes .vimrc configuration file.

```bash
vim .config/nvim/init.vim
...
set runtimepath+=~/.vimrc
set packpath+=~/.vim
source ~/.vimrc
...
```

## adds coc.vim

```bash
vim .vimrc
...
Plugin 'neoclide/coc.nvim',{'branch':'releaase'}
...
```
```

## adds coc extension and launguage server for LSP support

```
:CocInstall coc-json coc-tsserver coc-java
```

## CocConfig

```
```
