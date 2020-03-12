---
title: Hizmet bağlantısı Windows sanal masaüstü-Azure sorunlarını giderme
description: Windows sanal masaüstü kiracı ortamında istemci bağlantılarını ayarlarken oluşan sorunları çözme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 57d5198cb54dc096fb09bb52d76539b1e4bbc1f2
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127454"
---
# <a name="windows-virtual-desktop-service-connections"></a>Windows sanal masaüstü hizmeti bağlantıları

Windows sanal masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows sanal masaüstü [Teknik topluluğu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)'nda ürün ekibi ve diğer etkin topluluk üyeleriyle geri bildirimde bulunun ve Windows Sanal Masaüstü hizmetini tartışabilirsiniz.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Kullanıcı bağlanıyor, ancak hiçbir şey görüntülenmiyor (akış yok)

Kullanıcı, uzak masaüstü istemcileri başlatabilir ve kimlik doğrulaması yapabilir, ancak kullanıcı Web bulma akışında herhangi bir simge görmez.

Bu komut satırını kullanarak, sorunları raporlayan kullanıcının uygulama gruplarına atandığını doğrulayın:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Kullanıcının doğru kimlik bilgileriyle oturum açmasını onaylayın.

Web istemcisi kullanılıyorsa, önbelleğe alınmış kimlik bilgileri sorunları olduğunu doğrulayın.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Windows 10 Enterprise çoklu oturum sanal makineleri yanıt vermez

Bir sanal makine yanıt vermiyor ve RDP üzerinden erişemiyorsanız, konak durumunu denetleyerek tanılama özelliğiyle sorun gidermeniz gerekir.

Konak durumunu denetlemek için şu cmdlet 'i çalıştırın:

```powershell
Get-RdsSessionHost -TenantName $TenantName -HostPoolName $HostPool | ft SessionHostName, LastHeartBeat, AllowNewSession, Status
```

Ana bilgisayar durumu `NoHeartBeat`ise, VM yanıt vermiyor ve aracı Windows sanal masaüstü hizmetiyle iletişim kuramaz.

```powershell
SessionHostName          LastHeartBeat     AllowNewSession    Status 
---------------          -------------     ---------------    ------ 
WVDHost1.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost2.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost3.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost4.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost5.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
```

Nosinyal durumunu onarmak için yapabileceğiniz birkaç nokta vardır.

### <a name="update-fslogix"></a>FSLogix 'i Güncelleştir

FSLogix 'niz güncel değilse, özellikle frxdrvvt. sys ' nin 2.9.7205.27375 sürümü varsa, kilitlenme olabilir. [FSLogix ' i en son sürüme güncelleştirdiğinizden](https://go.microsoft.com/fwlink/?linkid=2084562)emin olun.

### <a name="disable-bgtaskregistrationmaintenancetask"></a>BgTaskRegistrationMaintenanceTask devre dışı bırak

FSLogix ' i güncelleştirmek işe yaramazsa, bir haftalık bakım görevi sırasında bir Bırv bileşeninin sistem kaynaklarını tüketme sorunu olabilir. Aşağıdaki iki yöntemden biriyle BgTaskRegistrationMaintenanceTask 'yi devre dışı bırakarak bakım görevini geçici olarak devre dışı bırakın:

- Başlat menüsüne gidin ve **Görev Zamanlayıcı**arayın. **Microsoft** > **Windows** > **brokerınfrastructure** > **Görev Zamanlayıcı Kitaplığı** 'na gidin. **BgTaskRegistrationMaintenanceTask**adlı bir görev arayın. Bunu bulduğunuzda, sağ tıklayın ve açılan menüden **devre dışı bırak** ' ı seçin.
- Yönetici olarak bir komut satırı menüsünü açın ve şu komutu çalıştırın:
    
    ```cmd
    schtasks /change /tn "\Microsoft\Windows\BrokerInfrastructure\BgTaskRegistrationMaintenanceTask" /disable 
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme izlemelerinin sorunlarını giderme hakkında genel bilgi için bkz. [sorun giderme genel bakış, geri bildirim ve destek](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında kiracı ve konak havuzu oluştururken oluşan sorunları gidermek için bkz. [kiracı ve konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Sorun giderme öğreticisini öğrenmek için bkz. [öğretici: Kaynak Yöneticisi şablonu dağıtımlarının sorunlarını giderme](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
