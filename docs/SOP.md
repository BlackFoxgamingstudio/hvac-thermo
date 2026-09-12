# Standard Operating Procedure: Sovereign Hvac Thermo

## 1. Service Health Verification
Run `sovereign-hvac-thermo --health` to confirm the engine responds with status `HEALTHY`.

## 2. Webhook Adapter Operation
The webhook adapter listens on port `8782`:
```bash
python3 n8n/webhook_adapter.py
```
If port 8782 is occupied, check active processes:
```bash
lsof -i :8782
```

## 3. n8n Integration Testing
Send a probe POST request:
```bash
curl -X POST http://localhost:8782/api/v1/execute \
  -H "Content-Type: application/json" \
  -d '{"action": "health_ping", "payload": {"test": true}}'
```
