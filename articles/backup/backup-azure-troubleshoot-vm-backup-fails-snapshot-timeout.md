---
title: Aracı ve uzantı sorunlarını giderme
description: Belirtiler, nedenler ve Aracı, uzantısı ve disklerle ilgili Azure Backup hatalarının çözümleri.
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.service: backup
ms.openlocfilehash: 4583c02b52ab6b3a4e5056a47db096d4e34399ca
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79248027"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Sorun giderme Azure Backup hatası: aracıdaki veya uzantıdaki sorunlar

Bu makalede, VM Aracısı ve uzantısıyla iletişim ile ilgili Azure Backup hatalarını çözmenize yardımcı olabilecek sorun giderme adımları sunulmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable-VM Aracısı Azure Backup ile iletişim kuramadı

**Hata kodu**: usererrorguestagentstatusunavailable <br>
**Hata iletisi**: VM Aracısı Azure Backup ile iletişim kuramadı<br>

Azure VM Aracısı durdurulmuş, süresi geçmiş, tutarsız durumda veya yüklü olmayabilir. Bu durumlar Azure Backup hizmetinin anlık görüntüleri tetiklemesini önler.

- VM **durumunun** **çalıştığından** ve **aracı durumunun** **Azure portal > VM > ayarları > Özellikler > bölmesini açın** **.** VM Aracısı durdurulmuşsa veya tutarsız bir durumdaysa aracıyı yeniden başlatın<br>
  - Windows VM 'Leri için, Konuk aracısını yeniden başlatmak için aşağıdaki [adımları](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms) izleyin.<br>
  - Linux VM 'Ler için, Konuk aracısını yeniden başlatmak için aşağıdaki [adımları](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms) izleyin.
- Tüm uzantıların **sağlama başarılı** durumunda olduğundan emin olmak için **Azure portal > VM > ayarları > > uzantıları** ' nı açın. Aksi takdirde, bu sorunu çözmek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state) izleyin.

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError-anlık görüntü durumu için VM aracısıyla iletişim kurulamadı

**Hata kodu**: guestagentsnapshottaskstatuserror<br>
**Hata iletisi**: anlık görüntü durumu için VM aracısıyla iletişim kurulamadı <br>

Azure Backup hizmeti için bir VM 'yi kaydettikten ve zamanladıktan sonra, yedekleme işlemi, bir noktadan noktaya anlık görüntü almak için VM yedekleme uzantısıyla iletişim kurarak işi başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntünün tetiklenmesi önlenebilir. Anlık görüntü tetiklenmemişse bir yedekleme hatası oluşabilir. Aşağıdaki sorun giderme adımlarını listelenen sırayla doldurun ve ardından işleminizi yeniden deneyin:  

**Neden 1: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  

**Neden 2: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

**Neden 3: [anlık görüntü durumu alınamaz veya bir anlık görüntü](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken) alınamaz**

