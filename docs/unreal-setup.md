# Unreal C++ setup and first run

Task: [Set up the Unreal C++ project and run it for the first time](https://trello.com/c/NPOiyTjY).

## Project baseline

- Engine: Epic Games Launcher build **Unreal Engine 5.8.4**, changelist **58902052** (`UE_5.8/Engine/Build/Build.version`). The `.uproject` association is `5.8`; use patch 5.8.4 to reproduce this setup.
- Platform: Windows x64; build configuration: **Development Editor**.
- Project: `GTAHamburg.uproject`, placed at the repository root alongside the existing documentation.
- Source: the installed engine's `Templates/TP_ThirdPerson` C++ template. Only the basic Third Person scene and classes are included. Combat, Platforming, and Side Scrolling variants are omitted.
- Shared assets: `Templates/TemplateResources/High/{Characters,LevelPrototyping,Input}/Content`, copied into matching project content folders. These are Epic template assets, not original GTA Hamburg artwork. Preserve Epic's source copyright notices and follow the applicable Unreal Engine terms when distributing the project.
- Rendering baseline: desktop, DirectX 12 / SM6, screen-space reflections, no Lumen, no mesh distance field generation, and conventional shadow maps. These choices reduce the initial rendering workload; they are not evidence that the eventual game meets its 1080p/60 FPS target.

## Windows development tools

Use Visual Studio 2022 **17.14** with:

- Desktop development with C++ and Game development with C++.
- MSVC v143 **14.44**, patch **14.44.35211 or newer** within the 14.44 family.
- Windows SDK **10.0.22621.0**.
- Visual Studio Tools for Unreal Engine and the Blueprint debugger tools.

Import [`Config/VisualStudio.vsconfig`](../Config/VisualStudio.vsconfig) into the Visual Studio Installer to select these components. Update Visual Studio 2022 before importing if the 17.14 compiler is unavailable. Save work and close Visual Studio before installing; a Windows administrator prompt may appear. Restart Windows if the installer requests it.

UnrealBuildTool generates its own root `.vsconfig` with broader recommendations when generating solution files. That generated file is ignored; the maintained installation selection lives under `Config/` so regeneration cannot overwrite it.

The installed engine's `Engine/Config/Windows/Windows_SDK.json` is the version-specific source for compiler requirements: it prefers MSVC 14.44 (or a supported 14.50 compiler), rejects 14.39 through 14.43, and rejects early 14.44 patches through 35210. Do not assume that any compiler labeled “v143” is suitable.

The initial machine inspection found Visual Studio 17.13.0 without its MSVC tools and only Windows SDK 10.0.17763.0. These are insufficient for this project. Unreal provides its own .NET runtime for UnrealBuildTool; do not substitute an arbitrary system .NET installation.

References: [Epic's Visual Studio setup guide](https://dev.epicgames.com/documentation/en-us/unreal-engine/setting-up-visual-studio-development-environment-for-cplusplus-projects-in-unreal-engine), [Microsoft's Unreal tools installation guide](https://learn.microsoft.com/en-us/visualstudio/gamedev/unreal/get-started/vs-tools-unreal-install).

## Build and open

Run these commands in PowerShell **from the repository root**. Adjust the engine path if needed.

```powershell
$engineRoot = 'C:\Program Files\Epic Games\UE_5.8'
$projectFile = (Resolve-Path '.\GTAHamburg.uproject').Path

# Optional: generate the Visual Studio solution (never commit generated files).
& "$engineRoot\Engine\Build\BatchFiles\Build.bat" -projectfiles "-project=$projectFile" -game -rocket -progress

# Close Unreal Editor before a full C++ build, especially after header changes.
& "$engineRoot\Engine\Build\BatchFiles\Build.bat" GTAHamburgEditor Win64 Development "-Project=$projectFile" -WaitMutex
if ($LASTEXITCODE -ne 0) { throw 'Editor build failed; inspect the build output.' }

& "$engineRoot\Engine\Binaries\Win64\UnrealEditor.exe" $projectFile
```

Alternatively, open the generated solution, choose **Development Editor / Win64**, build the **GTAHamburg** project, then open `GTAHamburg.uproject`. The editor target builds the game module that Unreal Editor loads; the separate game target is used for standalone game builds. This task does not produce a packaged release.

Open `GTAHamburg.sln` in Visual Studio 2022. Project generation warns that Visual Studio 2022 cannot load the engine's .NET 10 C# projects and generates a separate `Automation_GTAHamburg` solution for Visual Studio 2026. That automation solution is not needed for this C++ prototype. The verified command-line editor build uses the engine's bundled .NET 10 SDK.

The editor should open `/Game/ThirdPerson/Lvl_ThirdPerson`. Allow initial shader compilation and asset preparation to finish. If opening the `.uproject` through Explorer fails because its engine association is missing, use the explicit editor command above.

## First-run acceptance check

1. Confirm the editor build exits successfully and the template level opens without missing classes or assets.
2. Press **Play**, then click inside the viewport to capture input.
3. Use **W/A/S/D** to walk, move the **mouse** to orbit the camera, and press **Space** to jump. Check movement in more than one direction.
4. Press **Esc** to stop. **Shift+F1** releases the mouse during Play In Editor.
5. Close Unreal Editor, reopen the same `.uproject`, and repeat the movement and camera check.
6. Check the Output Log for missing asset/class errors. Record build, first run, and reopening separately; a successful build alone does not prove that the playable scene works.

## Files to keep and regenerate

| Keep in version control | Purpose |
| --- | --- |
| `GTAHamburg.uproject` | Engine association, game module, editor plugin |
| `Source/` | C++ classes and UnrealBuildTool C# build rules |
| `Config/Default*.ini` | Shared maps, rendering, input, and project defaults |
| `Content/` | Maps, Blueprints, input actions, meshes, materials, and animations |
| `Content/__ExternalActors__/`, `Content/__ExternalObjects__/` | Source assets belonging to the template map; these are **not** caches |
| `Config/VisualStudio.vsconfig`, `.gitignore`, documentation | Development setup and workflow |

`Binaries/`, `Intermediate/`, `DerivedDataCache/`, `Saved/`, `.vs/`, and generated IDE projects are ignored. They can be regenerated with the engine and toolchain installed. Local editor logs live in `Saved/Logs/`. Keep future authored `Build/` resources such as application icons; do not blanket-ignore that folder. Assets are binary: use Unreal Editor to modify them, and review their purpose before staging them yourself.

## Learn the template through its code

- [`GTAHamburgCharacter.h`](../Source/GTAHamburg/GTAHamburgCharacter.h) declares `AGTAHamburgCharacter`, derived from Unreal's `ACharacter`. C++ inheritance, constructors, pointers, and `override` are standard C++. `ACharacter` supplies the capsule, skeletal mesh, and character movement component.
- [`GTAHamburgCharacter.cpp`](../Source/GTAHamburg/GTAHamburgCharacter.cpp) sets movement defaults and creates a `USpringArmComponent` and `UCameraComponent`. The spring arm follows control rotation and retracts around obstacles. `DoMove` uses the controller's yaw to turn input into camera-relative world directions; `DoLook` changes control rotation.
- `UCLASS`, `UPROPERTY`, `UFUNCTION`, `GENERATED_BODY`, and the generated header are Unreal-specific reflection mechanisms processed by UnrealHeaderTool. They make classes, properties, and functions available to the editor and Blueprints. Reflected object references also participate in Unreal's object tracking; do not manually `delete` these engine-owned components.
- [`GTAHamburgPlayerController.cpp`](../Source/GTAHamburg/GTAHamburgPlayerController.cpp) activates Enhanced Input mapping contexts for the local player. The character binds their actions to movement, looking, and jumping. The template's touch controls remain available, although Windows keyboard/mouse is the current target.
- [`GTAHamburgGameMode.cpp`](../Source/GTAHamburg/GTAHamburgGameMode.cpp) intentionally has an empty constructor. Its derived `BP_ThirdPersonGameMode` configures the default pawn and controller. `BP_ThirdPersonCharacter` supplies the mesh, animation Blueprint, and input-action references; C++ provides their underlying behavior. The native template classes are abstract, so the configured Blueprint subclasses are the playable classes.
- The redirects in `Config/DefaultEngine.ini` preserve the template assets' references after renaming the native module/classes from `TP_ThirdPerson` to `GTAHamburg`. Do not remove these redirects while assets still use the original references.
- `*.Target.cs` and `GTAHamburg.Build.cs` are **C#**, consumed by UnrealBuildTool. Targets select an editor or game build; module rules declare dependencies. AI and StateTree dependencies used only by the omitted variants are excluded.

### Small exercise: movement speed

1. Play once at the default walking speed.
2. Open `Content/ThirdPerson/Blueprints/BP_ThirdPersonCharacter`, select **Character Movement**, and note **Max Walk Speed** (the native constructor sets `500.f`, in centimeters per second).
3. Temporarily set it to **300**, compile the Blueprint, and play again. Walking should become slower while camera rotation stays the same.
4. Restore the original value (or reset to the inherited default), compile, and save. A Blueprint override takes precedence over the C++ default. To experiment with the C++ constructor instead, close the editor, change `MaxWalkSpeed`, rebuild, reopen, and ensure the Blueprint has no speed override.

No custom player-facing text is added in this step. Future text should start in English and use Unreal's localization-ready `FText` mechanisms as described in [the workflow](workflow.md).

## Verification record

- 2026-11-16: inspected installed engine/template files and initial compiler/SDK state; imported the base template and shared assets.
- 2026-11-16: updated Visual Studio Community to **17.14.45** and installed the components now recorded in `Config/VisualStudio.vsconfig`; both installer operations returned exit code 0. UnrealBuildTool selected **MSVC 14.44.35230** (tool directory `14.44.35207`) and **Windows SDK 10.0.22621.0**. Physical memory reported by the build tools: 15.93 GB (16 GB installed).
- 2026-11-16: `GTAHamburgEditor Win64 Development` build succeeded (11 actions, 103.09 seconds). Visual Studio project generation also succeeded (10.77 seconds), with the expected .NET 10 / Visual Studio 2022 notice described above.
- 2026-11-19: the user confirmed the manual template playtest passed, including WASD movement and jumping with Space. Movement physics tuning is deferred to a future task. A separate close-and-reopen test has not yet been explicitly confirmed.
