# User Profile Feature Design Document

## Overview
This document outlines the design and implementation approach for adding a user profile feature to the MyProject Unreal Engine game.

## Purpose
The user profile feature will allow players to:
- Create and customize their player identity
- Track game statistics and achievements
- Persist player preferences across game sessions
- Display player information in-game

## Core Components

### 1. User Profile Data Structure

The user profile should contain:
- **Player Name**: String (max 20 characters)
- **Player Level**: Integer (starts at 1)
- **Experience Points**: Integer
- **Game Statistics**:
  - Doors Destroyed: Integer
  - Zombies Defeated: Integer
  - Play Time: Float (in minutes)
  - Games Played: Integer
- **Preferences**:
  - Audio Volume: Float (0.0 - 1.0)
  - Mouse Sensitivity: Float
  - Graphics Quality: Enum (Low, Medium, High, Ultra)
- **Profile Creation Date**: DateTime
- **Last Played**: DateTime

### 2. Blueprint Implementation (To be created in Unreal Editor)

#### BP_UserProfile (Actor Blueprint)
This blueprint should handle all user profile operations:

**Variables:**
- PlayerName (String)
- PlayerLevel (Integer)
- ExperiencePoints (Integer)
- DoorsDestroyed (Integer)
- ZombiesDefeated (Integer)
- TotalPlayTime (Float)
- GamesPlayed (Integer)
- AudioVolume (Float)
- MouseSensitivity (Float)
- GraphicsQuality (Integer)

**Functions:**
- `CreateNewProfile()` - Initialize a new user profile with default values
- `LoadProfile()` - Load profile data from save file
- `SaveProfile()` - Save profile data to disk
- `UpdateStatistic(StatType, Value)` - Update a specific game statistic
- `LevelUp()` - Increase player level when XP threshold is reached
- `GetProfileSummary()` - Return formatted string with profile info

#### BP_ProfileUI (Widget Blueprint)
User interface for displaying and editing profile:

**UI Elements:**
- Text box for player name
- Level and XP progress bar
- Statistics panel showing game stats
- Settings panel for preferences
- Save/Load buttons

#### BP_GameInstance_Extended
Extend the game instance to:
- Hold the current user profile data throughout the game session
- Auto-save profile data on game exit
- Load profile data on game start

### 3. Save System Integration

**File Location:** 
- Windows: `%LOCALAPPDATA%/MyProject/Saved/SaveGames/`
- Use Unreal's SaveGame system

**Save File Structure:**
- File name: `UserProfile.sav`
- Use Unreal's `USaveGame` class for serialization

### 4. Game Integration Points

**Statistics Tracking:**
- Increment `DoorsDestroyed` when BP_Door is destroyed
- Increment `ZombiesDefeated` when BP_zombie is defeated
- Track play time using level blueprint's Tick event
- Update `GamesPlayed` on level load

**Experience System:**
- Award XP for completing objectives:
  - 10 XP per door destroyed
  - 25 XP per zombie defeated
- Level up every 100 XP
- Max level: 50

## Implementation Steps

### Phase 1: Core Profile System
1. Create `BP_UserProfile` blueprint with all variables
2. Implement save/load functions using SaveGame system
3. Create save game blueprint class `BP_UserProfileSaveGame`
4. Test profile persistence across game sessions

### Phase 2: UI Development
1. Create `BP_ProfileUI` widget blueprint
2. Design profile display screen
3. Add profile customization options
4. Implement UI event handlers

### Phase 3: Game Integration
1. Modify `BP_Door` to trigger statistic updates
2. Modify `BP_zombie` to trigger statistic updates
3. Add XP and level-up logic
4. Create in-game profile display (HUD element)

### Phase 4: Testing & Polish
1. Test profile creation and loading
2. Verify statistics are tracked correctly
3. Test level progression system
4. Add visual feedback for level-ups and achievements

## Technical Considerations

### Data Validation
- Validate player name (no special characters, length limits)
- Clamp preference values to valid ranges
- Handle corrupted save file scenarios

### Performance
- Save profile asynchronously to avoid frame drops
- Cache profile data in Game Instance for quick access
- Avoid frequent disk writes (save on exit or every 5 minutes)

### Future Enhancements
- Multiple profile slots
- Cloud save support
- Profile comparison/leaderboards
- Achievement system integration
- Profile badges and customization

## Configuration

A default configuration file template is provided in `Config/DefaultUserProfile.ini`

## Testing Checklist

- [ ] Profile creation works correctly
- [ ] Profile saves and loads successfully
- [ ] Statistics update when in-game actions occur
- [ ] Experience points accumulate correctly
- [ ] Level-up triggers at correct XP thresholds
- [ ] Preferences persist across sessions
- [ ] UI displays all information correctly
- [ ] Multiple play sessions maintain data integrity
- [ ] Corrupted save file handling works properly

## Notes for Developers

- This is an Unreal Engine blueprint-based implementation
- All blueprints should be created in `Content/myBlueprint/UserProfile/` folder
- Follow existing blueprint naming conventions (BP_ prefix)
- Ensure thread-safe operations for save/load functions
- Add appropriate error handling and user feedback
