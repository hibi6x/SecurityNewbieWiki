# Datadog Fundamentals — Study Notes

## Exam Topics

- Infrastructure Monitoring
- Metrics, Tags & Dashboards
- APM & Distributed Tracing
- Log Management
- Alerts & Monitors
- Integrations
- Datadog Agent

---

## Key Resources

- [Datadog Learning Center](https://learn.datadoghq.com/)
- [Datadog Docs](https://docs.datadoghq.com/)
- [Fundamentals Certification Info](https://www.datadoghq.com/certification/fundamentals/)

---

## Lesson 1 — Infrastructure Monitoring Basics

### What is Infrastructure in Datadog?

Infrastructure refers to the foundational components that support developing, running, and scaling applications. Datadog organises these into four standard categories:

| Category | Description | Examples |
|---|---|---|
| **Hosts** | Physical or virtual machines running your apps | On-prem servers, VMs, cloud instances (AWS, GCP, Azure) |
| **Containers** | Lightweight portable software units with their dependencies | Docker, Kubernetes, Amazon ECS |
| **Processes** | Individual instances of a service or program running on a host or container | Nginx, MySQL, ECS tasks |
| **Serverless** | Cloud computing model that runs code on demand, charging only for execution time | AWS Lambda |

> 📌 The course focuses primarily on **hosts and containers**. Serverless and processes are covered in advanced courses.

---

### Why Monitor Infrastructure?

Proactive monitoring is essential as infrastructure becomes more distributed and ephemeral. Key benefits:

- **Catch issues early** — detect failures, resource bottlenecks, and slow processes before they escalate
- **Deploy with confidence** — integrate monitoring into DevOps workflows to spot issues in new releases
- **Boost visibility** — gain insights into system performance and pinpoint problems faster
- **Optimise performance** — track CPU, memory, and disk metrics to refine resource allocation
- **Make smarter decisions** — use infrastructure data to anticipate needs, scale efficiently, and right-size environments

---

### Challenges of Monitoring Modern Infrastructure

Modern apps rely on distributed microservices spanning various cloud and containerised environments, each using different languages, frameworks, and tools. Services operate across multiple servers, data centres, and providers — with ephemeral instances that spin up and down dynamically.

This means infrastructure data is often **scattered across disparate tools**, forcing engineers to manually correlate information from multiple sources — time-consuming and error-prone.

---

### How Datadog Addresses This

Datadog collects and organises all infrastructure data in a **central location**, including automatically detecting and monitoring ephemeral resources. Infrastructure data can be viewed alongside logs and traces, and used with built-in alerting, dashboards, workflow automation, and incident management.

---

### Getting Started — The Datadog Agent

To begin monitoring infrastructure, the **Datadog Agent** must be deployed to all hosts you wish to monitor. No further configuration is needed after installation.

The Agent collects key metrics including:

- CPU usage
- Memory usage
- Disk usage
- Network bytes sent / received

These measurements are sent to Datadog, aggregated into **metrics data**, and used in monitors, dashboards, SLOs, and error tracking.

**Installation references:**

- [Getting Started with the Agent](https://docs.datadoghq.com/getting_started/agent/)
- [The Agent on a Host](https://docs.datadoghq.com/agent/)
- [The Agent on Docker](https://docs.datadoghq.com/agent/docker/)

---

## Lesson 2 — Infrastructure Tagging

### What Are Tags?

Tags are labels attached to infrastructure components that tell Datadog what each host is, where it runs, and what it does. Hosting platforms like AWS, Google Cloud, Azure, and Kubernetes all support tags natively (sometimes called **labels** in Kubernetes).

There are two types of tags:

- **Automatically generated** — region, machine type/size, instance ID, etc. — created by the cloud provider
- **User-assigned** — custom tags you define manually (e.g. `env:production`, `team:security`, `app:sentinel`)

When Datadog ingests metrics from these sources, it **automatically imports all associated tags** — no extra configuration needed.

---

### How Datadog Uses Tags

Tags are the foundation for grouping, searching, and analysing infrastructure in Datadog. Key behaviours:

- Every piece of observability data (metrics, logs, traces) carries the tags of the host(s) and container(s) it flows through
- Tags enable you to **filter dashboards, monitors, and alerts** by environment, region, team, or any custom dimension
- Data from the same source can be grouped and analysed using consistent tags, even after the underlying resource is gone

---

### Why Tags Matter in Dynamic Environments

In cloud-native environments, resources scale up and down automatically — instances are **ephemeral** and may only exist for minutes. Tags solve a critical problem here:

> Even if a host or container no longer exists, all the data it generated can still be grouped, queried, and analysed — because that data carries the tags of the resource that produced it.

This makes tags essential for troubleshooting incidents involving short-lived infrastructure.

---

### Key Takeaway

Tags are the backbone of Datadog's ability to organise and correlate data across distributed, multi-cloud, ephemeral infrastructure. Using **consistent tagging conventions** across your environment is a best practice from day one.

---

## Lesson 3 — Cloud Network Monitoring (CNM)

### Why Network Monitoring Matters

Even when individual infrastructure components are healthy, issues can exist in the **communication between them**. CNM adds a layer of visibility into network traffic between services, containers, availability zones, and any other Datadog tag.

> 📌 Despite the name, CNM works for **on-premises and hybrid networks** too — it is not limited to cloud-native environments.

---

### Networking Fundamentals — Key Concepts

| Concept | What It Is | Why It Matters for Monitoring |
|---|---|---|
| **IP Address** | Unique numerical label identifying a device on a network | Identifies and tracks devices, hosts, and services; pinpoints source/destination of data flows |
| **Ports** | Virtual endpoints identifying specific processes/services on a device | Confirms which services run on which ports and whether they are available |
| **RTT (Round-Trip Time)** | Time for a signal to travel from sender to receiver and back | Key indicator of network latency; helps diagnose slow responses |
| **Latency** | Time for data to travel from source to destination (ms) | High latency = noticeable lag; low latency = minimal delay |
| **TCP** | Core internet protocol that breaks data into packets and ensures ordered delivery | Reveals connection drops, congestion, and data exchange issues |
| **DNS** | Translates domain names into IP addresses | Tracks domain resolution performance; identifies slow lookups, failed queries, misconfigurations |
| **Retransmits** | When a packet fails to arrive and must be resent (packet loss) | Signals network instability or bottlenecks; high rate = something is wrong |

---

### What CNM Detects and Solves

- **Networking issues** — DNS failures, latency spikes, congestion
- **Unexpected service dependencies** — outages caused by cloud provider regions or third-party tools
- **Packet loss** — hardware issues, misconfigurations, or overloaded routers
- **Cost optimisation** — reduces unnecessary cross-region traffic, identifies idle resources, fine-tunes load balancing
- **Security** — detects unusual traffic patterns and unauthorised communications *(advanced topic)*
- **Service dependency visualisation** — maps data flow between services to reveal dependencies and bottlenecks

---

### Setup

CNM setup and configuration differs depending on host/service technology. See the [Cloud Network Monitoring Setup documentation](https://docs.datadoghq.com/network_monitoring/cloud_network_monitoring/) for step-by-step instructions.

---

## Lesson 4 — Service Level Objectives (SLOs)

### What is an SLO?

A Service Level Objective (SLO) is a measurable performance target that defines acceptable operational goals for a service — setting specific thresholds for metrics like uptime, latency, or error rates. SLOs help businesses track service health over time and ensure both business requirements and user expectations are met.

Defining SLOs is a **cross-team effort** involving product managers, SREs, and stakeholders — everyone must agree on what's being tracked and what success looks like.

> 📌 **Example SLO:** "The web store frontend must be available 99.9% of the time over a 30-day period" — equating to ~43.2 minutes of allowable downtime per month.

---

### The 8 Characteristics of a Good SLO

| Characteristic | What It Means |
|---|---|
| **Attainable** | Realistic and achievable — 100% uptime is not a valid target |
| **Repeatable** | Consistent over time so patterns and trends can emerge |
| **Measurable** | Tied to a concrete metric — if you can't measure it, you can't track it |
| **Understandable** | Simple enough that anyone can immediately grasp what's being measured and why |
| **Meaningful** | Directly impacts service value and user satisfaction |
| **Controllable** | Measures something the team can actually influence — not third-party vendor behaviour |
| **Affordable** | Achievable within budget and team capacity without over-engineering |
| **Mutually Acceptable** | Everyone involved agrees on what's being tracked and what success looks like |

---

### Key Takeaway

SLOs are not just technical targets — they are **shared commitments** between engineering, product, and business. A well-designed SLO keeps teams aligned, focused on what matters to users, and gives a clear signal when action is needed.

---

## Lesson 5 — SLIs, SLAs, Number of Nines & Datadog SLO Types

### Service Level Indicator (SLI)

An SLI is a **quantitative measurement** of a service's performance or reliability. SLIs are the building blocks of SLOs — they provide the actual data used to judge whether an SLO is being met.

| SLI | Description | Example |
|---|---|---|
| **Availability** | % of time a service is online | 99.9% uptime |
| **Latency** | How quickly a service responds | 95% of responses within 200ms |
| **Error Rate** | % of requests that result in failures | 0.01% error rate |
| **Throughput** | Successful transactions processed per second | 500 req/s |

---

### Service Level Agreement (SLA)

An SLA is a **formal agreement between a service provider and a customer** that defines expected performance levels and consequences if those targets are not met. SLAs can be external (customer-facing) or internal (between teams).

> 📌 **Example SLA:** "The provider guarantees 99.9% API uptime over a rolling 30-day period. If uptime drops below this, the customer receives a 10% credit on their monthly fee."

---

### The Relationship Between SLI → SLO → SLA

> **SLI** = the measurement → **SLO** = the internal target → **SLA** = the external commitment

---

### Number of Nines — Availability Reference

| Nines | Availability | Downtime per Year |
|---|---|---|
| 1 Nine | 90% | 36.5 days |
| 2 Nines | 99% | 3.65 days |
| 3 Nines | 99.9% | 8.76 hours |
| 4 Nines | 99.99% | 52.56 minutes |
| 5 Nines | 99.999% | 5.26 minutes |

> 📌 Not all services need the same level. Critical services typically need 4–5 nines. Internal non-critical tools may only need 2 nines. More nines = more complexity and cost.

---

### Datadog SLO Types

| SLO Type | How It Works | Best For |
|---|---|---|
| **Metric Based** | Tracks a single performance metric (uptime, latency, error rate) | Simple, straightforward performance tracking |
| **Monitor Based** | Uses Datadog monitors — supports more complex multi-region or multi-condition tracking | Complex apps deployed across multiple regions or environments |
| **Time Slice** | Evaluates performance in small consistent intervals (e.g. every 5 minutes) rather than averaging over a long period | Catching short-term issues that would be hidden in broader averages |

> 📌 **Key difference:** Metric and Monitor SLOs average performance over time. Time Slice SLOs check each interval independently — much better for detecting brief but impactful outages.

---

## Lesson 6 — Log Management

### Why Logs Matter

Infrastructure and applications produce hundreds to millions of log events every day. Logs are essential for:

- Tracking and investigating performance issues
- Detecting potential and ongoing threats and attacks
- Every piece of infrastructure and every application emits logs

---

### Datadog Log Management — Key Concept: Decoupled Ingestion & Indexing

The core design principle of Datadog Log Management is that **log ingestion and log indexing are decoupled**. This is important for cost efficiency:

| Concept | What It Means |
|---|---|
| **Ingestion** | All logs are collected and brought into Datadog — you can ingest everything |
| **Indexing** | Only the logs you actually need for day-to-day analytics and monitoring are indexed (and paid for at index cost) |

> 📌 This means you don't have to choose between cost and visibility — ingest everything, index only what matters.

---

### Log Explorer — Your Home Base

Log Explorer is where you query and analyse your logs in Datadog. It has two main views:

**Main Log Explorer View**
- Queries and analyses **indexed logs**
- Indexed logs can be used in: **monitors, dashboards, and notebooks**

**Live Tail Tab**
- Queries **near real-time ingested logs** (not just indexed)
- Ingested logs can be used for:
  - **Watchdog** (automated) Insights
  - **Error Tracking**
  - **Generating metrics**
  - **Cloud SIEM detection rules**

> 📌 **Key distinction:** Indexed logs = structured analytics and alerting. Ingested logs (Live Tail) = real-time visibility and SIEM rules — even logs you haven't indexed are available here.

---

### What Does "Indexed" Actually Mean?

**Before indexing — raw ingested log:**

A log arrives at Datadog as a raw text string, exactly as the application produced it:

```
2026-03-22T10:45:03Z ERROR nginx 192.168.1.5 GET /api/orders 500 243ms
```

Datadog receives and stores it. You can see it in Live Tail in real time — but it's just a raw string. Datadog hasn't organised it in a way that lets you search, filter, aggregate, or alert on specific fields.

**After indexing — structured and searchable:**

Datadog parses the raw log and breaks it into structured fields:

```
timestamp:  2026-03-22T10:45:03Z
level:      ERROR
service:    nginx
ip:         192.168.1.5
method:     GET
endpoint:   /api/orders
status:     500
duration:   243ms
```

Now you can search `status:500`, build dashboards showing error rates over time, create monitors that alert on thresholds, and use the data in notebooks for investigation.

**Why it matters for cost:**

| Stage | What Happens | Cost | Use Case |
|---|---|---|---|
| **Ingestion** | Raw log received and stored temporarily | Low | Live Tail, SIEM rules, Watchdog |
| **Indexing** | Log parsed into structured fields, stored long-term | Higher | Search, dashboards, monitors, alerts |

You might ingest 10 million logs/day but only index 1 million — routine low-value logs (e.g. health checks) stay ingested but unindexed, saving cost while remaining visible in Live Tail if needed.

---

## Lesson 7 — Tags In Depth

### What Are Tags?

Tags are labels attached to data in Datadog to add context, enabling you to filter, group, and correlate data across the entire platform. Every datapoint in Datadog has four components:

- **Name / Identifier** — what metric or data it is
- **Value** — the measurement
- **Timestamp** — when it was recorded
- **Tags** — the context labels attached to it

---

### Two Tag Formats

| Format | Examples | Filter? | Group? |
|---|---|---|---|
| **Simple Value** | `staging`, `active`, `demo` | ✅ Yes | ❌ No |
| **Key-Value Pair** | `env:staging`, `region:us-east-1` | ✅ Yes | ✅ Yes |

> 📌 **Key-value pair tags are recommended** — they give you both filtering and grouping, adding a new dimension to your data for every unique key.

**Example:** Filter the Host Map by `env:staging` and `active`, then group by `availability-zone` — you immediately see all staging hosts organised by zone.

---

### Reserved Tags — Important for Correlation

Datadog has reserved tag keys with specific purposes. These should be used consistently and only for their defined use:

| Reserved Tag Key | Used For |
|---|---|
| `host` | Correlation between metrics, traces, processes, and logs |
| `device` | Segregation by device or disk |
| `source` | Span filtering and automated pipeline creation in Log Management |
| `env` | Scoping application data across metrics, traces, and logs |
| `service` | Scoping application data across metrics, traces, and logs |
| `version` | Scoping application data across metrics, traces, and logs |
| `team` | Assign ownership to any resources |

---

### Unified Service Tagging

The three reserved tags `env`, `service`, and `version` used together enable **Unified Service Tagging** — a best practice for both containerised and non-containerised environments.

This allows you to scope and compare data for the same service across different environments and deployment versions — e.g. see how `service:web-store` behaves in `env:production` for `version:2.1` vs `version:2.2`.

> 📌 Unified Service Tagging is especially powerful for tracking the impact of new deployments over time.

---

### Key Takeaway

Tags are the backbone of how Datadog connects and correlates all your data. As Datadog adds new products, **your existing tags automatically work with them** — no extra configuration needed. Investing in a consistent tagging strategy from the start pays dividends across every Datadog feature.

---

## Lesson 8 — Tag Categories & Use Cases

### Overview

How you use tags depends on your use case and the data you're monitoring. There are seven common tag categories in Datadog, each serving a different audience and purpose.

---

### Tag Categories Reference

| Category | Who Uses It | Purpose | Examples |
|---|---|---|---|
| **Native Tags** | Everyone | Auto-inherited from integrations (cloud providers, Kubernetes) — no manual work needed | `region:us-east-1`, `availability-zone:us-east-1a`, `instance-type:m3.xlarge` |
| **Scope Tags** | Engineers, SREs, Managers | Group data by technological organisation — environments, data centres | `env:production`, `env:staging`, `datacenter:eu-west-1` |
| **Function Tags** | Engineers, SREs, End Users | Track hosts and services by role or function | `service:payments`, `role:webserver`, `database:mysql` |
| **Ownership Tags** | Engineers, Managers | Identify who is responsible — power multi-alerts to notify the right team | `team:security`, `owner:john`, `creator:jane` |
| **Business Role Tags** | Managers, Finance | Cost allocation and chargebacks across departments and business units | `business_unit:engineering`, `cost_center:infra` |
| **Customer Tags** | Business Users, Executives | Track customer usage and SLOs | `customer.name:acme`, `customer.region:emea`, `product.name:sentinel` |
| **Monitor Tags** | Engineers | Denote which infrastructure to include in integrations and monitoring setup | `monitor:true`, `datadog:true` |

---

### Key Takeaway

Native tags come for free from integrations. All other categories should be planned deliberately as part of your tagging strategy. A well-thought-out tagging taxonomy means the right people can always find, filter, and alert on the data they own — without manual correlation.

> 💡 **Security angle:** `team:security`, `env:production`, and `service:siem` are immediately useful tags for filtering dashboards and setting up targeted alerts in a security operations context.

---

## Lesson 9 — Tagging Best Practices

### 1. Identify Your Use Cases First

Before assigning any tags, think through what you're monitoring and what questions you'll want to answer:

- **Which environments** will you monitor? → Use `env:xxx` key-value tags for filtering and grouping, and combine with `service` and `version` for Unified Service Tagging
- **Which services and hosts?** → Tag with reserved keys `host` and `service` for cross-product correlation
- **What native tags exist already?** → Check your cloud provider integrations — tags like `availability-zone` and `region` may already be inherited automatically
- **What custom dimensions matter to your org?** → Scope, function, ownership, business unit, customer — plan these deliberately

---

### 2. Consider All End Users of the Data

Tags serve different audiences — plan for all of them:

| End User | What They Need Tags For |
|---|---|
| Engineers / SREs | Filtering dashboards, setting up targeted alerts |
| Managers | Grouping by team, service, or environment |
| Finance | Cost allocation and chargebacks by business unit |
| Business Users | Domain-specific dashboards (e.g. customer usage, SLOs) |

> 💡 Tag keys like `customer`, `security_group`, or `application` can become dropdown **template variables** in dashboards — making them self-service for non-technical stakeholders.

---

### 3. Create an Organisation-Wide Tagging Convention

Inconsistent tags break correlation. A common real-world failure:

```
app, app_name, application, application_name
```

All four appear on the same metric — now you can't search across all applications consistently because the tag key is different everywhere.

**Best practices:**

- Agree on one tag key per concept across all teams (e.g. always use `service`, never `svc` or `app`)
- Create a **central tag registry** or config management doc that defines which keys and values to use
- Enforce conventions early — retrofitting tags across a large environment is painful

---

### Key Takeaway

A little upfront planning on tagging pays massive dividends. Consistent, well-thought-out tags mean:

- Less manual correlation during incidents
- Self-service dashboards for more teams
- Accurate cost allocation
- Monitoring that automatically extends to new Datadog products

> 💡 **Security angle:** In a SOC context, consistent tags like `env:production`, `team:security`, and `service:siem` mean you can instantly scope dashboards and alerts to production security services only — without having to manually filter every time.

---

## Lesson 10 — The Datadog Agent

### What Is the Datadog Agent?

The Datadog Agent is **open source software** that runs on your hosts. It listens for and collects events, logs, and metrics from the host and sends them to Datadog for processing and display. It can be installed across many different environments.

---

### Agent Components

| Component | Responsibility |
|---|---|
| **Collector** | Runs checks and collects metrics |
| **Forwarder** | Sends collected payloads to Datadog |
| **APM Agent** | Collects application performance traces |
| **Process Agent** | Collects live process information |

---

### Agent Integrations

The Agent ships with many built-in integrations beyond system-level checks. It can automatically run checks on popular services such as Nginx, PostgreSQL, and Redis. All Agent checks are configured using **YAML files** installed alongside the Agent.

---

### Agent Configuration Methods

There are three ways to configure the Agent, listed in order of **precedence** (highest to lowest):

| Priority | Method | Notes |
|---|---|---|
| **1 (Highest)** | Remote Configuration | Overrides everything — applied remotely from Datadog |
| **2** | Environment Variables | Overrides `datadog.yaml` |
| **3 (Lowest)** | `datadog.yaml` file | Base configuration file on the host |

> 📌 If the same setting is defined in multiple places, the highest priority method wins.

---

### Remote Configuration

Remote Configuration is a Datadog capability that lets you **remotely configure and change the behaviour of Datadog components** (Agents, tracing libraries, Observability Pipelines Workers) without touching the host directly.

When enabled, the Agent **periodically polls the configured Datadog site** to check for configuration changes and applies them automatically.

Useful for managing Agents at scale across large or distributed environments.

- [Remote Configuration documentation](https://docs.datadoghq.com/agent/remote_configuration/)
- [Agent Configuration Files documentation](https://docs.datadoghq.com/agent/configuration/agent-configuration-files/)

---

### Check Agent Status

Verify the Agent is running and sending data to Datadog:

```bash
datadog-agent status
```

In the output, scroll to the **Forwarder** section and find the **Transaction Successes** block — this confirms the Agent is successfully communicating with Datadog API endpoints:

```
Transaction Successes
=====================
  Total number: 79
  Successes By Endpoint:
    check_run_v1: 33
    intake: 5
    metadata_v1: 8
    series_v2: 33
```

> 📌 **What the endpoints mean:**
> - `check_run_v1` — results from checks (e.g. integrations like Nginx, PostgreSQL)
> - `intake` — general event and metadata intake
> - `metadata_v1` — host metadata (hostname, OS, tags)
> - `series_v2` — time series metrics data

---

## Lesson 12 — Datadog Agent in Containerised Environments

### Agent Capabilities Regardless of Where It Runs

Whether the Agent runs directly on a host or inside a container, it can always monitor the underlying host including:

- CPU usage
- Filesystem operations
- Memory utilisation
- Running processes
- Network activity

When running **inside a container**, it gains the **additional capability** of monitoring the container runtime itself and other coexisting containers.

---

### How the Agent Monitors Docker — Mounted Volumes

In a Docker environment the Agent runs as a container itself. To monitor the host and other containers it needs access to host resources, which is done by **mounting host paths as volumes** inside the Agent container:

| Mounted Volume | Purpose |
|---|---|
| `/var/run/docker.sock` | Access to the Docker daemon — allows monitoring other containers |
| `/proc` | Host process and system information |
| `/sys/fs/cgroups` | Host resource usage (CPU, memory limits per container) |

> 📌 The Agent doesn't need to be installed on the host OS directly; it reads host data through these mounted volumes.

---

### Container Orchestration Compatibility

The Datadog Agent is designed to work across all major container platforms:

| Platform | How Agent Runs |
|---|---|
| **Docker Compose** | As a container with mounted volumes |
| **Amazon ECS** | As a sidecar container |
| **Amazon EKS** | As a DaemonSet |
| **Azure AKS** | As a DaemonSet |
| **Google GKE** | As a DaemonSet |

> 📌 A **DaemonSet** in Kubernetes ensures one Agent pod runs on every node in the cluster automatically.

---

### Key Configuration Points for Containerised Deployments

- Agent is configured via **environment variables** set within its container (not `datadog.yaml`)
- **Service tags** are defined by assigning **labels** to their respective containers
- The Agent collects logs from services that write to their container's **standard output (stdout)**

---

### Agent Checks — System Defaults vs Explicit

| Type | Examples |
|---|---|
| **Auto (no config needed)** | `cpu`, `disk`, `io`, `load`, `memory` |
| **Explicit (must enable)** | `Live Processes`, custom integrations |

> 📌 **Live Processes** must be explicitly enabled — it gives real-time visibility into all processes running on your infrastructure.

---

### Agent Configuration Files — Location & Key File

- **Linux default location:** `/etc/datadog-agent/`
- **Main config file:** `datadog.yaml` — configures every aspect of the Agent, contains hundreds of options with defaults
- All integration checks are configured via **YAML files** in the same directory

Common things configured in `datadog.yaml`:

- Setting an explicit hostname
- Enabling logging
- Enabling Live Processes
- Setting tags applied to all data from the host

---

### Agent Status — Docker Container

Run the Agent `status` command inside a running Docker container:

```bash
docker compose exec dd-agent agent status
```

Key sections to check in the output:

**Logs Agent section** — confirms which containers are being monitored:

```
container_collect_all
---------------------
  - Type: docker
    Service: store-discounts
    Source: discounts
    Status: OK
```

**Forwarder section → Transaction Successes** — confirms data is reaching Datadog:

```
Transaction Successes
=====================
  Total number: 42
  Successes By Endpoint:
    check_run_v1: 15
    intake: 6
    metadata_v1: 6
    series_v2: 15
```

**API Keys status** — confirms the API key is valid:

```
API Keys status
===============
  API key ending with 6be0d: API Key valid
```

---

### Agent Config — Docker Container

List all resolved Agent configuration parameters inside a Docker container:

```bash
docker compose exec dd-agent agent config
```

Verify the environment tag is correctly applied:

```bash
docker compose exec dd-agent agent config | grep ^env
```

Expected output: `env: agent-docker-lab`

---

## Lesson 13 — Datadog Agent Docker Configuration (dd-agent service)

### Docker Image

```
gcr.io/datadoghq/agent:7.56.0
```

You can also use `:latest` tag instead of a pinned version.

---

### Key Environment Variables Reference

| Environment Variable | Purpose | Notes |
|---|---|---|
| `DD_API_KEY` | Authenticates the Agent with Datadog | If not set in the file, pulled from host's `DD_API_KEY` env var |
| `DD_HOSTNAME` | Explicitly sets the `host` tag on all telemetry | Overrides Agent's automatic hostname detection |
| `DD_ENV` | Sets the `env` tag on all telemetry | Useful for filtering data in Datadog by environment |
| `DD_LOGS_ENABLED=true` | Enables log collection | Must be true to collect any logs |
| `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true` | Auto-collects logs from all containers | Logs must be written to container stdout |
| `DD_PROCESS_AGENT_ENABLED=true` | Enables Live Process collection | Shows real-time process visibility |
| `DD_CONTAINER_LABELS_AS_TAGS` | Maps container labels to Datadog tags | e.g. label `my.custom.label.team` becomes tag `team` in Datadog |
| `DD_APM_NON_LOCAL_TRAFFIC=true` | Accepts APM traces from other containers | Required in multi-container environments |
| `DD_SYSTEM_PROBE_NETWORK_ENABLED=true` | Enables Cloud Network Monitoring (CNM) | |
| `DD_SYSTEM_PROBE_SERVICE_MONITORING_ENABLED=true` | Enables Universal Service Monitoring (USM) | |
| `HOST_ROOT=/host/root` | Identifies volume mount path for host filesystem | Required for USM |

---

### Ports

| Port | Purpose |
|---|---|
| `8126` | APM traces |
| `8125` | APM Runtime metrics and DogStatsD metrics |

---

### Volumes

| Volume | Purpose |
|---|---|
| `/var/run/docker.sock` | Docker daemon access — monitors other containers |
| `/proc` | Host process information |
| `/sys/fs/cgroups` | Host resource usage per container |
| `/:/host/root:ro` | Full host filesystem root (read-only) — required for USM |

---

### Elevated Privileges

`cap_add` and `security_opt` give the Agent container heightened privileges for **kernel-level monitoring** — required for USM and CNM.

---

## Lesson 14 — Tagging Containers in Docker

### How the Agent Tags Containers Automatically

The Agent uses simple heuristics to auto-tag containers. For example it uses the **container's name** as the `service` tag. You can override these defaults using two methods:

| Method | When to Use |
|---|---|
| **Environment Variables** | Your own services using APM (traces + logs + metrics) |
| **Docker Labels** | Third-party software or services NOT traced by APM (Redis, PostgreSQL, Nginx, etc.) |

> 📌 Both methods tag logs and metrics. Environment variables are additionally used by APM to identify services.

---

### Method 1 — Environment Variables (APM Services)

```yaml
environment:
  - DD_SERVICE=my-service
  - DD_ENV=production
  - DD_VERSION=1.2.3
```

These will tag all **traces, logs, and metrics** collected from that container.

---

### Method 2 — Docker Labels (Third-Party / Non-APM Services)

```yaml
labels:
  com.datadoghq.ad.logs: '[{"source": "nginx", "service": "web-frontend"}]'
  tags.datadoghq.com/env: "production"
  tags.datadoghq.com/service: "web-frontend"
  tags.datadoghq.com/version: "1.0.0"
```

Labels will be used to tag **logs and metrics** collected on the container.

---

### Custom Label-to-Tag Mapping

The Agent can be configured to convert custom container labels into Datadog tags using `DD_CONTAINER_LABELS_AS_TAGS`:

```yaml
DD_CONTAINER_LABELS_AS_TAGS: '{"my.custom.label.team": "team"}'
```

This means a container with label `my.custom.label.team: security` will automatically get the tag `team:security` in Datadog.

---

### Key Takeaway

- **Own services with APM** → use environment variables (`DD_SERVICE`, `DD_ENV`, `DD_VERSION`)
- **Third-party or non-APM services** → use Docker labels
- Both approaches feed into Unified Service Tagging and are visible across metrics, logs, and traces in Datadog

---

## Lesson 15 — Configuring Services for Datadog in Docker Compose

### APM-Instrumented Services — Use Environment Variables

For services instrumented with the Datadog APM trace library, configure them using environment variables directly in `docker-compose.yml`:

```yaml
environment:
  - DD_AGENT_HOST=dd-agent       # Hostname of the Agent container — APM library uses this to send traces
  - DD_SERVICE=my-service        # Sets the 'service' tag, overrides auto-detected name
  - DD_VERSION=1.2.4             # Sets the 'version' tag — typically matches the image version
```

> 📌 The `env` tag is typically set on the **Agent container** via `DD_ENV` and inherited by all services. Only set `DD_ENV` on a service container if you need to override the Agent's default.

---

### Log Source Configuration — Use Docker Labels

To tell the Agent how to parse and tag logs from a specific service, add a Docker label:

```yaml
labels:
  com.datadoghq.ad.logs: '[{"source": "python"}]'
```

The `source` value maps to a Datadog integration (e.g. `python`, `nginx`, `postgresql`). Setting the correct source:

- Enables automatic log parsing and formatting
- Displays the correct language/technology logo in Log Explorer
- Applies integration-specific processing pipelines

---

### Custom Label-to-Tag Mapping

To convert a custom Docker label into a Datadog tag, configure `DD_CONTAINER_LABELS_AS_TAGS` on the Agent and apply the label to the service container:

**Agent container:**

```yaml
environment:
  - DD_CONTAINER_LABELS_AS_TAGS={"my.custom.label.team": "team"}
```

**Service container:**

```yaml
labels:
  my.custom.label.team: 'payments'
```

Result: the service gets the tag `team:payments` in Datadog.

---

### Restart a Service After Changes

After editing `docker-compose.yml`, apply changes to a specific service without restarting everything:

```bash
docker compose up -d --build <service-name>
```

---

### Key Takeaway — Full Unified Service Tagging in Docker Compose

| Tag | Set Where | How |
|---|---|---|
| `env` | Agent container | `DD_ENV` env var (inherited by all services) |
| `service` | Service container | `DD_SERVICE` env var (APM) or Docker label |
| `version` | Service container | `DD_VERSION` env var (APM) or Docker label |
| Custom tags | Service container | Docker labels mapped via `DD_CONTAINER_LABELS_AS_TAGS` |

---

## Lesson 16 — Autodiscovery

### The Problem It Solves

In a single-host environment you know exactly which containers are running and you can configure monitoring manually. In a **multi-host environment** this breaks down — containers constantly move between hosts, spin up, and shut down. You can't manually keep track of what's running where.

**Autodiscovery** solves this by making the Agent automatically detect which services are running on any container it sees, find the right monitoring configuration for them, and start collecting data — all without any manual intervention.

> 📌 Autodiscovery is **enabled by default** in the Datadog Agent. No extra setup needed to turn it on.

---

### How It Works

1. A container starts anywhere in your infrastructure
2. The Agent detects it and inspects its labels or configuration
3. The Agent checks if any loaded monitoring templates match this container
4. If a match is found, the Agent substitutes any dynamic values (like the container's IP) into the template
5. The Agent starts running the check using that generated configuration
6. When the container stops, the Agent automatically disables the check

This whole process happens automatically every time a container starts or stops.

---

### How to Configure Autodiscovery Templates

You define monitoring configuration directly on the container using **Docker labels**.

**For Agent v7.36+ (single label — recommended):**

```yaml
labels:
  com.datadoghq.ad.checks: '{"<INTEGRATION_NAME>": {"instances": [<INSTANCE_CONFIG>], "logs": [<LOGS_CONFIG>]}}'
```

**For older Agent versions (multiple labels):**

```yaml
labels:
  com.datadoghq.ad.check_names: '[<INTEGRATION_NAME>]'
  com.datadoghq.ad.init_configs: '[<INIT_CONFIG>]'
  com.datadoghq.ad.instances: '[<INSTANCE_CONFIG>]'
  com.datadoghq.ad.logs: '[<LOGS_CONFIG>]'
```

---

### Real Example — Redis Service (Agent v7.36+)

```yaml
labels:
  com.datadoghq.ad.checks: '{"redisdb": {"instances": [{"host": "%%host%%", "port": "6379", "password": "%%env_REDIS_PASSWORD%%"}], "logs": [{"type": "file", "path": "/var/log/redis_6379.log", "source": "redis", "service": "redis_service"}]}}'
```

---

### Template Variables

| Variable | What It Resolves To |
|---|---|
| `%%host%%` | The container's IP address (changes every time the container restarts) |
| `%%env_REDIS_PASSWORD%%` | The value of the `REDIS_PASSWORD` environment variable as seen by the Agent |

> 📌 You don't need to hardcode IP addresses or credentials. The Agent figures them out at runtime.

---

### Built-in Auto-Configuration Templates

Datadog ships with ready-made Autodiscovery templates for common services including **Apache** and **Redis**. For these services, the Agent will attempt monitoring automatically with no labels needed at all.

---

### Key Takeaway

Autodiscovery = the Agent continuously watches for containers starting and stopping, matches them against monitoring templates, and enables or disables checks automatically. This is what makes Datadog practical at scale in dynamic container environments.

---

## Lesson 17 — Agent Troubleshooting: Log Levels & Flare

### Agent Log Levels

Control log verbosity with the `DD_LOG_LEVEL` environment variable:

| Level | What Gets Logged | When to Use |
|---|---|---|
| `OFF` | Nothing | You want complete silence |
| `CRITICAL` | Only critical failures | Minimal noise in stable environments |
| `ERROR` | Critical + errors | Something is broken |
| `WARN` | + warnings | Something might be wrong |
| `INFO` | + general info | Default — good for normal operations |
| `DEBUG` | + detailed diagnostics | Troubleshooting config issues or sending a flare |
| `TRACE` | Everything | Deep low-level debugging |

> 📌 Each level includes all levels above it. So `WARN` also captures `ERROR` and `CRITICAL`.

**Set the log level via environment variable:**

```yaml
environment:
  - DD_LOG_LEVEL='warn'
```

> ⚠️ Always include **quotes** around the value so the Agent parses it correctly.

**Change log level at runtime** (Agent v6.19+ / v7.19+) without restarting:

```bash
agent config set log_level debug
```

> 📌 This does **not** work for the trace agent container — you must redeploy it after changing `DD_LOG_LEVEL`.

> 📌 **Debug mode increases indexed logs** — only enable it for a short troubleshooting window, then turn it off.

---

### Sending a Flare to Datadog Support

A **flare** is a support package — it bundles all the Agent's config files and logs into a single archive and sends it to the Datadog Support team.

**What's included in a flare:**

- All Agent configuration files
- Agent logs
- Tracer debug logs (if APM is enabled)

**What's automatically removed (scrubbed) before sending:**

- Passwords
- API keys
- Proxy credentials
- SNMP community strings

> 📌 The flare **prompts for confirmation before uploading** — you can review it first.

**Send a flare from a Docker container:**

```bash
docker exec -it dd-agent agent flare <CASE_ID>
```

- If you have a support case ID: provide the case ID + associated email
- If you don't have a case ID: provide your Datadog login email to create a new case

---

## Lesson 18 — Metrics: Sources & Collection

### What Are Metrics?

Metrics are numerical measurements of your infrastructure and applications over time — things like CPU usage, error rates, or page load times. Datadog can collect, aggregate, display, and alert on a large number of metrics from many different sources.

---

### Metric Sources Overview

| Source | How It Works |
|---|---|
| **Datadog Agent** | Automatically collects host and container metrics out of the box |
| **Agent-based Integrations** | Core integrations bundled with the Agent (e.g. Nginx, Redis, PostgreSQL) |
| **DogStatsD** | Custom metrics and events sent from your own application code |
| **Datadog Integrations** | 800+ integrations for third-party tools (AWS, Azure, Slack, PagerDuty, etc.) |
| **APM** | Generates metrics from traces — latency, request volume, error rates |
| **Logs** | Generate metrics by counting/aggregating log data |
| **Real User Monitoring (RUM)** | Metrics from user sessions and browser behaviour |
| **Processes** | Metrics from live process data |
| **Events** | Metrics derived from event data |
| **Datadog API** | Send custom metrics directly via HTTP API |

---

### Agent Metrics — What Gets Collected Automatically

**Host-level:** CPU usage, memory usage, disk usage, disk I/O, network traffic

**Container-level:** Container CPU, memory, disk I/O, network metrics

**APM metrics (when APM is enabled):** Service latency, request volume, error rates

**Network metrics (when CNM is enabled):** TCP connections, DNS queries

---

### Three Types of Datadog Integrations

| Type | How It Works | Examples |
|---|---|---|
| **Agent-based** | Installed with the Agent, runs checks locally | Nginx, Redis, PostgreSQL |
| **Authentication (Crawler)** | Datadog makes API calls to the third-party tool using credentials you provide | AWS CloudWatch, Azure Monitor |
| **Library** | Uses Datadog API to monitor apps based on programming language | Node.js, Python APM libraries |

---

### Generating Metrics from Other Datadog Data

| Source | Example Metrics You Can Generate |
|---|---|
| **Logs** | HTTP request count, total API calls, count of 500 errors, % successful responses |
| **APM Spans** | Total requests, average request duration, throughput per service |
| **RUM Events** | Total user sessions, page load time, performance by browser type |

---

### Custom Metrics

When built-in sources don't cover your needs — business KPIs, custom algorithms, user signups — you can send your own custom metrics to Datadog via:

- **Datadog API** — HTTP POST to the Submit Metrics endpoint
- **DogStatsD** — Client library bundled with the Agent, implements the StatsD protocol with Datadog extensions
- **Custom Agent Check** — A custom Python check running inside the Agent
- **Certain integrations** — Some standard and Marketplace integrations support emitting custom metrics

> 📌 Custom metrics count toward your billing — be deliberate about what you define as a custom metric.

---

## Quick Reference — Docker Compose Commands

### Start All Containers

```bash
docker compose up -d
```

> 📌 The `-d` flag runs containers in **detached mode** (background).

### Start a Specific Container Only

```bash
docker compose up -d dd-agent
```

### Restart a Service After Config Changes

```bash
docker compose up -d --build <service-name>
```

> 📌 `--build` forces a rebuild of the image. Without it, Docker Compose may use a cached image and miss your config changes.

### Check Agent Configuration

```bash
datadog-agent config
```

Verify the environment tag:

```bash
datadog-agent config | grep ^env
```

Check the underlying shell environment variable:

```bash
env | grep DD_ENV
```

---

## Questions & Gaps

> Things you're unsure about or need to revisit.
