# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-03-13

### Added
- Initial release of Roblox Update Tracker skill
- Automatic version detection (local vs remote)
- Deep programming analysis with Luau syntax and API change detection
- Smart API keyword matching (866 classes + 556 enums)
- Community feedback analysis from DevForum
- Trend prediction based on historical versions (707-712)
- UGC platform insights and design philosophy analysis
- Chinese localization with accurate technical term translation
- Configurable analysis focus (programming/all/ui/security)
- On-demand API declaration loading (token optimization)
- Multi-page DevForum discussion scraping
- Code example generation for new Luau features
- Standardized report header format with emoji icons

### Technical Details
- Supports Roblox versions 707-712 (as of release)
- Fetches data from official docs and DevForum
- Generates comprehensive Markdown reports
- Saves reports to `workspace/roblox-updates/`
- Tracks latest analyzed version in `LATEST_VERSION.txt`

### Known Limitations
- zh-cn official docs page (SPA) may timeout, uses English docs as primary source
- DevForum pagination limited to first few pages (performance optimization)
- Network retry logic: 2 attempts with 3-second delays

## [Unreleased]

### Planned Features
- Differential analysis between two specific versions
- Customizable report templates
- Export to PDF/HTML formats
- Webhook notifications for new versions
- Integration with Roblox Studio API
