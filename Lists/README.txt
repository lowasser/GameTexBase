Lists/README

Contained in Lists/ is a set of files that define a database of
elements in the game, such as characters, bluesheets, and abilities.
This database is available throughout GameTeX.  These files are lists
of information and you should not run latex on them.  The database is
powered by the object-oriented system provided by datatypes.sty.  Each
-LIST.tex file in the directory defines a type of game element and a
list of such element, in the simpler cases.  This file describes in
some detail the basics of creating and using datatypes and datatype
macros.

From the point of view of production, the most relevant files are
char-LIST and place-LIST.  Generally, the \production command (see
Production/README) automates game production from those two files.
All game elements can be owned by either characters or places.  A
place may correspond to zero or more signs, items, and other things
not starting in a character packet.

Characters have a one-for-one relationship with names, charsheets,
badges, statcards, skilllists, playerlist entries, and box labels.
You can switch some of those off for given characters (see the
comments in char-LIST.tex).  If you want to give a character extra of
any of those (or give any to other elements), use the Identity
datatype.  See Extras/README-identity.

In addition to basic signs, sign-LIST.tex covers a variety of signs,
labels, and other kinds of paper slips that are useful for all sorts
of complicated mechanics and envelope-stuffing.

All elements have a \MYnumber field (blank by default).  When set to
something other than a blank value, the number will be automatically
typeset on the element.  Note that ability card numbers will print in
the effects and mempacket numbers will print in the contents.




Elements and Owners:

(Many of underlying commands used in examples here are described later
in this file.)

You create an element within the -LIST.tex file for that kind of
element.  For example, you create an ability card in abil-LIST.tex:

  \NEW{Abil}{\aMagic}{
    \s\MYname	{Magic Spell}
    \s\MYtext	{You can cast spells.  You must be standing still to
		cast.}
    \s\MYeffect	{I cast a spell.}
  }

For an element representing a sheet, you do the same, in addition to
creating the sheet itself.  The element contains a reference to the
sheet's filename.  In blue-LIST.tex, you might write

  \NEW{Blue}{\bArmy}{
    \s\MYname	{Army Base Personnel}
    \s\MYfile	{army.tex}
  }

You assign elements to an owner in the -LIST.tex for the owner type
(generally either char-LIST.tex or place-LIST.tex).  For example, you
assign elements to a PC in char-LIST.tex with something like

  \NEW{PC}{\cGomer}{
    \s\MYname	{Private \pre Gomer Pyle}
    \s\MYfile	{gomer.tex}
    \s\MYblues	{\bArmy{}}
    \s\MYabils	{\aMagic{}\aDriveTank{}\aMechanic{}}
    \s\MYitems	{\iMagicRock{}%
		\iToolbox{}%
		\iRifle[\num{22222}]{}\note{starts loaded}%
		\multi{3}{\iRifleAmmoClip{}}%
		}
  }

You can also assign one-shot macros to owners directly, as in

  \NEW{Place}{\pCathedral}{
    \s\MYname	{Cathedral}
    \s\MYwhere	{35-255}
    \s\MYitems	{\iCrucible{}%
		\iCrucifix{}%
		\iExpensiveBible{}%
		\pageof{\iHymnal{}}%
		\raw{\multi{20}{\iWafer{}}}%
		}
    \s\MYsigns	{\sAltar{}%
		\sBaptismalFont{}%
		\signbig{Stained Glass Windows}{These windows depict a
		variety of Christian Saints performing miracles.}%
		}
  }

Such use of one-shot macros can be useful in dynamic systems and other
automated tricks; however, they may obfuscate the game's organization
of elements.




Elements Owning Elements:

Characters and places can own elements.  So can other elements.
Characters and places are owners; elements are owned by them at a root
level.  An element can be a subowner which owns a subgroup of
elements.

