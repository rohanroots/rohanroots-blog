---
title: "Behind a Grafana Dashboard Migration: What JSON Can't Do"
description: "I had roughly 140 dashboards to move from Grafana v11 to v12. Exporting and importing the JSON was the quick part."
pubDate: 2026-09-28
image: "/blog/grafana-cover.png"
---

![Editorial illustration of dashboard panels moving between monitoring systems, with cables, security controls, certificates, and validation work around the transfer](/blog/grafana-cover.png)

| **~140** dashboards | **3** tracker views | **v11 → v12** Grafana versions |
| --- | --- | --- |

**On paper, the job sounded simple: export the JSON, import it, and move on to the next dashboard.**

That description left out most of the work.

I was moving roughly 140 dashboards from a Grafana OSS v11 instance to a Grafana Enterprise v12 instance. They were spread across folders owned by different applications, and copying every dashboard was never the goal. I had to find out which ones were still used, get confirmation from the application teams, keep non-production content out of production, and move only the dashboards that should remain.

Once I started, most of the job happened around the JSON. Every dashboard came with dependencies, an owner who had to make a decision, and a chance of landing in the wrong environment.

## The first inventory changed the plan

Before moving a dashboard, I compared the data sources it referenced with the data sources available in the target instance. Clicking through the Grafana screens one by one would have taken forever, so I opened the browser developer tools, docked the console beside each Grafana instance, and ran small JavaScript calls against the Grafana HTTP API from my authenticated browser session.

I pulled the data-source metadata from Grafana v11 and v12, kept comparable fields such as name, type, and unique identifier, and diffed the two lists. The administration team had already said the data sources were migrated. The comparison showed otherwise, which was useful to learn before the first dashboard import.

Some dashboard data sources were missing from the new instance. Others were still under review because nobody had confirmed whether they were active. Waiting for every dependency would have held up dashboards that were ready to move, and the deadline did not leave much room for that.

**The first useful rule:** Do not build a throwaway dashboard just to see whether a data source connects. Run Save & test when you have access. When you do not, assign that check to the administration team and track the handoff.

## I moved the least dependent dashboards first

I split the 140 dashboards into smaller batches based on dependency risk. The first batch came from folders where the required data sources already existed and the application owners had confirmed that the dashboards were still needed.

Starting there kept the project moving while the missing dependencies were sorted out. It also gave me a clean path to validate the target with lower-risk dashboards. Anything blocked stayed marked as blocked instead of disappearing into one large import and becoming tomorrow's mystery.

I still did not consider the target fully ready. This was a controlled pilot that let me make progress under the deadline without hiding the unfinished work.

## The jump from Grafana v11 to v12 changed the operator workflow

The source was Grafana OSS v11 and the target was Grafana Enterprise v12, so I was also crossing a major-version boundary. Even when an imported dashboard rendered correctly, the interface changed where I found actions and how I moved between viewing, editing, and validation. I kept reaching for controls where they used to be, which added time to every manual check.

Grafana v12 uses the Scenes-based dashboard architecture introduced in Grafana v11 and brings a newer dashboard experience. The enabled feature toggles can expose a dashboard outline, tabs, conditional rendering, an auto-grid layout, and a context-aware editing pane. Those changes affected how I worked in the target instance, but they did not tell me whether an imported dashboard was healthy.

Crossing versions also made the JSON checks more important. Grafana v12 can use a newer dashboard schema when Dynamic Dashboards and the new dashboards API are enabled. That feature was still documented as experimental, and saving a dashboard under it could convert the model in a way that affected schema-specific automation. Before troubleshooting any production export or import failure, I would record the exact 11.x and 12.x patch versions, feature toggles, dashboard schema version, and plugin versions.

**The human migration mattered too:** Leave time to relearn navigation and editing in Grafana v12. An import can succeed while the operating workflow still needs to be checked.

## A data source can exist and still be unusable

One of the more irritating discoveries was that a data source could appear in Grafana and still be unusable. Some were listed in the new instance, yet their panel queries failed. The username and password were there, but the connection also depended on certificate-related settings. Without the required trust or client-certificate configuration, authentication could not complete, so the dashboards stayed broken.

I changed the readiness status after that. Seeing a data-source name in Grafana was not enough. I wanted a successful Save & test result in the target instance, then a representative dashboard query. Depending on the backend, a failure can come from the endpoint, credentials, Transport Layer Security (TLS) trust, client certificates, network reachability, headers, or plugin configuration.

**The better status model:** Use these states: missing, present but untested, passed Save & test, validated through a representative panel query, or blocked with an owner and next action.

## My access boundary became part of the migration design

My access also changed between the two instances. In v11, I could open a data source and run Save & test. In v12, that action was unavailable to me. I could work with dashboards and capture failed panel queries, but I could not run Grafana's built-in connection test or inspect the target data-source configuration myself.

Save & test checks whether Grafana can reach and authenticate to the configured backend, so there was no reason to create a temporary dashboard just for connectivity. I still needed a representative dashboard query afterward because that checks the dashboard path as well.

The restriction was reasonable, although it meant every target-side configuration issue depended on the administration team. I would handle that by agreeing on the validation handoff before a migration starts. The handoff should name the person who runs Save & test, the result or error to capture, the owner of the correction, and the point when the data source is ready for panel-level validation.

