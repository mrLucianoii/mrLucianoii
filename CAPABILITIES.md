# Octonions — Technical Overview

*A private AI agent platform, built solo over ~2.5 years. Pre-launch.*

This is the costume-off version of the story: what the system is, how it's built, and how I keep
myself honest about the difference between what's shipped and what's still in progress.

---

## What it is

A private AI agent platform in which a coordinated crew of autonomous agents can plan, delegate
work to one another, and take real actions on a user's behalf — with a human kept in the loop at
every point where an action could do damage. It is a multi-service backend, a cross-platform
client, and a multi-agent orchestration layer, developed and operated by one person.

---

## Architecture at a glance

- **Multi-agent orchestration** — a crew of specialized agents with a coordinator that routes and
  delegates work. Agents genuinely hand tasks to one another rather than running in isolation.
- **Private-agent platform** — a service-oriented backend (dozens of independently deployable
  services) running on Kubernetes, fronted by an API gateway and edge tier.
- **Cross-platform client** — a Flutter application shipped to macOS and iOS.
- **Retrieval-augmented generation** — semantic search over a vector-enabled PostgreSQL store.
- **LLM integration** — provider-abstracted model access with prompt caching for cost efficiency,
  and a pluggable embedding layer.
- **Async job infrastructure** — durable job queues with worker polling, per-user quotas, and
  cost accounting for long-running operations.

**Representative stack:** TypeScript / Fastify, Flutter / Dart, Kubernetes, PostgreSQL + pgvector,
Redis, object storage + CDN, a service mesh for pod-to-pod encryption, and standard CI security
gating.

---

## Measured, not claimed

The differentiator I'd point to first. Rather than describe the system from memory, I built
tooling that reads the entire codebase, maps every module and dependency, and reports the
structure back as ground truth. Most recent snapshot:

| Metric | Value |
|---|---|
| Interconnected modules | 4,500+ |
| Connections between them | 6,500+ |
| Distinct subsystems | ~300 |
| Circular dependencies | 0 |
| Description inferred rather than directly verified | ~1% |

The last row is deliberate: the tooling tracks how much of its own account is guessed versus
read directly, and holds the guessing under one percent. It exists specifically to stop me from
quietly upgrading "planned" to "shipped" — a discipline I care about more than any single feature.

By that same measurement, the two most central abstractions in the entire backend are **identity**
(authentication) and **authorization** (permissions). The architecture is security-first by
measurement, not just by claim — the load-bearing core is access control.

---

## Security & safety posture

- **Human-in-the-loop by design.** Destructive agent actions are blocked pending explicit human
  approval, which can be granted from any device. Agents are autonomous up to the point an action
  becomes consequential, and then a person is always in the middle.
- **Access control as the core.** Layered authorization with an append-only audit trail — every
  permission check and change is recorded with actor, resource, action, and result.
- **Standard hardening.** Token-based authentication with short-lived credentials, encrypted
  pod-to-pod transport via a service mesh, secrets management, and automated secret-scanning in CI.
- **Content and rights safety.** Moderation checks on relevant paths and rights/licensing
  provenance recorded for generated media.

*(Specifics of internal topology, file layout, and the exact enforced-vs-in-progress boundary are
kept private — this overview is intentionally non-exploitable.)*

---

## Operations

Kubernetes orchestration with environment overlays, managed Postgres, edge DNS/WAF/CDN, a service
mesh, unified per-instance log streaming as the validation protocol, and a multi-instance parallel
development workflow that converges through an integration branch with collision detection before
merge.

---

## Honest status

**Shipped and real:** the multi-agent crew and their delegation, the platform they run on, the
measured architecture above, the human-approval safety layer, and an identity-and-permission-first
security posture.

**In progress:** this is a pre-launch system. Some of the more ambitious capabilities are still
being finished, and I'll describe those as in-progress rather than done. If you want to know
exactly what's live versus in flight, ask — you'll get a straight answer.

---

*Built solo. Austin, TX. → octonions.ai*
