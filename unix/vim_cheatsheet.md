# Vim commands

## Repeat Factor

* Can hit # before command, do command. It will be repeated x times

## Getting into Insert Mode

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

## Insert Mode

* [Ctrl-w] to delete last word

## Navigation Mode

For performing navigation and cut/paste operations.

* h j k l - navigation keys
* 0 - beginning of line
* $ - end of line

* b - moves back to beginning of word
* e - moves to end of word
* w - moves to beginning of word
* B E W - skip punctuation

* #G - goto line # (default is 1)

* H / M / L - high / medium / low points on screen
* zt / zz / zb - Move current line to top / middle / bottom of screen

* [Ctrl-g] - get line number

### Page Navigation

* [Ctrl-f] - page down
* [Ctrl-b] - page up

* [Ctrl-d] - half-page down
* [Ctrl-u] - half-page up

* [Ctrl-e] - scroll one line down
* [Ctrl-y] - scroll one line up

### Bookmarks

Disappear when file is close.

* m[letter] - creates bookmark
* '[letter] - goto bookmark

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

#### Search

* /pat - searches forward for pattern pat
* ?pat - searches backwards for pattern pat
* n - repeats search in same direction
* N - repeats search in opposite direction

* f[char] - goto char on line going forward
* F[char] - goto char on line going backward
* t / T - do the same thing, but stop a character before

## Last Line or ex Mode

* :sh - Shell escape
* :![cmd] - Run [cmd]
* :# - Goes to line #

* :n1,n2s/s1/s2 - replaces n1 with s1 and n2 with s2

* [Ctrl-z] to go into shell
  * ```$ fg``` to bring background job to foreground

* :e . - open file tree

## Window Management

* :sp - split window
* [Ctrl-w][Ctrl-w] - move between windows
* [Ctrl-w][+/-] - make window bigger / smaller
* :q - close active window
* :on - close all other windows

## Buffers

* multiple clipboards, figure out workflow
