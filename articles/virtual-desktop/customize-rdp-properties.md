---
title: PowerShell-Azure ile RDP özelliklerini özelleştirme
description: PowerShell cmdlet 'leriyle Windows sanal masaüstü için RDP özelliklerini özelleştirme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4a0f193437353bac1f5998b50b9d7b4d43bedefa
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128069"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Bir konak havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme

Çoklu izleyici deneyimi ve ses yeniden yönlendirme gibi bir konak havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirmek, kullanıcılarınız için gereksinimlerinize göre en iyi deneyimi sunmanızı sağlar. **Set-RdsHostPool** cmdlet 'inde **-customrdpproperty** parametresini kullanarak Windows sanal masaüstündeki RDP özelliklerini özelleştirebilirsiniz.

Desteklenen özelliklerin tam listesi ve bunların varsayılan değerleri için bkz. [desteklenen RDP dosyası ayarları](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) .

İlk olarak, henüz yapmadıysanız PowerShell oturumunuzda kullanmak üzere [Windows sanal masaüstü PowerShell modülünü indirip içeri aktarın](/powershell/windows-virtual-desktop/overview/) . Bundan sonra hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="default-rdp-properties"></a>Varsayılan RDP özellikleri

Varsayılan olarak, yayımlanan RDP dosyaları aşağıdaki özellikleri içerir:

|RDP özellikleri | Masaüstü bilgisayarlar | RemoteApps |
|---|---| --- |
| Çoklu monitör modu | Etkin | Yok |
| Sürücü yeniden yönlendirmeleri etkin | Sürücüler, pano, yazıcılar, COM bağlantı noktaları, USB cihazları ve akıllı kartlar| Sürücüler, pano ve Yazıcılar |
| Uzak ses modu | Yerel olarak oynat | Yerel olarak oynat |

Konak havuzu için tanımladığınız tüm özel özellikler, bu Varsayılanları geçersiz kılar.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Tek bir özel RDP özelliği ekleme veya düzenleme

Tek bir özel RDP özelliği eklemek veya düzenlemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```

![Name ve FriendlyName ile birlikte Get-RDSRemoteApp PowerShell cmdlet 'inin ekran görüntüsü.](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Birden çok özel RDP özelliği ekleme veya düzenleme

Birden çok özel RDP özelliği eklemek veya düzenlemek için, Özel RDP özelliklerini noktalı virgülle ayrılmış bir dize olarak sağlayarak aşağıdaki PowerShell cmdlet 'lerini çalıştırın:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```

![Name ve FriendlyName ile birlikte Get-RDSRemoteApp PowerShell cmdlet 'inin ekran görüntüsü.](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Tüm özel RDP özelliklerini Sıfırla

[Tek bir özel RDP özelliği ekleme veya düzenleme](#add-or-edit-a-single-custom-rdp-property)bölümündeki yönergeleri IZLEYEREK özel RDP özelliklerini varsayılan değerlerine sıfırlayabilirsiniz veya aşağıdaki PowerShell cmdlet 'ini çalıştırarak bir konak havuzu için tüm özel RDP özelliklerini sıfırlayabilirsiniz:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```

![Name ve FriendlyName ile birlikte Get-RDSRemoteApp PowerShell cmdlet 'inin ekran görüntüsü.](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık belirli bir konak havuzu için RDP özelliklerini özelleştirdiğinize göre, bir kullanıcı oturumunun parçası olarak test etmek için bir Windows sanal masaüstü istemcisinde oturum açabilirsiniz. Bu sonraki iki nasıl-TOS, tercih ettiğiniz istemciyi kullanarak bir oturuma nasıl bağlanacağınızı bildirir:

- [Windows Masaüstü istemcisiyle bağlantı](connect-windows-7-and-10.md)
- [Web istemcisiyle bağlantı](connect-web.md)
