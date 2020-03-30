---
title: Çok Kiracılı Barındırma Hakları ile Windows 10'u Azure'da dağıtma
description: Azure'a şirket içi lisanslar getirmek için Windows Yazılım Güvencesi avantajlarınızı nasıl en üst düzeye çıkarınızı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 1/24/2018
ms.author: xujing
ms.openlocfilehash: 9ff8cc64266375a2d439763b222870843136f67a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70101498"
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Çok Kiracılı Barındırma Hakları ile Windows 10'u Azure'da dağıtma 
Kullanıcı başına Windows 10 Enterprise E3/E5 veya kullanıcı başına Windows Sanal Masaüstü Erişimi (Kullanıcı Abonelik Lisansları veya Eklenti Kullanıcı Abonelik Lisansları) olan müşteriler için, Windows 10 için Multitenant Barındırma Hakları, Windows 10 Lisanslarınızı buluta getirmenize ve başka bir lisans için ödeme yapmadan Azure'da Windows 10 Sanal Makineler çalıştırmanıza olanak tanır. Daha fazla bilgi için lütfen [Windows 10 için Multitenant Hosting'e](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)bakın.

> [!NOTE]
> Bu makalede, Azure Marketi'nde Windows 10 Pro Desktop resimleriiçin lisans avantajı nı uygulamanız gösterilmektedir.
> - MSDN Abonelikleri için Azure Marketi'ndeki Windows 7, 8.1, 10 Kurumsal (x64) görseller için, [geliştirme/test senaryoları için lütfen Azure'daki Windows istemcisine](client-images.md) bakın
> - Windows Server lisanslama avantajları için lütfen [Windows Server görüntüleri için Azure Karma kullanım avantajlarına](hybrid-use-benefit-licensing.md)bakın.
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Azure Marketinden Windows 10 Görüntüsünü Dağıtma 
Powershell, CLI ve Azure Kaynak Yöneticisi şablon dağıtımları için Windows 10 resmi aşağıdaki publishername, offer, sku ile bulunabilir.

| İşletim Sistemi  |      PublisherName      |  Sunduğu | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS2-Pron  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS3-Pron  |

## <a name="uploading-windows-10-vhd-to-azure"></a>Windows 10 VHD'yi Azure'a Yükleme
Genelleştirilmiş bir Windows 10 VHD yüklüyorsanız, Windows 10'un varsayılan olarak yerleşik yönetici hesabı nın etkinleştirilemediğini lütfen unutmayın. Yerleşik yönetici hesabını etkinleştirmek için, Özel Komut Dosyası uzantısının bir parçası olarak aşağıdaki komutu ekleyin.

```powershell
Net user <username> /active:yes
```

Aşağıdaki powershell snippet yerleşik yönetici de dahil olmak üzere tüm yönetici hesaplarını etkin olarak işaretlemektir. Bu örnek, yerleşik yönetici kullanıcı adı bilinmiyorsa yararlıdır.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Daha fazla bilgi için: 
* [Azure'a VHD yükleme](upload-generalized-managed.md)
* [Azure'a yüklemek için Windows VHD nasıl hazırlanır?](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Windows 10'u Çok Kiracılı Barındırma Haklarıyla Dağıtma
En son [Azure PowerShell'i yüklediğinizden ve yapılandırdığınızdan](/powershell/azure/overview)emin olun. VHD'nizi hazırladıktan sonra, cmdlet'i kullanarak `Add-AzVhd` VHD'yi Azure Depolama hesabınıza yükleyin:

```powershell
Add-AzVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Azure Kaynak Yöneticisi Şablon Dağıtımı'nı kullanarak dağıtma** Kaynak Yöneticisi şablonlarınızda, ek `licenseType` bir parametre belirtilebilir. [Azure Kaynak Yöneticisi şablonlarını yazma](../../resource-group-authoring-templates.md)hakkında daha fazla bilgi edinebilirsiniz. VHD'niz Azure'a yüklendikten sonra, lisans türünü bilgi işlem sağlayıcısının bir parçası olarak eklemek ve şablonunuzu normal olarak dağıtmak için Kaynak Yöneticisi şablonunuzu edin:
```json
"properties": {
    "licenseType": "Windows_Client",
    "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
    }
```

**PowerShell üzerinden dağıt** Windows Server VM'nizi PowerShell üzerinden dağıtırken, `-LicenseType`'' için ek bir parametreniz var. VHD'niz Azure'a yüklendikten sonra, bir `New-AzVM` VM kullanarak bir VM oluşturur ve lisans türünü aşağıdaki gibi belirtirsiniz:
```powershell
New-AzVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM'nizin lisans avantajından yararlandığını doğrulayın
PowerShell veya Resource Manager dağıtım yöntemi yle VM'inizi dağıttıktan sonra, lisans türünü aşağıdaki gibi `Get-AzVM` doğrulayın:
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Çıktı, doğru lisans türüne sahip Windows 10 için aşağıdaki örneğe benzer:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Bu çıktı, doğrudan Azure Galerisi'nden dağıtılan bir VM gibi Azure Karma Kullanım Avantajı lisansı olmadan dağıtılan aşağıdaki VM ile tezat oluşturmaktadır:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Azure AD'ye katılma hakkında ek bilgiler
>[!NOTE]
>Azure, AAD'ye katılmak için kullanılamayacak yerleşik yönetici hesabıyla tüm Windows VM'lerini kullanır. Örneğin, *Ayarlar > Hesap > Access Work veya Okul > +Connect* çalışmaz. Azure AD'ye el ile katılmak için ikinci bir yönetici hesabı oluşturmanız ve oturum açmanız gerekir. Ayrıca bir sağlama paketi kullanarak Azure AD yapılandırabilirsiniz, daha fazla bilgi edinmek için *Sonraki Adımlar* bölümü bağlantısını kullanın.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
- [Windows 10 için VDA Yapılandırma](https://docs.microsoft.com/windows/deployment/vda-subscription-activation) hakkında daha fazla bilgi edinin
- [Windows 10 için Multitenant Hosting](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx) hakkında daha fazla bilgi edinin


