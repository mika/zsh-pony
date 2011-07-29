                             The Zsh Pony
                             ============

Author: 
Date: 2011-07-29 21:33:37 CEST


============

Table of Contents
=================
1 Preface 
2 Grml-zshrc 
3 Switching directories for lazy people 
4 Share history file amongst all Zsh sessions, ignoring dupes 
5 Option Setting in Zsh, AKA setopt* 
6 Replace spaces in filenames with a underline 
7 Fast Manpage access 
8 Completion System 
    8.1 Enable completion 
    8.2 Menu Selection 
    8.3 Use colors in completion 
    8.4 Pick item but stay in the menu 
9 Globbing / Glob Qualifiers 
10 Keybindings 
    10.1 Run `bindkey` to get a listing of currently active keybindings 
    10.2 Get emacs-like keybindings 
    10.3 Tip: run "bindkey ctrl-v <keys>" to find out which action is bount to a key 
    10.4 Some interesting keybindings 
    10.5 Remove last part from directory name 
    10.6 Keybindings {up,down}-line-or-search and history-beginning-search-{backward,forward}-end 
    10.7 Incremental search with history-incremental-pattern-search-backward: 
    10.8 Zsh Line Editor (AKA zle) 
    10.9 Edit command line in editor 
    10.10 Insert a timestamp on the command line (yyyy-mm-dd) 
    10.11 Insert last typed word 
    10.12 Complete word from history with menu 
11 Loadable modules 
    11.1 Play tetris 
    11.2 URL quoting 
12 Prompt 
    12.1 Exit code in prompt, if it's not exit code 0 
    12.2 Special functions 
        12.2.1 precmd(): executed before each prompt - e.g. for setting prompt information 
        12.2.2 preexec(): running before every command - e.g. for setting GNU screen title 
    12.3 RPOMPT with a smiley (note: the version in grml-zshrc is more sophisticated -> moving smiley) 
13 Get VCS information into your prompt - vcs\_info 
14 Hashed directories 
15 On-the-fly editing of variables 
16 History 
    16.1 fc 
    16.2 Top 10 commands 
    16.3 Check your history for most frequently used commands and create aliases/functions for them (AKA top10): 
17 Text replacing 
18 Suffix aliases 
19 Grml-zshrc specific stuff 
    19.1 List changelog of a Debian package 
    19.2 In-place mkdir to create directory under cursor or the selected area 
    19.3 Create a temporary directory and change cwd to it 
    19.4 Directory specific shell configuration with Zsh 
    19.5 Smart cd 
    19.6 grml-zsh-fg 
    19.7 sudo-command-line 
20 Fast directory switching 
    20.1 check out "dirstack handling" in grml-zshrc for persistent directory stack feature 
21 Speed up typing 
22 FAQ 
23 Important Resources 
24 Credits 
25 Copyright 


1 Preface 
----------

  The Zsh defaults to a minimalistic configuration which doesn't show the
  potential behind this powerful and flexible shell. The Zsh pony project
  provides a list of really hot stuff of what's possible with Zsh.

2 Grml-zshrc 
-------------
Grab a fully featured Zsh configuration:


  % wget -O .zshrc        http://git.grml.org/f/grml-etc-core/etc/zsh/zshrc


3 Switching directories for lazy people 
----------------------------------------


  % setopt autocd && /tmp


4 Share history file amongst all Zsh sessions, ignoring dupes 
--------------------------------------------------------------


  % setopt append_history share_history histignorealldups


5 Option Setting in Zsh, AKA setopt* 
-------------------------------------


  % setopt $OPTION
  % man zshoptions


6 Replace spaces in filenames with a underline 
-----------------------------------------------


  % autoload -U zmv
  % touch 1\ 2  3\ 4\ 5
  % zmv '* *' '$f:gs/ /_'


7 Fast Manpage access 
----------------------


  % autoload run-help
  % echo foo | xargs <esc-h>
  
  and then:
  
  % git commit<esc-h>
  
  or even ('g' being an alias for git and 'co' and git alias for commit):
  
  % g co<esc-h>


8 Completion System 
--------------------

8.1 Enable completion 
======================


  % autoload compinit && compinit
  % kill c<tab>
  % man z<tab>
  % dpkg -L <tab>


8.2 Menu Selection 
===================


  % zstyle ':completion:*' menu select



