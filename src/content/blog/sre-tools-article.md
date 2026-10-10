---
title: "The Tools the Enterprise Actually Lets Me Run: My SRE Stack"
description: "The honest list of tools I open every day as an SRE. It starts in the cluster and ends with the paperwork."
pubDate: 2026-10-10
image: "/blog/sre-tools-cover.png"
---

![Editorial illustration of an SRE desk at night with monitors showing dashboards and code, server racks, clouds, and a glowing pager](/blog/sre-tools-cover.png)

People ask what tools I use as an SRE. The better question is which tools survive enterprise security review. Most of the shiny stuff never makes it past procurement. Here is what I actually open every day. It starts in the cluster and ends with the paperwork.

**1. GitHub Copilot**
Microsoft runs the enterprise world, so GitHub Copilot is the LLM tool most companies actually allow. I use it inside VS Code, right where I am already working. I do not use it for anything fancy. I use it for writing Grafana dashboard queries, alert queries, and the PromQL I would otherwise type from memory. I also use it for creating custom shell and Python scripts. It saves me real time every day.

**2. k9s**
When I need to move fast inside a cluster, I do not reach for a console first. k9s is the terminal UI where I check pods, logs, and events without typing kubectl sixty times.

**3. kubecolor and k8s extensions**
On my Mac terminal I run kubecolor so kubectl output is actually readable, and on VS Code I have the k8s extensions for YAML completion and cluster views. Small things, but I stare at this output all day, so it matters. One personal habit: I like dark mode, so it is on system-wide and in every app where it is available.

**4. ArgoCD console**
GitOps means cluster state comes from git, and the ArgoCD console is where I watch it happen. Sync status, drift, rollbacks, all in one place.

**5. Rancher console**
The control plane view across multiple clusters. When I need to see the whole fleet instead of one cluster, this is where I go.

**6. Opsgenie**
Alerts have to go somewhere with an on-call rotation attached. Opsgenie is ours. It pages me, I acknowledge, I fix, I go back to sleep.

**7. Grafana dashboards**
I have been working on Grafana dashboards a lot lately. They are the single pane of glass. If it is not on a dashboard, it does not exist. I wrote about the messy side of migrating them here: [Behind a Grafana Dashboard Migration: What JSON Can't Do](/blog/grafana-dashboard-migration).

**8. Splunk**
Log aggregation and analysis. When something breaks at 2am, Splunk is where the container logs are searched.

**9. Grafana Tempo**
Traces. When a request is slow, dashboards tell me it is slow. Tempo tells me where.

**10. GCP console**
We run on GCP, so this is where the infrastructure lives: networking, IAM, the occasional billing surprise. The console is for the stuff that is faster to click than to script.

**11. Postman**
For hitting APIs directly: simulating webhook payloads, validating alert receivers, and poking at endpoints the dashboards do not cover.

**12. AppDynamics**
While built as a full APM suite, in our environment we use it heavily for host-level infrastructure monitoring. The machine agents run on the hosts, and I have alerts configured for various key system metrics on them.

**13. Microsoft Copilot (and Excel)**
For the unglamorous half of the job: drafting presentations, writing documents for internal teams, and SOPs for the offshore team. And yes, an Excel sheet for tracking multiple initiatives. Nobody brags about Excel, but that is where my week actually lives.

None of this is exotic. That is the point. Enterprise SRE is not about the newest tool. It is about the tools that are approved, reliable, and there at 2am.
