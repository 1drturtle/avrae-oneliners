Avrae One-Liners
================

A list of commonly used one-liners/code snippets for use in the Draconic Scripting language.

Table of Contents
=================
* [Get Combatants from Targets](#get-combatants-from-targets)
* [Basic Roll String Construction](#basic-roll-string-construction)
* [Load SVAR and overwrite from CVAR/UVAR](#load-svar-and-overwrite-from-cvaruvar)
* [Search Through a List of Dictionaries](#search-through-a-list-of-dictionaries)
* [Get a Subclass for a certain Class](#get-a-subclass-for-a-certain-class)
* [Change Die Size based on Level](#change-die-size-based-on-level)
* [Simple Text Paginiation](#simple-text-paginiation)
* [Detect if the User asks for Help](#detect-if-the-user-asks-for-help)
* [Optional Arguments](#optional-arguments)

Contributing
===========

If you'd like to contribute, open an issue with the one-liner you'd like to add, what it does, and some example use cases for it!

One Liners
==========

Get Combatants from Targets
---------------------------

This takes any targets made with `-t` and tries to convert them into combatants, and then stores them in a list called `combatants`.

```py
{{c = combat()}}
{{targets = args.get('t') if args.get('t') else []}}
{{combatants = [c.get_combatant(t) for t in targets if c.get_combatant(t)] if c else []}}
```

Basic Roll String Construction
------------------------------

*originally from Infinidoge#1337, modified*

This creates a simple roll string based off of a skill bonus (in this case Perception) and provided arguments (`-b`, `adv`/`dis`)

```py
<drac2>
args = argparse(&ARGS&) # Parse our Arguments
skill_mod = character().skills.perception.value # Store our perception mod
roll_str = f"{['1d20', '2d20kh1', '2d20kl1'][args.adv()]}{skill_mod:+}{f'+{b}' if (b := args.join('b', '+')) else ''}" # Construct our roll with adv parsing and bonuses.
</drac2>
```

Load SVAR and overwrite from CVAR/UVAR
---------------------

This loads an SVAR as a dictionary from JSON and then tries to update the JSON using a CVAR/UVAR. Used for server settings with local overrides.

Replaceables: 
* `varname` - The variable you wish to store the C/U/SVAR in.
* `CVAR_NAME` - The name of the CVAR/UVAR.
* `SVAR_NAME` - The name of the SVAR.
```py
{{varname = load_json(get_svar('SVAR_NAME', '{}')).update(load_json(get('CVAR_NAME', '{}')))}}
```

Search Through a List of Dictionaries
-------------------------------------

This will search through a list of dictionaries to compare a value in the dictionary to a certain string.
To make it an exact search, replace `in` with `==`. To make it case-sensitive, remove the two `.lower()`

Replaceables:
* `search_var` - The variable that we are searching for
* `search_key` - The key in each dictionary we are looking at.
* `search` - The list we are looking through.
* `out` - The resulting intersection (List of dictionaries that match the search.)

```py
{{out = [x for x in search if search_var.lower() in x["search_key"].lower()]}}
```

Get a Subclass for a certain Class
----------------------------------

*from TheReverendB#1377*

This will get the player's subclass for a certain class. Will only work if said player has setup their character with the `!level` alias. If the player does not have a subclass, it will be `""`, an empty string.

Replaceables:
* `sClass` - The variable to store the subclass in.
* `XLevel` - The Class you want to use. `ClericLevel`, `BardLevel`, etc.
```py
{{sClass=load_json(get("subclass", "{}")).get("XLevel", "")}}
```

Change Die Size based on Level
------------------------------

*from TheReverendB#1377*

This will create a dice string where the die string changes based on the character's level. The example shown below is for Monk's Martial Arts die.

Replaceables:
* `dice` - The variable to store the dice string in.

```py
{{L=level}}
{{dice = f"1d{4+2*((L>4)+(L>9)+(L>15))}"}}
```

Simple Text Paginiation
-----------------------

*from Croebh#5603*

This takes a string and splits it into a bunch of smaller strings so that it doesn't break fields.

Replaceables:
* `textList` - Variable to store the list of strings in.
* `1000` - The maximum length for each string.
* `text` - The string that is split up.
```py
{{textList = [text[i:i+1000] for i in range(0, len(text), 1000)]}}
```

Detect if the User asks for Help
--------------------------------

This checks if the user has asked for help by specifiying `help` or `?` as an argument, or by specifying no arguments.

Replaceables:
* `args` - Variable that has a list of arguments (usually called via `&ARGS&`)
* `help` - Variable to store the value of if the user needs help or not.

```py
help = args[0].lower() in '?help' if args else True
```

Optional Arguments
------------------

This allows you to get arguments that can be optional. To add more arguments, put more variables on the left side, and increase the amount of "N/A", as well as change the 3 to however many arguments you have.

Replaceables:
* `"N/A"` - What the variable should be if the user doesn't specify one. Possible option: `None`
* `arg1` - Variable for first argument
* `arg2` - Variable for second argument
* `arg3` - Variable for third argument

```py
{{arg1, arg2, arg3 = (&ARGS& + ["N/A", "N/A", "N/A"])[:3]}}
```
