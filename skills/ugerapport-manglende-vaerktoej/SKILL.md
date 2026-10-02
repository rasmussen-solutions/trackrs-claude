---
name: ugerapport-manglende-vaerktoej
description: Lav en ugentlig rapport over manglende værktøj i Trackrs - hvad er savnet, og hvor blev det sidst set. Brug ved "ugerapport", "manglende værktøj", "hvad mangler vi", "savnet udstyr". Use for a weekly missing-tools report in Trackrs with last known movements; offers to mark tools found only after the user confirms.
---

# Ugerapport: manglende værktøj / Weekly missing-tools report

Tool-navnene kan have et præfiks i din klient (fx mcp__trackrs__list_missing_tools); brug det de reelt hedder.
Tool names may carry a client prefix in your client; use whatever they are called.

Input: `dage` (valgfri, standard 7): hvor mange dage tilbage rapporten dækker.

## Fremgangsmåde / Steps

1. Kald `list_missing_tools` (limit 200). Er listen tom, så sig det og stop.
2. For hver tool på listen (start med de ældste, max ca. 15): kald `get_tool_movement_history` med toolId og take=5, og notér hvor og hvornår den sidst blev set.
3. Skriv rapporten på dansk, kort og læsbar:
   - Antal manglende i alt, og hvor mange der er kommet til inden for de seneste `dage` dage.
   - En tabel: værktøj, sidst set hvor, sidst set hvornår, hvor længe den har manglet.
   - Fremhæv det der har manglet længst, og mønstre (samme lokation eller projekt går igen).
4. Hvis historikken tyder på at noget er tilbage på en kendt lokation, så foreslå at markere det som fundet, men gør det kun hvis brugeren bekræfter at udstyret fysisk er til stede.
5. Efter bekræftelse: kald `mark_tools_found` med de bekræftede toolId'er. Det er alt eller intet. Fortæl bagefter hvad der blev markeret.

## Regler / Rules

- Rapporten er skrivebeskyttet. Read-only until the user explicitly confirms step 5.
- Marker aldrig noget som fundet ud fra et gæt; kun efter brugerens eksplicitte bekræftelse.
- `mark_tools_found` kræver administratorrettigheder; ellers vis kun rapporten.
