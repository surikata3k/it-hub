---
description: "Nano editor cheat sheet: the essential keyboard shortcuts and a full reference."
---

# Nano

Quick reference for the `nano` editor.

## Essentials

The shortcuts I use most:

| Action | Keys |
|---|---|
| Save | ++ctrl+s++ |
| Exit | ++ctrl+x++ |
| Search / search next | ++ctrl+w++ / ++alt+w++ |
| Go to line | ++alt+g++ |
| Cut line / copy line / paste | ++ctrl+k++ / ++alt+6++ / ++ctrl+u++ |
| Delete line | ++alt+del++ |
| Delete word left / right | ++alt+backspace++ / ++ctrl+del++ |
| Undo / redo | ++alt+u++ / ++alt+e++ |
| Select (set mark) | ++alt+a++ |
| Comment / uncomment | ++alt+3++ |
| Run command | ++ctrl+t++ |
| Line numbers | ++alt+n++ |
| Show spaces | ++alt+p++ |
| Softwrap | ++alt+s++ |

## Full reference

### File handling

| Keys | Action |
|---|---|
| ++ctrl+s++ | Save current file |
| ++ctrl+o++ | Offer to write file ("Save as") |
| ++ctrl+r++ | Insert a file into current one |
| ++ctrl+x++ | Close buffer, exit from nano |

### Editing

| Keys | Action |
|---|---|
| ++ctrl+k++ | Cut current line into cutbuffer |
| ++alt+6++ | Copy current line into cutbuffer |
| ++ctrl+u++ | Paste contents of cutbuffer |
| ++ctrl+"]"++ | Complete current word |
| ++alt+3++ | Comment/uncomment line/region |
| ++alt+u++ | Undo last action |
| ++alt+e++ | Redo last undone action |

### Search and replace

| Keys | Action |
|---|---|
| ++ctrl+b++ | Start backward search |
| ++ctrl+f++ | Start forward search |
| ++alt+b++ | Find next occurrence backward |
| ++alt+f++ | Find next occurrence forward |
| ++alt+r++ | Start a replacing session |

### Deletion

| Keys | Action |
|---|---|
| ++ctrl+h++ | Delete character before cursor |
| ++ctrl+d++ | Delete character under cursor |
| ++alt+backspace++ | Delete word to the left |
| ++ctrl+del++ | Delete word to the right |
| ++alt+del++ | Delete current line |

### Operations

| Keys | Action |
|---|---|
| ++ctrl+t++ | Execute some command |
| ++ctrl+t++ ++ctrl+s++ | Run a spell check |
| ++ctrl+t++ ++ctrl+y++ | Run a syntax check |
| ++ctrl+t++ ++ctrl+o++ | Run a formatter |
| ++tab++ | Indent marked region |
| ++shift+tab++ | Unindent marked region |
| ++ctrl+j++ | Justify paragraph or region |
| ++alt+j++ | Justify entire buffer |
| ++alt+t++ | Cut until end of buffer |
| ++alt+":"++ | Start/stop recording of macro |
| ++alt+";"++ | Replay macro |

### Moving around

| Keys | Action |
|---|---|
| ++arrow-left++ | One character backward |
| ++arrow-right++ | One character forward |
| ++ctrl+arrow-left++ | One word backward |
| ++ctrl+arrow-right++ | One word forward |
| ++ctrl+a++ | To start of line |
| ++ctrl+e++ | To end of line |
| ++ctrl+p++ | One line up |
| ++ctrl+n++ | One line down |
| ++ctrl+arrow-up++ | To previous block |
| ++ctrl+arrow-down++ | To next block |
| ++alt+home++ | To first row in viewport |
| ++alt+end++ | To last row in viewport |
| ++ctrl+y++ | One page up |
| ++ctrl+v++ | One page down |
| ++alt+backslash++ | To top of buffer |
| ++alt+"/"++ | To end of buffer |

### Special movement

| Keys | Action |
|---|---|
| ++alt+g++ | Go to specified line |
| ++alt+"]"++ | Go to complementary bracket |
| ++alt+arrow-up++ | Scroll viewport up |
| ++alt+arrow-down++ | Scroll viewport down |
| ++alt+"<"++ | Switch to preceding buffer |
| ++alt+">"++ | Switch to succeeding buffer |

### Information

| Keys | Action |
|---|---|
| ++ctrl+c++ | Report cursor position |
| ++alt+d++ | Report line/word/character counts |
| ++ctrl+g++ | Display help text |

### Various

| Keys | Action |
|---|---|
| ++alt+a++ | Set or unset the mark |
| ++alt+v++ | Enter next keystroke verbatim |
| ++alt+c++ | Turn constant position info on/off |
| ++alt+n++ | Turn line numbers on/off |
| ++alt+p++ | Turn visible whitespace on/off |
| ++alt+s++ | Turn softwrapping on/off |
| ++alt+x++ | Hide/unhide the help lines |
| ++alt+z++ | Hide/unhide the info bars |
| ++ctrl+l++ | Refresh the screen |
