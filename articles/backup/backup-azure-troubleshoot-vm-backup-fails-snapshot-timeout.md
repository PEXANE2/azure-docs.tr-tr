---
title: Sorun Giderme Aracısı ve uzantı sorunları
description: Aracı, uzantı ve disklerle ilgili Azure Yedekleme hatalarının belirtileri, nedenleri ve çözümleri.
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.service: backup
ms.openlocfilehash: 4583c02b52ab6b3a4e5056a47db096d4e34399ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248027"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Yedekleme hatası sorun giderme: Aracı veya uzantıyla ilgili sorunlar

Bu makalede, VM aracısı ve uzantısı ile iletişim ile ilgili Azure Yedekleme hatalarını çözmenize yardımcı olabilecek sorun giderme adımları sağlanmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="usererrorguestagentstatusunavailable---vm-agent-unable-to-communicate-with-azure-backup"></a><a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable - VM aracısı Azure Backup ile iletişim kuramıyor

**Hata kodu**: UserErrorGuestAgentStatusKullanılamaz <br>
**Hata iletisi**: VM Aracısı Azure Yedekleme ile iletişim kuramıyor<br>

Azure VM aracısı durdurulabilir, güncelliğini yitirebilir, tutarsız bir durumda veya yüklenmemiş olabilir. Bu durumlar, Azure Yedekleme hizmetinin anlık görüntüleri tetiklemesini engeller.

- VM **durumunun** **ve** **Aracı durumunun** **hazır**olmasını sağlamak > **Azure portal>VM > Ayarları > Özellikler bölmesi** açın. VM aracısı durdurulursa veya tutarsız bir durumdaysa, aracıyı yeniden başlatın<br>
  - Windows VM'ler için Konuk Aracıyı yeniden başlatmak için aşağıdaki [adımları](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms) izleyin.<br>
  - Linux VM'leri için Konuk Aracıyı yeniden başlatmak için aşağıdaki [adımları](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms) izleyin.
- **VM > Ayarları > Uzantıları > Azure portalı açın** > Tüm uzantıların başarılı bir durum **sağlamada** olduğundan emin olun. Değilse, sorunu gidermek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state) izleyin.

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError - Anlık görüntü durumu için VM aracısı ile iletişim kurulamadı

**Hata kodu**: GuestAgentSnapshotTaskStatusError<br>
**Hata iletisi**: Anlık görüntü durumu için VM aracısı ile iletişim kuramadı <br>

Azure Yedekleme hizmeti için bir VM kaydedip zamanladıktan sonra Yedekleme, zamanında anlık görüntü almak için VM yedekleme uzantısı ile iletişim kurarak işe başlar. Aşağıdaki koşullardan herhangi biri anlık görüntünün tetiklemesini engelleyebilir. Anlık görüntü tetiklenmezse, yedek bir hata oluşabilir. Listelenen sırada aşağıdaki sorun giderme adımlarını tamamlayın ve ardından işleminizi yeniden deneyin:  

