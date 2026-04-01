---
name: ZionHub Backend — contexto do projeto real
description: Stack, arquitetura, vulnerabilidades e estado do ZionHub para referência nas sessões
type: project
---

ZionHub backend está clonado em `projetos/zionhub-backend/`. Visão geral completa em `projetos/zionhub_overview.md`.

**Stack:** FastAPI + SQLAlchemy 2.0 async + PostgreSQL 15 + Alembic + Docker + nginx

**Arquitetura:** multi-tenant SaaS, padrão endpoint → service → crud → model, 60+ endpoints, 14 modelos, WebSockets, IoT (Tuya + Bridge), scheduler, email, YouTube API.

**Estado de segurança:** 15 vulnerabilidades documentadas no próprio `SECURITY_AUDIT.md` do repo. 3 críticas — a mais grave: `toggle_all_devices` sem filtro de org_id (IDOR cross-tenant).

**Por que esperar para estudar:** o aluno precisa das Fases 1-3 do roadmap para ter vocabulário suficiente para avaliar o código com intenção, não depender de IA para entender o que está errado.

**Fase 4 do roadmap** = code review real do ZionHub, endpoint por endpoint.
