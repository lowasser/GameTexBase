This file describes the Identity datatype and associated features.
Identities can be used to give things (extra) charsheets, badges,
statcards, skilllists, playerlist entries, and box labels.  An
identity acts as a "filter" or "lens" for a character for giving extra
of these to the character.

Identities have a special field called \MYmods which is a list of
modifications to apply to a Char macro.  Usually these are uses of the
\rs command.  For a set of mods, \badgefalse, \statsfalse,
\skillsfalse, \sheetfalse, \listfalse, and \labelfalse will all be set
by default, so the resulting identity will not produce any of those.
You explicitly set an identity to produce elements by using the
corresponding \...true commands (see Lists/char-LIST.tex).  For
example:

  \NEW{Identity}{\idDisguise}{
    \s\MYname	{Disguise}
    \s\MYmods	{\rs\MYname	{}
		\rs\MYdesc	{obviously disguised person}
		\rs\MYnumber	{????}
		\badgetrue
		}
  }

can be given to a character (in \MYids) to give them a new namebadge
with a blank name and a new description and number.  Any other
character information (such as the player name) will be unchanged.

An identity by itself, or owned by something transferable, will
ultimately apply its mods to a blank temporary character.  For any
resulting produced elements, the only distinguishing information will
come from the mods.  You will essentially be giving character-based
elements to non-characters.  Thus

  \NEW{Item}{\iMask}{
    \s\MYname	{nice mask}
    \s\MYnumber	{1234}
    \s\MYtext	{This mask works as an obvious disguise.  People may
		recognize you (the player), but it conceals your badge
		number and description.  Wear the included name badge.}
    \s\MYids	{\idDisguise{}}
  }

will be a mask item card with an associated namebadge.  The badge will
have a blank player name.  (If, instead, you want the badge to be
directly owned by the character that starts with the mask, make the
mask a suite with \suite; however, the badge will no longer be
transferable between characters.)



Identity Instances: Identities can be used as dynamic instances.
\identity{<mods>} is a one-shot macro for an identity with the given
mods.  \newid{<command>}{<mods>} creates <command>[<mods>]{<arg>} as
an identity instance.  The optional <mods> arg to <command> can be
used to change the mods on a per-use basis.  For example:

  \newid{\spiderman}{%
    \rs\MYname	{Spider-Man}
    \rs\MYnumber{????}
  }

  \NEW{PC}{\cPeterParker}{
    \s\MYname	{Peter Parker}
    \s\MYdesc	{college student}
    ...
    \s\MYids	{\spiderman[\badgetrue]{}%
		\identity{\rs\MYdesc{Daily Bugle photographer}%
			  \badgetrue}%
		}
    ...
  }

creates a Spider-Man identity, gives it to Peter Parker as a
namebadge, and gives him a namebadge for when he is using his press
pass.

A handful of standardized, pre-built instances are provided for
alternate names.  \nickname[<mods>]{<name>} creates
\nick[<mods>]{<arg>} for nicknames, \realname creates \real for secret
identities (when the normal name is just a cover), and \maidenname
creates \maiden, for a pre-marriage name.  These instances are defined
locally to the character.

  \NEW{PC}{\cTommy}{
    \s\MYname	{Thomas Barker}
    \nickname	{Tommy Barker}
    ...
  }

will give Thomas Barker a "Tommy" nickname.  Like other identity
instances, <mods> can be used on a per-use basis.

  \NEW{PC}{\cEmperor}{
    \s\MYname	{Emperor \pre Palpatine}
    \realname	{Darth \pre Sidious}
    ...
    \s\MYids	{\real[\badgetrue]{}}
  }

will give the Emperor his Sidious identity and a namebadge for when he
wants to ditch his cover.



Using Identities in Text: Identities can be used both as filters for
characters and directly in game text.  To use an identity as a filter,
nest it inside a character:

  \cTommy{\nick{\informal}}

will give the informal form of Thomas' nickname, i.e. "Tommy."  Note
that use of the pre-built identity instances (like \nick and \real)
are incorporated into the namemappings system at a highly-automated
level (see Extras/README-namemappings).

An identity used by itself will imitate a character.  \spiderman{},
used directly in text, will produce the name "Spider-Man"; if used in
\name{\spiderman{}}, it will give a sheet with that for a name.
\idHyde{\informal} will give the informal form of Hyde's name.



Extra Namebadges and Box Labels: You can give a character (or any
other owner or subowner) an extra namebadge or box label with
degenerate uses of identities.

