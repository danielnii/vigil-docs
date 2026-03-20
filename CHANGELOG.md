# Vigil Backend Changelog

## [1.0.0] - 2026-03-12
### Added
- Initial release of Vigil backend
- JWT authentication with admin/driver roles
- Vehicle CRUD operations
- Shift management with odometer photo uploads
- Revenue discrepancy flagging system
- Fleet analytics with ROI calculations
- PostgreSQL database with full schema

### Changed
- Currency from KES to GHS for Ghana market
- Registration endpoint forces 'driver' role (security)

### Fixed
- Current_odometer column added to vehicles table
- Fleet analytics GROUP BY error in PostgreSQL
- CORS configuration for production deployment

### Security
- Dockerized with hidden database (port 5432 not exposed)
- Environment variables for all secrets
- No admin registration via API
