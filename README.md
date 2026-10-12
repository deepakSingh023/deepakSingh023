<h1 align="center">Deepak Singh</h1>

<p align="center">
  Backend and distributed systems.<br>
  I build things from scratch to find out how they actually work.
</p>

<p align="center">
  <a href="https://linkedin.com/in/deepak-singh-b60107252">LinkedIn</a> ·
  <a href="https://github.com/deepakSingh023?tab=repositories">Repositories</a>
</p>

<br>

> *"What I cannot create, I do not understand."*
> Richard Feynman, left on his blackboard in 1988

```
$ cat ~/.plan

name       Deepak Singh
role       backend / distributed systems
now        core-reactor-kv: a thread-per-core key-value store in Rust
learning   Rust, Linux networking, async I/O
likes      taking a system apart by rebuilding it
languages  Java, C, Rust, TypeScript
reach      linkedin.com/in/deepak-singh-b60107252
```

## What I've built

| Project | What it is | Built with |
|---|---|---|
| [**social-backend**](https://github.com/deepakSingh023/social-backend) | 11 microservices behind a reactive API gateway. Kafka for async work, Redis for idempotency and locks, fan-out-on-write feeds, and chat that routes each message straight to the instance holding the recipient's socket. Traced with OpenTelemetry, metrics in Prometheus and Grafana. | Java 17, Spring Boot, Kafka, Redis, MongoDB |
| [**e-meet**](https://github.com/deepakSingh023/e-meet) | WebRTC video calls with my own signaling server. Screen sharing renegotiates the live connection instead of opening a new one, and signaling nodes scale out over Redis Pub/Sub. | React, TypeScript, Spring Boot, Redis |
| [**core-reactor-kv**](https://github.com/deepakSingh023/core-reactor-kv) | Shared-nothing key-value store. One epoll reactor per core, keys hashed to cores, leader election and replicated writes across a 4-node cluster. | Rust, epoll |
| [**lsm-engine**](https://github.com/deepakSingh023/lsm-engine) | LSM-tree storage engine written from scratch: write-ahead log, skip-list memtable, SSTables with a sparse index, tombstone deletes, and a CLI that simulates a crash. | Java |
| [**tcp-server**](https://github.com/deepakSingh023/tcp-server) | HTTP server in C. Non-blocking epoll accept loop, a worker thread pool, and a completion queue that wakes the loop through an eventfd. Parser and router are hand-written. | C |

## One node of core-reactor-kv

```
 client ── TCP ──▶ epoll reactor
                        │
                 hash(key) % 4
                        │
      ┌─────────┬───────┴───┬─────────┐
      ▼         ▼           ▼         ▼
   core 0    core 1      core 2    core 3
   epoll     epoll       epoll     epoll
      ▲         ▲           ▲         ▲
      └──── eventfd + channel mesh ───┘
                        │
         leader election · replicated writes
                        │
               peers (4-node cluster)
```

No shared state between cores. A core that doesn't own a key hands the request to the one that does.

## Languages

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=deepakSingh023&layout=compact&hide_border=true&langs_count=6&hide=html,css,shell,dockerfile,makefile&theme=github_dark">
    <img alt="Most used languages" src="https://github-readme-stats.vercel.app/api/top-langs/?username=deepakSingh023&layout=compact&hide_border=true&langs_count=6&hide=html,css,shell,dockerfile,makefile">
  </picture>
</p>

## Margin notes

> *"A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable."*
> Leslie Lamport

> *"Simplicity is prerequisite for reliability."*
> Edsger W. Dijkstra

> *"Everything fails all the time."*
> Werner Vogels
