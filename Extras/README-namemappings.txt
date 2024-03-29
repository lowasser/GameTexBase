This file describes a set of commands for using and controlling
various forms of character names.

LaTeX/parsename.sty provides name-parsing functionality, included
high-level logical names like \formal and \informal (see the comments
at the top of LaTeX/parsename.sty for thorough documentation).  The
identity datatype provides the ability to give characters alternate
names (see Extras/README-identity).  GameTeX's namemapping
functionality combines these two features with a high-level (and often
very simple) system for controlling name forms.



Overview:

This file describes namemapping commands in full detail.  However,
most of the time, you'll only be using a limited set of high-level
commands.

When writing sheets and other game text, you'll generally refer to
characters with no argument, like \cJamesBond{}, or as a character
introduction with \cJameBond{\intro}, which will usually print out a
more full version of their name.  Leaving the argument empty is a
shortcut for \cJamesBond{\usual}.  The majority of the rest of the
namemapping system is devoted to intelligently controlling what \usual
and \intro do for each character.

In the .cls file, you can set \defaultnamemappings for every
character:

  \def\defaultnamemappings{
    \titlemap{\full}
    \mapnickinformal
  }

will set every character to print their full name as their charsheet
title and every character to generally be referred to by the informal
version of their nickname (or the informal version of their normal
name, if that have no nickname).  This is for when most characters are
on an informal, first-name basis with each other.  If most characters
are more formal towards each other, use \maptrueformal instead of
\mapnickinformal.

In Lists/char-LIST.tex, you may make certain characters to have a
non-default namemapping:

  \NEW{PC}{\cBobSmith}{
    \s\MYname	{Dr.\null\pre Robert Smith}
    \maptrueformal
    ...
  }

specifies that \cBobSmith will be generally known as "Dr. Smith," the
formal form of his name.

In a character sheet, you can specify variations on how that character
refers to certain other characters, by using namemapping commands
after \begin{document}:

  \updatemacro{\cBobSmith}{
    \mapnickinformal
    \unknownplayer
  }

would mean that the character whose sheet this is in refers to
\cBobSmith as "Robert" (or perhaps "Bob") and doesn't know who is
playing them.




Commands For Using Name Forms:

These commands usually go in the datatype macro argument,
e.g. \cTest{\intro}, or as part of Char mappings.

The low-level commands are the raw parts of a name
(e.g. \cTest{\parseprefix} gives you the prefix title(s), and
\cTest{\parseword{1}} gives you the first name).  See
LaTeX/parsename.sty.  You will rarely if ever use these.

The mid-level commands are logical combinations (e.g. \cTest{\formal}
for a formal name, \cTest{\informal} for a more familiar version).
See LaTeX/parsename.sty.  You will only rarely use these directly.

The high-level commands are structured by how you use them rather than
what they generate (e.g. \cTest{\intro} to introduce a character,
\cTest{} is used most everywhere else).  See below for an exhaustive
list.  You control what they generate with a set of controlling
commands (see below).  Most of the high-level commands are used
automatically, based on context (when you use Char macros with a blank
argument).



High-Level Name Mappings: These commands incorporate the other name
commands and identities (see Extras/README-identity).  What they
generate is controlled by other commands (covered later in this file).
You will probably never use \usual, \contactmapping, or \titlemapping
directly; instead, they are used automatically.

\intro is for when you want to introduce a character in a sheet, by
giving something like their full name.  It should be the argument to
the Char macro, e.g. "\cBobSmith{\intro}"

\usual is what a Char macro in the general text of sheets will
generate with a blank arg, e.g. "\cBobSmith{}" in a character sheet.

\contactmapping is what a Char macro with a blank arg will generate
inside \contact{} (itself inside the contacts environment), e.g.
"\contact{\cBobSmith{}}".  This is in addition to the playername also
generated.

\titlemapping is what a Char macro with a blank arg will generate in
the sheet title, e.g. "\name{\cBobSmith{}}".



Commands For Controlling Name Forms:

These commands are generally used in \defaultnamemappings, when
creating Char macros, or inside \updatemacro.



Controlling Name Mappings: Each of \intro, \usual, and \contactmapping
have two parts: an identity and a mapping.  \titlemapping does not
have an identity part.  For example, if the identity part of \usual is
\nick, and the mapping part of \usual is \informal, then \usual will
generate \nick{\informal}.



High-Level Control: These commands change both the identity and
mapping parts for all of \intro, \usual, and \contactmapping.  They
are common total values for the name mappings (except \titlemapping).

