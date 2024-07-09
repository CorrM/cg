# CheatGear

This repository manages the Cheat Gear [CLI tool](https://cheatgear.com).
For community support please visit our [Discord Server](http://discord.gg/P9Pddgz).

## Requirements

For version 4:
- [.Net 7](https://dotnet.microsoft.com/download/dotnet/7.0/runtime).

For version 5(beta):
- [.Net 8](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-8.0.0-windows-x64-installer).
  - You can install using WinGet `winget install Microsoft.DotNet.DesktopRuntime.8`.

## Donwload

- [Version 4](https://github.com/CorrM/cg/releases/tag/v4.0.11)
- [Version 5](https://github.com/CorrM/cg/releases/latest) (Recommended)

## How to use CLI

### Use your API key

There is two methods to provide your API key to CLI:
- Pass your key as a command line arg `--api CG-XXXXXXXXXXXXXXXXXXXXXXXXXXXXX`
- Set it in your environment variables using your terminal `setx CG_API_KEY CG-XXXXXXXXXXXXXXXXXXXXXXXXXXXXX`

### Update configs

It's recommended to upgrade configs before starting:

```
CheatGear.CLI.exe unreal config upgrade
```

### Search for GNames/GObjects

The main objective of using this tool is to generate SDK, You will need two things to do that,GNames and GObjects addresses.
To obtain these address please utilize the following commands:
```
CheatGear.CLI.exe unreal search names -p PID -v UNREAL_VER -l SEARCH_LVL -c CONFIG_NAME
CheatGear.CLI.exe unreal search objects -p PID -v UNREAL_VER -l SEARCH_LVL -c CONFIG_NAME
```

- `PID`: Target process id in decimal
- `UNREAL_VER`: Unreal version your target are using. (eg. unreal3/unreal4/unreal5)
- `SEARCH_LVL`: Each level uses a different algorithm going up means takes more time. (eg. 1/2/3)
- `CONFIG_NAME`: Engine config that defines how the tool will interact with target memory. (eg. 3.0/3.1/4.21/4.25/5.0/5.3)

### Generate SDK

After getting GNames/GObjects address we are ready to generate our SDK now to do that use this command:
```
CheatGear.CLI.exe unreal sdk generate -p PID -v UNREAL_VER -c CONFIG_NAME -n GNAMES_ADDRESS -o GOBJECTS_ADDRESS -gn GAME_NAME -gv GAME_VERSION
```

- `PID`: Target process id in decimal
- `UNREAL_VER`: Unreal version your target are using. (eg. unreal3/unreal4/unreal5)
- `CONFIG_NAME`: Engine config that defines how the tool will interact with target memory. (eg. 3.0/3.1/4.21/4.25/5.0/5.3)
- `GNAMES_ADDRESS`: GNames address you got yourself or by using `search` command in hex format. (eg. 0x7FF745F1E908)
- `GOBJECTS_ADDRESS`: GObjects address you got yourself or by using `search` command in hex format. (eg. 0x7FF745F22C18)
- `GAME_NAME`: Game name. (eg. MyNiceGame)
- `GAME_VERSION`: Game version. (eg. 1.0)

### Convert SDK

Generate SDK give us a `.cgs` file allowing us to convert it to a different programming language syntaxes:
```
CheatGear.CLI.exe unreal sdk convert -f "CGS_FILE_PATH" -l LANGUAGE -t SYNTAX_TYPE
```

- `CGS_FILE_PATH`: `.cgs` file path that you get from `sdk generate` command.
- `LANGUAGE`: Programming language name to generate syntax for. (eg. cpp)
- `SYNTAX_TYPE`: There are two types of syntax you can generate `internal` or `external`.

### Instance search/dump

TODO

## Fix generated SDK

You have two options:

**1. Delete this function from header and cpp file**
```
FUObjectItem::IsUnreachable // BasicTypes_FUObjectItem.h, BasicTypes.cpp
FUObjectItem::IsPendingKill // BasicTypes_FUObjectItem.h, BasicTypes.cpp
FWeakObjectPtr::SerialNumbersMatch // BasicTypes_FWeakObjectPtr.h, BasicTypes.cpp
FWeakObjectPtr::IsValid // BasicTypes_FWeakObjectPtr.h, BasicTypes.cpp
FWeakObjectPtr::Get // BasicTypes_FWeakObjectPtr.h, BasicTypes.cpp
```

**2. Add required fields**
```
FUObjectItem::Flags // int32
FUObjectItem::SerialNumber // int32
```
