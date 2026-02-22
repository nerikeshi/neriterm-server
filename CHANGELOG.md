# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.8] - 2026-02-22

### Fixed

- Add pre-flight server startup check before registering with launchctl, preventing KeepAlive restart loops on failure
- Auto-retry with `npm rebuild` if initial startup fails due to native module issues
- Fully stop existing service on reinstall (`launchctl remove` + kill port process)
- Clear logs before service registration to avoid stale error confusion

## [1.0.7] - 2026-02-22

### Fixed

- Install script now runs `npm rebuild` to recompile native modules (node-pty) for the target architecture, fixing startup failures on different CPU architectures

## [1.0.6] - 2026-02-22

### Changed

- Increase directory listing limit from 100 to 1000 to support projects with many subdirectories

## [1.0.5] - 2026-02-22

### Security

- Fix CRITICAL path traversal vulnerability in TranscriptReader (restrict to `~/.claude/*.jsonl`)
- Add request body size limit (1MB) on hook endpoint
- Add transcript file size check (10MB max)
- Add input validation with length limits and control character rejection
- Add rate limiting (100 req/min) on hook endpoint
- Add payload sanitization to strip unknown fields
- Add stale session cleanup to prevent memory leaks
- Remove sensitive data from debug logs to prevent log injection

## [1.0.4] - 2026-02-18

### Added

- Claude Code Hooks-based event detection (replaces PTY output pattern matching)
- Transcript reader to extract last assistant message for push notifications
- `/hook` HTTP endpoint for receiving hook events from Claude Code
- `configure-hooks.sh` script for standalone hooks configuration
- Automatic hooks configuration during server installation

### Changed

- Push notifications now show actual Claude response instead of generic "Task completed"
- Simplified notification categories to `task_stop` and `input_required`
- All hook events support detailed message passthrough from payload

### Removed

- OutputAnalyzer (PTY output pattern matching) - replaced by Hooks
- `error` and `claude_response` notification categories

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
