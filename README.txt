GameTeX/README

This is GameTeX 1.7.  The copyright notice for any and all of GameTeX
as distributed is at the end of this file.




Documentation:

Nearly all GameTeX documentation is in the form of README files and
the comments of certain files that you'll be editing.  All GMs should
read this file, as well as the README files in each of the
directories.  You should also skim through the -LIST.tex files in
Lists/.

This file covers the overall organization and installation of GameTeX,
as well as a few key concepts.  Lists/README describes in detail how
things in your game are organized, and Production/README describes how
that translates into producing your game.  LaTeX/game.cls (or whatever
it's been renamed to) contains comments on a variety of high-level
customizations.  The comments in LaTeX/gametex.sty are useful if you
want to know how things work or are planning on more serious
customization.  The Extras/ directory contains a few README- files
that cover advanced and optional features.



Requirements:

GameTeX is designed to run on Athena.  It can be run elsewhere; this
would require a complete LaTeX installation and possibly some changes
reflective of different platforms, notably the definitions of certain
filepaths.  GMs are assumed to have basic working knowledge of LaTeX.
SIPB's Inessential LaTeX can be a good starting place.  It is
available at the following locations:

  http://www.mit.edu/afs/sipb.mit.edu/project/doc/current/iLaTeX.dvi
  http://www.mit.edu/afs/sipb.mit.edu/project/doc/current/iLaTeX.PS
  http://www.mit.edu/afs/sipb.mit.edu/project/www/latex/guide/guide.html

For a more complete introduction to LaTeX, read the excellent LaTeX: A
Document Preparation System by Leslie Lamport, the original author of
LaTeX.  Though both GameTeX and LaTeX are built around the TeX
typesetting system by Donald Knuth, most GMs do not need to know its
deep mysteries.  The TeXbook, also by Donald Knuth, is the canonical
source of information about TeX, and The Advanced TeXbook, by David
Salomon, is an excellent introduction to TeX as a programming
language.



e-TeX: GameTeX requires e-TeX (an extension of TeX) to work properly.
On Athena 10, this is the default when you run latex.  On some other
systems, and older versions of Athena, you simply latex files with
"elatex filename" rather than "latex filename."  elatex should not
break in any way when run on valid latex files, so it is generally
safe to alias latex to elatex.



Don't Use the newtex Locker: The LaTeX installation in the newtex
locker (on Athena) is old and obsolete.  If you have "add -f newtex"
in your .environment file, some things may not work.  Don't use the
newtex locker; the default Athena install of LaTeX (the TeX Live
distribution, as of Athena 10) is sufficient and supported.




Setup:

To install GameTeX, simply copy the files from GameTeX/ to wherever
you are keeping files for your game.  You will probably rename the top
GameTeX/ directory to something representative of your game, such that
Blusheets/, Charsheets/, etc. are under YourGame/, or you'll put the
entire GameTeX/ tree under YourGame/GameTeX/.



The Class File: In the LaTeX/ directory is a file called game.cls.
This is the LaTeX2e class file, which is the root of GameTeX.

Since the default class name for GameTeX is game, the class file is
game.cls, and LaTeX files start with

  \documentclass[<option>]{game}

You may want to change the class name.  If you're writing multiple
games, you will definitely want to change it to make sure each game
has a unique class name.

Changing the class name requires changing the class filename and the
definition of the \gameclassname macro, set in the class file, to
match.  In addition, all game files should point to the new class
file.  For example, if you are writing a game called "Ten Days in
London," a good class name might be "london."  To set this up, you
would rename game.cls to london.cls, change the \def of \gameclassname
to london, and make sure to change the

  \documentclass[<option>]{game}

lines to

  \documentclass[<option>]{london}

in files already created, i.e. Bluesheets/README.tex,
Charsheets/README.tex, Greensheets/README.tex, Whitesheets/README.tex,
and files in Production/.

When coming up with a class name, keep in mind that the class name
will be used as an environment variable (see below).  For example,
\def\gameclassname{SHELL} would be very, very bad.  Use printenv to
see your current environment.

If you have not done major restructuring to the game's directory tree,
you may be able to use the perl script Extras/changeclass.pl (see
Extras/README) to automate changing the class name.  This is
especially useful when you first set GameTeX up and choose a class
name.

The .cls file is also for basic customization.  It includes macros
like \gamename and \gamedate.  These are, respectively, the name and
run-date of your game, and they are available and used throughout
GameTeX.  The file also includes a variety of high-level switches for
high-level customizations.



Your Environment File: In short, to be able to use GameTeX, add the
following lines to your .bash_environment file (these lines should
also work in a Mac's .zshrc file):

  export <gameclassname>=<pathtoyourgame>
  export TEXINPUTS=.:$<gameclassname>/LaTeX/:

If your Athena account is older, or you otherwise use tcsh instead of
bash, you add the following to your .environment file:

  setenv <gameclassname> <pathtoyourgame>
  setenv TEXINPUTS .:$<gameclassname>/LaTeX/:

<gameclassname> is whatever you set \gameclassname to, e.g. london or
somegame.  <pathtoyourgame> is the filepath, such as
/mit/assassin/games/SomeGame.  For example:

  export somegame=/mit/assassin/games/SomeGame
  export TEXINPUTS=.:$somegame/LaTeX/:

The TEXINPUTS environment variable tells LaTeX where to find the class
file for your game, and should at least include the path to the LaTeX/
directory for your game.  The game-specific environment variable
(e.g. $london) is used by the class file for relative filepaths.  If
you are writing multiple games using GameTeX, use something like this:

  export somegame=/mit/assassin/games/SomeGame
  export london=/mit/assassin/games/London
  export TEXINPUTS=.:$somegame/LaTeX/:$london/LaTeX/:

Once you have edited your environment file, the simplest way to load
the changes is logging out and in.




Directory Organization:

The default directories included in GameTeX are organized for basic
game design.  If your game needs a different directory structure, it
can be changed by editing the class file.  Each directory has a README
file.  You should read these.  The sheet directories (Charsheets/,
Bluesheets/, Greensheets/) each include a _template.tex file, which is
a bare, empty sheet of the right type.  "cp _template.tex blah.tex" is
a convenient way to start a new sheet.

Bluesheets/ is where bluesheets are kept.  The directory contains a
README.tex file that covers what special commands are available in
bluesheets.  This file is also an example bluesheet that can be
latex'ed.

Charsheets/ is where character sheets are kept.  The directory
contains a README.tex file that covers what special commands are
available in character sheets and is an example sheet that can be
latex'ed.

Extras/ contains a few things that don't fit directly within basic
GameTeX, including a few prebuilt customizations and examples.  See
Extras/README.

Greensheets/ is where greensheet files are kept.  Similar to the
Bluesheets/ and Charsheets/ directories, it contains a README.tex that
covers special greensheet commands, if any, and works as a latex'able
example greensheet.

Handouts/ is a place to keep your rules, scenario, playerlist, and
other documents you intend to hand out to all players.

LaTeX/ is for LaTeX style and class files.  The class file and
gametex.sty (where most of the code is) are intended to be customized;
the other style files aren't.  See LaTeX/README.

Lists/ is where -LIST.tex files are kept, for globally available
macros that act as a database for the game.  Each file defines a type
or family of types of macros, such as characters or bluesheets, etc.
These datatypes store fields of data and other code.  You should read
through and be familiar with most of this directory.  Lists/README
describes in some detail how to use datatypes in -LIST.tex files.

Notebooks/ is where fold-and-staple choose-your-own-adventure notebook
files are kept.  It contains a README.tex that covers how to make an
in-game research notebook and a README-green.tex that covers how to
make an out-of-game notebook.  Both files work as latex'able example
notebooks.

Postscript/ is where dvips header files are kept, and is a convenient
place for postscript images.  The header files are included for
special functions of the dvips program which might not be available in
all distributions.

Production/ is a convenient directory from which to run the actual
production of your game, i.e. the printing of sheets.  You tend to
edit and latex the files there to print the game.  See
Production/README.

Whitesheets/ is where whitesheets (in-game documents) are kept.  The
directory contains a README.tex file that covers what special commands
are available in whitesheets.  This file is also an example whitesheet
that can be latex'ed.




Game Elements and Owners:

Elements, like bluesheets and abilities, are created and assigned in
the Lists/ directory.  You create an element in the -LIST.tex file
matching its type, and you assign it to an owner (such as a character)
in the -LIST.tex file matching the owner's type.  Elements can own
other elements as well.  Generally, everything in the game can have an
associated element or owner.

If an element has an associated sheet, you then create the sheet in
the appropriate directory (the README.tex file is a useful guide for
this).

More details of elements and owners, including syntax, is covered in
Lists/README.




Producing Your Game:

Generally, you produce a game by latex'ing files in the Production/
directory; there is one file per type of element you produce,
e.g. listblue-PRINT.tex and abil-PRINT.tex.  Each file will produce
all the elements of the given type.  They will be sorted by owner and
generally ready to collate and stuff into packets.

Each page produced will be marked with the name of its owner (usually
the character whose packet will get it) in the page header.  This mark
will cover the full ownership path if the elements are owned by other
elements.  Things owned together will be printed together.

Some aspects of production, like printing specific elements during
game, can be handled by changing the files in Production/, with
Extras/gametex.pl, or with a scratch file.

Production/README covers game production in more detail.




Game Files:

Starting Documents: In general, most latex'able game files start with
a \documentclass command like all LaTeX2e files.  This command points
the file at the game's class file (<gameclassname>), and defines the
game option (<option>) for the file:

  \documentclass[<option>]{<gameclassname>}

Some examples:

  \documentclass[char]{somegame}
  \documentclass[abils]{london}

The <option> tells which type of game document the file is, one of a
list of options defined in gametex.sty.  Some options are built on
other options, meaning they inherit the code.  For example, the char,
blue, and green options, for character sheets, bluesheets, and
greensheets respectively, all inherit the sheet option.  The sheet
option, which can be used for generic game "sheets" (such as a
scenario or rules document), contains LaTeX code which defines the
"interface" and "look and feel" of all the game's sheets.  Options
such as char customize this and create new commands unique to their
document type.  All GameTeX options are defined in gametex.sty; it's a
good idea to read through that file there to see how the different
options are organized and what they do.

Only one GameTeX option is directly used for a given file.  Though
other options may happen to be inherited, e.g. the char option's
inclusion of the sheet option, you cannot directly specify more than
one option.  Extra GameTeX options will be ignored with a warning
message.  If, for example, you create a file with

  \documentclass[char,blue]{somegame}

the blue option will be ignored.  Anytime you specify more than one
GameTeX option, the first will will be executed properly, and the
following ones will be ignored.  You can also pass options to the base
class (article is the default); this works just like passing options
to standard LaTeX2e classes.

The Charsheets/, Bluesheets/, Greensheets/, and Whitesheets/
directories each contain a README.tex file which is a latex'able
example sheet for the respective directory.  Each of these files
includes examples of commands provided for the given type of sheet.

Most game materials, whether or not they are documents in and of
themselves, are defined in the database set up in the Lists/
directory.  Each -LIST.tex file in Lists/ defines a type of game
element, its attributes, and a list of macros for that type (using the
functionality of datatypes.sty; see Lists/README).  For game elements
that are documents, notably most sheets, the macros contain pointers
to the given filepaths.  When creating a new character sheet, for
example, you should create both the file in Charsheets/ and the
character macro in Lists/char-LIST.tex.



Sheet Extraction: GameTeX accesses the contents of charsheets,
bluesheets, etc. by reading the associated files and extracting the
text between \begin{document} and \end{document} (using the
functionality of extraction.sty).  This text is available throughout
GameTeX via datatype macros in Lists/ such that, for example, you can
easily create a file that will print all of a character's bluesheets
(instead of having to print them each by hand).  Any document setup
code included before the \begin{document}, i.e. in the LaTeX preamble,
will not be extracted.

If you want to define local LaTeX macros (or other, similar LaTeX
hacking) for a sheet, you should put them after the \begin{document}
command, so that they will be included with the sheet text when it is
extracted.  Also, in general, you should avoid manipulating global
things.  When extracting multiple sheets for use in a single file,
GameTeX gives the text of each sheet its own TeX scope; however,
\global and similar constructs can defeat this.  Make sure you have at
least a basic understanding of how GameTeX handles sheet extraction
and collation before doing arbitrary global hacking.

Table of Contents, List of Figures, List of Tables, and Cross
Referencing (\label, \ref, and \pageref) are all standard LaTeX
features that are supported within extracted sheets.  Be gentle with
these features, as the code supporting them does a few tricks for
which LaTeX was never intended.  Bibliography, Index, and Glossary are
not supported, primarily because they require preamble code.




Copyright:

Copyright (c) Ken Clary and the MIT Assassins' Guild.

You may use, copy, and/or modify this software for any purpose with or
without fee, provided that the above copyright notice and this
permission notice appear in all copies.  You may not distribute
modifications of this software, in whole or in part, under its
original name, or otherwise cause version splintering.
