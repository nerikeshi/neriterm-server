# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.3] - 2025-01-20

### Added

- Review token support for App Store review with limited project access
- WebSocket-based image upload (faster than HTTP multipart)
- `REVIEW_TOKEN` and `REVIEW_ALLOWED_PROJECT_IDS` environment variables

### Changed

- Increase max WebSocket message size to 15MB for base64 encoded images
- Review clients have read-only access (cannot create/delete/update projects)

## [1.0.2] - 2025-01-17

### Changed

- Remove authentication requirement from `/status` page (localhost only, read-only)
- Remove Close button from status page UI (session close API still requires auth)

### Security

- Status page no longer exposes auth token in URL (prevents browser history/referer leakage)

## [1.0.1] - 2025-01-17

### Security

- Add cols/rows validation in WebSocket resize handler (1-500 range)
- Reject path special characters in project names to prevent path traversal
- Add path validation to cleanupProject method
- Add symlink re-validation in saveFile to mitigate TOCTOU attacks
- Add UUID format validation for sessionId

## [1.0.0] - 2025-01-17

### Added

- Initial release
- WebSocket server for real-time terminal output
- Push notification support via Firebase Cloud Messaging
- Multi-project management
- Secure authentication with token-based auth
- Auto-start via macOS LaunchAgent
- Status page at `/status`
- Health check endpoint at `/health`
- Image upload support
- Session state tracking

### Security

- Rate limiting for authentication attempts
- Path validation for file operations
- Token-based authentication with minimum 32 characters
- Request timeout handling
