# User Profile Feature - Quick Reference

## Overview
Comprehensive player profile system with statistics, progression, and preferences.

## Key Files
- `USERPROFILE_DESIGN.md` - Full design specification
- `IMPLEMENTATION_GUIDE.md` - Step-by-step implementation
- `Config/DefaultUserProfile.ini` - Default settings

## Profile Data Points

| Field | Type | Description |
|-------|------|-------------|
| PlayerName | String | Player's display name (max 20 chars) |
| PlayerLevel | Integer | Current level (1-50) |
| ExperiencePoints | Integer | Current XP |
| DoorsDestroyed | Integer | Total doors destroyed |
| ZombiesDefeated | Integer | Total zombies defeated |
| TotalPlayTime | Float | Total time played (minutes) |
| GamesPlayed | Integer | Number of game sessions |
| AudioVolume | Float | Master volume (0.0-1.0) |
| MouseSensitivity | Float | Mouse sensitivity setting |
| GraphicsQuality | Integer | Graphics preset (0-3) |

## Experience System
- **Door Destroyed**: +10 XP
- **Zombie Defeated**: +25 XP
- **Level Up**: Every 100 XP
- **Max Level**: 50

## Blueprints to Create

### Core System
1. **BP_UserProfileSaveGame** (SaveGame class)
   - Contains all profile data variables
   
2. **BP_UserProfileManager** (Actor)
   - `CreateNewProfile()`
   - `SaveProfile()`
   - `LoadProfile()`
   - `UpdateStatistic(StatType, Value)`
   - `CheckLevelUp()`

### UI Components
3. **WBP_UserProfile** (Widget)
   - Display player info
   - Show statistics
   - Edit preferences

## Integration Points

### Game Instance
```
On Init:
- Spawn UserProfileManager
- Call LoadProfile()

On Shutdown:
- Call SaveProfile()
```

### Door Blueprint
```
On Destroyed:
- UpdateStatistic("DoorsDestroyed", 1)
```

### Zombie Blueprint
```
On Death:
- UpdateStatistic("ZombiesDefeated", 1)
```

### Level Blueprint
```
On Tick (every 60 seconds):
- UpdateStatistic("PlayTime", 1.0)
```

## Save File Info
- **Location**: `%LOCALAPPDATA%/MyProject/Saved/SaveGames/`
- **Filename**: `UserProfile.sav`
- **Slot Name**: `"UserProfile"`
- **User Index**: `0`

## Testing Checklist
- [ ] Create new profile
- [ ] Save and load profile
- [ ] Destroy door → stat updates
- [ ] Defeat zombie → stat updates + XP gained
- [ ] Accumulate 100 XP → level up
- [ ] Restart game → profile persists
- [ ] Modify preferences → changes persist

## Default Values
```ini
PlayerName = "Player"
PlayerLevel = 1
ExperiencePoints = 0
All Statistics = 0
AudioVolume = 0.7
MouseSensitivity = 0.5
GraphicsQuality = 2 (High)
```

## Quick Tips
- Auto-save every 5 minutes (configurable)
- Always save on game exit
- Cache profile in Game Instance for performance
- Validate player name input
- Handle corrupted save files gracefully
- Add visual feedback for level-ups

## Support
For detailed implementation instructions, see `IMPLEMENTATION_GUIDE.md`
For design rationale and future enhancements, see `USERPROFILE_DESIGN.md`
