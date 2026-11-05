# IPRO Workbench

The development environment for INTELPRO IPRO7, IPRO7AI and IPRO6S, built on Eclipse Theia. One desktop
app sets up the SDK and tools, creates and builds applications, flashes the board and shows its serial
console.

| System | 0.1.0 (Preview) |
|---|---|
| macOS, Apple silicon, macOS 12 or later | [`IPRO-Workbench-0.1.0-mac-arm64.dmg`](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.1.0/IPRO-Workbench-0.1.0-mac-arm64.dmg) (199 MB) |
| Windows 11, x64 | [`IPRO-Workbench-0.1.0-win-x64.exe`](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.1.0/IPRO-Workbench-0.1.0-win-x64.exe) (160 MB) |
| Linux | not available yet |

0.1.0 is a preview: the installers are not signed by Apple or Microsoft, so the first start needs one
extra step (below).

## What it does

- **Setup**: checks the IPRO SDK, RISC-V toolchain, CMake / Ninja / Python, IntelProCube and OpenOCD, and
  installs what is missing from INTELPRO's GitHub releases with one button.
- **Applications**: lists the SDK's applications, creates new ones from templates, and builds, flashes and
  monitors with one command.
- **Board tools**: serial monitor with filter, timestamps, log and plotter; Kconfig settings editor; pin
  configurator; partition table; memory map and size budget; crash decoding; register reference.
- **Tests and measurement**: runs the SDK's on-board tests, flashes AI models, BLE DTM RF tests, power
  measurement with a Nordic PPK2.
- **AI**: an MCP server, so AI coding assistants can build, flash and read test results through the IDE.

## Install on macOS

1. Download the DMG and check it. In Terminal:

   ```bash
   curl -LO https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.1.0/IPRO-Workbench-0.1.0-mac-arm64.dmg
   shasum -a 256 IPRO-Workbench-0.1.0-mac-arm64.dmg
   ```

   You should see `f1c562e6e0584dedb78b173a37fcd1b51b30fdf906927dc3f91d757cfe210d5b`.

2. Copy the app to Applications and start it:

   ```bash
   hdiutil attach IPRO-Workbench-0.1.0-mac-arm64.dmg
   cp -R "/Volumes/IPRO Workbench 0.1.0-arm64/IPRO Workbench.app" /Applications/
   hdiutil detach "/Volumes/IPRO Workbench 0.1.0-arm64"
   open "/Applications/IPRO Workbench.app"
   ```

   You should see the IPRO Workbench window with **IPRO Home** in the middle and the IPRO side bar
   (**Applications**, **Board**, **Environment**) on the left.

If you downloaded the DMG with a browser and dragged the app to Applications instead, macOS says it cannot
check the app the first time. Open **System Settings → Privacy & Security**, find IPRO Workbench and click
**Open Anyway**, or run `xattr -dr com.apple.quarantine "/Applications/IPRO Workbench.app"` once.

## Install on Windows

1. Download the installer and check it. In PowerShell, in the download folder:

   ```powershell
   (Get-FileHash .\IPRO-Workbench-0.1.0-win-x64.exe -Algorithm SHA256).Hash
   ```

   You should see `B517D14225CC1A99B7B0CC3D382B89049FC1C5966B6E6A374690E4CE0BECE586`.

2. Run `IPRO-Workbench-0.1.0-win-x64.exe`. When Windows SmartScreen says "Windows protected your PC",
   click **More info**, then **Run anyway**. Pick the folder and finish the installer.
3. Start **IPRO Workbench** from the Start menu.

   You should see the IPRO Workbench window with **IPRO Home** in the middle and the IPRO side bar on the
   left.

To install without the wizard, run `.\IPRO-Workbench-0.1.0-win-x64.exe /S /D=C:\IPRO\Workbench`.

## First run

