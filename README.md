# Trackrs for Claude

Trackrs is a tool and equipment tracking platform for trades and construction companies: RFID readers, locations, projects, loans and service schedules. This plugin connects Claude to your Trackrs account and teaches Claude four everyday Trackrs workflows, so you can ask in plain language (Danish or English) where things are, move equipment to a project, or get a weekly report of what is missing.

The plugin contains:

- **The Trackrs connector**, a remote MCP server at `https://trackrs.dk/mcp`. It gives Claude tools to search and read tools, locations, projects, employees and service schemas, and, for administrators, to create, update, move and delete them.
- **Four skills** that tell Claude the sequence, defaults and safety checks for common tasks.

## Skills

| Skill | What it does | Writes to Trackrs? |
| --- | --- | --- |
| `ny-lokation` | Create a new location (warehouse, site or van), then register new equipment in it or move existing equipment there. | Yes, after you confirm each step |
| `udstyr-til-projekt` | Find a project and the equipment you name, show the plan, and assign the equipment to the project. Equipment that is already somewhere else is only moved after you confirm the transfer. | Yes, after you confirm |
| `ugerapport-manglende-vaerktoej` | Weekly report of missing tools with where and when each was last seen. Offers to mark tools as found only after you confirm they are physically back. | Only if you confirm |
| `service-forfalder` | Lists equipment that is due or overdue for service and explains each service schema. | No, read-only |

Every skill asks for your confirmation before any write. Claude never guesses ids and never moves equipment that is already on another project or location without your explicit yes.

## Requirements

- A Trackrs account on the **Pro, Business or Enterprise** plan, or an active trial. Accounts on other plans cannot connect.
- **Administrator rights** in your Trackrs company for the workflows that create, move or update data. Other users can run the read-only parts, such as reports and lookups.
- If you belong to more than one Trackrs company, Claude asks which company you mean.

## Install

### In Claude (claude.ai, desktop, mobile and Cowork)

Open **Customize > Plugins**, find **Trackrs** under **Discover**, and add it. Then open the plugin's **Connectors** tab and connect Trackrs. You sign in with your normal Trackrs login on `idp.rs-systemer.dk`, the Rasmussen Solutions sign-in service, and approve access.

Before the plugin is listed in the directory, you can add this repository as a marketplace instead: **Customize > Plugins > Add > Add marketplace**, and enter `rasmussen-solutions/trackrs-claude`.

### In Claude Code

```
/plugin marketplace add rasmussen-solutions/trackrs-claude
/plugin install trackrs@rasmussen-solutions
```

The first time Claude uses a Trackrs tool, Claude Code opens your browser to sign in. Run `/mcp` to see the connection state. In Claude Code the tools are named with the plugin prefix, for example `mcp__plugin_trackrs_trackrs__find_tool`; the skills work with either form.

## Data and privacy

The plugin itself stores nothing and runs no code on your computer. It contains only Markdown skills, this README, an icon and the connector address.

When you use it, Claude sends the requests you make (search terms, names, ids and the fields of anything you ask it to create or change) to the Trackrs MCP server at `https://trackrs.dk/mcp`, operated by Rasmussen Solutions ApS. Sign-in uses OAuth through `https://idp.rs-systemer.dk`. Trackrs returns the data you have access to, such as tool names, locations, projects, movement history and employee names. No data is sent anywhere else.

Read the [Trackrs privacy policy](https://trackrs.dk/en/privacy.html) and [terms](https://trackrs.dk/en/terms.html).

## Support

Email support@trackrs.dk or use the [Trackrs contact page](https://trackrs.dk/en/contact.html).

## Dansk

Trackrs-pluginet forbinder Claude med din Trackrs-konto, så du kan spørge på dansk, hvor værktøjet er, oprette en ny lokation, sende udstyr til et projekt, få en ugerapport over manglende værktøj eller se hvad der skal til service. Claude beder altid om din bekræftelse, før noget bliver skrevet i Trackrs.

Krav: en Trackrs-konto på Pro, Business eller Enterprise (eller en prøveperiode). Skrivende forløb kræver administratorrettigheder. Pluginet gemmer intet selv; dine forespørgsler sendes kun til Trackrs på `https://trackrs.dk/mcp`. Se [privatlivspolitikken](https://trackrs.dk/da/privacy.html). Spørgsmål: support@trackrs.dk.

## License

MIT. See [LICENSE](LICENSE).
