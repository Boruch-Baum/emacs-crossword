# crossword.el [![MELPA](https://melpa.org/packages/crossword-badge.svg)](https://melpa.org/#/crossword)

   * Download and play crossword puzzles in Emacs.

   * Includes a browser to view puzzles' detailed metadata, including
     progress of partially played puzzles.

   * Optionally, play against the clock, with the built-in timer.


## Dependencies: (all already part of core Emacs)

   * seq            - for `seq--into-vector`
   * tabulated-list - for `tabulated-list-mode`, etal.
   * hl-line        - for `hl-line-mode`
   * calendar       - for `calendar-read` etal.


## Dependencies: (external to Emacs)

   The package uses either `wget` or `curl` to download packages from
   the network. These are both long-established standard programs and
   at least one is probably already installed on your computer.


## Installation:

   1) Evaluate or load or install this file.

   2) Optionally, define a global keybinding or defalias for function
      `crossword`
```
         (global-set-key (kbd "foo") 'crossword))
         (defalias 'crossword 'foo)
```
   3) Depending on your temperament and style, you might also want to
      set direct keybindings and aliases for functions
      `crossword-download`, `crossword-display`, and `crossword-load`;
      however, they're just one menu-selection away from function
      `crossword`.


## Configuration:

   M-x `customize-group` <RET> crossword <RET>

   Initially, you may want to change the default download path
   `crossword-save-path`, but otherwise, try using the mode first
   without any customization.

   There exist four customizations for how `POINT` advances after you
   fill-in a square or navigate: `crossword-arrow-changes-direction`,
   `crossword-wrap-on-entry-or-nav`, `crossword-tab-to-next-unfilled`,
   `crossword-auto-nav-only-within-clue`.

   If you don't usually play more than one crossword in a sitting,
   you may want to set `crossword-quit-to-browser` to `NIL` to save
   yourself a keystroke on exit.

   There are also several 'faces' defined to allow custom colorization
   and fontification. Knock yoursel out.


## Operation:

   M-x `crossword` presents a menu with three options. Unless you
   already have local puzzle files, you'll want to connect to the
   network, and decide what puzzle to download and for what date.
   Select a download source from the download menu, noting that the
   label for each download source includes the days of the week on
   which puzzles are published. Then enter the date of the puzzle you
   want. Different download sources have different archive retention
   policies, so you can try downloading 'old' puzzles. The puzzle files
   are tiny, so the download should be instantaneous for most, but
   YMMV.

   M-x `crossword` a second time. If you choose to directly load a
   puzzle, you will be prompted to navigate to it. If you choose the
   browser, you're in for a small treat, as you'll be presented with a
   handy-dandy nifty screenshot-suitable metadata browser of all your
   known puzzle files, courtesy of Emacs' built-in
   `tabulated-list-mode`. Entries can be sorted by any column by
   navigating `POINT` there and pressing 'S' once or twice. Press \<RET\>
   to play the puzzle at point. You can also delete files here ('d').

   * IMPORTANT: The mode supports a single save file for each puzzle.
     These save files do not over-write the original file, so the
     partially- or completely- played puzzles appear in the browser
     alongside the untouched original. Save files can be identified in
     the browser and on youor compuer disk by their file-name extension
     `puz-emacs`. You can at any time start a puzzle over from scratch
     by selecting the untouched copy. BUT... If you don't want to lose
     your partially played session, you will need to back-up the save
     file.

   Play occurs in a single window of single dedicated frame, although
   that frame is divided into three dedicated windows, each displaying
   its own dedicated buffer. You should never need to leave the playable
   'Crossword grid' buffer.

   At any time, you can save your current state-of-play or restore your
   saved stated. Quitting a game will prompt you to save or discard it:

                   M-q           crossword-quit
                   C-x C-s       crossword-backup
                   C-c C-x C-f   crossword-restore


   If, while playing, you delete the crossword frame or one of its
   windows, you can use command M-x `crossword-recover-game-in-progress`.
   If you kill a clue buffer, you'll need to save, quit, and restore
   the saved game.

   General navigation within the grid buffer should be intuitive, using
   all the usual keys. Additionally, filling in a square will advance
   `POINT` to the next sensible one. Grids begin with an 'across'
   navigation for solving across clues, but that's easily changed:

                   M-a           crossword-nav-dir-across
                   M-d           crossword-nav-dir-down
                   M-<SPC>       crossword-nav-dir-toggle

   Notice that when you change direction, the font of the current clue
   changes accordingly, within the grid, and for the clues' text below
   the grid and in their dedicated buffers.

   Additionally, you can always navigate directly to any specific clue
   by its number, using the '/' key:

                   /             crossword-goto-clue

   You can enter pretty much any character I can think of into any
   square, but any non-alphabetic characters of the puzzle's language
   will be ignored (ie. you can make signs for yourself). All lower case
   alphabetic characters are immediately converted to uppercase.

   At some point, you'll want to check your work. You have three options
   for this:

                   M-c l         crossword-check-letter
                   M-c w         crossword-check-word
                   M-c p         crossword-check-puzzle

   All correctly and incorrectly solved squares will be fontified
   accordingly. Correctly solved square can no longer be edited. Errors
   for each square are logged and you are reminded of them at the bottom
   of the grid buffer when you visit the square in the future.

   You can also ask the program to give you a break and solve parts of
   the puzzle for you:

                   M-s l         crossword-solve-letter
                   M-s w         crossword-solve-word
                   M-s p         crossword-solve-puzzle

   You can browse the list of clues in the other two buffers by
   setting your current navigation direction (ie. across or down) and
   using an intuitive keybinding with a 'Meta' prefix. I don't expect
   that you'll need to use these features much because as you navigate
   within the grid, the program auto-magically re-centers the other
   buffers around the whatever are the current clues. Anyway, here are
   the command names ad default key-bindings:

                   M-up>         crossword-clue-scroll-up
                   M-down>       crossword-clue-scroll-down
                   M-<next>      crossword-clue-scroll-page-up
                   M-<prior>     crossword-clue-scroll-page-down
                   M-<home>      crossword-clue-scroll-page-home
                   M-<end>       crossword-clue-scroll-page-end

   If you want to play against the clock, toggle the timer on (and off):

                   M-p           crossword-pause-unpause-time
                   M-t           crossword-pause-unpause-timer

   That about does it, doesn't it? Here are some of the 'intuitive'
   navigation keybindings given special attentionL

                  <left>         crossword-prior-char
                  <right>        crossword-next-char
                  <RET>          crossword-next-char
                  <delete>       crossword-del-char
                  SPC            crossword-del-char
                  <baskspace>    crossword-bsp-char
                  C-a            crossword-begin-field
                  C-e            crossword-end-field
                  <TAB>          crossword-next-field
                  <backtab>      crossword-prior-field


