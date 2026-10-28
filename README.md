# ReSkate Glass 0.11.2 — Creator Guide

Create transparent, reflective glass in Blender, add optional dirt, and build a playable ReSkate map.

**Start here:** install the add-on ZIP in Blender. Keep this guide open while setting it up. This is the packaged 0.11.2 public beta; the working code and shaders are unchanged. Have confirmed the city test and Kamurocho integration work on their setup. Other hardware and mod combinations still need testing.

## 1. What you need BEFORE building

| Requirement | What to install or check |
| --- | --- |
| Blender | The working creator workflow uses Blender 5.2. The original baseline was tested on 5.2.2; local export checks also used 4.5.3. Other versions are not guaranteed. |
| ReSkate and supported game data | Game build **25414733**. Launch ReSkate once so its game folder and Mods folder exist. A different Steam game build will not pass the check. |
| Compatible ReSkate Studio | You need **reskate_cli.exe AND its complete Studio folder**, including the bundled Blender converter. The CLI is the command-line tool the add-on runs automatically. ReSkate Studio.exe is a different file. |
| Microsoft C++ Build Tools | Install the **Desktop development with C++** workload, x64/x86 MSVC compiler tools, and a Windows 10 or 11 SDK. Visual Studio 2022 Build Tools is the tested baseline. If another Visual Studio version is already installed, use its Installer > Modify to check these components; newer toolchains have not been comprehensively validated. |
| A saved Blender map | Include glass, a floor/collision, and an Empty named **spawn** above the floor. Work on a saved copy. |

**Important Studio compatibility requirement:** this release accepts one checked CLI binary, not every Studio download. Obtain the compatible Studio package from the release distributor if your copy fails the version check. Keep all its files together. Studio and game files are not bundled here. Do not bypass compatibility checks.

Official download/setup pages:
- Blender: https://www.blender.org/download/
- ReSkate: https://github.com/Dingo-Shenanigans/ReSkate/releases
- Visual Studio 2022 Build Tools installer: https://aka.ms/vs/17/release/vs_BuildTools.exe
- Microsoft C++ installation guide: https://learn.microsoft.com/cpp/build/vscpp-step-0-installation

You do not need PIX, RenderDoc, ReShade, a capture launcher, or a separate custom runtime DLL. No manual command-line work is normally required.

## 2. Install the add-on

1. Open Blender. Choose **Edit > Preferences > Add-ons**.
2. Open the menu at the top right and choose **Install from Disk** (called Install in some versions).
3. Select **ReSkate_Glass_0_11_2.zip**. Install the ZIP itself; do not select an individual Python file.
4. Enable **ReSkate Glass Authoring**. If upgrading, replace the old installation rather than keeping duplicate copies.
5. Fully close and reopen Blender.
6. In the 3D Viewport, press **N** to open the sidebar, then choose **ReSkate Glass**. Its heading still says **0.11.2 City Experimental**; that is expected.

## 3. Set the three paths

In **Edit > Preferences > Add-ons**, search for ReSkate Glass and expand the arrow beside its name. Use each field's folder/file picker:

| Field | Select this | Example |
| --- | --- | --- |
| Studio CLI | The file **reskate_cli.exe** inside the compatible Studio folder | D:\ReSkateStudio\reskate_cli.exe |
| ReSkate game folder | The folder containing **Skate.exe** and **Data\layout.toc** | D:\Reskate\Skate\ |
| Build output folder | Leave blank for the default, or choose a separate writable folder outside Mods | D:\GlassBuilds\ |

Do not select ReSkate Studio.exe for the CLI. Do not select the Data or Mods folder as the game folder. The exact example paths may differ on your PC. Save Preferences if Blender does not save automatically.

The default build location is **%LOCALAPPDATA%\ReSkateGlass\Builds**. Blank tool paths can also prompt for selection during a build.

## 4. Try the included example first

1. Extract a separate copy of the ZIP somewhere convenient to browse its examples.
2. Open **reskate_glass\examples\City_Materials_Test.blend**.
3. Choose **File > Save As** and save a working copy.
4. Close skate and ReSkateLauncher completely.
5. In the ReSkate Glass sidebar, click **Build Map with Glass**.
6. Let the PowerShell window finish. Wait for **Installed in ReSkate**.
7. Launch ReSkate. Ensure both generated **glass_author_...** and matching **glass_shader_...** components are enabled. Load the map ending in **Glass Preview**.

The example has four nearby panes with different dirt and reflection settings, plus matching groups 160 metres to either side. The dirt patterns are intentional. Set Dirt coverage to 0 for clean glass. Changing one material should affect only panes using that material. No in-game LOD adjustment is needed.

## 5. Add glass to your own map