**Neden 1: [Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  

**Neden 2: [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

**Neden 3: [Anlık görüntü durumu alınamıyor veya anlık görüntü alınamıyor](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**

**Neden 4: [VM-Agent yapılandırma seçenekleri ayarlanmaz (Linux VM'leri için)](#vm-agent-configuration-options-are-not-set-for-linux-vms)**

## <a name="usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state"></a>UserErrorVmProvisioningStateFailed - VM başarısız sağlama durumunda

**Hata kodu**: UserErrorVmProvisioningStateFailed<br>
**Hata iletisi**: VM başarısız sağlama durumunda<br>

Bu hata, uzantı hatalarından biri VM'yi başarısız duruma soktuğunda oluşur.<br>**Azure portalı> VM > Ayarları > Uzantıları > Uzantıları durumunu açın** ve tüm uzantıların başarılı durum **sağlamada** olup olmadığını denetleyin. Daha fazla bilgi edinmek için Bkz. [Provisioning durumları.](https://docs.microsoft.com/azure/virtual-machines/windows/states-lifecycle#provisioning-states)

- VMSnapshot uzantısı başarısız durumdaysa, başarısız uzantıya sağ tıklayın ve kaldırın. İsteğe bağlı yedeklemeyi tetikle. Bu eylem uzantıları yeniden yükler ve yedekleme işini çalıştırın.  <br>
- Başka bir uzantı başarısız durumdaysa, yedeklemeyi etkileyebilir. Bu uzantı sorunlarının çözüldüğünden emin olun ve yedekleme işlemini yeniden deneyin.
- VM sağlama durumu güncelleniyorsa, yedeklemeyi etkileyebilir. Sağlıklı olduğundan emin olun ve yedekleme işlemini yeniden deneyin.

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached - Geri Yükleme Noktası koleksiyonu maksimum sınırına ulaşıldı

**Hata kodu**: UserErrorRpCollectionLimitReached <br>
**Hata iletisi**: Geri Yükleme Noktası toplama max sınırına ulaşıldı. <br>

- Kurtarma noktası kaynak grubunda kurtarma noktalarının otomatik olarak temizlenmesini engelleyen bir kilit varsa, bu sorun ortaya çıkabilir.
- Günde birden çok yedekleme tetiklenirse, bu sorun da olabilir. Şu anda, anlık geri yükleme noktaları yapılandırılmış anlık görüntü tutma başına 1-5 gün boyunca tutulduğundan ve herhangi bir zamanda bir VM ile yalnızca 18 anlık RPs ilişkilendirilebildiği için, günde yalnızca bir yedekleme öneririz. <br>
- Bir VM için geri yükleme noktası koleksiyonları ve kaynak grupları arasında geri yükleme noktalarının sayısı 18'i geçemez. Yeni bir geri yükleme noktası oluşturmak için varolan geri yükleme noktalarını silin.

Önerilen Eylem:<br>
Bu sorunu gidermek için VM'nin kaynak grubundaki kilidi kaldırın ve temizlemeyi tetiklemek için işlemi yeniden deneyin.
> [!NOTE]
> Yedekleme hizmeti, geri yükleme noktası koleksiyonunu depolamak için VM'nin kaynak grubundan ayrı bir kaynak grubu oluşturur. Müşterilerin Yedekleme hizmeti tarafından kullanılmak üzere oluşturulan kaynak grubunu kilitlememesi önerilir. Yedekleme hizmeti tarafından oluşturulan kaynak grubunun adlandırma`<Geo>`biçimi: AzureBackupRG_ _`<number>` Örn: AzureBackupRG_northeurope_1

**Adım 1: [Geri yükleme noktası kaynak grubundan kilidi kaldırma](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Adım 2: [Geri yükleme noktası koleksiyonunu temizleme](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured - Yedekleme şifreli VM yedekleme için anahtar kasasına yeterli izinleri yok

**Hata kodu**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Hata iletisi**: Yedekleme, şifreli VM'lerin yedeklemesi için anahtar kasasına yeterli izine sahip değildir. <br>

Bir yedekleme işleminin şifreli VM'lerde başarılı olabilmesi için anahtar kasasına erişmek için izinlere sahip olması gerekir. İzinler [Azure portalı](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) veya [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection)üzerinden ayarlanabilir.

## <a name="extensionsnapshotfailednonetwork---snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a><a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork - Sanal makinede ağ bağlantısı olmaması nedeniyle Anlık Görüntü işlemi başarısız oldu

**Hata kodu**: ExtensionSnapshotFailedNoNetwork<br>
**Hata iletisi**: Sanal makinede ağ bağlantısı olmadığı için anlık görüntü işlemi başarısız oldu<br>

Azure Yedekleme hizmeti için bir VM kaydedip zamanladıktan sonra Yedekleme, zamanında anlık görüntü almak için VM yedekleme uzantısı ile iletişim kurarak işe başlar. Aşağıdaki koşullardan herhangi biri anlık görüntünün tetiklemesini engelleyebilir. Anlık görüntü tetiklenmezse, yedek bir hata oluşabilir. Aşağıdaki sorun giderme adımını tamamlayın ve işleminizi yeniden deneyin:

**[Anlık görüntü durumu alınamıyor veya anlık görüntü alınamıyor](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  

## <a name="extensionoperationfailedformanageddisks---vmsnapshot-extension-operation-failed"></a><a name="ExtensionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtensionOperationFailedForManagedDisks - VMSnapshot uzantı işlemi başarısız oldu

**Hata kodu**: ExtensionOperationFailedForManagedDisks <br>
**Hata iletisi**: VMSnapshot uzantı işlemi başarısız oldu<br>

Azure Yedekleme hizmeti için bir VM kaydedip zamanladıktan sonra Yedekleme, zamanında anlık görüntü almak için VM yedekleme uzantısı ile iletişim kurarak işe başlar. Aşağıdaki koşullardan herhangi biri anlık görüntünün tetiklemesini engelleyebilir. Anlık görüntü tetiklenmezse, yedek bir hata oluşabilir. Listelenen sırada aşağıdaki sorun giderme adımlarını tamamlayın ve ardından işleminizi yeniden deneyin:  
**Neden 1: [Anlık görüntü durumu alınamıyor veya anlık görüntü alınamıyor](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Neden 2: [Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Neden 3: [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed/BackUpOperationFailedV2 - Yedekleme başarısız, iç hata

**Hata kodu**: BackUpOperationFailed / BackUpOperationFailedV2 <br>
**Hata iletisi**: Yedekleme dahili bir hata ile başarısız oldu - Lütfen işlemi birkaç dakika içinde yeniden deneyin <br>

Azure Yedekleme hizmeti için bir VM kaydedip zamanladıktan sonra Yedekleme, zamanında anlık görüntü almak için VM yedekleme uzantısı ile iletişim kurarak işi başlatır. Aşağıdaki koşullardan herhangi biri anlık görüntünün tetiklemesini engelleyebilir. Anlık görüntü tetiklenmezse, yedek bir hata oluşabilir. Listelenen sırada aşağıdaki sorun giderme adımlarını tamamlayın ve ardından işleminizi yeniden deneyin:  
**Neden 1: [VM yüklü aracı, ancak yanıt vermiyor (Windows VM'ler için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Neden 2: [VM yüklü aracı (Linux VM'ler için) güncel değil](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Neden 3: [Anlık görüntü durumu alınamıyor veya anlık görüntü alınamıyor](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Neden 4: [Yedekleme hizmeti, kaynak grubu kilidi nedeniyle eski geri yükleme noktalarını silme iznine sahip değildir](#remove_lock_from_the_recovery_point_resource_group)**<br>

## <a name="usererrorunsupporteddisksize---the-configured-disk-sizes-is-currently-not-supported-by-azure-backup"></a>UserErrorUnsupportedDiskSize - Yapılandırılmış disk boyutu(lar) şu anda Azure Yedekleme tarafından desteklenmiyor

**Hata kodu**: UserErrorUnsupportedDiskSize <br>
**Hata iletisi**: Yapılandırılan disk boyutu(lar) şu anda Azure Yedekleme tarafından desteklenmiyor. <br>

32 TB'dan büyük bir disk boyutuna sahip bir VM'yi yedeklediğinizde yedekleme işleminiz başarısız olabilir. Ayrıca, 4 TB boyutundan büyük şifreli disklerin yedeklemesi şu anda desteklenmez. Disk boyutunun(lar) disk(ler'i bölerek desteklenen sınırdan küçük veya eşit olduğundan emin olun).

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>UserErrorBackupOperationInProgress - Başka bir yedekleme işlemi şu anda devam ederken yedekleme başlatılamıyor

**Hata kodu**: UserErrorBackupOperationInProgress <br>
**Hata iletisi**: Başka bir yedekleme işlemi şu anda devam ederken yedekleme başlatılamadı<br>

Son yedekleme işiniz, devam eden bir yedekleme işi olduğu için başarısız oldu. Geçerli iş bitene kadar yeni bir yedekleme işine başalamazsınız. Başka bir yedekleme işlemini tetiklemeden veya planlamadan önce şu anda devam eden yedekleme işleminin tamamlandığından emin olun. Yedekleme işleri durumunu denetlemek için aşağıdaki adımları yapın:

1. Azure portalında oturum açın, **Tüm hizmetler'i**tıklatın. Kurtarma Hizmetleri yazın ve **Kurtarma Hizmetleri kasaları**’na tıklayın. Kurtarma hizmetleri kasalarının listesi görünür.
2. Kurtarma hizmetleri kasaları listesinden, yedeklemenin yapılandırıldığı bir kasa seçin.
3. Kasa panosu menüsünde, **Yedekleme İşleri'ni** tıklatın, tüm yedekleme işlerini görüntüler.
   - Yedekleme işi devam ediyorsa, yedekleme işini tamamlamasını veya iptal etmesini bekleyin.
     - Yedekleme işini iptal etmek için yedekleme işini sağ tıklatın ve **İptal'i** tıklatın veya [PowerShell'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0)kullanın.
   - Yedeklemeyi farklı bir kasada yeniden yapılandırıldıysanız, eski kasada çalışan yedek işler olmadığından emin olun. Varsa, yedekleme işini iptal edin.
     - Yedekleme işini iptal etmek için, yedekleme işine sağ tıklayıp **İptal**'e tıklayın veya [PowerShell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0) kullanın
4. Yedekleme işlemini yeniden deneyin.

Zamanlanan yedekleme işlemi daha uzun sürüyorsa, bir sonraki yedekleme yapılandırmasıyla çakışıyorsa, [en iyi uygulamalar,](backup-azure-vms-introduction.md#best-practices) [Yedekleme Performansı](backup-azure-vms-introduction.md#backup-performance)ve Geri [Yükleme değerlendirmesini](backup-azure-vms-introduction.md#backup-and-restore-considerations)gözden geçirin.

## <a name="usererrorcrpreportedusererror---backup-failed-due-to-an-error-for-details-see-job-error-message-details"></a>UserErrorCrpReportedUserError - Yedekleme bir hata nedeniyle başarısız oldu. Ayrıntılar için İş Hatası İleti ayrıntılarına bakın

**Hata kodu**: UserErrorCrpReportedUserError <br>
**Hata iletisi**: Yedekleme bir hata nedeniyle başarısız oldu. Ayrıntılar için İş Hatası İleti ayrıntılarına bakın.

Bu hata IaaS VM'den bildirilir. Sorunun temel nedenini belirlemek için Kurtarma Hizmetleri kasa ayarlarına gidin. **İzleme** bölümünde, durumu filtrelemek ve görüntülemek için **Yedekleme işleri'ni** seçin. Altta yatan hata iletisi ayrıntılarını gözden geçirmek için **Hatalar'ı** tıklatın. Hata ayrıntıları sayfasındaki önerilere göre daha fazla işlem alın.

## <a name="causes-and-solutions"></a>Nedenleri ve çözümleri

### <a name="the-agent-is-installed-in-the-vm-but-its-unresponsive-for-windows-vms"></a><a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Aracı VM'de yüklü, ancak yanıt vermiyor (Windows VM'ler için)

#### <a name="solution"></a>Çözüm

VM aracısı bozulmuş veya hizmet durdurulmuş olabilir. VM aracısını yeniden yüklemek en son sürümü niçin elde edinmesine yardımcı olur. Ayrıca, hizmetle iletişimin yeniden başlatılmasına yardımcı olur.

1. Windows Azure Konuk Aracıhizmetinin VM hizmetlerinde (services.msc) çalışıp çalışmadığını belirleyin. Windows Azure Konuk Aracı hizmetini yeniden başlatmayı ve yedeklemeyi başlatmayı deneyin.
2. Windows Azure Konuk Aracısı hizmeti hizmetlerde görünmüyorsa, Denetim Masası'nda, Windows Azure Konuk Aracısı hizmetinin yüklü olup olmadığını belirlemek için **Programlar ve Özellikler'e** gidin.
3. **Programlar ve Özellikler'de**Windows Azure Konuk Aracısı görünüyorsa, Windows Azure Konuk Aracısını kaldırın.
4. [Ajan MSI'ın en son sürümünü indirin](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)ve kurun. Yüklemeyi tamamlamak için Yönetici haklarına sahip olmalısınız.
5. Windows Azure Konuk Aracı hizmetlerinin hizmetlerde göründüğünü doğrulayın.
6. İsteğe bağlı yedekleme çalıştırın:
   - Portalda, **Şimdi Yedekle'yi**seçin.

Ayrıca, [Microsoft .NET 4.5'in](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) VM'de yüklü olduğunu doğrulayın. .NET 4.5, VM aracısının hizmetle iletişim kurması için gereklidir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM yüklü aracı (Linux VM'ler için) güncel değil

#### <a name="solution"></a>Çözüm

Linux VM'leri için aracıyla veya uzantılarla ilgili hataların çoğu, eski bir VM aracıyı etkileyen sorunlardan kaynaklanır. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. [Linux VM aracısını güncellemek](../virtual-machines/linux/update-agent.md)için yönergeleri izleyin.

   > [!NOTE]
   > Aracıyı yalnızca bir dağıtım deposu aracılığıyla güncelleştirmenizi *şiddetle öneririz.* Aracı kodunu doğrudan GitHub'dan indirmenizi ve güncellemenizi önermiyoruz. Dağıtımınız için en son aracı kullanılamıyorsa, nasıl yüklenire ilişkin talimatlar için dağıtım desteğine başvurun. En son aracıyı kontrol etmek için GitHub deposundaki [Windows Azure Linux aracısı](https://github.com/Azure/WALinuxAgent/releases) sayfasına gidin.

2. Azure aracısının aşağıdaki komutu çalıştırarak VM'de çalıştığını sağlayın:`ps -e`

   İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

   - Ubuntu için:`service walinuxagent start`
   - Diğer dağıtımlar için:`service waagent start`

3. [Otomatik yeniden başlatma aracısını yapılandırın.](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash)
4. Yeni bir test yedeklemesi çalıştırın. Hata devam ederse, VM'den aşağıdaki günlükleri toplayın:

   - /var/lib/waagent/*.xml
   - /var/log/waagent.log
   - /var/log/azure/*

Waagent için ayrıntılı günlüğe kaydetmeniz gerekiyorsa, aşağıdaki adımları izleyin:

1. /etc/waagent.conf dosyasında aşağıdaki satırı bulun: **Verbose günlüğe kaydetmeyi etkinleştirin (y|n)**
2. **Logs.Verbose** değerini *n'den* *y'ye*değiştirin.
3. Değişikliği kaydedin ve bu bölümde daha önce açıklanan adımları tamamlayarak waagent'ı yeniden başlatın.

### <a name="vm-agent-configuration-options-are-not-set-for-linux-vms"></a>VM-Agent yapılandırma seçenekleri ayarlanmaz (Linux VM'ler için)

Bir yapılandırma dosyası (/etc/waagent.conf) waagent eylemlerini denetler. Yapılandırma Dosya Seçenekleri **Uzantıları.Enable** ve **Provisioning.Agent** Yedekleme çalışması için **y** olarak ayarlanmalıdır.
VM-Agent Yapılandırma Dosya Seçeneklerinin tam listesi için bkz.<https://github.com/Azure/WALinuxAgent#configuration-file-options>

### <a name="the-snapshot-status-cant-be-retrieved-or-a-snapshot-cant-be-taken"></a><a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamıyor veya anlık görüntü alınamıyor

VM yedeklemesi, temel depolama hesabına anlık görüntü komutu verilmesine dayanır. Yedekleme, depolama hesabına erişimi olmadığı veya anlık görüntü görevinin yürütülmesi geciktiğinden başarısız olabilir.

#### <a name="solution"></a>Çözüm

Aşağıdaki koşullar anlık görüntü görevinin başarısız olması için neden olabilir:

| Nedeni | Çözüm |
| --- | --- |
| VM Uzak Masaüstü Protokolü 'nde (RDP) kapatıldığından VM durumu yanlış bildirilir. | RDP'de VM'yi kapatırsanız, VM durumunun doğru olup olmadığını belirlemek için portalı denetleyin. Doğru değilse, VM panosundaki **Kapatma** seçeneğini kullanarak portaldaki VM'yi kapatın. |
| VM, ANA bilgisayar veya kumaş adresini DHCP'den alamaz. | IaaS VM yedeklemesinin çalışması için konuk içinde DHCP etkinleştirilmelidir. VM, DHCP yanıt 245'ten ana bilgisayar veya kumaş adresini alamıyorsa, herhangi bir uzantıyı karşıdan yükleyemez veya çalıştıramaz. Statik bir özel IP'ye ihtiyacınız varsa, bunu **Azure portalı** veya **PowerShell** üzerinden yapılandırmanız ve VM içindeki DHCP seçeneğinin etkin olduğundan emin olmalısınız. PowerShell ile statik IP adresi ayarlama hakkında [daha fazla bilgi edinin.](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)

### <a name="remove-lock-from-the-recovery-point-resource-group"></a><a name="remove_lock_from_the_recovery_point_resource_group"></a>Kurtarma noktası kaynak grubundan kilidi kaldırma

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Tüm **Kaynaklar seçeneğine**gidin, aşağıdaki biçimde geri yükleme`<Geo>``<number>`noktası toplama kaynak grubunu seçin AzureBackupRG_ _ .
3. **Ayarlar** bölümünde, kilitleri görüntülemek için **Kilitler'i** seçin.
4. Kilidi kaldırmak için elipsleri seçin ve **Sil'i**tıklatın.

    ![Kilidi silme](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean-up-restore-point-collection"></a><a name="clean_up_restore_point_collection"></a>Geri yükleme noktası koleksiyonunu temizleme

Kilidi çıkardıktan sonra, geri yükleme noktaları nın temizlenmesi gerekir.

VM'nin Kaynak Grubunu veya VM'nin kendisini silerseniz, yönetilen disklerin anlık görüntüleri etkin kalır ve bekletme kümesine göre süresi dolur. Geri Yükleme Noktası Koleksiyonu'nda depolanan anlık görüntüleri (artık ihtiyacınız yoksa) silmek için, geri yükleme noktası koleksiyonunu aşağıda verilen adımlara göre temizleyin.

Geri yükleme noktalarını temizlemek için yöntemlerden herhangi birini izleyin:<br>

- [İsteğe bağlı yedekleme çalıştırarak geri yükleme noktası koleksiyonunu temizleme](#clean-up-restore-point-collection-by-running-on-demand-backup)<br>
- [Azure portalından geri yükleme noktası koleksiyonunu temizleme](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-on-demand-backup"></a><a name="clean-up-restore-point-collection-by-running-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırarak geri yükleme noktası koleksiyonunu temizleme

Kilidi çıkardıktan sonra isteğe bağlı yedeklemeyi tetikleyin. Bu eylem, geri yükleme noktalarının otomatik olarak temizlenmesini sağlar. Bu isteğe bağlı işlemin ilk kez başarısız olmasını bekleyin; ancak, geri yükleme noktalarının el ile silinmesi yerine otomatik temizleme sağlayacaktır. Temizlemeden sonra, bir sonraki zamanlanmış yedeklemeniz başarılı olmalıdır.

> [!NOTE]
> Otomatik temizleme, isteğe bağlı yedeklemeyi birkaç saat tetikledikten sonra gerçekleşir. Zamanlanmış yedeklemeniz hala başarısız olursa, [burada](#clean-up-restore-point-collection-from-azure-portal)listelenen adımları kullanarak geri yükleme noktası koleksiyonunu el ile silmeyi deneyin.

#### <a name="clean-up-restore-point-collection-from-azure-portal-br"></a><a name="clean-up-restore-point-collection-from-azure-portal"></a>Azure portalından geri yükleme noktası koleksiyonunu temizleme <br>

Kaynak grubundaki kilit nedeniyle temizlenmeyen geri yükleme noktaları koleksiyonunu el ile temizlemek için aşağıdaki adımları deneyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Hub** menüsünde Tüm **kaynaklar'ı**tıklatın, VM'nizin bulunduğu yerde aşağıdaki biçimi AzureBackupRG_`<Geo>`_`<number>` ile Kaynak grubunu seçin.

    ![Kilidi silme](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Kaynak grubunu tıklatın, **Genel Bakış** bölmesi görüntülenir.
4. Tüm gizli kaynakları görüntülemek için **gizli türleri göster** seçeneğini seçin. Aşağıdaki biçimi AzureBackupRG_`<VMName>`_`<number>`ile geri yükleme noktası koleksiyonları seçin.

    ![Kilidi silme](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Geri yükleme noktası koleksiyonunu temizlemek için **Sil'i** tıklatın.
6. Yedekleme işlemini yeniden deneyin.

> [!NOTE]
 >Kaynak (RP Collection) çok sayıda Geri Yükleme Noktası varsa, bunları portaldan silen zaman acısı ve başarısız olabilir. Bu, tüm geri yükleme noktalarının öngörülen süre içinde silinmediğini ve işlem sürelerinin dolduğu bilinen bir CRP sorunudur; ancak silme işlemi genellikle 2 veya 3 yeniden denemeden sonra başarılı dır.