For example, a memory packet can own a greensheet and ability card,
that a character gains when they open the packet:

  \NEW{MemEnvelope}{\mAmazing}{
    \s\MYname	{if you see something amazing}
    \s\MYtext	{Wow!  That was amazing!  Hey, you seem to be able to
		do amazing things, too.  See the enclosed greensheet
		and ability.}
    \s\MYabils	{\aTest{}}
    \s\MYgreens	{\gTest{}}
  }

Items, Money, Whitesheets, and other transferable items are
transferable elements.  When such a one owns elements, the elements
are transferred with the item.  For example, an item card can have an
ability card associated with it.  This can be used for complex
devices:

  \NEW{Item}{\iWhatzit}{
    \s\MYname	{Whatzit}
    \s\MYnumber	{12345}
    \s\MYtext	{If you press it, open packet a.  If you twirl it, open
		packet b.  If you pull it, open packet c.}
    \bulky	{1}
    \s\MYsigns	{\signstrip{a}{it goes ``beep.''}%
		\signstrip{b}{it goes ``whoop.''}%
		\signstrip{c}{it goes ``bang.''}%
		}
    \s\MYabils	{\ability{Stop Crying}{By futzing with the Whatzit,
		you can make babies stop crying.}{I make the baby stop
		crying.}
		}
  }

subowner-LIST.tex is a file for subowners that are not elements
themselves.  It contains the SubOwner and TransSubOwner datatypes.
You can use them if you want a subowner (or suite; see below) that has
no single identifying element to be owned by.



Suites: A suite is a group of elements put together in a package
without an actual subowner.  A character given a suite will own all of
its components.  A suite can be built just like a subowner, but with
the \suite command added.  For example, giving a character the
\aSpecial{} ability will also give them the \gTest{} greensheet:

  \NEW{Abil}{\aSpecial}{
    \s\MYname	{Special Powers}
    \s\MYtext	{You have special powers, as detailed in your \gTest{}
		greensheet.}
    \s\MYeffect	{I have special powers!}
    \s\MYgreens	{\gTest{}}
    \suite
  }

The type of macro used to create a suite is flexible.  Use whichever
type is most fitting to your design.

  \NEW{Blue}{\bRunners}{
    \s\MYname	{Marathon Runners}
    \s\MYfile	{runners.tex}
    \s\MYgreens	{\gRunning{}}
    \s\MYabils	{\aRunner{}}
    \s\MYitems	{\iRunningShoes{}}
    \suite
  }

Giving a character the \bRunners{} bluesheet will also give them the
\gRunning{} greensheet, the \aRunner{} ability, and the
\iRunningShoes{} item.

Note that elements which only own stats (in \MYstats) or skills (in
\MYskills), and own no other types of elements, will automatically
behave as suites.  This is because stats and skills are not produced
as separate elements, but as entries on the character's statcard and
skilllist.



Detailed Control: You can directly control an element's ownership
status and related features.  You may want to force a subowner to act
as a suite (as above).  You may also need to override previous
commands, such as when you have a parent type specifying suite
behavior but want a child type to have non-suite behavior.

\owner forces an element to be a subowner, regardless of whether it
owns anything (\suite and \notsuite separately control if it is a
suite).  \notowner forces an element to not be a subowner (similar to
all instances of the element being wrapped in \raw; see below).
\ownerdefault makes an element a subowner if it owns elements, which
is the default behavior.  The most recent of \owner, \notowner, or
\ownerdefault takes precedence.

\suite makes a subowner behave as a suite.  \notsuite makes a subowner
behave like a regular subowner, which is the default behavior.  The
most recent of \suite or \notsuite takes precedence.

\transowner makes an element transferable (the default for Items,
Money, Whitesheets, and other transferable things).  \nottransowner
turns this off (the default for everything else).




Useful Commands:

\multi can be used to generate multiple copies of the same thing:

  \multi{5}{\iGun{}}

will give 5 copies of the gun item.  If this is given to a character,
the itemized list of contents at the bottom of their sheet will list
"Gun(x5)".

  \raw{<code>}

