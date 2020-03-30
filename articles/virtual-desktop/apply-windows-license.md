---
title: Oturum barındırma sanal makineleri için Windows lisansı uygulama - Azure
description: Windows Sanal Masaüstü SANAL M'leri için Windows lisansının nasıl uygulanacağı açıklanır.
services: virtual-desktop
author: ChristianMontoya
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/14/2019
ms.author: chrimo
ms.openlocfilehash: 2543dd12e8a75a038a1fc04371b8c562ef696e25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254241"
---
# <a name="apply-windows-license-to-session-host-virtual-machines"></a>Oturum barındırma sanal makineleri için Windows lisansı uygulama

Windows Sanal Masaüstü iş yüklerini çalıştırmak için uygun şekilde lisans alan müşteriler, oturum barındırma sanal makinelerine Windows lisansı uygulayıp başka bir lisans için ödeme yapmadan çalıştırabilirsiniz. Daha fazla bilgi için [Windows Sanal Masaüstü fiyatlandırması'na](https://azure.microsoft.com/pricing/details/virtual-desktop/)bakın.

## <a name="ways-to-use-your-windows-virtual-desktop-license"></a>Windows Sanal Masaüstü lisansınızı kullanma yolları
Windows Sanal Masaüstü lisanslama, ana bilgisayar havuzunda oturum ana bilgisayar olarak kayıtlı ve kullanıcı bağlantısı alan herhangi bir Windows veya Windows Server sanal makineye lisans uygulamanıza olanak tanır. Bu lisans, dosya paylaşım sunucuları, etki alanı denetleyicileri ve benzeri olarak çalışan sanal makineler için geçerli değildir.

Windows Sanal Masaüstü lisansını kullanmanın birkaç yolu vardır:
- [Azure Marketi teklifini](./create-host-pools-azure-marketplace.md)kullanarak bir ana bilgisayar havuzu ve oturum barındırma sanal makineleri oluşturabilirsiniz. Bu şekilde oluşturulan sanal makineler otomatik olarak lisans uygulanır.
- [GitHub Azure Kaynak Yöneticisi şablonunu](./create-host-pools-arm-template.md)kullanarak bir ana bilgisayar havuzu ve oturum ana bilgisayar sanal makineleri oluşturabilirsiniz. Bu şekilde oluşturulan sanal makineler otomatik olarak lisans uygulanır.
- Varolan bir oturum ana bilgisayar sanal makineye lisans uygulayabilirsiniz. Bunu yapmak için, önce [powershell ile bir ana bilgisayar havuzu](./create-host-pools-powershell.md) ve ilişkili VM'ler oluşturmak için bir ana bilgisayar havuzu oluşturma yönergeleriizleyin, sonra lisans nasıl uygulanacağını öğrenmek için bu makaleye dönün.

## <a name="apply-a-windows-license-to-a-session-host-vm"></a>Oturum ana bilgisayar VM'ye Windows lisansı uygulama
En son [Azure PowerShell'i yüklediğinizden ve yapılandırdığınızdan](/powershell/azure/overview)emin olun. Windows lisansını uygulamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
$vm = Get-AzVM -ResourceGroup <resourceGroupName> -Name <vmName>
$vm.LicenseType = "Windows_Client"
Update-AzVM -ResourceGroupName <resourceGroupName> -VM $vm
```

## <a name="verify-your-session-host-vm-is-utilizing-the-licensing-benefit"></a>Oturum ana bilgisayarınızın VM'nin lisans avantajından yararlandığını doğrulayın
VM'nizi dağıttıktan sonra, lisans türünü doğrulayın şu cmdlet ot'u çalıştırın:
```powershell
Get-AzVM -ResourceGroupName <resourceGroupName> -Name <vmName>
```

Uygulamalı Windows lisansına sahip bir oturum ana bilgisayar VM size şöyle bir şey gösterecektir:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Uygulamalı Windows lisansı olmayan VM'ler size şöyle bir şey gösterir:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

Azure aboneliğinizde Windows lisansı uygulanan tüm oturum ana bilgisayar VM'lerinin listesini görmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
$vms = Get-AzVM
$vms | Where-Object {$_.LicenseType -like "Windows_Client"} | Select-Object ResourceGroupName, Name, LicenseType
```
