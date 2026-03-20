# Vigil - The Digital Eye

A fleet management system built to catch revenue theft through odometer triangulation. Built with PERN stack (PostgreSQL, Express, React, Node.js).

## 🚀 Quick Links
- **Live App**: [https://vigil247.app](https://vigil247.app)


## 📋 Project Status: Phase 1 Complete ✅
- ✅ MVP deployed to production
- ✅ 20+ beta testers actively using
- ✅ 10-pass Postman test suite
- ✅ Dockerized with Cloudflare tunnel
- ✅ 99.9% uptime (after the iptables incident 😅)

## 🎯 Core Business Logic
- **Odometer Triangulation**: Distance = End - Start
- **Revenue Flagging**: If revenue < distance × 2.5 GHS → ALERT
- **Tamper Detection**: Start odometer must be ≥ last recorded reading

## 📚 Documentation Sections
- [Architecture Decisions](./decisions/)
- [Challenges Overcome](./challenges/)
- [Deployment Guides](./deployments/)
- [Security Hardening](./security/)
- [Monitoring Setup](./monitoring/)

## 🧪 Test Credentials
| Role | Email | Password | Permissions |
|------|-------|----------|-------------|
| Admin | addo@vigil.com | ********* | Full access |
| Demo | demo@vigil.com | vigil123 | Read-only admin |
| Driver | john.driver@vigil.com | vigil123 | Start/end shifts |
