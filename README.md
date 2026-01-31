# MyProject - Unreal Engine Game

## Description
An Unreal Engine 5.6 puzzle game featuring door destruction mechanics and zombie enemies.

## Features
- Door destruction puzzle mechanics
- Zombie AI and animations
- Third-person player character
- User profile system with statistics tracking

## User Profile Feature

This project now includes a comprehensive user profile system that tracks player progress and preferences. See [USERPROFILE_DESIGN.md](USERPROFILE_DESIGN.md) for complete implementation details.

### Key Features:
- Player identity customization
- Game statistics tracking (doors destroyed, zombies defeated, playtime)
- Experience points and level progression
- Persistent player preferences
- Auto-save functionality

### Configuration
Default user profile settings can be customized in `Config/DefaultUserProfile.ini`

### Implementation
The user profile system is designed to be implemented using Unreal Engine blueprints. Refer to the design document for detailed blueprint specifications and implementation steps.

## Project Structure
- `Content/` - Game assets, blueprints, and materials
- `Config/` - Configuration files including user profile defaults
- `USERPROFILE_DESIGN.md` - User profile feature specification

## Requirements
- Unreal Engine 5.6
- Windows 64-bit (TextureGraph plugin requirement)