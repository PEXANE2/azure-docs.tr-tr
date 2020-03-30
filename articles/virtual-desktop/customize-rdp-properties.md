---
title: PowerShell ile RDP Özelliklerini Özelleştirin - Azure
description: PowerShell cmdlets ile Windows Sanal Masaüstü için RDP Özellikleri özelleştirmek için nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4a0f193437353bac1f5998b50b9d7b4d43bedefa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128069"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Ana bilgisayar havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme

Çok monitör deneyimi ve ses yeniden yönlendirme gibi bir ana bilgisayar havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirmek, kullanıcılarınız için gereksinimlerine göre en iyi deneyimi sunmanıza olanak tanır. **Set-RdsHostPool** cmdlet'teki **-CustomRdpProperty** parametresini kullanarak Windows Sanal Masaüstü'ndeki RDP özelliklerini özelleştirebilirsiniz.

Desteklenen özelliklerin ve bunların varsayılan değerlerinin tam listesi için [desteklenen RDP dosya ayarlarına](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) bakın.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="default-rdp-properties"></a>Varsayılan RDP özellikleri

Varsayılan olarak, yayınlanan RDP dosyaları aşağıdaki özellikleri içerir:

|RDP özellikleri | Masaüstü Bilgisayarlar | RemoteApps |
|---|---| --- |
| Çoklu monitör modu | Etkin | Yok |
| Sürücü yeniden yönlendirmeleri etkin | Sürücüler, pano, yazıcılar, COM bağlantı noktaları, USB aygıtları ve akıllı kartlar| Sürücüler, pano ve yazıcılar |
| Uzak ses modu | Yerel olarak oynayın | Yerel olarak oynayın |

Ana bilgisayar havuzu için tanımladığınız tüm özel özellikler bu varsayılanları geçersiz kılar.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Tek bir özel RDP özelliği ekleme veya bu özelliği ni de

Tek bir özel RDP özelliği eklemek veya yönetmek için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```

![PowerShell cmdlet Get-RDSRemoteApp'ın Ad ve FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Birden çok özel RDP özelliği ekleme veya bu özellikleri ni da ekle

Birden çok özel RDP özelliği eklemek veya düzenlemek için, özel RDP özelliklerini yarı sütunlu ayrılmış dize olarak sağlayarak aşağıdaki PowerShell cmdlets'i çalıştırın:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```

![PowerShell cmdlet Get-RDSRemoteApp'ın Ad ve FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Tüm özel RDP özelliklerini sıfırlama

Tek bir özel RDP özelliği ekle'deki yönergeleri izleyerek [veya tek bir özel RDP özelliğini düzenleyerek](#add-or-edit-a-single-custom-rdp-property)tek tek özel RDP özelliklerini varsayılan değerlerine sıfırlayabilir veya aşağıdaki PowerShell cmdlet'i çalıştırarak ana bilgisayar havuzu için tüm özel RDP özelliklerini sıfırlayabilirsiniz:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```

![PowerShell cmdlet Get-RDSRemoteApp'ın Ad ve FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık belirli bir ana bilgisayar havuzu için RDP özelliklerini özelleştirdiğinize göre, kullanıcı oturumunun bir parçası olarak test etmek için Windows Sanal Masaüstü istemcisinde oturum açabilirsiniz. Bu sonraki iki Nasıl Yapılacaklar, seçtiğiniz istemciyi kullanarak bir oturuma nasıl bağlanacağınızı size söyleyecektir:

- [Windows Masaüstü istemcisine bağlanma](connect-windows-7-and-10.md)
- [Web istemcisiyle bağlanma](connect-web.md)
