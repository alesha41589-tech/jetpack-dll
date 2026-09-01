# Jetpack Public Lobby Fix

## Issue
The original Jetpack DLL (v1.0.1) only worked in modded lobbies because it had a hardcoded check that would exit early if the game mode didn't contain "MODDED".

## Root Cause
In `JetpackPlugin.cs` lines 59-73, the `FixedUpdate()` method was checking:
```csharp
bool inModdedRoom =
    NetworkSystem.Instance.InRoom &&
    NetworkSystem.Instance.GameModeString.Contains("MODDED");

if (!inModdedRoom)
{
    if (wasInModdedRoom)
        CleanUp();
    wasInModdedRoom = false;
    return;  // ← Exits early in public lobbies
}
```

This prevented the jetpack functionality from working in any public lobby.

## Solution
Removed the modded-room check and related variables, allowing the jetpack to work in all lobbies (both modded and public).

### Changes Made:
1. Removed the `inModdedRoom` check from `FixedUpdate()`
2. Removed the `wasInModdedRoom` variable (no longer needed)
3. Removed cleanup logic that only triggered in modded rooms

## Comparison with MonkeyWallWalk DLL
- **MonkeyWallWalk** uses HarmonyLib for patching (more robust for anti-cheat)
- **Jetpack** was just applying rotation overrides, which doesn't require Harmony patching

## How to Use
Simply compile the modified source code and replace the old Jetpack_1.dll with the new version.

## Building
```
dotnet build
```

The compiled DLL will be copied to your BepInEx plugins folder automatically (see PostBuild target in Jetpack.csproj).

## Technical Details
- **Language**: C#
- **Framework**: .NET Standard 2.1
- **Dependencies**: BepInEx, Unity Engine
- **License**: As per original Jetpack project
