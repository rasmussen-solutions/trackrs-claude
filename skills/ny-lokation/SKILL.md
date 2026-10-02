---
name: ny-lokation
description: Opret en ny lokation i Trackrs (lager, byggeplads, bil) og læg udstyr ind i den i ét forløb. Brug når brugeren siger "ny lokation", "opret lager", "ny bil", "ny byggeplads" eller vil registrere udstyr på et nyt sted. Use when the user wants to create a new Trackrs location (warehouse, site, van) and optionally register or move tools into it.
---

# Ny lokation / New location

Du hjælper med at oprette en lokation i Trackrs og fylde den med udstyr. Tool-navnene nedenfor kan i din klient have et præfiks (fx mcp__trackrs__create_location); brug det de reelt hedder.
You create a Trackrs location and fill it with equipment. Tool names below may carry a client prefix; use whatever they are called in your tool list.

Input: `navn` (lokationens navn), `type` (valgfri: warehouse, site eller van).

## Fremgangsmåde / Steps

1. Kald `list_locations` med query=navn og tjek om der allerede findes en lokation med samme navn. Findes den, så spørg brugeren om de vil bruge den eksisterende i stedet for at oprette en dublet.
2. Sig tydeligt hvad du vil oprette (navn, type) og **vent på brugerens bekræftelse** før du skriver noget.
3. Kald `create_location` med navn og evt. type. Gem det returnerede locationId. Brug ikke `allowDuplicate` medmindre brugeren udtrykkeligt beder om det.
4. Spørg om der er nyt udstyr der skal registreres. Hvis ja: saml listen (navn, evt. mærke, model, serienummer, kategori), vis den og få bekræftelse, og kald derefter `create_tools` med locationId. Alt eller intet; dubletter springes over og meldes tilbage. Max 50 ad gangen.
5. Spørg om eksisterende udstyr skal flyttes hertil. Find det med `find_tool`, vis hvad der flyttes, få bekræftelse, og kald `assign_tools_to_location`.
6. Hvis svaret fra `assign_tools_to_location` viser konflikter (udstyr der allerede er et andet sted), så ingenting er skrevet endnu. Vis konflikterne, spørg brugeren, og kald kun igen med confirmTransfer=true hvis de siger ja. Udstyr der er udlejet, blokerer altid.
7. Afslut med en kort oversigt: hvad blev oprettet, hvad blev sprunget over, hvad blev flyttet.

## Regler / Rules

- Bekræft altid før hvert skrivende kald. Confirm before every write.
- Er brugeren med i flere virksomheder, så bed dem bekræfte hvilken, og send organizationId med på alle kald (se `list_organizations`).
- Gæt aldrig id'er; brug kun id'er returneret af list_locations eller find_tool.
- Skriveværktøjer kræver administratorrettigheder. Får du en afvisning, så sig det og stop.
