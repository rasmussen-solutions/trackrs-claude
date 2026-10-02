---
name: udstyr-til-projekt
description: Tildel udstyr til et projekt i Trackrs - find projektet og værktøjerne, vis hvad der flyttes, og håndter værktøj der allerede er på et andet projekt eller en anden lokation. Brug ved "udstyr til projekt", "send værktøj til byggeplads", "tildel til projekt". Use to assign tools to a Trackrs project with a confirmed transfer flow.
---

# Udstyr til projekt / Equipment to project

Tool-navnene kan have et præfiks i din klient (fx mcp__trackrs__find_tool); brug det de reelt hedder.
Tool names may carry a client prefix; use whatever they are called.

Input: `projekt` (navn eller del af navn), `udstyr` (hvad der skal bruges, fx "2 boremaskiner og stillads").

## Fremgangsmåde / Steps

1. Kald `find_projects` med projekt som query og status Active. Er der flere bud, så lad brugeren vælge. Er der ingen, så spørg efter et andet navn. Gæt aldrig.
2. For hvert stykke udstyr i `udstyr`: kald `find_tool`. Er der flere bud, så lad brugeren vælge præcis hvilke. Notér toolId'erne.
3. Vis en klar plan: projektets navn og den præcise liste af værktøj, inklusive hvor hvert stykke står nu. **Vent på bekræftelse.**
4. Kald `assign_tools_to_project` med projectId og toolIds (max 50) og confirmTransfer=false.
5. Viser svaret konflikter (udstyr der allerede er på et andet projekt eller en anden lokation), er der ikke skrevet noget. Vis konflikterne, spørg brugeren, og kald først igen med confirmTransfer=true hvis de udtrykkeligt siger ja. Udlejet udstyr blokerer altid hele kaldet; fjern det fra listen og spørg igen.
6. Bekræft resultatet kort: hvad blev tildelt, og hvad blev flyttet fra hvor.

## Regler / Rules

- Bekræft før skrivning, og sæt aldrig confirmTransfer=true uden brugerens ja til netop de konflikter. Never set confirmTransfer=true on your own.
- Er brugeren med i flere virksomheder, så bed dem bekræfte hvilken, og send organizationId med på alle kald (se `list_organizations`).
- Skrivning kræver administratorrettigheder. Får du en afvisning, så sig det og stop.
