---
name: service-forfalder
description: Vis hvilket udstyr i Trackrs der skal til service snart eller er overskredet, og forklar hvad servicen indebærer. Brug ved "service forfalder", "hvad skal til service", "eftersyn", "overskredet service". Use to list Trackrs tools due or overdue for service and explain the service schema behind them.
---

# Service der forfalder / Service due

Tool-navnene kan have et præfiks i din klient (fx mcp__trackrs__list_tools_due_for_service); brug det de reelt hedder.
Tool names may carry a client prefix; use whatever they are called.

Input: `dage` (valgfri, standard 30): hvor mange dage frem der kigges. 0 giver kun det der allerede er overskredet.

## Fremgangsmåde / Steps

1. Kald `list_tools_due_for_service` med withinDays=`dage` og limit=200.
2. Er listen tom, så sig det, og nævn at kun udstyr med et serviceskema har en servicedato.
3. Opsummer på dansk, mest presserende først, med overskredet adskilt fra kommende:
   - Værktøj, servicedato (hvor mange dage over eller til), hvor det befinder sig.
   - Gruppér gerne efter lokation, så det er let at samle udstyret.
4. Listen viser skemaernes navne, ikke deres id'er. Kald `list_service_schemas` én gang og match navn til id. For hvert serviceskema der optræder (højst 5 forskellige): kald `get_service_schema` med serviceSchemaId og forklar kort hvad servicen går ud på og hvor ofte.
5. Tilbyd næste skridt, fx at samle det udstyr der er overskredet, men udfør ikke ændringer. Dette forløb er skrivebeskyttet.

## Regler / Rules

- Read-only. Du skriver intet i Trackrs i dette forløb.
- Gæt ikke servicedatoer; brug kun det værktøjerne returnerer.