\extrabadge{<name>}{<number>}{<desc>} gives a new namebadge with the
given information.  If you leave an argument blank, that part of the
badge will be blank.  If you use \same in an argument, that part will
be unchanged from the originating character.  So

  \NEW{PC}{\cJanitor}{
    \s\MYname	{Bob Smith}
    \s\MYdesc	{man in a janitor's uniform}
    \s\MYnumber	{1234}
    ...
    \s\MYids	{\extrabadge{\same}{\same}{man in plain clothing}}
    ...
  }

will give the janitor a namebadge he can wear when he's out of
uniform.  The name and badge number will not change.

\boxlabel{<label>} will give an extra box label.  Therefore

  \NEW{Place}{\pPostman}{
    \s\MYname	{Postman mechanic}
    ...
    \s\MYids	{\boxlabel{Postman}}
    ...
  }

Will generate a "Postman" label for The Box for the Postman mechanic.



Using Identies for Character Sheets: When you use an identity to give
someone an extra character sheet, in addition to the mods, it will
also set the lists of owned elements to be the same as the identity.
This means the lists generated at the end of the new sheet will
reflect what the identity owns, not the original character.
\listcharstuff sets an identity to not change the lists.  For example:

  \NEW{Identity}{\idHyde}{
    \s\MYname	{Hyde}
    \s\MYmods	{\rs\MYname	{Mr.\null\pre Hyde}
		\rs\MYfile	{hyde.tex}
		\rs\MYdesc	{deranged madman}
		\badgetrue
		\sheettrue
		}
    \s\MYabils	{\aBigAndStrong{}%
		\aClimbing{}%
		\aHardToKill{}%
		}
    \s\MYgreens	{\gClimbing{}}
    \listcharstuff
  }

  \NEW{PC}{\cJekyl}{
    \s\MYname	{Dr.\null\pre Jekyl}
    \s\MYfile	{jekyl.tex}
    ...
    \s\MYids	{\idHyde{}}
    ...
  }

will give Dr. Jekyl a Mr. Hyde charsheet, a Mr. Hyde badge, a set of
abilities, and a greensheet.  The abilities and greensheet will be
subowned by the Hyde identity, but the Hyde charsheet will list all of
Jekyl's abilities, sheets, items, etc (those directly owned, not
subowned).  If you remove \listcharstuff from \idHyde{}, the Hyde
charsheet will list only the abilities and greensheet owned by Hyde.
If you make \idHyde{} a suite and use \listcharstuff, Hyde's elements
will show up on Jekyl's charsheet and Hyde's charsheet.



Extra Statcards and SkillLists: Stats and Skills, produced as entries
on statcards and skilllists, are just special type of elements.  When
you use an identity to give someone an extra statcard or skilllist, it
will set the stats and skills to be just the identity's stats and
skills (similar to the lists of owned elements at the end of the
character sheets, as above).  For example:

  \NEW{Identity}{\idWerewolf}{
    \s\MYname	{Werewolf Form}
    \s\MYstats	{\stat{Theta}{$\theta$}{2}}
    \s\MYskills	{\aMaul{}
    		\aRegenerate{}
		}
    \s\MYmods	{\rs\MYdesc	{Hairy Werewolf}
		\statstrue
		\skillstrue
		\badgetrue
    		}
  }

will, when given to a character, give them a second statcard and
skilllist that only list the identity's skills and stats (Theta 2,
Maul, and Regenerate).  These cards will not list any of the
character's normal stats.  These stats and skills will also appear on
the character's normal statcard and skilllist (via the normal
ownership rules), so the new ones may not be useful.

\listcharstuff sets an identity to not automatically mod these fields;
adding it to \idWerewolf{} would make the second statcard and
skilllist no different from the primary ones.

To have an identity create a separate statcard/skilllist with a set of
stats/skills not shared with the normal ones, use \notowner:

  \NEW{Identity}{\idWerewolf}{
    \s\MYname	{Werewolf Form}
    \s\MYstats	{\stat{Theta}{$\theta$}{2}}
    \s\MYskills	{\aMaul{}
    		\aRegenerate{}
		}
    \s\MYmods	{\rs\MYdesc	{Hairy Werewolf}
		\statstrue
		\skillstrue
		\badgetrue
    		}
    \notowner
  }

Now, \idWerewolf{}'s ownership (of skills at stats) will not be
processed, and thus won't show up on the normal statcard or skilllist.
The statcard and skilllist created by \idWerewolf{} will still include
them.
