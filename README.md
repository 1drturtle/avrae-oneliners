Avrae One-Liners
================

A list of commonly used one-liners/code snippets for use in the Draconic Scripting language.

Table of Contents
=================
* [Get Combatants from Targets](#get-combatants-from-targets)
* [Basic Roll String Construction](#basic-roll-string-construction)
* [Get Cvar or Uvar](#get-cvar/uvar-or-svar)


Get Combatants from Targets
---------------------------

This takes any targets made with `-t` and tries to convert them into combatants, and then stores them in a list called `combatants`.

```py
{{c = combat() if combat() else None}}
{{targets = args.get('-t') if args.get('t') else None}}
{{combatants = [c.get_combatant(t) for t in targets] if c else []}}
```

Basic Roll String Construction
------------------------------

This creates a simple roll string based off of a skill bonus (in this case Perception) and provided arguments (`-b`, `adv`/`dis`)

```py
<drac2>
args = argparse(&ARGS&) # Parse our Arguments
skill_mod = character().skills.perception.value # Store our perception mod
roll_str = f"{['1d20', '2d20kh1', '2d20kl1'][args.adv()]}{skill_mod:+}{f'+{b}' if (b := args.join('b', '+')) else ''}" # Construct our roll with adv parsing and bonuses.
</drac2>
```

Get CVAR/UVAR or SVAR
---------------------

This fetches the CVAR or UVAR from a player, or tries to get the SVAR if it cannot find a CVAR/UVAR. It will return `None` if it does not find anything.

Replaceables: 
* `varname` - The variable you wish to store the C/U/SVAR in.
* `CVAR_NAME` - The name of the CVAR/UVAR.
* `SVAR_NAME` - The name of the SVAR.
```py
{{varname = get('CVAR_NAME', get_svar('SVAR_NAME.'))}}
```