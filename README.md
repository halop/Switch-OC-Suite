# Switch OC Suite

[![License: GPL v2](https://img.shields.io/badge/License-GPL_v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html) 

Overclocking suite for Horizon OS (HOS) running on Atmosphere CFW.

[Project Homepage](https://kazushime.github.io/Switch-OC-Suite)

**DISCLAIMER: USE AT YOUR OWN RISK!**

- Overclocking in general will shorten the lifespan of some hardware components. **YOU ARE RESPONSIBLE for any problem or potential damage** if unsafe frequencies are ENABLED in sys-clk-OC. Issues like asking for bypassing limit will BE IGNORED OR CLOSED WITHOUT REPLY.

- Due to HorizonOS design, instabilities from unsafe RAM clocks may cause filesystem corruption. **Always make backup before enabling DRAM OC.**

## Features

- Erista variant (HAC-001)
  - CPU Overclock (Safe: 1785 MHz)
    - Unsafe
      - Due to the limit of board power draw or power IC
      - Unlockable frequencies up to 2091 MHz
      - See [README for sys-clk-OC](https://github.com/KazushiMe/Switch-OC-Suite/blob/master/Source/sys-clk-OC/README.md)

  - DRAM Overclock (Safe: 1862.4 MHz)
    - Unsafe: Up to 2131 MHz with DRAM bus overvolting depending on your DRAM chip

  - Modded sys-clk and ReverseNX-RT
    - CPU & GPU frequency governor (Experimental)
    - Set charging current (100 mA - 2000 mA) and charging limit (20% - 100%)
    - Global Profile
    - Sync ReverseNX Mode
    
- Mariko variant (HAC-001-01, HDH-001, HEG-001)
  - CPU / GPU Overclock (Safe: 1963 / 998 MHz)
    - Unsafe
      - Due to the limit of board power draw or power IC
      - Unlockable frequencies up to 2397 / 1305 MHz or 2295 / 1267 MHz
      - See [README for sys-clk-OC](https://github.com/KazushiMe/Switch-OC-Suite/blob/master/Source/sys-clk-OC/README.md)

  - DRAM Overclock (Safe: 1996.8 MHz)

  - Modded sys-clk and ReverseNX-RT
    - Auto CPU Boost
    - CPU & GPU frequency governor (Experimental)
    - Set charging current (100 mA - 2000 mA) and charging limit (20% - 100%)
    - Global Profile
    - Sync ReverseNX Mode

- Auto CPU Boost
  - For faster game loading
  - Enable CPU Boost (1785 MHz) when CPU Core#3 (System Core) is stressed (mainly I/O operations).
  - Effective only when charger is connected.

- CPU & GPU frequency governor (Experimental)
  - Adjust frequency based on load. Might decrease power draw but can introduce stutters. Can be turned off for specific titles.

- Setting charge limit (20% - 100%)
  - Long-term use of charge limit may render the battery gauge inaccurate. Performing full cycles could help recalibration, or try [battery_desync_fix_nx](https://github.com/CTCaer/battery_desync_fix_nx).

- Global profile
  - Designated a dummy title id `0xA111111111111111`.
  - Priority: "Temp overrides" > "Application profile" > "Global profile" > "System default".

- Sync ReverseNX Mode
  - No need to change clocks manually after toggling modes in ReverseNX (-RT and -Tool)

- **[System Settings (Optional)](https://github.com/KazushiMe/Switch-OC-Suite/blob/master/system_settings.md)**


## Installation

1. Download latest [release](https://github.com/KazushiMe/Switch-OC-Suite/releases/latest).

2. Copy all files in `SdOut` to the root of SD card.

3. Grab `x.x.x_loader.kip` for your Atmosphere version, rename it to `loader.kip` and place it in `/atmosphere/kips/`.

4. Customization via [online loader configurator](https://kazushime.github.io/Switch-OC-Suite/):
    <details>

    | Defaults   | Mariko        | Erista       |
    | ---------- | ------------- | ------------ |
    | CPU OC     | 2397 MHz Max  | 2091 MHz Max |
    | CPU Boost  | 1785 MHz      | N/A          |
    | CPU Volt   | 1235 mV Max   | 1257 mV Max  |
    | GPU OC     | 1305 MHz Max  | N/A          |
    | RAM OC     | 1996 MHz Max  | 1862 MHz Max |
    | RAM Volt   | Disabled      | Disabled     |
    | RAM Timing | Auto-Adjusted | Disabled     |

    </details>

5. Hekate-ipl bootloader Only
   - Add `kip1=atmosphere/kips/loader.kip` to boot entry section in `bootloader/hekate_ipl.ini`.

<details>
  <summary>Deprecated: patching sysmodules manually</summary>

  - This method is only served as reference as it could damage your MMC file system if not handled properly.

  - Patched sysmodules would be persistent until pcv or ptm was updated in new HOS (normally in `x.0.0`).

  - Tools:
    - Lockpick_RCM
    - TegraExplorer
    - [hactool](https://github.com/SciresM/hactool)
    - [nx2elf](https://github.com/shuffle2/nx2elf)
    - elf2nso from [switch-tools](https://github.com/switchbrew/switch-tools/)
    - [hacpack](https://github.com/The-4n/hacPack)

  1. Dump `prod.keys` with Lockpick_RCM
  2. Dump HOS firmware with TegraExplorer
  3. Configure and run `test_patch.sh` to generate patched pcv & ptm sysmodules in nca
  4. Replace nca in `SYSTEM:/Contents/registered/` with TegraExplorer
  5. `ValidateAcidSignature()` should be stubbed to allow unsigned sysmodules to load (a.k.a. `loader_patch`)

</details>


## Build

<details>
Grab necessary patches from the repo, then compile sys-clk, ReverseNX-RT and Atmosphere loader with devkitpro.

Before compiling Atmosphere loader, run `patch.py` in `Atmosphere/stratosphere/loader/source/` to insert oc module into loader sysmodule.

When compilation is done, uncompress the kip to make it work with configurator: `hactool -t kip1 Atmosphere/stratosphere/loader/out/nintendo_nx_arm64_armv8a/release/loader.kip --uncompress=./loader.kip`
</details>


## Acknowledgement

- CTCaer for [Hekate-ipl](https://github.com/CTCaer/hekate) bootloader, RE and hardware research
- [devkitPro](https://devkitpro.org/) for All-In-One homebrew toolchains
- masagrator for [ReverseNX-RT](https://github.com/masagrator/ReverseNX-RT) and info on BatteryChargeInfoFields in psm module
- Nvidia for [Tegra X1 Technical Reference Manual](https://developer.nvidia.com/embedded/dlc/tegra-x1-technical-reference-manual)
- RetroNX team for [sys-clk](https://github.com/retronx-team/sys-clk)
- SciresM and Reswitched Team for the state-of-the-art [Atmosphere](https://github.com/Atmosphere-NX/Atmosphere) CFW of Switch
- Switchbrew [wiki](http://switchbrew.org/wiki/) for Switch in-depth info
- Switchroot for their [modified L4T kernel and device tree](https://gitlab.com/switchroot/kernel)
- ZatchyCatGames for RE and original OC loader patches for Atmosphere