will produce <code>, except <code> will not be processed for ownership
purposes.  If <code> is a subowner or suite, those properties will be
ignored.  This is useful when you want raw TeX code to be produced
without interfering with ownership processing.  Something like
\raw{\multi{100}{\iRock{}}} will be processed quickly by bypassing
ownership information (and have no problems if \iRock{} owns nothing).

  \pageof{<card>}

will create enough copies of <card> to fill a page.  In game text
(including itemized lists), it will produce "<card>(x page)".  Similar
to \raw, \pageof will prevent <card> from being processed for
ownership purposes.  Use \pageof with only one card at a time.




Using Datatypes in Lists/:

(La)TeX and Datatypes syntax: TeX has no real syntax-checking
mechanism.  When combined with the complexity of datatypes, this can
cause bizarre output and/or strange errors in strange places.  Be
mindful of correct syntax, especially within -LIST.tex files and the
datatype commands described below and therein.

Note that some of the datatype commands (like \PRESETS, \POSTSETS, and
\NEW) are somewhat forgiving of extraneous whitespace.  Some of the
examples below illustrate that you do not need to put a % after every
line inside certain commands.  (Inside (La)TeX definitions, you would
do this to avoid extra whitespace showing up when you don't mean it.)
There are ways to get around this and get extraneous whitespace and
similar anyway, so you still want to use discipline when writing code.



Datatypes, and Creating Them: Datatypes act like classes in object
oriented programming.  They can inherit parent types (superclasses).
Datatype macros are effectively objects or instances of the given
datatype.  For example, a "person" class would be a subclass of a
"mammal" class, and "John Doe" would be an object of class "person;" a
CombatAbility datatype would have the Ability datatype as a
parenttype, and \aFlyingJumpKick{} would be a CombatAbility macro.

  \DECLARETYPE{<typestring>}

creates a new datatype with the given <typestring>.  The <typestring>
is the unique identifier for a given datatype.  For example,
\DECLARETYPE{Foo} creates the Foo datatype.

  \DECLARESUBTYPE{Foobar}{Foo}

creates the new Foobar datatype with the Foo datatype as its
parenttype.  Foobar is then a subtype of Foo.

  \PRESETS{Foobar}{<presetscontents>}

is a good place to declare fields (see the Declaring, Setting, and
Using Fields section below) for a datatype.  If the datatype is a
subtype, then it inherits the <presetscontents> of its parenttype,
i.e. it has the same declared fields as its parenttype, in addition to
its own.

  \POSTSETS{Foobar}{<postsetscontents>}

is a good place to do postprocessing of fields, i.e. after they are
set by \NEW; see below.  Subtypes inherit the parenttype's
<postsetscontents>.

Multiple uses of \PRESETS or \POSTSETS will accumulate more
<presetcontents> or <postsetcontents>.



Creating Datatype Macros: Datatype macros, which act like "objects" or
"instances" of datatypes, can be created by \NEW.  \NEW includes
<contents> which is unique to the given macro.  To create a Foobar
macro:

  \NEW{Foobar}{<command>}{<contents>}

When a datatype macro is created as such, it is added to a list of
macros for the given datatype, which is accessible via the \EVERY
command (see the Using Datatype Macros section below).  If a datatype
has a parenttype, macros of that datatype are also added to the \EVERY
list of the parent datatype.

Sometimes you may want to create a datatype macro that also takes some
number of arguments:

  \newinstance{Foobar}{\somecommand[<num>]}{<contents>}

creates the command \somecommand{...}{...}{...} which takes <num>
arguments.  Just as with standard (La)TeX macros, you are limited to
nine arguments.  You can use them inside <contents>, for example:

  \newinstance{Foobar}{\somecommand[3]}{
    \s\somefield  {#1 blah #2 blah}
    \s\otherfield {#3 blah blah}
  }

\somecommand{...}{...}{...} will be a wrapper around
\INSTANCE{Foobar}, so it won't be added to any \EVERY list.  It will
always act like <option> is left blank.  See the Using Datatype Macros
section below.  The \newinstance command is defined in the class file,
not in datatypes.sty.



The Optional Argument: Datatype macros take an optional argument
(between []'s) which can be used to make local, lasting changes to that
specific use of the macro.  Within that use, the definition of \ME
(see below) is changed to reflect the optional argument.  When GameTeX
uses datatypes programmatically, it does so based on \ME, so the
optional argument lets the changes work correctly.  For example:

  \iGun[\rs\MYnumber{5432}]{}

will give you an \iGun{} item with a custom number.  The number will
persist for that item, but not for other uses of \iGun{}.  This is
most useful when you want multiple copies of the same item with
different numbers, but it can be used for any parameterized values
(though in some cases, an instance created by \newinstance is more
convenient).

The \num command is a convenient shortcut for changing the number of
an element.  \iGun[\num{5432}]{} will do the same as above.



Declaring, Setting, and Using Fields: Within datatypes, the following
commands are used to declare, set, and manipulate information
"fields."  Fields are essentially basic TeX macros, i.e. are expanded
the same.  These commands are a good way to set up error checking to
catch typos, declare fields that must be explicitly set before being
used, set fields with defaults, catch accidentally setting them twice,
etc.

The commands \F, \FD, and \FS are usually used in the
<presetscontents> of \PRESETS; the commands \s, \rs, \append, and
\prepend are usually used in the <contents> of \NEW:

  \F\field

declares \field with no default, i.e. it must be set before it is
expanded or it gives an error.

  \FD\field{<default>}

declares \field with <default> as the default value.

  \FS\field{<value>}

declares \field, and explicitly sets it to <value>.

  \s\field{<value>}

explicitly sets \field to <value> and gives a warning if \field has
already been explicitly set.

  \rs\field{<value>}

explicitly sets \field to <value>.

  \sd\field{<default>}

gives the already declared \field a default value of <default>.

  \append\field{<value>}

explicitly sets \field by appending <value> to the end of the previous
expansion of \field.  If \field has not been explicitly set before, it
is explicitly set to <value> (possibly replacing a <default> value).

  \prepend\field{<value>}

explicitly sets \field by prepending <value> to the beginning of the
previous expansion of \field.  If \field has not been explicitly set
before, it is explicitly set to <value> (possibly replacing a
<default> value).



Using Datatype Macros: By using a given datatype macro, you can access
the information inside, namely its fields.  A datatype macro takes a
single argument (<option>), which explicitly requires surrounding
braces.

Given \NEW{Foobar}{\fCommand}{<contents>}:

  \fCommand{<option>}

will call the Foobar macro \fCommand and do <option> inside the
environment of \fCommand.  With this you can reference anything within
<contents>, including fields.  For example, \fCommand{\somefield} will
give you the value of \somefield inside the \fCommand macro.

Leaving <option> blank makes the macro output whatever the Foobar
datatype is currently "mapped" to.  If \MAP{Foobar}{\somefield} has
been done, then \fCommand{} will have basically the same effect as
\fCommand{\somefield}.  See the Changing Mappings section below.

With the \EVERY command, you can use all of the macros created for a
given datatype, in the order they were created with \NEW.
Essentially, using

  \EVERY{Foobar}{<option>}

will give the same result as if you called every Foobar macro with
<option>.  Note that any datatype macro is also included in its
parenttype's \EVERY list, assuming it has a parenttype.

With \INSTANCE, it is possible to create and use, at the same time, a
unique and temporary instance of a datatype.  <contents> for \INSTANCE
would substitute for <contents> in an equivalent use of \NEW, while
<option> would work the same as for a macro created by \NEW.  Hence:

  \INSTANCE{Foobar}{<contents>}{<option>}

would have a similar effect to the following:

  \NEW{Foobar}{\temp}{<contents>}
  \temp{<option>}

However, it does not create an actual macro.  This temporary instance
is not added to any \EVERY list.  Many of the -LIST.tex files define
wrapper macros around \INSTANCE, for simple use.  See each file for
details.

For datatype macros (such as \cTest{<option>}) and
\INSTANCE{...}{...}{<option>}, the <option> arg has required braces.
For example, even if you are leaving <option> blank, which will happen
often, you still need \fCommand{}, or else you will get an error.

For all datatypes, within both <contents> (as specified by \NEW) and
<option> (as specified by each individual macro) the following
commands are available:

  \MYTYPESTRING

is the typestring of the datatype, e.g. "Foobar" for Foobar
datatypes.

  \MYPARENTSTRING

is the typestring of the datatype's parenttype (if it has one), e.g.\
"Foo" for Foobar datatypes.

  \MYCOMMAND

is the command name for the datatype macro, e.g. \fCommand.  In
\INSTANCE, it is "\INSTANCE{<typestring>}".

  \ME

is the same as \MYCOMMAND in datatype macros.  In \INSTANCE, it is
"\INSTANCE{<typestring>}{<contents>}".

  \MYMAP

is the mapping of the datatype.  Almost any use of this inside a
mapping command will cause an infinite loop.



Changing Mappings: A mapping defines the default action of a given
datatype.  When a datatype is first created, it has a null mapping.
When a subtype is created, its mapping defaults to the equivalent its
parent's mapping.

  \MAP{Foobar}{<option>}

sets the mapping of Foobar datatypes to <option>.

  \SURFACEMAP{Foobar}{<option>}

"temporarily" sets the mapping of Foobar datatypes.  The current
mapping is stored, and the mapping is reset to it for Foobar macros
_nested inside_ Foobar macros, i.e. a \SURFACEMAP mapping only applies
to the "first level" (present level) of Foobar macro nesting (this is
useful in preventing infinite recursion).  Repeated uses of
\SURFACEMAP only change the \SURFACEMAP mapping, not the stored
mapping.  \MAP changes both the current mapping and the stored mapping
and "turns off" the effects of \SURFACEMAP.

  \REMAP{<typestring>}

"turns off" the effects of \SURFACEMAP and sets the <typestring>
mapping to the stored mapping.



GameTeX Conventions: The files in Lists/ use a few conventions in
naming datatype macros and fields.  None of the conventions are
required.  Datatype typestrings use uppercase letters for each word
(a.k.a. StudlyCaps), e.g. CombatAbility.  Datatype macro commands do
the same, except they start with an all-lowercase string, probably a
single character, that works as an abbreviation of their type.  For
example, a Char macro command name might be \cDickJones{}.  An Abil
macro command name might be \aSpecialPowers{}.

Field command names tend to start with the string MY, to help avoid
namespace collision with other macros.  For example, \MYname is used
for a macro's name (instead of \name, which is a command already used
by GameTeX).

Some of the -LIST.tex files define one-shot macros, via \newinstance.
Commands like these tend to have all-lowercase command-strings (to
distinguish them from direct datatype macros).  For example, you can
use \itemfold for a one-shot ItemFold macro-equivalent.  Since these
are defined with \newinstance, they are wrappers around \INSTANCE and
behave like normal datatype macros.  \newinstance can also be used to
create datatype macro-equivalent commands that take arguments. For
example:

  \newinstance{Abil}{\avanish[1]}{
    ...
    \s\MYtext{You may vanish into thin air #1 times during the game.}
    ...
  }

Note that, given \ability{...}{...}{...} defined in abils-LIST.tex,
this would effectively be the same as

  \newcommand{\avanish}[1]{%
    \ability{%
      ...%
    }{%
      You may vanish into thin air #1 times during the game.%
    }{%
      ...%
    }%
  }

Also note that commands created by \newinstance (or similar) use only
lowercase letters in their command name, just like the one-shot
macros.

Most of the -LIST.tex files create an example/test macro of their
given datatype, e.g. char-LIST.tex creates \cTest{}, blue-LIST.tex
creates \bTest{}, etc.
