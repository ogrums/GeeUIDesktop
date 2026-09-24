# GeeUIDesktop

App grid and display-mode switcher. It is a normal launcher activity, not the HOME shell. HOME is `LetianpaiOS`.

## Package

- `com.letianpai.robot.desktop`
- system uid
- `MainActivity` is `MAIN` / `LAUNCHER`

## What it does

`MainActivity` shows `LetianpaiMainView`, starts `GeeUIDesktopService`, and refreshes the app menu (`RobotAppListManager.updateAppMenuList`, `GeeUINetResponseManager.getLogoInfo`). A comment says that switching from the home page to the desktop must request the logo. `changeMode` sends `changeShowModule` through `ModeChangeCmdCallback`.

Other screens:

- `AppListActivity` — vertical app list. The adapter comment says it is the vertical list. System apps are filtered out.
- `AppModeSwitchActivity` — one module's mode page
- `UserAppMainActivity` — user apps and a delete confirm
- `AppDefaultSettingActivity` — notice to open the matching page in the Letianpai phone app

`OpenTypeConsts` names those screens: `OPEN_MODE_SWITCH`, `OPEN_APP_MAIN`, `OPEN_APP_SETTINGS`, `OPEN_APP_DEFAULT_SETTINGS`, `OPEN_APP_USER_APP`.

## How it talks to the rest of the robot

`GeeUIDesktopService` binds `com.renhejia.robot.letianpaiservice` (`android.intent.action.LETIANPAI`). The comment "链接服务端" means "connect to the AIDL server". On a mode change it calls `setLongConnectCommand`. On a successful install it calls `setAppCmd`. It also listens for package added, replaced, removed, and `PackageInstaller.ACTION_SESSION_COMMITTED`.

`PackageConsts` is the launch table for the other apps: face, time, settings, guide, launcher, camera, weather, stock, news, alarm, OTA, app store, Wi-Fi, MCU. One comment marks the service that must be started to take a photo.

Depends on the `GeeUIComponets` submodule (`CommChannel`, `Components`). `settings.gradle` spells the path `GeeUIComponents`.

## Comment glossary

| Where | Chinese | English |
|---|---|---|
| `MainActivity` | 首页切换到桌面页，需要主动请求LOGO | When home switches to the desktop, request the logo |
| `MainActivity` | 禁用关闭动画 | Disable the close animation |
| `PackageConsts` | 拍照需要启动的服务 | Service that must be started to take a photo |
| `GeeUIDesktopService` | 链接服务端 | Connect to the AIDL server |
| `AppListVerticalViewPagerAdapter` | 纵向应用列表Adapter | Vertical app-list adapter |
| `AppListManager` | 过滤掉系统应用 | Filter out system apps |
| `RobotConfigManager` | 机器人偏好设置管理器 | Robot preference manager |
| `RobotSharedPreference` | 文件操作，重新加载配置文件 | Reload the config file |
