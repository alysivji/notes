# Vim commands

## Repeat Factor

* Can hit # before command, do command. It will be repeated x times

## Insert Mode

Mode where we enter text into the editor. This isn't what we are always supposed to do.

* [Ctrl-w] to delete last word
* [Ctrl-n] - activate completion
  * [Ctrl-n] / [Ctrl-p] | arrow keys - to move thru list

### Getting into Insert Mode

* i - insert before cursor
* I - insert at start of line

* a - insert after cursor
* A - insert at end of line

* o - open new line after cursor
* O - open new line before cursor

* r - replace single character, stay in Commande Mode
* R - replace more than a single character

* s - replace current character and go into insert
* S - delete line and go into insert

* c - change operator

## Normal Mode

This is the mode you are supposed to be in. It's **Normal** operation.

### Navigation

For performing navigation and cut/paste operations.

* h j k l - navigation keys
* 0 - beginning of line
* ^ - first chracater (nonblank character)
* gm - move to middle of line
* $ - end of line

* Fx - previous x on line
* fx - next x on line
* ; - repeats character jump in same direction
* , - repeats chracxter jump in other direction

* b - moves back to beginning of word
* e - moves to end of word
* w - moves to beginning of word
* B E W - skip punctuation

* #G - goto line # (default is 1)

* H / M / L - high / medium / low points on screen
* zt / zz / zb - Move current line to top / middle / bottom of screen

* [Ctrl-g] - get line number

### Text Navigation

* { / } - move to start / end of paragrah
* ( / ) - move to start / end of sentence

### Page Navigation

* [Ctrl-f] - page down
* [Ctrl-b] - page up

* [Ctrl-d] - half-page down
* [Ctrl-u] - half-page up

* [Ctrl-e] - scroll one line down
* [Ctrl-y] - scroll one line up

### Marks

Disappear when file is close.

* m[letter] - creates bookmark
* '[ltter] - goto bookmark

* lowercase - local
* uppercase - global

* Can combine marks with operators so delete till mark: ```d`[char]```

* '0 - goes to last line before vim closed
* '1 - goes to last line before vim was closed two times ago

* '' - jump back to previous spot
* ', - jump to last edit
* :marks - to see a list of marks

### Operators

* d - delete operator (d$ deletes till end of line). Can also be used to cut text
* D - deletes from cursor to end of line
* y - yank (copy)

* p - puts back taken by d after cursor
* P - puts back taken by d before cursor

* c - change operator (c$ changes all text until end of line)
* C - change all text until end of line

* ! - filter to act on text
* x - deletes character under cursor
* x - deletes previous character
* J - joins current line with next line (removes new line char b/w two lines)

* . - repeat last command

* u - undo last edit
* U - reverses changes made on a single line

### Search

* /pat - searches forward for pattern pat
* ?pat - searches backwards for pattern pat
* n - repeats search in same direction
* N - repeats search in opposite direction

* If word is already selected:
  * ```*``` - search forward
  * # - search backward

* f[char] - goto char on line going forward
* F[char] - goto char on line going backward
* t / T - do the same thing, but stop a character before

* :iab ff Firefox - replace abbreviations with long words

## Folds

TODO look into

* :set foldmethod=
  * ```indent``` or ```syntax``` is probably best for now

* z[x] - lots of fold commands
* look into plugins

## Registers

* Specify register you want to use before entering in command you would use anyways
* "[char] - to access [char] register
  * a-zA-Z

* "" - unnamed register (contained last deleted text)
* "0p - always contains contents of last yank
* "+ - system clipboard register

## Macros

TODO lok into

* qa
* @a

## Visual Mode

Mode where you can highlight text

* V - Visual line mode, select lines
* v - visual mode, select parts of text

* ggVG - selects from first to last line

### Miscellaneous Commands

* << / >> - indent / outdent if selected in ```VISUAL``` mode
* = - format selection

## Last Line or ex Mode

* :sh - Shell escape
* :![cmd] - Run [cmd]
* :# - Goes to line #

* :n1,n2s/s1/s2 - replaces n1 with s1 and n2 with s2

* [Ctrl-z] to go into shell
  * ```$ fg``` to bring background job to foreground

* :e . - open file tree

* :setpaste - Disables automatic indentation

## Window Management

* :sp - split window
* [Ctrl-w][Ctrl-w] - move between windows
* [Ctrl-w][+/-] - make window bigger / smaller
* :q - close active window
* :on - close all other windows

## Buffers

* multiple clipboards, figure out workflow
