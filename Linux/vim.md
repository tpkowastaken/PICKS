# rebinding 'jk' to escape

## Neovim vim configuration

```
vim ~/init.lua
```

```
local options = { noremap = true }
vim.keymap.set("i", "jj", "<Esc>", options)
```

## vim jk

```
vim ~/.vimrc
```

```
inoremap jk <esc>

```

# Make the same configurations for root:

```
sudo cp ~/.vimrc ~/init.lua /root/
```
