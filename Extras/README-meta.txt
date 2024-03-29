GameTeX is currently at version 1.7.



Contents of this directory: GameTeX/ is a complete copy of the current
version of GameTeX.

gametex.tar.bz2 is a compressed tarball of GameTeX/.  You can
uncompress it with "bunzip2 gametex.tar.bz2" and untar the result with
"tar xf gametex.tar" (or, on linux systems, do both with "tar xfj
gametex.tar.bz2").  gametex.zip is a zipfile of GameTeX/.  You can
open it with "unzip gametex.zip".



Setting up GameTeX: To install GameTeX, simply copy the files from
GameTeX/ to wherever you are keeping files for your game.  You will
probably rename the top GameTeX/ directory to something representative
of your game, such that Bluesheets/, Charsheets/, etc. are under
YourGame/, or you'll put the entire GameTeX/ tree under
YourGame/GameTeX/.

Next you will (probably) change the GameTeX classname and set up your
environment to be able to latex files.  This is documented in
GameTeX/README.



Documentation: Nearly all GameTeX documentation is in the form of
README files and the comments of certain files that you'll be editing.
All GMs should read GameTeX/README, as well as the README files in
each of the sub-directories.  You should also skim through the
-LIST.tex files in Lists/.

GameTeX/README covers the overall organization and installation of
GameTeX, as well as a few key concepts.  Lists/README describes in
detail how things in your game are organized, and Production/README
describes how that translates into producing your game.
LaTeX/game.cls contains comments on a variety of high-level
customizations.  The comments in LaTeX/gametex.sty are useful if you
want to know how things work or are planning on more serious
customization.  The Extras/ directory contains a few README- files
that cover advanced and optional features.  A copy of this file
resides at Extras/README-meta.



Mailing List: the list gametex-gms@mit.edu is for GameTeX
announcements and occasional discussion.  You can add yourself using
blanche, listmaint, or other moira programs.



Version Changes: Major number changes (1.x -> 2) reflect a major
restructuring of how GameTeX works.  Minor number changes (1.1 -> 1.2)
are added/changed functionality.  Revision number changes (1.0.1 ->
1.0.2) are minor functionality changes that require code changes only
in .sty files (documentation may be changed elsewhere).  Letter
changes (1.1 -> 1.1a, 1.0.2b -> 1.0.2c) are bugfixes, code polish, and
isolated documentation improvements.  Some minor fixes, like
spelling/grammar fixes in documentation, won't get any version change.




Version History:

1.0 (12/29/2008): First formal release.

1.0a (7/15/2009): Fixed a bug involving notebook page numbering, added
mentions of the _template.tex files into their respective README.tex
files, and created playerlist variable \MYrole for Char and GM
datatypes, so that column can be used as-is.

1.0b (12/16/2009): Documentation for latex/elatex updated for TeX Live
distributions, now that Athena 10 uses TeX Live.  Extras/gametex.pl
now uses latex instead of elatex, for TeX Live.  GM macros are
sortable by \MYplayer, and Handouts/rules-scenario.tex does so.  Typos
fixed in Handouts/rules-scenario.tex and Extras/README-namemappings.
\EVERY commands can now contain paragraph breaks.  itemz/enum/desc
environments have slightly smarter code for calculating nested
indentation.

1.1 (2/6/2010): Skill lists are now a standard feature, produced by
Production/skilllist-PRINT.tex.  \MYstats and \MYskills are proper
ownership fields.  Stats are now an element type (skills are just
abilities assigned to \MYskills).  Expanded commands for detailed
control of element ownership (\owner, \notowner, \ownerdefault,
\suite, \notsuite, \transowner, \nottransowner).  Removed use of
deprecated pstcol package (standard pstricks package now incorporates
pstcol's features).

1.1a (4/4/2010): Fixed a bug with using \EXTEND on datatypes that use
\OPTIONHOOK (e.g. using \updatemacro directly on Identities).  Fixed
an infinite loop bug that occurred during certain invocations of
Identities.  Removed obsolete paragraph about texmf.cnf from
LaTeX/README.

1.1.1 (7/10/2010): Updated documentation for bash users (the new
Athena default).  Made quote macros, like \cenquote and \bigquote,
available in most any document.

1.1.1a (7/25/2010): Fixed a bug in Identity code that prevented
cheatsheets from compiling.

1.2 (7/16/2011): Fixed another minor Identity bug.  Moved Stat
datatype to gametex.sty, and added the \resolvestats command.  Added
the skilllist environment for convenience.  Added workflow
documentation to Production/README, and fixed some documentation
typos.

1.3 (4/22/2012): Name-parsing now supports both \western and \eastern
parsing styles (with support for \custom as well); low-level name
parsing much more powerful; parsename.sty has much more full
documentation.  \loc{<location>} is a shortcut provided for signs.
EOG signs smarter, with new \sEOG{} macro.  \unpunt tweaked so serial
uses work intuitively.  \nickname, \realname, and \maidenname take an
optional [<mods>] argument.  Places and signs put location in
ownership string (e.g. page headers).  ulem and alltt packages now
included everywhere.  Fixed bug with making change from multiple
currencies.  Other bugs and typos fixed.  Various expansions and
changes made to example rules document, including new guideline
comments, generic interruptible mechanic, new bag-searching rules,
bodies are written as 3-hands bulky, draft stub of "New Rule Summary"
section, and other small additions.

1.3a (6/3/2012): Sign and place ownership strings correctly leave out
() for blank locations.  Rope rules have explicit tying/untying
mechanic.

1.3b (7/21/2012): Fixed an obscure bug for using \notowner inside
\raw.  Fixed some cheatsheet formatting and recursion detection.

1.4 (7/12/2014): json-PRINT.tex produces a .json file with select
parts of game's database; \jsonkeyval provided for populating \MYjson
with sanitized custom values; README-json suspiciously blank.  Added
smoother version of the dagger graphic for smaller image use.  Added
support for point-based skills on statcards (see char-LIST.tex).
Added \MYwrapup field for characters.  Added wrapup document template
to Handouts/.

1.4.1 (8/9/2014): \MYfile added to cheatsheet; cheatsheet formatting
corrected.  Added bluesheets template to wrapup.tex; added example
wrapup entry to \cTest; added wrapup.tex mention to Handouts/README.
Some typos fixed.

1.4.1a (11/12/2014): Fixed gametex.pl to account for ghostscript bug.

1.4.1b (7/15/2016): Notebook-style labels use \bookspace instead of
\packetspace, to ease some customization hackery.  Fixed bugs with
transferable owners, subowners, and \unlist.  Some typos fixed in
comments.

1.5 (6/30/2017): Reformatted nearly all 'code' for slightly better
readability.  Added gender-neutral pronouns (Spivak, 'ey' variation)
as a gender option.  Fixed bug with landscape playerlist, accounting
for newer versions of dvips.

1.6 (11/28/2023): All instances of \MYsex replaced with \MYgender.
\updatePEG (player, email, gender) added alongside \updateplayer,
\updateplayeremail, etc.  \mfn (shortcut for creating a pronoun
without a neuter option) added.  \Gender and \gender added as pronouns
(like \Sex and \sex).  \badgedagger added to .cls file, to control
optional easter-egg of using the dagger image as a badge background.

1.6.1 (12/1/2023): Various internal changes to increase compatibility
with pdflatex.  bigdagger.pdf and smalldagger.pdf added.  Skill cards
and box labels now implemented as double-sided (skill cards have a
death report, box labels have blank backs), so every document type is
intended to be duplexed.  Various copy/paste errors in comments fixed.

1.7 (12/16/2023): Added latexmkrc file for Overleaf projects.  Added
.txt extension to all plain-text README files (so Overleaf will
display them).  Distribution includes gametex.zip file.  Fixed
incorrect type for \eog macro.  Added \sEOG{} and \sMediumEOG{} as
verbose alternatives to \eog and \eogmedium.  JSON sanitation no
longer escapes single quotes or back quotes.  Fixed missing first fold
label for fold-and-staple packets.  Fixed typo in
Charsheets/README.tex.




Known Issues:

None currently.




Copyright (c) Ken Clary and the MIT Assassins' Guild.

You may use, copy, and/or modify this software for any purpose with or
without fee, provided that the above copyright notice and this
permission notice appear in all copies.  You may not distribute
modifications of this software, in whole or in part, under its
original name, or otherwise cause version splintering.
