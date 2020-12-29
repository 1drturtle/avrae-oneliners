Avrae One-Liners
================

A list of commonly used one-liners/code snippets for use in the Draconic Scripting language.

Get Combatants from Targets
---------------------------

This takes any targets made with `-t` and tries to convert them into combatants, and then stores them in a list called `combatants`.

```py
{{c = combat() if combat() else None}}
{{targets = args.get('-t') if args.get('t') else None}}
{{combatants = [c.get_combatant(t) for t in targets] if c else []}}
```