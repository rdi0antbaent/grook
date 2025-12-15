
#  Clockwork Scheduler

**distributed cron with failover capabilities**

![Go](https://img.shields.io/badge/go-1.22-blue)
![PostgreSQL](https://img.shields.io/badge/db-postgres-informational)
![Redis](https://img.shields.io/badge/queue-redis-red)

## docker setup

```bash
docker run -d -p 9000:9000 \
  -e POSTGRES_URL=postgresql://... \
  -e REDIS_URL=redis://... \
  clockwork/scheduler:latest
```

dashboard → [localhost:9000](http://localhost:9000)

## create job

```bash
curl -X POST http://localhost:9000/api/jobs \
  -H "Content-Type: application/json" \
  -d '{
    "name": "cleanup-temp",
    "schedule": "0 */6 * * *",
    "command": "bash /scripts/clean.sh",
    "retries": 2
  }'
```

## architecture

```
┌──────────┐     ┌──────────┐
│ worker 1 │────▶│ worker 2 │────▶ Redis
└──────────┘     └──────────┘
       │                │
       └────▶ PostgreSQL (state)
```

## highlights

* cluster-aware (raft consensus)
* job chaining
* prometheus metrics `/metrics`
* REST API + dashboard

docs → [clockwork.dev/docs](https://clockwork.dev/docs)
community → [discord.gg/clockwork](https://discord.gg/clockwork)

Apache-2.0 © 2025
