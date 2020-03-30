---
title: Windows Sanal Masaüstü kullanıcıları için özet akışı özelleştirme - Azure
description: PowerShell cmdlets ile Windows Sanal Masaüstü kullanıcıları için besleme özelleştirmek için nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 24a295d220cfaa7efe2fdc0d4eee53bb5c409708
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128089"
---
# <a name="customize-feed-for-windows-virtual-desktop-users"></a>Windows Sanal Masaüstü kullanıcıları için akışı özelleştirme

Uzaktan Uygulama ve uzak masaüstü kaynaklarının kullanıcılarınız için tanınabilir bir şekilde görünmesi için akışı özelleştirebilirsiniz.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="customize-the-display-name-for-a-remoteapp"></a>RemoteApp'ın ekran adını özelleştirme

Yayınlanan bir RemoteApp'ın ekran adını, dost adı ayarlayarak değiştirebilirsiniz. Varsayılan olarak, dostu ad RemoteApp programının adı ile aynıdır.

Bir uygulama grubu için yayınlanan RemoteApps listesini almak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![PowerShell cmdlet Get-RDSRemoteApp'ın Ad ve FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/get-rdsremoteapp.png)

Bir RemoteApp'a dostça bir ad atamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```
![PowerShell cmdlet Set-RDSRemoteApp'ın Adıyla ve Yeni FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Uzak Masaüstü için görüntü adını özelleştirme

Yayımlanmış bir uzak masaüstünün görüntü adını, kolay bir ad ayarlayarak değiştirebilirsiniz. PowerShell aracılığıyla bir ana bilgisayar havuzu ve masaüstü uygulama grubu el ile oluşturduysanız, varsayılan dostu ad "Oturum Masaüstü" olur. GitHub Azure Kaynak Yöneticisi şablonu veya Azure Marketi teklifi aracılığıyla bir ana bilgisayar havuzu ve masaüstü uygulama grubu oluşturduysanız, varsayılan ortak ad ana bilgisayar havuzu adı ile aynıdır.

Uzak masaüstü kaynağını almak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![PowerShell cmdlet Get-RDSRemoteApp'ın Ad ve FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/get-rdsremotedesktop.png)

Uzak masaüstü kaynağına uygun bir ad atamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```
![PowerShell cmdlet Set-RDSRemoteApp'ın Adıyla ve Yeni FriendlyName ile ekran görüntüsü vurgulanmıştır.](media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık kullanıcılar için özet akışını özelleştirdiğinize göre, bunu test etmek için bir Windows Sanal Masaüstü istemcisinde oturum açabilirsiniz. Bunu yapmak için, Windows Sanal Masaüstü Nasıl Yap'a Bağlan'a devam edin:
    
 * [Windows 10 veya Windows 7'den bağlanma](connect-windows-7-and-10.md)
 * [Web tarayıcısından bağlanma](connect-web.md) 
