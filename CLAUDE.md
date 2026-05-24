# HA Rebuild — Project Configuration

## Home Assistant Server
- **IP & Port**: 10.0.0.74:8123
- **Web UI**: http://10.0.0.74:8123
- **Nabu Casa Cloud**: https://jteqyvfsrfc1yhtafwv7npsvjwdyugrs.ui.nabu.casa

## Hardware Configuration
- **Zigbee Coordinator**: Nabu Casa ZBT-2 (Ember/EZSP firmware)
  - Serial: DCB4D910E890
  - Status: Reflashed with latest firmware, plugged in and ready
  
- **Z-Wave Dongle**: Nabu Casa ZWA-2
  - Serial: 80B54EE5DDE4

## Project Status
- **Current Phase**: Phase 0 (ha-mcp Setup)
- **HA Installation**: Fresh reinstall complete
- **Hardware**: ZBT-2 reflashed and reconnected

## Git Workflow
- **Default Branch**: main
- **Branch Strategy**: Feature branches per phase
  - Format: phase-X/description (e.g., phase-0/ha-mcp-setup, phase-1/zigbee-setup)
  - Review and merge process: All phase branches must be reviewed before merging to main
- **Commit Messages**: Clear, descriptive messages with context

## MCP Server
- **ha-mcp**: Home Assistant AI integration
  - Source: https://github.com/homeassistant-ai/ha-mcp
  - Installation: uvx --python 3.13 --refresh ha-mcp@latest
  - Environment: HOMEASSISTANT_URL and HOMEASSISTANT_TOKEN configured

## Key Documentation
Existing migration docs preserved as baseline reference:
- MIGRATION_GUIDE_START_HERE.md — High-level overview
- Phase-specific execution guides (PHASE2, PHASE3)
- Device-specific migration notes (bedroom, dining room)
- Z2M configuration and testing guides
