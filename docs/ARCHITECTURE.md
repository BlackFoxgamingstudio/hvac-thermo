# Architecture: Sovereign HVAC Thermodynamics

## Overview

**Package ID:** `PKG-004`  
**Domain:** Thermodynamics & Domain-Driven Design  
**Microservice Port:** `8782`  
**n8n Webhook Path:** `hvac-thermo-trigger`  
**GitHub:** [BlackFoxgamingstudio/hvac-thermo](https://github.com/BlackFoxgamingstudio/hvac-thermo)

Thermodynamic simulation engine for HVAC systems. Calculates load profiles, duct sizing, refrigerant cycles, and energy efficiency ratings per ASHRAE standards.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign HVAC Thermodynamics │
                     │       Port: 8782            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  LoadCalculator  | DuctSizer       | RefrigerantC  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `LoadCalculator`
Handles all loadcalculator operations. Exposes async methods callable from the core dispatcher.

### `DuctSizer`
Handles all ductsizer operations. Exposes async methods callable from the core dispatcher.

### `RefrigerantCycleSimulator`
Handles all refrigerantcyclesimulator operations. Exposes async methods callable from the core dispatcher.

### `EnergyRater`
Handles all energyrater operations. Exposes async methods callable from the core dispatcher.

### `ASHRAEValidator`
Handles all ashraevalidator operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-hvac-thermo", "port": 8782}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-hvac-thermo:
  image: sovereign-hvac-thermo:latest
  ports: ["8782:8782"]
  healthcheck:
    test: curl -f http://localhost:8782/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`hvac`, `thermodynamics`, `ashrae`, `simulation`
