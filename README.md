# IPRO Workbench

The development environment for INTELPRO IPRO7, IPRO7AI and IPRO6S, built on Eclipse Theia. One desktop
app sets up the SDK and tools, creates and builds applications, flashes the board and shows its serial
console.

| System | 0.2.1 (Preview) |
|---|---|
| macOS, Apple silicon, macOS 12 or later | [`IPRO-Workbench-0.2.1-mac-arm64.dmg`](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-0.2.1-mac-arm64.dmg) (201 MB) |
| Windows 11, x64 | [`IPRO-Workbench-0.2.1-win-x64.exe`](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-0.2.1-win-x64.exe) (161 MB) |
| Linux | not available yet |

Operation manual V1.2, written for 0.1.1 (a manual for 0.2 follows): [English](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-Operation-Manual-V1.2-en.pdf) (73 pages, 10.8 MB) · [Traditional Chinese](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-Operation-Manual-V1.2-zh-TW.pdf) (67 pages, 11.0 MB).

0.2.1 is a preview: the installers are not signed by Apple or Microsoft, so the first start needs one
extra step (below).

## What it does

- **Setup**: checks the IPRO SDK, RISC-V toolchain, CMake / Ninja / Python, IntelProCube and OpenOCD, and
  installs what is missing from INTELPRO's GitHub releases with one button.
- **Applications**: lists the SDK's applications and their configurations (`defconfig_*`), creates new
  ones from templates, and builds, flashes and monitors with one command. Boards are flashed through the IPRO
  Debug Probe, or over their own USB (boot2's USB downloader).
- **OTA**: makes the OTA package of an application and sends it to the board over its console UART.
- **Board tools**: serial monitor with filter, timestamps, log and plotter; a sensor dashboard laid out by
  the application's `dashboard.yaml`; Kconfig settings editor; pin
  configurator; partition table; memory map and size budget; crash decoding; register reference.
- **Tests and measurement**: runs the SDK's on-board tests, flashes AI models, BLE DTM RF tests, Wi-Fi
  scan and iperf, power measurement with a Nordic PPK2.
- **AI**: an MCP server, so AI coding assistants can build, flash and read test results through the IDE.

## Install on macOS

1. Download the DMG and check it. In Terminal:

   ```bash
   curl -LO https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-0.2.1-mac-arm64.dmg
   shasum -a 256 IPRO-Workbench-0.2.1-mac-arm64.dmg
   ```

   You should see `85320f4ef469bb8e436f6cd0575d6f2410502e05040da3f137e3d343a17533f9`.

2. Copy the app to Applications and start it. If an older version is in Applications, quit it first; the
   `rm` line removes it so no file of the old version stays behind:

   ```bash
   rm -rf "/Applications/IPRO Workbench.app"
   hdiutil attach IPRO-Workbench-0.2.1-mac-arm64.dmg
   cp -R "/Volumes/IPRO Workbench 0.2.1-arm64/IPRO Workbench.app" /Applications/
   hdiutil detach "/Volumes/IPRO Workbench 0.2.1-arm64"
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
   (Get-FileHash .\IPRO-Workbench-0.2.1-win-x64.exe -Algorithm SHA256).Hash
   ```

   You should see `0BEB0DAFFCBD30F6B37E2F252EE9B75C00E41507B426DA3045036319E4918660`.

2. Run `IPRO-Workbench-0.2.1-win-x64.exe`. When Windows SmartScreen says "Windows protected your PC",
   click **More info**, then **Run anyway**. Pick the folder and finish the installer.
3. Start **IPRO Workbench** from the Start menu.

   You should see the IPRO Workbench window with **IPRO Home** in the middle and the IPRO side bar on the
   left.

To install without the wizard, run `.\IPRO-Workbench-0.2.1-win-x64.exe /S /D=C:\IPRO\Workbench`.

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

The interface follows the system language (English, Traditional Chinese, Simplified Chinese or
Japanese). To change it, open the
command palette (⇧⌘P / Ctrl+Shift+P) and run **Configure Display Language**.

## Known limitations in 0.2.1

- Not signed by Apple (not notarized) or Microsoft; see the install steps for the first start.
- JTAG debugging is not ready yet.
- Automatic updates are off (`updates.checkForUpdates`); install new versions from this page.
- IPRO6 boards print no partition table at boot, so it is not compared with the board's. They print the
  build stamp the board view uses from IPRO SDK V2.3.0 on.
- On Windows, build the IPRO SDK with the RISC-V toolchain 14.2.1 (what **Fix Environment** installs);
  13.1.1 cannot link the SDK's prebuilt libraries.
- Wi-Fi Throughput (iperf): the board's network name and password cannot contain spaces, and the board
  echoes the password in the monitor. When the board sends to a Windows PC, the firewall must allow
  incoming TCP port 5001.

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
建立與建置應用程式、燒錄開發板、看序列埠輸出，都在同一個桌面程式裡。0.2.1 是預覽版，提供 macOS（Apple
silicon，macOS 12 以上）與 Windows 11 x64；Linux 版尚未提供。

安裝時照上面各系統的步驟：下載、核對 SHA-256、安裝。安裝檔沒有 Apple 或 Microsoft 簽章：macOS 用瀏覽器下載時，
第一次開啟要到「系統設定 → 隱私權與安全性」按「仍要打開」；Windows 的 SmartScreen 按「其他資訊 → 仍要執行」。
第一次使用：按首頁的「補齊環境」安裝工具，「開啟 SDK…」或「從 GitHub 安裝 SDK…」，接上除錯探棒與開發板，
選 `ipro_unit_test` 按「建置、燒錄並監看」。介面跟著系統語言（英文、繁體中文、簡體中文、日文），系統是繁體中文時就是繁體中文介面。每項功能的逐步操作見[操作手冊](https://github.com/INTELPRO-INC/ipro-workbench/releases/download/v0.2.1/IPRO-Workbench-Operation-Manual-V1.2-zh-TW.pdf)（PDF）。

## Related

[IPRO SDK](https://github.com/INTELPRO-INC/IPRO-SDK) · [Documentation](https://intelpro-inc.github.io/ipro-sdk-docs/) ·
Tools: [ipro-toolchain](https://github.com/INTELPRO-INC/ipro-toolchain), [ipro-cube](https://github.com/INTELPRO-INC/ipro-cube),
[ipro-build-tools](https://github.com/INTELPRO-INC/ipro-build-tools), [ipro-openocd](https://github.com/INTELPRO-INC/ipro-openocd)
