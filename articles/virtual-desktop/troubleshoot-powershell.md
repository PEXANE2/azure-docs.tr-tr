---
title: Windows Sanal Masaüstü PowerShell - Azure
description: Windows Sanal Masaüstü kiracı ortamını ayarlarken PowerShell ile ilgili sorunları giderme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3fb5436c2b5c30c5336385792d0597bdcea2b538
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127476"
---
# <a name="windows-virtual-desktop-powershell"></a>Windows Sanal Masaüstü PowerShell

Windows Virtual Desktop ile PowerShell kullanırken hataları ve sorunları gidermek için bu makaleyi kullanın. Uzak Masaüstü Hizmetleri PowerShell hakkında daha fazla bilgi için [Windows Sanal Masaüstü Powershell'e](/powershell/module/windowsvirtualdesktop/)bakın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows Sanal Masaüstü Hizmeti'ni ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows Sanal Masaüstü Teknik Topluluğu'nu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>Windows Sanal Masaüstü kurulumu sırasında kullanılan PowerShell komutları

Bu bölümde, Windows Sanal Masaüstü'nü kurarken genellikle kullanılan PowerShell komutları listelenir ve bunları kullanırken oluşabilecek sorunları çözmenin yollarını sağlar.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-is-already-assigned-to-a-remoteapp-app-group-in-the-specified-host-pool"></a>Hata: Add-RdsAppGroupUser komutu -- Belirtilen UserPrincipalName, belirtilen Ana Bilgisayar Havuzundaki RemoteApp uygulama grubuna zaten atanır

```Powershell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName 'Desktop Application Group' -UserPrincipalName <UserName>
```

**Sebep:** Kullanılan kullanıcı adı zaten farklı türde bir uygulama grubuna atanmıştır. Kullanıcılar, aynı oturum ana bilgisayar havuzu altında hem uzak bir masaüstü hem de uzak uygulama grubuna atanamaz.

**Düzeltme:** Kullanıcının hem uzak uygulamalara hem de uzak masaüstüne ihtiyacı varsa, farklı ana bilgisayar havuzları oluşturun veya kullanıcıya uzak masaüstüne erişim izni vererek oturum ana bilgisayar VM'deki herhangi bir uygulamanın kullanımına izin verir.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-doesnt-exist-in-the-azure-active-directory-associated-with-the-remote-desktop-tenant"></a>Hata: Add-RdsAppGroupUser komutu -- Belirtilen UserPrincipalName Uzak Masaüstü kiracıile ilişkili Azure Etkin Dizini'nde yok

```PowerShell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName "Desktop Application Group" -UserPrincipalName <UserPrincipalName>
```

**Sebep:** -UserPrincipalName tarafından belirtilen kullanıcı, Windows Sanal Masaüstü kiracısına bağlı Azure Etkin Dizini'nde bulunamaz.

**Düzeltme:** Aşağıdaki listedeki öğeleri onaylayın.

- Kullanıcı Azure Etkin Dizin'e senkronize edilir.
- Kullanıcı, işletmeye tüketici (B2C) veya işletmelerarası (B2B) ticaretle bağlı değildir.
- Windows Sanal Masaüstü kiracısı Azure Etkin Dizini'ni düzeltmeye bağlıdır.

### <a name="error-get-rdsdiagnosticactivities----user-isnt-authorized-to-query-the-management-service"></a>Hata: Get-RdsDiagnosticActivities -- Kullanıcı yönetim hizmetini sorgulama yetkisine izin vermez

```PowerShell
Get-RdsDiagnosticActivities -ActivityId <ActivityId>
```

**Neden:** -TenantName parametresi

**Düzeltme:** -TenantName Name \<> ile Sorun Get-RdsDiagnosticActivities.

### <a name="error-get-rdsdiagnosticactivities----the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Get-RdsDiagnosticActivities -- kullanıcı yönetim hizmetini sorgulama yetkisine izin vermez

```PowerShell
Get-RdsDiagnosticActivities -Deployment -username <username>
```

**Sebep:** -Dağıtım anahtarını kullanma.

**Düzeltme:** -Dağıtım anahtarı yalnızca dağıtım yöneticileri tarafından kullanılabilir. Bu yöneticiler genellikle Uzak Masaüstü Hizmetleri/Windows Sanal Masaüstü ekibinin üyeleridir. -Dağıtım anahtarını -TenantName \<TenantName> ile değiştirin.

### <a name="error-new-rdsroleassignment----the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Yeni-RdsRoleAssignment -- kullanıcı yönetim hizmetini sorgulama yetkisine izin vermiyor

**Neden 1:** Kullanılan hesabın kiracıda Uzak Masaüstü Hizmetleri Sahibi izinleri yoktur.

**Düzeltme 1:** Uzak Masaüstü Hizmetleri sahibi izinlerine sahip bir kullanıcının rol atamasını yürütmesi gerekir.

**Neden 2:** Kullanılan hesap Uzak Masaüstü Hizmetleri sahibinin izinlerine sahiptir, ancak kiracının Azure Etkin Dizininin bir parçası değildir veya kullanıcının bulunduğu Azure Etkin Dizini sorgulama izni yoktur.

**Düzeltme 2:** Active Directory izinlerine sahip bir kullanıcının rol atamasını yürütmesi gerekir.

>[!Note]
>Yeni RdsRoleAssignment, Azure Etkin Dizini'nde (AD) bulunmayan bir kullanıcıya izin veremez.

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Windows Sanal Masaüstü ortamında kiracı ve ana bilgisayar havuzu oluştururken sorunları gidermek için [Bkz. Kiracı ve ana bilgisayar havuzu oluşturma.](troubleshoot-set-up-issues.md)
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Sanal Masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için [Windows Sanal Masaüstü hizmet bağlantılarına](troubleshoot-service-connection.md)bakın.
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için Bkz. [Uzak Masaüstü istemcisi sorun giderme](troubleshoot-client.md)
- Hizmet hakkında daha fazla bilgi edinmek için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)
- Denetim eylemleri hakkında bilgi edinmek için [Kaynak Yöneticisi ile Denetim işlemlerine](../azure-resource-manager/management/view-activity-logs.md)bakın.
- Dağıtım sırasında hataları belirlemek için eylemler hakkında bilgi edinmek için [bkz.](../azure-resource-manager/templates/deployment-history.md)
