
# WalletConnectUnity
This repository is a monorepo of packages that extend [WalletConnectSharp](https://github.com/WalletConnect/WalletConnectSharp) and brings WalletConnect to Unity.

## Packages
| Package | Description                                                                                                                                                                                                                                                                                                                                                                              | OpenUPM |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Core | High-level, Unity-friendly extension of [WalletConnectSharp](https://github.com/WalletConnect/WalletConnectSharp)<br>- Automatic active session management<br>- Option to resume session from storage<br>- Deep linking support<br>- IL2CPP support<br>- Lightweight `IJsonRpcConnection` implementation<br>- QR Code generation utility<br>- API to load wallets data and visual assets | [![openupm](https://img.shields.io/npm/v/com.walletconnect.core?label=openupm&registry_uri=https://package.openupm.com)](https://openupm.com/packages/com.walletconnect.core/) |
| Modal | Simplest and most minimal way to connect your players with WalletConnect                                                                                                                                                                                                                                                                                                                 | [![openupm](https://img.shields.io/npm/v/com.walletconnect.modal?label=openupm&registry_uri=https://package.openupm.com)](https://openupm.com/packages/com.walletconnect.modal/) |
| UI | This is a technical package that provides UI for WalletConnect Modal. It is not intended to be used directly, but rather as a dependency of WalletConnect Modal.                                                                                                                                                                                                                         | [![openupm](https://img.shields.io/npm/v/com.walletconnect.ui?label=openupm&registry_uri=https://package.openupm.com)](https://openupm.com/packages/com.walletconnect.ui/) |

### Supported Platforms
* Unity Editor 2021.3 or above
* Android
* iOS
* macOS
* Windows
* WebGL (soon)

#### :warning: **This is beta software**: This software is currently in beta and under development. Please proceed with caution, and open a new issue if you encounter a bug. Older versions of  WalletConnectUnity are available under `legacy/*` branches :warning:

## Installation
<details>
  <summary>Install via OpenUPM CLI</summary>

To install packages via OpenUPM, you need to have [Node.js](https://nodejs.org/en/) and [openupm-cli](https://openupm.com/docs/getting-started.html#installing-openupm-cli) installed. Once you have them installed, you can run the following commands:

- **WalletConnect Modal**:
  ```bash
  openupm add com.walletconnect.modal
  ```
- **WalletConnectUnity Core**:
  ```bash
  openupm add com.walletconnect.core
  ```
</details>

<details>
  <summary>Install via Package Manager with OpenUPM</summary>

0. Open `Advanced Project Settings` from the gear ⚙ menu located at the top right of the Package Manager’s toolbar
1. Add a new scoped registry with the following details:
   - Name: `OpenUPM`
   - URL: `https://package.openupm.com`
   - Scope(s): `com.walletconnect`
2. Press plus ➕ and then `Save` buttons
3. In the Package Manager windows open the add ➕  menu from the toolbar
4. Select `Add package by name...`
5. Enter the name of the package you want to install:
   - **WalletConnectUnity Modal**: `com.walletconnect.modal`
   - **WalletConnectUnity Core**: `com.walletconnect.core`
6. Press `Add` button

</details>

<details>
  <summary>Install via Package Manager with Git URL</summary>
 
  0. Open the add ➕  menu in the Package Manager’s toolbar
  1. Select `Add package from git URL...`
  2. Enter the package URL. Note that when installing via a git URL, the package manager won't install git dependencies automatically. Follow the error messages from the console and add all necessary packages manually
     - **WalletConnectUnity Modal**: `https://github.com/WalletConnect/WalletConnectUnity.git?path=Packages/com.walletconnect.modal`
     - **WalletConnectUnity UI**: `https://github.com/WalletConnect/WalletConnectUnity.git?path=Packages/com.walletconnect.ui`
     - **WalletConnectUnity Core**: `https://github.com/WalletConnect/WalletConnectUnity.git?path=Packages/com.walletconnect.core`
  3. Press `Add` button

  It's possible to lock the version of the package by adding `#{version}` at the end of the git URL, where `#{version}` is the git tag of the version you want to use. 
  For example, to install version `1.0.0` of WalletConnectUnity Modal, use the following URL: 
  ```
  https://github.com/WalletConnect/WalletConnectUnity.git?path=Packages/com.walletconnect.modal#modal/1.0.0
  ```
</details>

## Usage
0. Set up in  project id and metadata `WalletConnectProjectConfig` ScriptableAsset (created automatically located at `Assets/WalletConnectUnity/Resources/WalletConnectProjectConfig.asset`, do NOT move it outside of `Resources` directory).
1. Initialize `WalletConnect` and connect wallet:
```csharp
// Initialize singleton
await WalletConnect.Instance.InitializeAsync();

// Or handle instancing manually
var walletConnectUnity = new WalletConnect();
await walletConnectUnity.InitializeAsync();


// Try resume last session
var sessionResumed = await WalletConnect.Instance.TryResumeSessionAsync();              
if (!sessionResumed)                                                                         
{                                                                                            
    var connectedData = await WalletConnect.Instance.ConnectAsync(connectOptions);

    // Use connectedData.Uri to generate QR code

    // Wait for wallet approval
    await connectedData.Approval;                                                            
}                                                                                            
```

### WalletConnectProjectConfig Fields
* Project Id - The id of your project. This will be used inside the relay server.
* Client Metadata
  * Name - The name of your app. This will be used inside the authentication request.
  * Description - The description of your app. This will be used inside the authentication request.
  * Url - The url of your app. This will be used inside the authentication request.
  * Icons - The icons of your app. This will be used inside the authentication request.
  * Very Url - The verification URL of your app. Currently used but not enforced

