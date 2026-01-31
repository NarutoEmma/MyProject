# User Profile Feature - Implementation Guide

## Quick Start for Developers

This guide provides step-by-step instructions for implementing the user profile feature in Unreal Engine.

## Prerequisites
- Unreal Engine 5.6 installed
- MyProject opened in Unreal Editor
- Basic understanding of Unreal Engine blueprints

## Step 1: Create Save Game Class

1. In Content Browser, navigate to `Content/myBlueprint/`
2. Create new folder called `UserProfile`
3. Right-click → Blueprint Class → SaveGame
4. Name it `BP_UserProfileSaveGame`
5. Open it and add these variables:
   - `PlayerName` (String)
   - `PlayerLevel` (Integer)
   - `ExperiencePoints` (Integer)
   - `DoorsDestroyed` (Integer)
   - `ZombiesDefeated` (Integer)
   - `TotalPlayTime` (Float)
   - `GamesPlayed` (Integer)
   - `AudioVolume` (Float)
   - `MouseSensitivity` (Float)
   - `GraphicsQuality` (Integer)
   - `ProfileCreationDate` (DateTime)
   - `LastPlayed` (DateTime)

## Step 2: Create User Profile Manager Blueprint

1. Create new Blueprint Class → Actor
2. Name it `BP_UserProfileManager`
3. Add all the same variables as the SaveGame class
4. Implement these functions:

### CreateNewProfile Function
```
1. Set default values for all variables
2. Set ProfileCreationDate to current DateTime
3. Set LastPlayed to current DateTime
4. Call SaveProfile()
```

### SaveProfile Function
```
1. Create SaveGame Object (BP_UserProfileSaveGame)
2. Copy all variables to the SaveGame object
3. Update LastPlayed to current DateTime
4. Use "Save Game to Slot" node
   - Slot Name: "UserProfile"
   - User Index: 0
```

### LoadProfile Function
```
1. Check if save file exists using "Does Save Game Exist"
2. If exists:
   - Use "Load Game from Slot" node
   - Cast to BP_UserProfileSaveGame
   - Copy all variables from SaveGame object
3. If not exists:
   - Call CreateNewProfile()
```

### UpdateStatistic Function
```
Parameters: 
- StatType (Enum or String)
- Value (Integer or Float)

Switch on StatType:
- "DoorsDestroyed": Add Value to DoorsDestroyed
- "ZombiesDefeated": Add Value to ZombiesDefeated, Add (Value * 25) to ExperiencePoints
- "PlayTime": Add Value to TotalPlayTime
- "GamesPlayed": Add Value to GamesPlayed

After updating, check for level up
```

### CheckLevelUp Function
```
1. Calculate XP needed: PlayerLevel * 100
2. While ExperiencePoints >= XP needed:
   - Increment PlayerLevel
   - Subtract XP needed from ExperiencePoints
   - Call LevelUpEvent (for UI feedback)
   - Recalculate XP needed
```

## Step 3: Integrate with Game Instance

1. Open your Game Instance blueprint (or create one)
2. Add variable: `UserProfileManager` (Reference to BP_UserProfileManager)
3. On Init:
   - Spawn BP_UserProfileManager
   - Store reference in UserProfileManager variable
   - Call LoadProfile()
4. On Shutdown:
   - Call SaveProfile()

## Step 4: Integrate with Door Blueprint

1. Open `Content/myBlueprint/01_doorblueprint/BP_Door` (or similar)
2. Find the event when door is destroyed
3. Add these nodes:
   - Get Game Instance
   - Cast to your Game Instance
   - Get UserProfileManager
   - Call UpdateStatistic("DoorsDestroyed", 1)

## Step 5: Integrate with Zombie Blueprint

1. Open `Content/myBlueprint/BP_zombie`
2. Find the death/defeat event
3. Add these nodes:
   - Get Game Instance
   - Cast to your Game Instance
   - Get UserProfileManager
   - Call UpdateStatistic("ZombiesDefeated", 1)

## Step 6: Create Profile UI Widget

1. Create Widget Blueprint: `WBP_UserProfile`
2. Add UI elements:
   - Text Block: Player Name
   - Progress Bar: XP Progress
   - Text Block: Level Display
   - Vertical Box with statistics:
     - Doors Destroyed
     - Zombies Defeated
     - Play Time
     - Games Played
3. In Graph:
   - Get UserProfileManager reference from Game Instance
   - Bind text/progress bars to profile variables
   - Add buttons for profile customization

## Step 7: Add Profile Menu Access

1. Open your main menu widget
2. Add "Profile" button
3. On button click:
   - Create WBP_UserProfile widget
   - Add to viewport

## Step 8: Add Play Time Tracking

1. Open your main level blueprint
2. In Event Tick:
   - Add Delta Seconds to a running timer
   - Every 60 seconds:
     - Call UpdateStatistic("PlayTime", 1.0) (adds 1 minute)
     - Reset timer

## Step 9: Testing

1. Play the game
2. Check that profile is created on first launch
3. Destroy a door - verify statistic increases
4. Defeat a zombie - verify statistic increases and XP gained
5. Stop and restart the game
6. Verify all statistics persisted

## Common Issues & Solutions

### Save file not persisting
- Check that save path is writable
- Verify "Save Game to Slot" is actually being called
- Check Unreal Engine logs for save errors

### Statistics not updating
- Verify UpdateStatistic function is being called
- Add Print String nodes to debug
- Check Game Instance reference is valid

### Level not increasing
- Verify XP calculation is correct
- Check that CheckLevelUp is called after XP changes
- Print current XP and XP needed values

## Advanced Features

### Multiple Profile Slots
Modify SaveProfile and LoadProfile to accept a slot name parameter:
- "UserProfile_1", "UserProfile_2", etc.

### Cloud Save Integration
Use Unreal's Online Subsystem for cloud saves:
- Implement cloud save/load functions
- Add conflict resolution logic

### Profile Comparison
Create a function to compare two profiles:
- Useful for leaderboards
- Can display "better than X% of players"

## File Locations

After implementation, blueprint files should be in:
- `Content/myBlueprint/UserProfile/BP_UserProfileSaveGame.uasset`
- `Content/myBlueprint/UserProfile/BP_UserProfileManager.uasset`
- `Content/myBlueprint/UserProfile/WBP_UserProfile.uasset`

## Additional Resources

- Unreal Engine Documentation: Save Games
- Unreal Engine Documentation: UMG UI Designer
- USERPROFILE_DESIGN.md - Detailed design specification
- Config/DefaultUserProfile.ini - Default configuration values
