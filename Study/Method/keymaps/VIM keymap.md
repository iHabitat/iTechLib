# VIM Keymap

## Basic
`”*y` : copy to the clipboard

## Navigate (Move Cursor)

`gk` / `gl` …: go to display line.

`nG` / `:n`: go to the nth line.
`gg`: go to the first line.
`G`: go to the end line.

`w`: go to the beginning of next word.
`e`: go to the end of next word.
`b`: back
> word : program variable, includes letters, digitals and underscore.

`W`: go to the beginning of next word(program statement, split by blanks).
`E`: go to the end of next word(program statement, split by blanks).
`B`: back

`%`: move between the braces.
`#` / `*`: move the previous or next same word. 

## delet
`d`: delete action.
`dl`: delete a single character under the cursor.
`2dl`/`d2l`: delete two characters.
`dd`: delete current line.
`d2d`/`2dd`: delete two lines.
`diw`: delete a word.

## change
`ciw`: change a word


## Tagbar
> install ctags: `brew install ctags`

## NERDCommnenter
- `,cc` comment out the current line or text selected in visual mode.
- `,cu` uncomment the selected lines

## CtrlP
- `Ctrl+p`: launching.

## Buffers
- `:ls`: list all buffers. [`%` indicate current buffer in window, `#` indicate the alternative buffer, `Ctrl + ^` toggle between the current and alternative buffers.]
- `:bfirst`/ `:bprev` / `:bnext` / `:blast`: switch to the matched buffer.
- `:buffer N`: switch to the number N buffer.
- `:buffer {bufferName}`: switch to the contained the bufferName buffer.
- `:bdelete N` \ `:N, M bdelete`: delete the buffer
- `:args`: show arguments for vim command.