Layout is :completion:FUNCTION:COMPLETER:COMMAND-OR-MAGIC-CONTEXT:ARGUMENT:TAG

Tip: Get completion help running 'ctrl-x h'.

8.3 Use colors in completion 
=============================


  zstyle ':completion:*:default'         list-colors ${(s.:.)LS_COLORS}


8.4 Pick item but stay in the menu 
===================================


  % bindkey -M menuselect "+" accept-and-menu-complete
  % ls <tab> +


9 Globbing / Glob Qualifiers 
-----------------------------
Makes find(1) useless for many jobs.


  % setopt extendedglob
  % rm ../debianpackage(.)   # remove files only
  % ls -d *(/)               # list directories only
  % ls /etc/*(@)             # list symlinks only
  % ls -l *.(png|jpg|gif)    # list pictures only
  % ls *(*)                  # list executables only
  % ls /etc/**/zsh           # which directories contain 'zsh'?
  % ls **/*(-@)              # list dangling symlinks ('**' recurses down directory trees)
  % ls foo*~*bar*            # match everything that starts with foo but doesn't contain bar



The e glob qualifier -  e.g. to match all files of which file
says that they are JPEGs:



  % ls *(e:'file $REPLY | grep -q JPEG':)



- (#s) or (#e) for what ^ and $ are in regexps (beginning of line/end of line)
- (#b) or (#m) to enable backreferences
- (#i) to match case insensitive
- (#a) to match approximately (certain errors are ignored, e.g. "(#a1)foo*" matches the string "ofobar")

Tip: run e.g. `ls *(<tab>` to get help regarding globbing.

10 Keybindings 
---------------

10.1 Run `bindkey` to get a listing of currently active keybindings 
====================================================================
Notes:
1) \^ := ctrl
2) \^[ := esc

10.2 Get emacs-like keybindings 
================================
Zsh defaults to vi keybindings ('bindkey -v') if $VISUAL or $EDITOR contain string 'vi'.
Run 'bindkey -e' to get emacs-like keybindings then.

10.3 Tip: run "bindkey ctrl-v <keys>" to find out which action is bount to a key 
=================================================================================

10.4 Some interesting keybindings 
==================================
  Keybinding   Meaning                                                             
 ------------+--------------------------------------------------------------------
  ctrl-d       complete + EOF                                                      
  ctrl-l       clear screen                                                        
  ctrl-w       delete last word                                                    
  ctrl-\_      undo                                                                
  tab          complete and take first result                                      
  esc-.        insert last parameter of last typed command (similar to typing !$)  
  ctrl-a       begin of line                                                       
  ctrl-e       end of line                                                         
  alt-'        quote-line ('')                                                     
  alt-?        which-command                                                       
  ctrl-k       kill line                                                           
  ctrl-u       kill while line (kill-ring)                                         
  ctrl-w       copy last word (kill-ring)                                          
  ctrl-y       yank (insert kill-ring)                                             
  esc-q        push line                                                           

10.5 Remove last part from directory name 
==========================================


  % slash-backward-kill-word() {
      local WORDCHARS="${WORDCHARS:s@/@}"
      zle backward-kill-word
  }
  % zle -N slash-backward-kill-word
  % bindkey '\e^?' slash-backward-kill-word
  % cd /usr/share/doc/mutt/examples/<alt+backspace>
  
  Note: configured by default in grml-zshrc, so ready for usage out-of-the-box.


10.6 Keybindings {up,down}-line-or-search and history-beginning-search-{backward,forward}-end 
==============================================================================================


  % echo 123
  % echo 234
  % ls
  and then:
  % echo <cursor-up|down>
  vs.
  % echo 2<page-up|down>


10.7 Incremental search with history-incremental-pattern-search-backward: 
==========================================================================


  % <ctrl-r>scp*r


10.8 Zsh Line Editor (AKA zle) 
===============================
1) It's what readline is for bash (move, delete, copy words/lines/...)
2) Basic layout of custom widgets, used like functions:


  % foobar() { LBUFFER="foobar $LBUFFER"; } # function
  % zle -N foobar         # declare function as bindable widget
  % bindkey '^x^s' foobar # bind command to a keybinding


3) ctrl-x-z provides help_zle_parse_keybindings in grml-zshrc

10.9 Edit command line in editor 
=================================


  % autoload edit-command-line && zle -N edit-command-line
  % bindkey '\ee' edit-command-line
  % $SOME_COMMAND_LINE <esc-e>


10.10 Insert a timestamp on the command line (yyyy-mm-dd) 
==========================================================


  insert-datestamp() { LBUFFER+=${(%):-'%D{%Y-%m-%d}'}; }
  zle -N insert-datestamp
  bindkey '^Ed' insert-datestamp


10.11 Insert last typed word 
=============================


  % insert-last-typed-word() { zle insert-last-word -- 0 -1 };
  % zle -N insert-last-typed-word;
  % bindkey "\em" insert-last-typed-word
  % mv foobar <esc-m>


10.12 Complete word from history with menu 
===========================================


  % zle -C hist-complete complete-word _generic
  % zstyle ':completion:hist-complete:*' completer _history
  % bindkey "^X^X" hist-complete


11 Loadable modules 
--------------------

11.1 Play tetris 
=================


  % autoload -U tetris
  % tetris


11.2 URL quoting 
=================


  % autoload -U url-quote-magic
  % zle -N self-insert url-quote-magic


Disclaimer: annoying when using e.g. [http://example.org/foo{1,2,3}.tgz]

12 Prompt 
----------


  % autoload -U promptinit
  % promptinit
  % prompt fire
  % prompt <tab>


12.1 Exit code in prompt, if it's not exit code 0 
==================================================

12.2 Special functions 
=======================

12.2.1 precmd(): executed before each prompt - e.g. for setting prompt information 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

12.2.2 preexec(): running before every command - e.g. for setting GNU screen title 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

12.3 RPOMPT with a smiley (note: the version in grml-zshrc is more sophisticated -> moving smiley) 
===================================================================================================


  precmd () { RPROMPT="%(?..:()%" }


13 Get VCS information into your prompt - vcs\_info 
----------------------------------------------------


  autoload -Uz vcs_info
  precmd() {
    psvar=()
    vcs_info
    [ -n $vcs_info_msg_0_ ] && psvar[1]="$vcs_info_msg_0_"
  }
  PS1="%m%(1v.%F{green}%1v%f.)%# "


14 Hashed directories 
----------------------


  % hash -d doc=/usr/share/doc
  % cd ~doc
  % hash -d deb=/var/cache/apt/archives
  % sudo dpkg -i ~deb/foobar*deb


15 On-the-fly editing of variables 
-----------------------------------


  % vared PATH


16 History 
-----------
Supports csh style bang history expansion.


  % history  # last 16 events
  % history -E 0  # all history events including date/time information
  % !23      # Re-execute history command 23
  % !!       # The last command.
  % !$       # Last word of the last command.
  % !-2      # The last but one command.
  % !-2$     # The last word of the command before the last command.
  % !#$      # The last word of the current command line.
  % !#0      # The first word of the current command line.
  % !?foo    # The last command that matches the pattern `foo'.
  % !?foo?1  # The second word of the last command line that matches `foo'.



...and that's really just the start. History expansion is extremely versatile
and powerful - but also a bit cryptic for the untrained eye. Practice, young
padawan, makes perfect. .o( man zshexpn | less -p '\^HISTO.*ANSION$' )

16.1 fc 
========
+ fc -p/fc -a/fc -P deals with the "history stack"
+ "fc -p" clears out the current history and starts with a new one,
  until you run fc -P, which will restore the old history again
+ You can use that to "bind" certain histories to specific directories.

16.2 Top 10 commands 
=====================

16.3 Check your history for most frequently used commands and create aliases/functions for them (AKA top10): 
=============================================================================================================


  % print -l -- ${(o)history%% *} | uniq -c | sort -nr | head -n 10


17 Text replacing 
------------------


  % mkdir -p /tmp/linux-2.6.3{8,9}/demo
  % cd /tmp/linux-2.6.38/demo
  % cd 38 <tab>
  
  % echo foo
  % ^foo^bar
  
  % echo foo_bar
  % echo !$:s/foo/baz/


18 Suffix aliases 
------------------


  % alias -s txt=vim
  % foobar.txt
  % alias -s pdf=xpdf
  % print.pdf


19 Grml-zshrc specific stuff 
-----------------------------

19.1 List changelog of a Debian package 
========================================


  % dchange $DEBIAN_PACKAGE


19.2 In-place mkdir to create directory under cursor or the selected area 
==========================================================================


  % cp file /tmp/doesnotexist/<ctrl-xM>


19.3 Create a temporary directory and change cwd to it 
=======================================================


  % cdt


19.4 Directory specific shell configuration with Zsh 
=====================================================
See [http://michael-prokop.at/blog/2009/05/30/directory-specific-shell-configuration-with-zsh/]
Hint: do you remember the fc section? You can combine the directory specific shell configuration with 'fc -p $file'!

19.5 Smart cd 
==============


  % which cd
  cd () {
          if [ -f ${1} ]
          then
                  [ ! -e ${1:h} ] && return 1
                  print "Correcting ${1} to ${1:h}"
                  builtin cd ${1:h}
          else
                  builtin cd ${1}
          fi
  }
  % cd /etc/fstab


19.6 grml-zsh-fg 
=================


  % vim # ... <ctrl-z>
  % echo foobar
  % <ctrl-z>


19.7 sudo-command-line 
=======================


  % which sudo-command-line
  sudo-command-line () {
          [ -z $BUFFER ] && zle up-history
          if [ $BUFFER != sudo\ * ]
          then
                  BUFFER="sudo $BUFFER"
                  CURSOR=$(( CURSOR+5 ))
          fi
  }
  % gparted /dev/sda <ctrl-o s>


20 Fast directory switching 
----------------------------


  % cd -<tab>


20.1 check out "dirstack handling" in grml-zshrc for persistent directory stack feature 
========================================================================================

21 Speed up typing 
-------------------
  Long version                             Short version                                            
 ----------------------------------------+---------------------------------------------------------
  for i in $(seq 2 9); do echo $i ; done   for i in {2..9}; echo $i                                 
  ls $(which vim)                          ls =vim                                                  
  cat bar baz $PIPECHAR sort               sort <b{ar,az}                                           
  ls /usr/share/doc/mutt/examples          ls /u/s/d/m/e<tab>                                       
  gzip -cd foo.gz && less foo              less <(gzip -cd foo.gz)                                  
  ls >file1; ls >file2; ls >file3          ls >file1 >file2 >file3                                  
  -                                        less <file1 <file2                                       
  -                                        diff <(sort foo) <(sort bar)                             
  -                                        xpdf =(zcat ~doc/grml-docs/zsh/grml-zsh-refcard.pdf.gz)  

22 FAQ 
-------
1) Q: How to I get a listing of all my currently in use options?

  Answer:


    setopt ksh_option_print && setopt
  
  or:
  
    printf '%s=%s\n' "${(@kv)options}"


2) Q: Why do I get "zsh: command not found:" even though I just installed the program?

  Answer: execute:


  % rehash


  or use completion system as provided by grml-zshrc (completion will rehash automatically).
3) Q: What's this strange word splitting thing?

  Answer: see [http://zsh.sourceforge.net/FAQ/zshfaq03.html]


  % var="foo bar"
  % args() { echo $#; }
  % args $var
  1
  % setopt shwordsplit
  % args $var
  2


23 Important Resources 
-----------------------
1) Zsh Homepage: [http://zsh.sourceforge.net/]
2) Zsh Wiki: [http://zshwiki.org]
3) Zsh Manpages: man zshall
4) Zsh Reference Card: [http://www.bash2zsh.com/zsh\_refcard/refcard.pdf]
5) User's Guide to ZSH: [http://zsh.sourceforge.net/Guide/] (old but still interesting)
6) Zsh Talk by caphuso:  [http://ft.bewatermyfriend.org/comp/zshtalk.html]
7) English Book: [http://www.bash2zsh.com/]
8) German Book: [http://zshbuch.org/]
9) Grml's Zsh stuff: [http://grml.org/zsh/]


[http://www.bash2zsh.com/zsh\_refcard/refcard.pdf]: http://www.bash2zsh.com/zsh_refcard/refcard.pdf

24 Credits 
-----------

Thanks to Frank Terbeck for reviewing and his valuable feedback (which isn't limited to this document :)).

25 Copyright 
-------------
(c) 2011 by Michael Prokop <mika@grml.org>

[1] DEFINITION NOT FOUND: 1

