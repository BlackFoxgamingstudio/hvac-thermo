# Changelog — Sovereign HVAC Thermodynamics

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign HVAC Thermodynamics (PKG-004)
- Core microservice on port `8782`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: LoadCalculator, DuctSizer, RefrigerantCycleSimulator, EnergyRater, ASHRAEValidator
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-hvac-thermo --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Thermodynamics & Domain-Driven Design
Thermodynamic simulation engine for HVAC systems. Calculates load profiles, duct sizing, refrigerant cycles, and energy efficiency ratings per ASHRAE standards.

[Unreleased]: https://github.com/BlackFoxgamingstudio/hvac-thermo/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/hvac-thermo/releases/tag/v1.0.0