1. Save a copy of your map before editing.
2. Select the window geometry. Use **Assign New Glass Material** in the ReSkate Glass panel. Replace existing glass instead of adding overlapping panes. For objects with frame and glass material slots, check the intended glass slot/faces and verify the frame stays unchanged.
3. Choose a preset and click **Apply Preset**, then adjust reflection strength, roughness, face opacity and edge opacity as needed. These values export per material.
4. Leave Studio's **Shader Override** checkbox OFF for this glass material. The glass builder handles the shader setup.
5. For clean glass, set **Dirt coverage = 0**. For dirt, choose the mode and an image below.
6. UV-map the glass using its active UV layer. UVs control where the dirt image appears. Use Blender's Material Preview to inspect it; the game result can differ.
7. Save, close skate and its launcher, then use **Build Map with Glass**. Ordinary Studio import alone does not apply the glass patch.

### Dirt images made simple

| Mode | Image to use | How it works |
| --- | --- | --- |
| Color + Alpha | A PNG with transparency | RGB supplies dirt color; alpha controls where dirt appears. Try examples\Window_Dirt_RGBA.png. |
| Grayscale Mask | A grayscale image | The red channel adds neutral grime. Try examples\dirt_test_mask.png. |

Choose the file in **Dirt image** and start with **Dirt coverage around 0.6**. Higher coverage makes dirt stronger and may hide the view through the pane. Different materials can use different images and modes. Materials shared by several windows share settings; make the material unique before changing just one group. Change Dirt coverage in the panel, not generated preview nodes.

Images export at up to 1024 x 1024 pixels; larger images are reduced. Use ordinary byte-buffer images in sRGB or linear Rec.709. HDR, unsupported color spaces, UDIM, movie and image-sequence inputs are refused. Keep UV coordinates within -16 to 16.

## 6. Build, update and share a map

- Save before every build and close both the game and launcher.
- A build exports the map, patches glass, compiles and installs the matching map/support pair. Keep the PowerShell window open until it finishes.
- Reopen the game after installing or changing shader support.
- For players, distribute the generated **Installable-Glass-Test.zip** from the build output. It contains the map, matching support component, installer and remover. Do not give players the intermediate authoring ZIP.
- Players need the compatible ReSkate/game setup and both installed components. They do not need Blender, Studio or C++ Build Tools.
- To remove a built package, close the game/launcher and use its **Remove-Glass-Test.ps1**. Remove both components together. Disabling the map alone leaves its shader support active.
- To uninstall the Blender add-on, use Blender Preferences > Add-ons > ReSkate Glass > Remove. This does not remove already installed game packages.

## 7. Common problems

| Message or symptom | What to do |
| --- | --- |
| Unsupported game executable / missing layout.toc | Check the game-folder field points to the folder containing Skate.exe and Data\layout.toc. Install/verify supported build 25414733 through ReSkateLauncher. |
| CLI version does not match | Select reskate_cli.exe, not the Studio GUI. If already correct, obtain the compatible Studio package from the release distributor. Reinstalling C++ tools will not fix this check. |
| C++ compiler / Build Tools missing | Open Visual Studio Installer > Modify. Add Desktop development with C++, x64/x86 MSVC tools and a Windows SDK, then retry. |
| No Mods folder | Launch the supported ReSkate installation once, close it, then retry. |
| Build or install fails | Use Open Build Folder and Copy Build Details. Keep the logs; a compilation success does not mean installation finished. |
| Glass is noisy, watery or missing | Confirm the matching shader support is enabled and restart the game. Send a screenshot, GPU model and runtime ReSkate.log. This appearance has been reported on another setup; the cause is unresolved. |
| Several windows change together | They share a material. Make it unique before editing only one group. |
| Dirt hides everything | Lower Dirt coverage, check the PNG alpha, or set coverage to 0 to test clean glass. |
| Preview differs from game | Blender preview is approximate. Verify the final build in game. |

When asking for help, include add-on/Blender/game versions and **Copy Build Details**. Build failures: include relevant Prepare.log, Compile.log, Patch.log or Install.log. Visual problems: also include BUILD_AUDIT.json from the playable folder, a screenshot/video, GPU model and ReSkate.log. Do not send the full city source unless requested.

## 8. Current limits and release status

- Confirmed independent reflection controls, different dirt images/modes, distant test groups and a successful 0.11.2 Kamurocho integration. This is a public beta, not a promise of universal compatibility.
- Distance filtering/mip selection is unfinished; dirt can shimmer at distance or steep angles.
- LODs and streaming are not generated. Large maps and generated reflection probes can affect performance.
- Reflection probes do not guarantee live mirror reflections of moving skaters. Moving objects and local-light behavior are not fully validated.
- Multiple glass maps, other shader mods and broad GPU/driver compatibility remain unverified. One external tester's watery-glass report remains unresolved.
- Glass can span a city; the old 100-metre restriction is replaced by local cells. Extreme geometry or unsupported native instances can still be refused.

## Included files and license

Examples are inside **examples**. **TECHNICAL_NOTES.md** retains development history for advanced users. The native helper source and its GPLv3 license are included in **builder\native\COPYING**. Preserve included licenses when redistributing. This archive does not include Studio, game data or Microsoft development tools.

Advanced compatibility reference: accepted Studio CLI SHA-256:
001F613310E263F06B3479129E0E314D71BE703989D2A84178332BF4F43B1248