**Neden 4: [VM Aracısı yapılandırma seçenekleri ayarlanmamış (Linux sanal makineleri için)](#vm-agent-configuration-options-are-not-set-for-linux-vms)**

## <a name="usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state"></a>UserErrorVmProvisioningStateFailed-VM başarısız sağlama durumunda

**Hata kodu**: usererrorvmprovisioningstatefailed<br>
**Hata iletisi**: VM başarısız sağlama durumunda<br>

Bu hata, uzantı hatalarından biri VM 'yi sağlama başarısız durumuna koyarken oluşur.<br>**Azure portal > VM > ayarları > uzantıları > uzantıları durumunu açın** ve tüm uzantıların **sağlama başarılı** durumunda olup olmadığını denetleyin. Daha fazla bilgi için bkz. [sağlama durumları](https://docs.microsoft.com/azure/virtual-machines/windows/states-lifecycle#provisioning-states).

- VMSnapshot uzantısı hatalı durumdaysa, başarısız olan uzantıya sağ tıklayın ve kaldırın. İsteğe bağlı yedekleme tetikleyin. Bu eylem, uzantıları yeniden yükleyerek yedekleme işini çalıştıracaktır.  <br>
- Başka herhangi bir uzantı başarısız durumdaysa, yedeklemeyi kesintiye uğratabilirler. Uzantı sorunlarının çözümlendiğinden emin olun ve yedekleme işlemini yeniden deneyin.
- VM sağlama durumu bir güncelleştirme durumundaysa, yedeklemeyi kesintiye uğratabilirler. Sağlıklı olduğundan emin olun ve yedekleme işlemini yeniden deneyin.

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>Usererrorrpcollectionlimitulaşıldı-geri yükleme noktası koleksiyonu en yüksek sınırına ulaşıldı

**Hata kodu**: usererrorrpcollectionlimitulaşıldı <br>
**Hata iletisi**: geri yükleme noktası koleksiyonu en yüksek sınırına ulaşıldı. <br>

- Kurtarma noktası kaynak grubunda bir kilit varsa, kurtarma noktalarının otomatik temizlenmesini engelliyorsa bu sorun oluşabilir.
- Bu sorun, günlük birden çok yedeklemenin tetiklenmesi durumunda da gerçekleşebilir. Şu anda günde yalnızca bir yedekleme yapmanız önerilir, çünkü anında geri yükleme noktaları yapılandırılan anlık görüntü bekletmesine göre 1-5 gün boyunca korunur ve belirli bir zamanda bir VM ile yalnızca 18 anlık RPs ilişkilendirilebilir. <br>
- Bir VM için geri yükleme noktası koleksiyonları ve kaynak grupları genelinde geri yükleme noktalarının sayısı 18 ' i aşamaz. Yeni bir geri yükleme noktası oluşturmak için, mevcut geri yükleme noktalarını silin.

Önerilen eylem:<br>
Bu sorunu çözmek için VM 'nin kaynak grubundaki kilidi kaldırın ve temizleme işlemini tetiklemek için işlemi yeniden deneyin.
> [!NOTE]
> Yedekleme hizmeti, geri yükleme noktası koleksiyonunu depolamak için VM 'nin kaynak grubundan farklı bir kaynak grubu oluşturur. Müşteriler, yedekleme hizmeti tarafından kullanılmak üzere oluşturulan kaynak grubunu kilitlemez. Yedekleme hizmeti tarafından oluşturulan kaynak grubunun adlandırma biçimi: AzureBackupRG_`<Geo>`_`<number>` örn. AzureBackupRG_northeurope_1

**1. Adım: [kilidi geri yükleme noktası kaynak grubundan kaldırma](#remove_lock_from_the_recovery_point_resource_group)** <br>
**2. Adım: [geri yükleme noktası koleksiyonunu Temizleme](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured-yedekleme, şifrelenmiş VM 'lerin yedeklenmesi için Anahtar Kasası üzerinde yeterli izinlere sahip değil

**Hata kodu**: usererrorkeyvaultpermissionsnotconfigured <br>
**Hata iletisi**: yedekleme, şifrelenmiş VM 'lerin yedeklenmesi için Anahtar Kasası üzerinde yeterli izinlere sahip değil. <br>

Bir yedekleme işleminin şifreli VM 'lerde başarılı olması için anahtar kasasına erişmek için gereken izinlere sahip olması gerekir. İzinler [Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) veya [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection)aracılığıyla ayarlanabilir.

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork-sanal makinede ağ bağlantısı olmaması nedeniyle anlık görüntü işlemi başarısız oldu

**Hata kodu**: extensionsnapshotfailednonetwork<br>
**Hata iletisi**: sanal makinede ağ bağlantısı olmaması nedeniyle anlık görüntü işlemi başarısız oldu<br>

Azure Backup hizmeti için bir VM 'yi kaydettikten ve zamanladıktan sonra, yedekleme işlemi, bir noktadan noktaya anlık görüntü almak için VM yedekleme uzantısıyla iletişim kurarak işi başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntünün tetiklenmesi önlenebilir. Anlık görüntü tetiklenmemişse bir yedekleme hatası oluşabilir. Aşağıdaki sorun giderme adımını tamamlayıp işleminizi yeniden deneyin:

**[Anlık görüntü durumu alınamaz veya bir anlık görüntü alınamaz](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  

## <a name="ExtensionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtensionOperationFailedForManagedDisks-VMSnapshot genişletme işlemi başarısız oldu

**Hata kodu**: extensionoperationfailedformanageddisks <br>
**Hata iletisi**: VMSnapshot genişletme işlemi başarısız oldu<br>

Azure Backup hizmeti için bir VM 'yi kaydettikten ve zamanladıktan sonra, yedekleme işlemi, bir noktadan noktaya anlık görüntü almak için VM yedekleme uzantısıyla iletişim kurarak işi başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntünün tetiklenmesi önlenebilir. Anlık görüntü tetiklenmemişse bir yedekleme hatası oluşabilir. Aşağıdaki sorun giderme adımlarını listelenen sırayla doldurun ve ardından işleminizi yeniden deneyin:  
**Neden 1: [anlık görüntü durumu alınamaz veya bir anlık görüntü](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken) alınamaz**  
**Neden 2: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Neden 3: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed/BackUpOperationFailedV2-yedekleme, bir iç hata ile başarısız oluyor

**Hata kodu**: backupoperationfailed/BackUpOperationFailedV2 <br>
**Hata iletisi**: yedekleme bir iç hata ile başarısız oldu-lütfen işlemi birkaç dakika içinde yeniden deneyin <br>

Azure Backup hizmeti için bir VM 'yi kaydettikten ve zamanladıktan sonra, yedekleme işlemi bir zaman noktaya anlık görüntü almak için VM yedekleme uzantısıyla iletişim kurarak işi başlatır. Aşağıdaki koşullardan herhangi biri, anlık görüntünün tetiklenmesi önlenebilir. Anlık görüntü tetiklenmemişse bir yedekleme hatası oluşabilir. Aşağıdaki sorun giderme adımlarını listelenen sırayla doldurun ve ardından işleminizi yeniden deneyin:  
**Neden 1: [Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'leri için)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Neden 2: [VM 'de yüklü olan aracı güncel değil (Linux VM 'ler için)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Neden 3: [anlık görüntü durumu alınamaz veya bir anlık görüntü](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken) alınamaz**  
**Neden 4: [yedekleme hizmeti 'nin, bir kaynak grubu kilidi nedeniyle eski geri yükleme noktalarını silme izni](#remove_lock_from_the_recovery_point_resource_group) yok**<br>

## <a name="usererrorunsupporteddisksize---the-configured-disk-sizes-is-currently-not-supported-by-azure-backup"></a>UserErrorUnsupportedDiskSize-yapılandırılan disk boyutu (ler) Şu anda Azure Backup tarafından desteklenmiyor

**Hata kodu**: usererrorunsupporteddisksize <br>
**Hata iletisi**: yapılandırılan disk boyutu (ler) şu anda Azure Backup tarafından desteklenmiyor. <br>

Disk boyutu 32 TB 'den büyük olan bir VM yedeklenirken yedekleme işlemi başarısız olabilir. Ayrıca, boyutu 4 TB 'tan büyük şifrelenmiş disklerin yedeklenmesi Şu anda desteklenmemektedir. Diskleri bölerek disk boyutlarının desteklenen sınıra eşit veya ondan daha az olduğundan emin olun.

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>Usererrorbackupoperationınprogress-Şu anda başka bir yedekleme işlemi sürdüğünden yedekleme başlatılamıyor

**Hata kodu**: usererrorbackupoperationınprogress <br>
**Hata iletisi**: Şu anda başka bir yedekleme işlemi devam ettiğinden yedekleme başlatılamadı<br>

Sürmekte olan bir yedekleme işi olduğundan son yedekleme işiniz başarısız oldu. Geçerli iş bitene kadar yeni bir yedekleme işi başlatamazsınız. Başka bir yedekleme işlemini tetiklemeden veya planlamadan önce sürmekte olan yedekleme işleminin tamamlandığından emin olun. Yedekleme işlerinin durumunu denetlemek için aşağıdaki adımları uygulayın:

1. Azure portal oturum açın, **tüm hizmetler**' e tıklayın. Kurtarma Hizmetleri yazın ve **Kurtarma Hizmetleri kasaları**’na tıklayın. Kurtarma hizmetleri kasalarının listesi görünür.
2. Kurtarma Hizmetleri kasaları listesinden yedeklemenin yapılandırıldığı bir kasa seçin.
3. Kasa panosu menüsünde **yedekleme işleri** ' ne tıklayarak tüm yedekleme işlerini görüntüler.
   - Devam eden bir yedekleme işi varsa işlemin tamamlanmasını bekleyin veya yedekleme işini iptal edin.
     - Yedekleme işini iptal etmek için yedekleme işine sağ tıklayın ve **iptal** ' e tıklayın veya [PowerShell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0)'i kullanın.
   - Yedeği farklı bir kasada yeniden yapılandırdıysanız, eski kasada çalışan bir yedekleme işi olmadığından emin olun. Varsa, yedekleme işini iptal edin.
     - Yedekleme işini iptal etmek için, yedekleme işine sağ tıklayıp **İptal**'e tıklayın veya [PowerShell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0) kullanın
4. Yedekleme işlemini yeniden deneyin.

Zamanlanmış yedekleme işlemi daha uzun sürüyorsa, sonraki yedekleme yapılandırması ile çakışıyor ve [En Iyi uygulamaları](backup-azure-vms-introduction.md#best-practices), [yedekleme performansını](backup-azure-vms-introduction.md#backup-performance)ve [geri yükleme](backup-azure-vms-introduction.md#backup-and-restore-considerations)konusunu gözden geçirin.

## <a name="usererrorcrpreportedusererror---backup-failed-due-to-an-error-for-details-see-job-error-message-details"></a>UserErrorCrpReportedUserError-bir hata nedeniyle yedekleme başarısız oldu. Ayrıntılar için bkz. Iş hata Iletisi ayrıntıları

**Hata kodu**: usererrorcrpreportedusererror <br>
**Hata iletisi**: bir hata nedeniyle yedekleme başarısız oldu. Ayrıntılar için bkz. Iş hata Iletisi ayrıntıları.

Bu hata IaaS VM 'den raporlanır. Sorunun kök nedenini belirlemek için kurtarma hizmetleri Kasası ayarları ' na gidin. **İzleme** bölümünde, filtre uygulamak ve durumu görüntülemek için **yedekleme işleri** ' ni seçin. Temeldeki hata iletisi ayrıntılarını gözden geçirmek için **hatalar** ' a tıklayın. Hata ayrıntıları sayfasındaki önerilere göre daha fazla eylem gerçekleştirin.

## <a name="causes-and-solutions"></a>Nedenler ve çözümler

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Aracı VM 'ye yüklendi, ancak yanıt vermiyor (Windows VM 'Leri için)

#### <a name="solution"></a>Çözüm

VM Aracısı bozulmuş olabilir veya hizmet durdurulmuş olabilir. VM Aracısı 'nı yeniden yüklemek en son sürümü almaya yardımcı olur. Hizmet ile iletişimin yeniden başlatılmasına de yardımcı olur.

1. Windows Azure Konuk Aracısı hizmetinin VM hizmetlerinde çalışıp çalışmadığını belirleme (Services. msc). Windows Azure Konuk Aracısı hizmetini yeniden başlatmayı deneyin ve yedeklemeyi başlatın.
2. Windows Azure Konuk Aracısı hizmeti Hizmetler 'de görülemiyorsa, Denetim Masası 'nda, Windows Azure Konuk Aracısı hizmetinin yüklenip yüklenmediğini öğrenmek için **Programlar ve Özellikler** ' e gidin.
3. Windows Azure Konuk Aracısı **Programlar ve Özellikler**' de görünürse, Microsoft Azure Konuk aracısını kaldırın.
4. [Aracı MSI ' nın en son sürümünü](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)indirip yükleyin. Yüklemeyi gerçekleştirmek için yönetici haklarına sahip olmanız gerekir.
5. Windows Azure Konuk Aracısı hizmetlerinin hizmetler 'de göründüğünü doğrulayın.
6. İsteğe bağlı yedekleme çalıştırın:
   - Portalda **Şimdi Yedekle**' yi seçin.

Ayrıca, VM 'de [Microsoft .NET 4,5 ' nin yüklü](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) olduğunu doğrulayın. VM aracısının hizmetle iletişim kurması için .NET 4,5 gerekir.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM 'de yüklü olan aracı güncel değil (Linux VM 'Leri için)

#### <a name="solution"></a>Çözüm

Linux VM 'Leri için aracıyla ilgili veya uzantı ile ilgili çoğu başarısızlık, süresi geçmiş bir VM aracısını etkileyen sorunlardan kaynaklanır. Bu sorunu gidermek için aşağıdaki genel yönergeleri izleyin:

1. [LINUX VM aracısını güncelleştirme](../virtual-machines/linux/update-agent.md)yönergelerini izleyin.

   > [!NOTE]
   > Aracıyı yalnızca bir dağıtım deposu aracılığıyla güncelleştirmenizi *önemle tavsiye* ederiz. Doğrudan GitHub 'dan aracı kodunu indirmenizi ve güncelleştirmeyi önermiyoruz. Dağıtım için en son aracı kullanılamıyorsa, nasıl yükleneceğine ilişkin yönergeler için dağıtım desteğiyle iletişim kurun. En son aracıyı denetlemek için GitHub deposundaki [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) sayfasına gidin.

2. Şu komutu çalıştırarak Azure aracısının VM 'de çalıştığından emin olun: `ps -e`

   İşlem çalışmıyorsa, aşağıdaki komutları kullanarak yeniden başlatın:

   - Ubuntu için: `service walinuxagent start`
   - Diğer dağıtımlar için: `service waagent start`

3. [Otomatik yeniden başlatma aracısını yapılandırın](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Yeni bir test yedeklemesi çalıştırın. Hata devam ederse, VM 'den aşağıdaki günlükleri toplayın:

   - /var/lib/waagent/*. xml
   - /var/log/waagent.log
   - /var/log/azure/*

Waagent için ayrıntılı günlük kaydı gerekiyorsa, şu adımları izleyin:

1. /Etc/waagent.exe dosyasında şu satırı bulun: **ayrıntılı günlüğü etkinleştir (y | n)**
2. **Logs. Verbose** değerini *n* ile *y*arasında değiştirin.
3. Değişikliği kaydedin ve ardından bu bölümde daha önce açıklanan adımları tamamlayarak waagent 'ı yeniden başlatın.

### <a name="vm-agent-configuration-options-are-not-set-for-linux-vms"></a>VM-aracı yapılandırma seçenekleri ayarlanmadı (Linux sanal makineleri için)

Yapılandırma dosyası (/etc/waagent.exe) waagent 'un eylemlerini denetler. Yapılandırma dosyası seçenekleri **uzantıları.** **yedekleme 'nin çalışması için,** etkinleştirme ve **sağlama.**
VM Aracısı yapılandırma dosyası seçeneklerinin tam listesi için bkz. <https://github.com/Azure/WALinuxAgent#configuration-file-options>

### <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Anlık görüntü durumu alınamaz veya bir anlık görüntü alınamaz

VM yedeklemesi, temel alınan depolama hesabına anlık görüntü komutu vermeyi kullanır. Yedekleme, depolama hesabına erişimi bulunmadığından ya da anlık görüntü görevinin yürütülmesi geciktiğinden başarısız olabilir.

#### <a name="solution"></a>Çözüm

Aşağıdaki koşullar anlık görüntü görevinin başarısız olmasına neden olabilir:

| Nedeni | Çözüm |
| --- | --- |
| VM, Uzak Masaüstü Protokolü (RDP) ' de kapatıldığından VM durumu yanlış bildirilir. | VM 'yi RDP 'de kapatırsanız, VM 'nin durumunun doğru olup olmadığını öğrenmek için portalı denetleyin. Doğru değilse, sanal makine panosundaki **kapatma** seçeneğini kullanarak portalda VM 'yi kapatın. |
| VM, DHCP 'den konak veya doku adresini alamıyor. | IaaS VM yedeklemesinin çalışması için, Konuk içinde DHCP 'nin etkinleştirilmesi gerekir. VM, DHCP yanıt 245 ' den ana bilgisayar veya doku adresini alamıyor, hiçbir uzantıyı indiremez veya çalıştıramıyorum. Statik bir özel IP gerekiyorsa, **Azure Portal** veya **PowerShell** aracılığıyla yapılandırmanız gerekır ve VM içindeki DHCP seçeneğinin etkin olduğundan emin olun. PowerShell ile statik IP adresi ayarlama hakkında [daha fazla bilgi edinin](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) .

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Kurtarma noktası kaynak grubundan kilidi kaldır

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm kaynaklar seçeneğine**gidin, aşağıdaki biçimdeki geri yükleme noktası koleksiyonu kaynak grubunu seçin AzureBackupRG_`<Geo>`_`<number>`.
3. **Ayarlar** bölümünde, kilitleri göstermek için **kilitler** ' ı seçin.
4. Kilidi kaldırmak için üç noktayı seçin ve **Sil**' e tıklayın.

    ![Kilidi Sil](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a>Geri yükleme noktası koleksiyonunu temizle

Kilidi kaldırdıktan sonra geri yükleme noktalarının temizlenmesi gerekir.

VM 'nin veya VM 'nin kaynak grubunu silerseniz, yönetilen disklerin anlık geri yükleme anlık görüntüleri etkin kalır ve bekletme kümesine göre sona erer. Geri yükleme noktası koleksiyonunda depolanan anlık geri yükleme anlık görüntülerini silmek için (artık ihtiyacınız yoksa), aşağıda verilen adımlara göre geri yükleme noktası koleksiyonunu temizleyin.

Geri yükleme noktalarını temizlemek için yöntemlerden birini izleyin:<br>

- [İsteğe bağlı yedekleme çalıştırarak geri yükleme noktası koleksiyonunu Temizleme](#clean-up-restore-point-collection-by-running-on-demand-backup)<br>
- [Geri yükleme noktası koleksiyonunu Azure portal temizle](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırarak geri yükleme noktası koleksiyonunu Temizleme

Kilidi kaldırdıktan sonra isteğe bağlı yedekleme tetikleyin. Bu eylem geri yükleme noktalarının otomatik olarak temizlendiğinden emin olur. Bu isteğe bağlı işlemin ilk kez başarısız olmasını bekler; Ancak, geri yükleme noktalarını el ile silmek yerine otomatik temizlemeyi sağlayacaktır. Temizleme işleminden sonra, bir sonraki zamanlanmış yedeklemeniz başarılı olmalıdır.

> [!NOTE]
> İsteğe bağlı yedeklemeyi tetiklemenin birkaç saati sonrasında otomatik temizleme gerçekleşecektir. Zamanlanmış yedeğiniz hala başarısız olursa, [burada](#clean-up-restore-point-collection-from-azure-portal)listelenen adımları kullanarak geri yükleme noktası koleksiyonunu el ile silmeyi deneyin.

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Geri yükleme noktası koleksiyonunu Azure portal temizle <br>

Kaynak grubundaki kilit nedeniyle temizlenmediği geri yükleme noktaları koleksiyonunu el ile temizlemek için aşağıdaki adımları deneyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Hub** menüsünde **tüm kaynaklar**' a tıklayın, aşağıdaki biçime sahip kaynak grubunu seçin AZUREBACKUPRG_`<Geo>`_`<number>` sanal makinenizin bulunduğu yer.

    ![Kilidi Sil](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Kaynak grubu ' na tıklayın, **genel bakış** bölmesi görüntülenir.
4. Tüm gizli kaynakları görüntülemek için **gizli türleri göster** seçeneğini belirleyin. Aşağıdaki biçime sahip geri yükleme noktası koleksiyonlarını seçin AzureBackupRG_`<VMName>`_`<number>`.

    ![Kilidi Sil](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Geri yükleme noktası koleksiyonunu temizlemek için **Sil** ' e tıklayın.
6. Yedekleme işlemini yeniden deneyin.

> [!NOTE]
 >Kaynağın (RP koleksiyonu) çok sayıda geri yükleme noktası varsa, bunları portaldan silmek zaman aşımına uğrayabilir ve başarısız olabilir. Bu, tüm geri yükleme noktalarının, belirlenen sürede silinmediği ve işlemin zaman aşımına uğraydığı bilinen bir CRP sorunudur; ancak silme işlemi genellikle 2 veya 3 yeniden denemeden sonra başarılı olur.
