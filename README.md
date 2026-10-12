<h1 align="center">Deepak Singh</h1>

<p align="center">
  Backend and distributed systems.<br>
  I rebuild things from scratch until I stop being surprised by them.
</p>

<p align="center">
  <a href="https://linkedin.com/in/deepak-singh-b60107252">linkedin</a> ·
  <a href="https://github.com/deepakSingh023?tab=repositories">repos</a>
</p>

```console
deepak@home:~$ whoami
deepak singh, backend / distributed systems

deepak@home:~$ cat /proc/now
building   core-reactor-kv, a thread-per-core KV store in Rust
reading    epoll(7), man pages, other people's storage engines
learning   Rust, Linux networking, async I/O

deepak@home:~$ ls ~/langs
java  c  rust  typescript
```

## About

I like the layer below the framework. Using Kafka is fine, but I would rather know why a partition log is fast, so I end up writing a smaller, worse version of it and learning what the real one is protecting me from.

That habit produced an LSM engine, an HTTP server in C, a WebRTC signaling server, and now a key-value store that gives every CPU core its own event loop. I'm at my best when the problem is "why does this fall over at 3 a.m." rather than "which button does this".

## Currently

**core-reactor-kv** is the main thing. One epoll reactor per core, no shared state, keys hashed to the core that owns them. A core that gets a key it doesn't own forwards the request over an eventfd-backed channel mesh. On top of that sit leader election and replicated writes across a 4-node cluster. This is also where I'm learning Rust properly, so expect the commit history to get less embarrassing over time.

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

## Things I've built

**[social-backend](https://github.com/deepakSingh023/social-backend)** `java 17 · spring boot · kafka · redis · mongodb`
A social platform split into 11 microservices behind a reactive gateway. Feeds are fanned out on write. Redis handles idempotency keys and distributed locks. Chat messages go straight to the instance that holds the recipient's WebSocket instead of being broadcast. Everything is traced with OpenTelemetry and graphed in Prometheus and Grafana, because I wanted to see the failures, not guess at them.

**[e-meet](https://github.com/deepakSingh023/e-meet)** `react · typescript · spring boot · redis`
WebRTC video calls on my own signaling server. Screen sharing renegotiates the existing peer connection rather than opening a second one, and signaling nodes scale horizontally over Redis Pub/Sub.

**[core-reactor-kv](https://github.com/deepakSingh023/core-reactor-kv)** `rust · epoll`
A shared-nothing key-value store: one epoll reactor per core, keys hashed to their owning core, and leader election plus replicated writes across a 4-node cluster. Still in progress, details in the section above.

**[tcp-server](https://github.com/deepakSingh023/tcp-server)** `c`
An HTTP server with a non-blocking epoll accept loop and a worker thread pool. Finished work comes back through a completion queue that wakes the loop via eventfd. The parser and router are hand-written, so every malformed request was my own bug to find.

**[lsm-engine](https://github.com/deepakSingh023/lsm-engine)** `java`
A storage engine from the ground up: write-ahead log, skip-list memtable, SSTables with a sparse index, tombstone deletes. The CLI can simulate a crash so I could watch recovery actually work.

## Language mix

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=deepakSingh023&layout=compact&hide_border=true&langs_count=6&hide=html,css,shell,dockerfile,makefile&theme=github_dark">
    <img alt="Most used languages" src="https://github-readme-stats.vercel.app/api/top-langs/?username=deepakSingh023&layout=compact&hide_border=true&langs_count=6&hide=html,css,shell,dockerfile,makefile">
  </picture>
</p>

## Get in touch

If you're working on storage engines, networking, or anything where the failure modes are the interesting part, I'd like to hear about it. Find me on [LinkedIn](https://linkedin.com/in/deepak-singh-b60107252).

<sub>"What I cannot create, I do not understand." Feynman, left on his blackboard in 1988.</sub>
