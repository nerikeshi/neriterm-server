# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
