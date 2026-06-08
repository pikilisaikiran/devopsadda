---
title: "Monitoring with Prometheus & Grafana"
date: 2026-02-01
draft: false
weight: 10
summary: "You can't fix what you can't see. Learn observability with metrics, Prometheus, Grafana dashboards, and alerts."
description: "A beginner's guide to monitoring and observability with Prometheus and Grafana."
tags: ["monitoring", "observability", "prometheus", "grafana"]
categories: ["Monitoring"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

You've built, shipped, and scaled your app. But is it healthy *right now*? Is it slow? About to run out of memory? Monitoring answers those questions, and it's what separates a hobby project from a production system.

## The three pillars of observability

| Pillar | Question it answers | Example tool |
|--------|--------------------|--------------|
| **Metrics** | How much / how many? | Prometheus |
| **Logs** | What exactly happened? | Loki, ELK |
| **Traces** | Where did the time go? | Jaeger, Tempo |

We'll focus on **metrics**, the best starting point.

## Meet the dynamic duo

- **Prometheus** collects and stores metrics by *scraping* your apps at intervals.
- **Grafana** turns those metrics into beautiful, readable dashboards.

They're the open-source standard for monitoring, and they pair perfectly.

## How Prometheus works

1. Your app exposes metrics at an endpoint like `/metrics`.
2. Prometheus scrapes that endpoint every few seconds.
3. It stores the data as time series.
4. You query it with **PromQL**.

A metrics endpoint looks like plain text:

```text
http_requests_total{method="GET",status="200"} 1027
process_cpu_seconds_total 4.5
```

## Quick start with Docker Compose

```yaml
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

A minimal `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

```bash
docker compose up -d
```

- Prometheus UI → `http://localhost:9090`
- Grafana → `http://localhost:3000` (default login: `admin` / `admin`)

## A taste of PromQL

```promql
# Total HTTP requests
http_requests_total

# Per-second request rate over the last 5 minutes
rate(http_requests_total[5m])

# Only 500 errors
http_requests_total{status="500"}
```

## Building a Grafana dashboard

1. In Grafana, add Prometheus as a **data source** (`http://prometheus:9090`).
2. Create a dashboard → add a panel.
3. Enter a PromQL query like `rate(http_requests_total[5m])`.
4. Pick a visualization (graph, gauge, stat) and save.

You can also import community dashboards by ID from [grafana.com/dashboards](https://grafana.com/grafana/dashboards/).

## Alerting

Dashboards are great, but you can't stare at them 24/7. Set alerts to notify you (via Slack, email, PagerDuty) when something's wrong:

> "Alert me if error rate is above 5% for 5 minutes."

Good alerts are **actionable** and **rare**. Alert fatigue, too many noisy alerts, is real, so only alert on things that need a human.

## What good monitoring looks like

- Track the **Four Golden Signals**: latency, traffic, errors, and saturation.
- Build dashboards your whole team understands at a glance.
- Alert on symptoms users feel, not every tiny blip.

## Practice challenge 🏋️

1. Spin up Prometheus + Grafana with the Compose file above.
2. Add Prometheus as a Grafana data source.
3. Build a panel showing Prometheus's own request rate.

## You did it! 🎉

That's the full beginner DevOps journey, from "what is DevOps?" all the way to monitoring production systems. You now understand how modern software is built, shipped, run, and watched.

Keep practicing, keep building projects, and revisit the [Roadmap](/roadmap/) whenever you want to go deeper. Welcome to DevOps. 🚀