\mapnickinformal changes the identity mapping of \intro and
\contactmapping to \blankid, the identity mapping of \usual to \nick,
and changes the mapping parts of all the same as \mapinformal.  This
means that the character's full name will be generated in contact
lists and when you introduce them in a sheet, but will generate
\nick{\informal} (probably the most familiar form of their name) for
\usual.  So "\cBobSmith{\intro}" will generate "Dr. Robert Smith",
"\contact{\cBobSmith{}}" will generate "Dr. Robert Smith (<player>)"
and "\cBobSmith{}" will generate "Bob".

\maptrueformal changes the identity mappings all according to
\idnormal and the mapping parts of all the same as \mapformal.  So
"\cBobSmith{\intro}" will generate "Dr. Robert Smith",
"\contact{\cBobSmith{}}" will generate "Dr. Robert Smith (<player>)"
and "\cBobSmith{}" will generate "Dr. Smith".

\maplessformal changes the identity mappings all according to
\idnormal and the mapping parts of all the same as \mapformalplain.
So "\cSamJackson{\intro}" will generate "Sam Jackson",
"\contact{\cSamJackson{}}" will generate "Mr. Sam Jackson (<player>)"
and "\cSamJackson{}" will generate "Mr. Jackson".



Controlling the Identity Part: If the identity part is \blankid, then
the mapping will not use an identity (because \blankid{<mapping>}
generates <mapping> directly).  \real, \nick, and \maiden can be used
safely even if \realname, \nickname, and \maidenname, respectively,
have not been used (they act similarly to \blankid).

\introid{<id>} changes the identity part of \intro to <id>.

\usualid{<id>} changes the identity part of \usual to <id>.

\contactid{<id>} changes the identity part of \contactmapping to <id>.

\allid{<id>} changes the identity part of \intro, \usual, and
\contactmapping to <id>.

\idreal changes the identity part of \intro, \usual, and
\contactmapping to \real.  This is useful for when a character knows
another only by their real name.

\idnick changes the identity part of \intro, \usual, and
\contactmapping to \nick.  This is useful for when a character knows
another only by their nickname.

\idmaiden changes the identity part of \intro, \usual, and
\contactmapping to \maiden.

\idnormal changes the identity part of \intro, \usual, and
\contactmapping to \blankid.



Controlling the Mapping Part: The mapping part can be any combination
of name commands, though is usually one of \full, \fullplain, \formal,
and \informal.

\titlemap{<mapping>} changes the mapping part of \titlemapping to
<mapping>.  It has no identity part, so <mapping> is the entirety of
what it will generate.

\intromap{<mapping>} changes the mapping part of \intro to <mapping>.

\usualmap{<mapping>} changes the mapping part of \usual to <mapping>.

\contactmap{<mapping>} changes the mapping part of \contactmapping to
<mapping>.

\allmap{<mapping>} changes the mapping part of \intro, \usual, and
\contactmapping to <mapping>.

\mapformal changes the mapping part of \intro to \full, the mapping
part of \usual to \formal, and the mapping part of \contactmapping to
\full.

\mapformalplain changes the mapping part of \intro to \fullplain, the
mapping part of \usual to \formal, and the mapping part of
\contactmapping to \full.  It also changes \titlemapping to
\fullplain.

\mapinformal changes the mapping part of \intro and \contactmapping to
\full and the mapping part of \usual to \informal.



Controls Local to Sheets: You can use the below commands for
controlling character names when you create the Char macro.  However,
if you want a particular sheet to view a particular character
differently, you can make changes to the Char macro that are local to
the given sheet.

Using \updatemacro{<datatype macro>}{<new updates>} in a sheet, after
\begin{document}, applies <new updates> to <datatype macro> within the
scope of the sheet.  For example:

  \updatemacro{\cBobSmith}{
    \unknownplayer
  }

means that the given sheet does not know who is playing \cBobSmith{}.

\defaultnamemappings, defined in the .cls file, declares the default
settings for the namemappings for all characters.  For example:

  \def\defaultnamemappings{
    \titlemap{\full}
    \mapnickinformal
  }

Will make \titlemap{\full} and \mapnickinformal default for all
characters.



Changing Names and Players: These commands change \MYname and
\MYplayer for a character.

\changeplayer{<playername>} changes \MYplayer to <playername>.

\unknownplayer changes \MYplayer to "unknown".

\deceased changes \MYplayer to "deceased".

\changename{<name>} changes \MYname to <name>.

\unknownname changes \MYname to "unknown".
