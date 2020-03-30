---
title: Sorun giderme hizmeti bağlantısı Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü kiracı ortamında istemci bağlantıları ayarlarken sorunları nasıl giderebilirsiniz.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 57d5198cb54dc096fb09bb52d76539b1e4bbc1f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127454"
---
# <a name="windows-virtual-desktop-service-connections"></a>Windows Sanal Masaüstü hizmet bağlantıları

Windows Sanal Masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Bize geribildirim verebilir ve [Windows Sanal Masaüstü Teknik Topluluğu'nda](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)ürün ekibi ve diğer aktif topluluk üyeleri ile Windows Sanal Masaüstü Hizmeti tartışmak.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Kullanıcı bağlanır ancak hiçbir şey görüntülenmez (özet akışı yok)

Bir kullanıcı Uzak Masaüstü istemcilerini başlatabilir ve kimlik doğrulaması yapabilir, ancak kullanıcı web bulma akışında herhangi bir simge görmez.

Sorunları bildiren kullanıcının bu komut satırını kullanarak uygulama gruplarına atandığını doğrulayın:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Kullanıcının doğru kimlik bilgileriyle oturum açarak oturum açadığını doğrulayın.

Web istemcisi kullanılıyorsa, önbelleğe alınmış kimlik bilgileri sorunları olmadığını doğrulayın.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Windows 10 Enterprise çok oturumlu sanal makineler yanıt vermiyor

Sanal bir makine yanıt vermiyorsa ve RDP üzerinden erişemiyorsanız, ana bilgisayar durumunu kontrol ederek tanılama özelliğiyle sorun gidermeniz gerekir.

Ana bilgisayar durumunu kontrol etmek için şu cmdlet'i çalıştırın:

```powershell
Get-RdsSessionHost -TenantName $TenantName -HostPoolName $HostPool | ft SessionHostName, LastHeartBeat, AllowNewSession, Status
```

Ana bilgisayar durumu, `NoHeartBeat`VM'nin yanıt vermediği ve aracının Windows Sanal Masaüstü hizmetiyle iletişim kuramayacağı anlamına gelir.

```powershell
SessionHostName          LastHeartBeat     AllowNewSession    Status 
---------------          -------------     ---------------    ------ 
WVDHost1.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost2.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost3.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost4.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost5.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
```

NoHeartBeat durumunu düzeltmek için yapabileceğiniz birkaç şey vardır.

### <a name="update-fslogix"></a>FSLogix'i güncelleştir

FSLogix güncel değilse, özellikle frxdrvvt.sys sürümü 2.9.7205.27375 ise, bir kilitlenme neden olabilir. [FSLogix'i en son sürüme güncellediğinden](https://go.microsoft.com/fwlink/?linkid=2084562)emin olun.

### <a name="disable-bgtaskregistrationmaintenancetask"></a>Devre dışı kalan bgtaskregistrationMaintenanceTask

FSLogix'i güncelleştirmek işe yaramazsa, sorun bir BiSrv bileşeninin haftalık bakım görevi sırasında sistem kaynaklarını tüketiyor olması olabilir. BgTaskRegistrationMaintenanceTask'ı şu iki yöntemden biriyle devre dışı bırakarak bakım görevini geçici olarak devre dışı bırak:

- Başlat menüsüne gidin ve **Görev Zamanlayıcısı'nı**arayın. Görev **Zamanlayıcısı Kitaplığı** > **Microsoft** > **Windows** > **BrokerInfrastructure**gidin. **BgTaskRegistrationMaintenanceTask**adlı bir görev arayın. Bulduğunuzda, sağ tıklatın ve açılan menüden **Devre Dışı** bırak'ı seçin.
- Yönetici olarak komut satırı menüsünü açın ve aşağıdaki komutu çalıştırın:
    
    ```cmd
    schtasks /change /tn "\Microsoft\Windows\BrokerInfrastructure\BgTaskRegistrationMaintenanceTask" /disable 
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Windows Sanal Masaüstü ortamında kiracı ve ana bilgisayar havuzu oluştururken sorunları gidermek için [Bkz. Kiracı ve ana bilgisayar havuzu oluşturma.](troubleshoot-set-up-issues.md)
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Virtual Desktop ile PowerShell kullanırken sorunları gidermek için [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)
