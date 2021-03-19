---
title: Bıcep geliştirme ve dağıtım ortamlarını ayarlama
description: Bıcep geliştirme ve dağıtım ortamlarını yapılandırma
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: d665a863affdec2009fc208f76b85a7f25de451d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104594402"
---
# <a name="setup-bicep-development-and-deployment-environment"></a>Bıcep geliştirme ve dağıtım ortamını ayarlama

Bıcep geliştirme ve dağıtım ortamlarını ayarlamayı öğrenin.

## <a name="development-environment"></a>Geliştirme ortamı

En iyi bicep yazma deneyimini almak için iki bileşene ihtiyacınız vardır:

- **Visual Studio Code Için Bıcep uzantısı**. Bicep dosyaları oluşturmak için, iyi bir Bıcep Düzenleyicisi gerekir. [Bıcep uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)ile [Visual Studio Code](https://code.visualstudio.com/) önerilir. Bu araçlar, dil desteği ve kaynak otomatik tamamlama sağlar. Bicep dosyalarını oluşturma ve doğrulamaya yardımcı olurlar. Visual Studio Code ve Bıcep uzantısını kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Visual Studio Code Ile bıcep dosyaları oluşturma](./quickstart-create-bicep-use-visual-studio-code.md).
- **Bıcep CLI**. Bıcep CLı kullanarak bicep dosyalarını ARM JSON şablonlarına derleyin ve ARM JSON şablonlarını Bıcep dosyalarına derlemeyi bozun. Daha fazla bilgi için bkz. [Bıcep CLI 'Yı yüklemeyin](#install-bicep-cli).

## <a name="deployment-environment"></a>Dağıtım ortamı

Azure CLı veya Azure PowerShell kullanarak Bıcep dosyalarını dağıtabilirsiniz. Azure CLı için sürüm 2.20.0 veya daha yeni bir sürüme ihtiyacınız vardır; Azure PowerShell için sürüm 5.6.0 veya sonraki bir sürüme ihtiyacınız vardır. Yükleme yönergeleri için bkz.:

- [Azure PowerShell’i yükleme](/powershell/azure/install-az-ps)
- [Windows'da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows)
- [Linux 'ta Azure CLı 'yı yükler](/cli/azure/install-azure-cli-linux)
- [macOS’ta Azure CLI'yi yükleme](/cli/azure/install-azure-cli-macos)

> [!NOTE]
> Şu anda hem Azure CLı hem de Azure PowerShell yalnızca yerel bicep dosyalarını dağıtabilir. Azure CLı kullanarak Bıcep dosyalarını dağıtma hakkında daha fazla bilgi için bkz. [Deploy-CLI](/deploy-cli.md#deploy-remote-template). Azure PowerShell kullanarak Bıcep dosyalarını dağıtma hakkında daha fazla bilgi için bkz. [Deploy-PowerShell](/deploy-powershell.md#deploy-remote-template).

Azure PowerShell veya Azure CLı 'nin desteklenen sürümü yüklendikten sonra bir Bıcep dosyasını şu ile dağıtabilirsiniz:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateFile <path-to-template-or-bicep> `
  -storageAccountType Standard_GRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file <path-to-template-or-bicep> \
  --parameters storageAccountType=Standard_GRS
```

---

## <a name="install-bicep-cli"></a>Bıcep CLı 'yı yükler

Azure PowerShell veya el ile Azure CLı kullanarak Bıcep CLı yükleyebilirsiniz.

### <a name="use-azure-cli"></a>Azure CLI kullanma

Az CLı Version 2.20.0 veya üzeri yüklü olduğunda, kendisine bağımlı bir komut yürütüldüğünde Bıcep CLı otomatik olarak yüklenir. Örneğin `az deployment ... -f *.bicep` veya `az bicep ...` olabilir.

Ayrıca, yerleşik komutları kullanarak CLı 'yı el ile yükleyebilirsiniz:

```bash
az bicep install
```

En son sürüme yükseltmek için:

```bash
az bicep upgrade
```

Belirli bir sürümü yüklemek için:

```bash
az bicep install --version v0.2.212
```

> [!NOTE]
> Az CLı, Bıcep CLı 'nin sahip olduğunuz herhangi bir bicep yüklemesi ile çakışmadan ayrı bir sürümünü yüklerse ve az CLı, yolunuza bicep eklemez.

Yüklü sürümleri göstermek için:

```bash
az bicep version
```

Bıcep CLı 'nın tüm kullanılabilir sürümlerini listelemek için:

```bash
az bicep list-versions
```

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Azure PowerShell, Bıcep CLı 'yı henüz yükleyeme yeteneğine sahip değil. Azure PowerShell (v 5.6.0 veya üzeri), Bıcep CLı 'nın yolunda zaten yüklü ve kullanılabilir olmasını bekler. [El ile yüklenen yöntemlerden](#install-manually)birini izleyin. Bıcep CLı yüklendikten sonra, bir dağıtım cmdlet 'i için gerektiğinde Bıcep CLı çağırılır. Örneğin, `New-AzResourceGroupDeployment ... -TemplateFile main.bicep`.

### <a name="install-manually"></a>El ile yükleme

Aşağıdaki yöntemler Bıcep CLı 'yı yükler ve yolunuza ekler.

#### <a name="linux"></a>Linux

```sh
# Fetch the latest Bicep CLI binary
curl -Lo bicep https://github.com/Azure/bicep/releases/latest/download/bicep-linux-x64
# Mark it as executable
chmod +x ./bicep
# Add bicep to your PATH (requires admin)
sudo mv ./bicep /usr/local/bin/bicep
# Verify you can now access the 'bicep' command
bicep --help
# Done!

```

#### <a name="macos"></a>macOS

##### <a name="via-homebrew"></a>Homebrew aracılığıyla

```sh
# Add the tap for bicep
brew tap azure/bicep https://github.com/azure/bicep

# Install the tool
brew install azure/bicep/bicep
```

##### <a name="macos-manual-install"></a>macOS el ile yüklemesi

```sh
# Fetch the latest Bicep CLI binary
curl -Lo bicep https://github.com/Azure/bicep/releases/latest/download/bicep-osx-x64
# Mark it as executable
chmod +x ./bicep
# Add Gatekeeper exception (requires admin)
sudo spctl --add ./bicep
# Add bicep to your PATH (requires admin)
sudo mv ./bicep /usr/local/bin/bicep
# Verify you can now access the 'bicep' command
bicep --help
# Done!

```

#### <a name="windows"></a>Windows

##### <a name="windows-installer"></a>Windows Installer

[En son Windows Installer](https://github.com/Azure/bicep/releases/latest/download/bicep-setup-win-x64.exe)sürümünü indirip çalıştırın. Yükleyici, yönetici ayrıcalıkları gerektirmez. Yüklemeden sonra, Bıcep CLı Kullanıcı yolunuza eklenir. YOL değişikliğinin etkili olması için tüm açık komut kabuğu pencerelerini kapatın ve yeniden açın.

##### <a name="chocolatey"></a>Chocolatey

```powershell
choco install bicep
```

##### <a name="winget"></a>Winget

```powershell
winget install -e --id Microsoft.Bicep
```

##### <a name="manual-with-powershell"></a>PowerShell ile el ile

```powershell
# Create the install folder
$installPath = "$env:USERPROFILE\.bicep"
$installDir = New-Item -ItemType Directory -Path $installPath -Force
$installDir.Attributes += 'Hidden'
# Fetch the latest Bicep CLI binary
(New-Object Net.WebClient).DownloadFile("https://github.com/Azure/bicep/releases/latest/download/bicep-win-x64.exe", "$installPath\bicep.exe")
# Add bicep to your PATH
$currentPath = (Get-Item -path "HKCU:\Environment" ).GetValue('Path', '', 'DoNotExpandEnvironmentNames')
if (-not $currentPath.Contains("%USERPROFILE%\.bicep")) { setx PATH ($currentPath + ";%USERPROFILE%\.bicep") }
if (-not $env:path.Contains($installPath)) { $env:path += ";$installPath" }
# Verify you can now access the 'bicep' command.
bicep --help
# Done!
```

## <a name="install-the-nightly-builds"></a>Gecelik derlemelerini yükler

Kullanıma sunulmadan önce Bıcep 'nin en son yayın öncesi bitlerini denemek istiyorsanız, bkz. [gecelik derlemelerini Install](https://github.com/Azure/bicep/blob/main/docs/installing-nightly.md).

> [!WARNING]
> Bu yayın öncesi derlemeler bilinen veya bilinmeyen hatalara sahip olma olasılığını çok daha yüksektir.

## <a name="next-steps"></a>Sonraki adımlar

[Bıcep hızlı](./quickstart-create-bicep-use-visual-studio-code.md)başlangıcı ile çalışmaya başlayın.