## The failures needed to stay specific

Two failures were still unresolved. One dashboard rendered normally, but its export returned an undefined result. Grafana rejected another dashboard during import.

I left both issues open rather than guessing at a cause. To narrow them down, I still needed the exact error text, the browser console and network response, the exported JSON, the source and target Grafana versions, the panel plugins involved, and the destination folder and data-source mappings.

For the import failure, I would also check dashboard identifiers and version fields, folder identifiers, permissions, plugin compatibility, and data-source names or unique identifiers. If the user interface keeps giving an unhelpful result, I would try the supported API to see whether the failure comes from the interface or the dashboard model.

**Do not turn a hypothesis into a postmortem:** Keep the issue open until the evidence shows which layer failed.

## The hard part was deciding what deserved to move

The request was clear about one thing: an existing dashboard did not automatically deserve a place in the new instance.

Each application team had to confirm which dashboards were still useful, and I had to keep non-production dashboards out of the production folder structure. Much of that work happened in conversations with owners. Valid JSON did not make a dashboard in scope, and a successful import could still put the wrong thing in the wrong place.

The review also exposed dashboards that nobody needed anymore. Copying them into the enterprise instance would have carried over stale ownership and the false comfort of dashboards that nobody maintained. I used the migration to leave that clutter behind.

## The workbook became the control plane

The import itself often took less time than recording the decision around it. I kept that work in one Excel workbook with three worksheets:

1. **Summary:** A compact roll-up with the total dashboard count and separate counts for reviewed, migrated, skipped or not migrated, and blocked. It showed the overall position without making anyone read every dashboard row.
2. **Dashboard drill-down:** One row per dashboard, with columns for the dashboard name, source folder or application, environment, application-team decision, required data source, migration status, and blocker reference. I filtered this sheet by folder, environment, or status when I chose the next batch.
3. **Blocker details:** One row per blocker, linked to the affected dashboard or dashboards. It captured the failure stage, exact error, evidence collected, previous attempts, current owner, next step, and resolution status. That kept the troubleshooting history out of free-form chat and stopped us from repeating the same checks.

Each sheet served a different reader. The summary showed overall progress, the drill-down showed the state of a specific dashboard, and the blocker sheet held the history and next action for anything stuck.

Leadership could use the summary without parsing technical errors. I used the drill-down to build the next migration batch, while administrators could work from the blocker sheet without reconstructing an issue from messages or screenshots.

By then, the workbook was doing real operational work. It showed me what had been skipped, where troubleshooting was being repeated, and which failures still had no owner.

## Copilot helped, but it did not own the workbook

I used Microsoft Copilot to structure parts of the workbook and handle some formatting. It saved setup time when it worked. After several revisions, though, it sometimes lost the thread and asked me to upload the latest workbook again. Nothing improves a migration afternoon like being asked for the file you just supplied.

It was useful for getting an initial structure onto the page, but I still checked every revision, kept the current file safe, and verified that formulas and status labels stayed consistent. A migration tracker that looks finished but contains the wrong status is worse than an unfinished one.

I ended up using Copilot for small, contained tasks such as proposing columns, normalizing status values, generating a formula, or summarizing a blocker list. The workbook remained the source of truth, and I saved a new version before any broad rewrite.

## What I would change on the next migration

If I ran this migration again, I would put the following readiness gate in place before the first batch:

- **Inventory first:** List the dashboards, folders, owners, environments, data sources, plugins, and alert dependencies before migrating.
- **Freeze the version baseline:** Record the exact 11.x and 12.x patch releases, enabled feature toggles, dashboard schema version, and plugin versions.
- **Map old to new:** Record the source and target data-source names or unique identifiers instead of relying on similar display names.
- **Test, do not count:** Run Save & test in the target data-source settings, then validate a representative panel query. If the migration owner lacks access, assign the first check to the administrator.
- **Pilot by risk:** Start with a small folder whose owners are known and whose dependencies are complete.
- **Capture exact failures:** Keep the response text, console evidence, JSON, and ownership in the blocker log.
- **Validate beyond the import:** Check variables, time ranges, links, transformations, panel queries, permissions, and environment boundaries.
- **Automate after the pattern is stable:** Use manual JSON export and import while discovering the pattern. For a repeatable migration at this scale, the HTTP API, provisioning, Terraform, or a Git-based workflow can reduce clicks and make changes reviewable.

Manual JSON export and import worked well for the first pass because I could inspect each move and isolate failures. Once I understood the exceptions, I would shift toward a repeatable method rather than keep clicking through the same steps.

## Dashboards were only phase one

Alert migration comes next, and I will handle it separately from the dashboard batch. A broken dashboard is usually visible when someone opens it. A bad alert migration can stay quiet, send duplicate notifications, or leave a production service uncovered.

Before touching the alerts, I need a separate inventory of alert rules, data-source dependencies, contact points, notification policies, mute timings, ownership, and expected firing behavior. I also need a controlled overlap or cutover plan so the old rules stay active until the new path has been proven.

For the alert phase, I will spend less time asking whether the configuration moved and more time proving that the new path behaves correctly. The old rules will stay in place until those checks are complete and the owning teams are comfortable with the cutover.
