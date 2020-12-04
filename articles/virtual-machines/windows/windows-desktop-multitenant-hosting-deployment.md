---
title: Azure 'da çok kiracılı barındırma haklarıyla Windows 10 ' un dağıtımı
description: Çoklu kiracı barındırma haklarıyla Azure 'a şirket içi lisanslar getirmek için Windows yazılım güvencesi avantajlarınızı nasıl en üst düzeye çıkaracağınızı öğrenin.
author: xujing
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 1/24/2018
ms.author: xujing
ms.openlocfilehash: 101f2cfe57624502764d145351a6343cfdd2a334
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96572873"
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Azure 'da çok kiracılı barındırma haklarıyla Windows 10 ' un dağıtımı 
Windows 10 Enterprise E3/E5 veya Kullanıcı başına Windows sanal masaüstü erişimi (Kullanıcı aboneliği lisansları veya eklenti Kullanıcı aboneliği lisansları) olan müşteriler için, Windows 10 için çok kiracılı barındırma hakları, Windows 10 lisanslarınızı buluta getirmenize ve Azure 'da Windows 10 sanal makinelerini başka bir lisans ödemeksizin çalıştırmanıza olanak sağlar. Daha fazla bilgi için lütfen bkz. [Windows 10 Için çok kiracılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx).

> [!NOTE]
> Bu makalede, Azure Market 'te Windows 10 Pro Desktop görüntüleri için lisans avantajı 'nı uygulamanız gösterilmektedir.
> - MSDN abonelikleri için Azure Market 'te Windows 7, 8,1, 10 Enterprise (x64) görüntüleri için lütfen [geliştirme ve test senaryoları Için Azure 'Da Windows istemcisine](client-images.md) bakın
> - Windows Server lisanslama avantajları için lütfen [Windows Server görüntüleri Için Azure hibrit kullanım avantajları](hybrid-use-benefit-licensing.md)bölümüne bakın.
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Azure Marketi 'nden Windows 10 görüntüsünü dağıtma 
PowerShell, CLı ve Azure Resource Manager şablon dağıtımları için, Windows 10 görüntüsü aşağıdaki PublisherName, teklif, SKU ile bulunabilir.

| İşletim Sistemi  |      PublisherName      |  Sunduğu | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS2-ProN  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS3-ProN  |

## <a name="qualify-for-multi-tenant-hosting-rights"></a>Çok kiracılı barındırma haklarına uygun hale getir 
Çok kiracılı barındırma haklarını nitelemek ve Azure kullanıcıları üzerinde Windows 10 görüntülerini çalıştırmak için aşağıdaki aboneliklerden birine sahip olması gerekir: 

-   Microsoft 365 E3/E5 
-   Microsoft 365 F3 
-   Microsoft 365 a3/a5 
-   Windows 10 Enterprise E3/E5
-   Windows 10 eğitim a3/a5 
-   Windows VDA E3/E5


## <a name="uploading-windows-10-vhd-to-azure"></a>Windows 10 VHD 'yi Azure 'a yükleme
Genelleştirilmiş bir Windows 10 VHD 'yi karşıya yüklüyorsanız, lütfen Windows 10 ' un yerleşik yönetici hesabının varsayılan olarak etkin olmadığını unutmayın. Yerleşik yönetici hesabını etkinleştirmek için, Özel Betik uzantısının parçası olarak aşağıdaki komutu ekleyin.

```powershell
Net user <username> /active:yes
```

Aşağıdaki PowerShell kod parçacığı, yerleşik yönetici de dahil olmak üzere tüm yönetici hesaplarını etkin olarak işaretlemenize olanak sağlar. Bu örnek, yerleşik Yönetici Kullanıcı adı bilinmiyorsa yararlı olur.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Daha fazla bilgi için: 
* [VHD 'yi Azure 'a yükleme](upload-generalized-managed.md)
* [Bir Windows VHD 'yi Azure 'a yüklemek üzere hazırlama](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Windows 10 ' un çok kiracılı barındırma haklarıyla dağıtımı
[En son Azure PowerShell yükleyip yapılandırdığınızdan](/powershell/azure/)emin olun. VHD 'nizi hazırladıktan sonra cmdlet 'ini kullanarak Azure depolama hesabınıza VHD 'yi `Add-AzVhd` aşağıdaki gibi yükleyin:

```powershell
Add-AzVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Azure Resource Manager şablonu dağıtımını kullanarak dağıtma** Kaynak Yöneticisi şablonlarınız içinde, için ek bir parametre `licenseType` belirtilebilir. [Azure Resource Manager şablonları yazma](../../azure-resource-manager/templates/template-syntax.md)hakkında daha fazla bilgi edinebilirsiniz. VHD 'nizi Azure 'a yükledikten sonra, işlem sağlayıcısının bir parçası olarak lisans türünü içerecek şekilde Kaynak Yöneticisi şablonu düzenleyin ve şablonunuzu normal olarak dağıtın:
```json
"properties": {
    "licenseType": "Windows_Client",
    "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
    }
```

**PowerShell aracılığıyla dağıtma** PowerShell aracılığıyla Windows Server VM 'nizi dağıttığınızda, için ek bir parametreye sahip olursunuz `-LicenseType` . VHD 'nizi Azure 'a yükledikten sonra, kullanarak bir VM oluşturun `New-AzVM` ve lisans türünü aşağıdaki gibi belirtin:
```powershell
New-AzVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM 'nizin lisans avantajını kullanarak emin olun
VM 'nizi PowerShell veya Kaynak Yöneticisi dağıtım yöntemiyle dağıttıktan sonra, lisans türünü `Get-AzVM` aşağıdaki gibi doğrulayın:
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı, doğru lisans türüyle Windows 10 için aşağıdaki örneğe benzer:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Bu çıktı, Azure hibrit kullanım avantajı lisanslaması olmadan dağıtılan aşağıdaki VM ile karşıttır. Örneğin, Azure galerisinden doğrudan dağıtılan bir VM.

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Azure AD 'ye katılma hakkında ek bilgiler
>[!NOTE]
>Azure, yerleşik yönetici hesabı olan tüm Windows sanal makinelerini AAD 'ye katmak için kullanılamaz. Örneğin, *ayarlar > hesap > erişim iş veya okul > + Bağlan* çalışmaz. Azure AD 'yi el ile birleştirmek için ikinci yönetici hesabı olarak oluşturmanız ve oturum açmanız gerekir. Azure AD 'yi bir sağlama paketi kullanarak da yapılandırabilirsiniz, daha fazla bilgi edinmek için *sonraki adımlar* bölümündeki bağlantıyı kullanın.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
- [Windows 10 için VDA yapılandırma](/windows/deployment/vda-subscription-activation) hakkında daha fazla bilgi edinin
- [Windows 10 Için çok kiracılı barındırma](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx) hakkında daha fazla bilgi edinin