1. **Set up the tools.** When something is missing, the banner at the top of IPRO Home says
   **Finish the setup**. Click **Fix Environment**.

   You should see the **IPRO Setup** output download and install each tool into `IPRO/tools` in your
   home folder. When it is done, every row of the **Environment** view has a green check. A toolchain or
   IntelProCube you already have can be used instead: click **Use one already on this computer…** on its
   row.
2. **Open the SDK.** On IPRO Home, click **Open SDK…** and choose an IPRO SDK folder, or click
   **Install SDK from GitHub…**.

   You should see the SDK's applications in the **Applications** view, grouped by category.
3. **Connect the board.** Plug the IPRO Debug Probe into the computer and the board.

   You should see the probe and its UART and CCI ports in the **Board** view.
4. **Build, flash and monitor.** Select `ipro_unit_test` in **Applications** and click
   **Build, Flash and Monitor** (⌘⌥R on macOS, Ctrl+Alt+R on Windows).

   You should see the build in the **IPRO Build** output, ending with the flash and RAM usage,
   `[All Success]` in **IPRO Flash**, and `ALL TESTS PASSED` in the **IPRO Monitor** terminal.

The interface follows the system language (English or Traditional Chinese). To change it, open the
command palette (⇧⌘P / Ctrl+Shift+P) and run **Configure Display Language**.

## Known limitations in 0.1.0

- Not signed by Apple (not notarized) or Microsoft; see the install steps for the first start.
- JTAG debugging is not ready yet.
- Automatic updates are off (`updates.checkForUpdates`); install new versions from this page.
- A serial monitor filter also hides lines the IDE reads itself (build stamp, partition table, test
  results); clear the filter for those steps.
- On Windows the environment check does not install clangd; code navigation offers to download it.
- Recording with the PPK2 power meter is not verified yet.

## Updates

Each version is a release of this repository. The release also carries `latest-mac.yml`, `latest.yml`
and the blockmap files that the built-in updater reads; you do not need to download them.

## License

IPRO Workbench © 2026 INTELPRO. It is built on [Eclipse Theia](https://github.com/eclipse-theia/theia)
(EPL-2.0 OR GPL-2.0-only WITH Classpath-exception-2.0) and Electron (MIT); the Electron and Chromium
license texts are inside the app. The bundled VS Code plugins from Open VSX keep their own licenses, in the
app's `plugins` folder.

## 繁體中文

IPRO Workbench 是 INTELPRO IPRO7、IPRO7AI、IPRO6S 的開發環境，以 Eclipse Theia 為基礎：補齊 SDK 與工具、
建立與建置應用程式、燒錄開發板、看序列埠輸出，都在同一個桌面程式裡。0.1.0 是預覽版，提供 macOS（Apple
silicon，macOS 12 以上）與 Windows 11 x64；Linux 版尚未提供。

安裝時照上面各系統的步驟：下載、核對 SHA-256、安裝。安裝檔沒有 Apple 或 Microsoft 簽章：macOS 用瀏覽器下載時，
第一次開啟要到「系統設定 → 隱私權與安全性」按「仍要打開」；Windows 的 SmartScreen 按「其他資訊 → 仍要執行」。
第一次使用：按首頁的「補齊環境」安裝工具，「開啟 SDK…」或「從 GitHub 安裝 SDK…」，接上除錯探棒與開發板，
選 `ipro_unit_test` 按「建置、燒錄並監看」。介面跟著系統語言，系統是繁體中文時就是中文介面。

## Related

[IPRO SDK](https://github.com/INTELPRO-INC/IPRO-SDK) · [Documentation](https://intelpro-inc.github.io/ipro-sdk-docs/) ·
Tools: [ipro-toolchain](https://github.com/INTELPRO-INC/ipro-toolchain), [ipro-cube](https://github.com/INTELPRO-INC/ipro-cube),
[ipro-build-tools](https://github.com/INTELPRO-INC/ipro-build-tools), [ipro-openocd](https://github.com/INTELPRO-INC/ipro-openocd)
