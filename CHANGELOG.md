# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-01-29

### Added

- Plugin manifest with metadata and hook configuration
- 6 slash commands: `/spec`, `/tasks`, `/next-task`, `/new-task`, `/task-status`, `/replan`
- 7 skills: spec-generator, task-atomizer, complexity-scorer, dependency-grapher, task-from-spec, overlap-detector, quality-gate
- 3 agents: spec-writer, tech-analyzer, task-planner
- Spec templates: spec-lite (medium complexity) and spec-full (high complexity)
- JSON schemas for state.json, metadata.json, and index.json validation
- SessionStart hook for active task resume detection
- Session resume shell script with jq and fallback support
- Project-level configuration support via `.claude/task-master.config.json`
- README with installation, usage, and architecture documentation