## Feedback:

  * It's best to contact me by opening an 'issue' on the program's
    github repository (see above) or, distant second-best, by direct
    e-mail.

  * Code contributions are welcome and github starring is appreciated.

  * Please share your knowledge of other download resources, free
    software resources, and puzzle formats.


## Compatibility:

   The software has been tested on emacs version 26.1 and and emacs 28
   snapshot, both in debian on a linux terminal (non-GUI).

   Several puzzle file formats exist. This software, as currently
   written, parses the `.puz` file format created sometime in the 1990's
   by Literate Software LLC[1] and used initially by them for their
   'across' line of software for creating, solving and sharing crossword
   puzzles. This format has supposedly since become the de facto
   standard for the genre[2][3].

   [1] http://www.litsoft.com/

   [2] http://fileformats.archiveteam.org/wiki/PUZ_%28crossword_puzzles%29

   [3] https://code.google.com/archive/p/puz/wikis/FileFormat.wiki
       The contents at this URL were mangled, so I reformatted it to
       properly display its tables and code blocks in an `org-mode`
       file. See `docs/crossword_puz_format.org`.


## Download resources:

   This project wouldn't be useful without people sharing puzzles for
   free network download. A big thanks are due to **Martin_Herbach** for
   being the provider of all the default download resources
   pre-configured in this sotware.

   Please let me know of other resources that can be added.

   Check these URLs for links to other puzzles. In many cases, you
   need to look for a link labeled something like 'across-lite format'
   or 'puz format':

   * https://crosswordlinks.substack.com/?no_cover=true
     * An aggregator, pointing to pages which have download links
     * RSS: https://crosswordlinks.substack.com/feed

   * https://crosswordfiend.com/download/
     * The download links appear only with javascript enabled


## Comparable software:

   I'm not aware of any other similar software for the linux terminal.
   The following may be of interest to readers. Please let me know of
   other projects.

   * xword - Linux GUI package
     * https://sourceforge.net/projects/wx-xword/

   * shortyz - Android GUI package
     * https://f-droid.org/en/packages/com.totsp.crossword.shortyz/


## Colophon

* Copyright (C) 2018-2021 Boruch Baum <boruch-baum@gmx.com>
* Author/Maintainer:  Boruch Baum <boruch-baum@gmx.com>
* Homepage: https://github.com/Boruch-Baum/emacs-crossword
* License: GPL3+

## Some other Emacs software that I've published

* Diredc [![MELPA](https://melpa.org/packages/diredc-badge.svg)](https://melpa.org/#/diredc) [![MELPA Stable](https://stable.melpa.org/packages/diredc-badge.svg)](https://stable.melpa.org/#/diredc)
  * Large collection of interoperable dired extensions
  * https://github.com/Boruch-Baum/emacs-diredc

* Emacs-w3m
  * Extensions to the classic web browser (fork)
    * Advanced downloader (bulk, regex, queue management, resume aborted)
    * Scrub history
    * More ...
  * https://github.com/Boruch-Baum/emacs-w3m

* Crossword [![MELPA](https://melpa.org/packages/crossword-badge.svg)](https://melpa.org/#/crossword)
  * Download and play crossword puzzles, in Emacs!
  * https://github.com/Boruch-Baum/emacs-crossword

* Key-assist
  [![MELPA](https://melpa.org/packages/key-assist-badge.svg)](https://melpa.org/#/key-assist)
  [![MELPA Stable](https://stable.melpa.org/packages/key-assist-badge.svg)](https://stable.melpa.org/#/key-assist)
  * Simple keybinding cheatsheet and launcher
  * https://github.com/Boruch-Baum/emacs-key-assist

* Home-end
  [![MELPA](https://melpa.org/packages/home-end-badge.svg)](https://melpa.org/#/home-end)
  [![MELPA Stable](https://stable.melpa.org/packages/home-end-badge.svg)](https://stable.melpa.org/#/home-end)
  * Turn home and end keys to multi-use navigation keys
  * https://github.com/Boruch-Baum/emacs-home-end

* Keypress-multi-event
  [![MELPA](https://melpa.org/packages/keypress-multi-event-badge.svg)](https://melpa.org/#/keypress-multi-event)
  [![MELPA Stable](https://stable.melpa.org/packages/keypress-multi-event-badge.svg)](https://stable.melpa.org/#/keypress-multi-event)
  * perform different actions when repeating a key
  * https://github.com/Boruch-Baum/emacs-keypress-multi-event

* Post-mode  - Updates to the abandoned email editing package (fork)
  * https://github.com/Boruch-Baum/post-mode
