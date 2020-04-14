---
title: Azure Site Kurtarma' yı kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure' a olağanüstü kurtarma sorunlarıyla sorun giderme | Microsoft Dokümanlar
description: Bu makalede, Azure Site Kurtarma kullanarak VMware VM'lerin ve fiziksel sunucuların Azure'a olağanüstü kurtarma sırasında sık karşılaşılan çoğaltma sorunları için sorun giderme bilgileri sağlanmaktadır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/2/2019
ms.author: mayg
ms.openlocfilehash: f91ee5654b4add37d3cce4f875be1f9c2b398ab9
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259502"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware VM'ler ve fiziksel sunucular için sorun giderme çoğaltma sorunları

Bu makalede, [Site Kurtarma'yı](site-recovery-overview.md)kullanarak şirket içi VMware VM'leri ve fiziksel sunucuları Azure'da çoğaltırken karşılaşabileceğiniz bazı sık karşılaşılan sorunlar ve belirli hatalar açıklanmaktadır.

## <a name="step-1-monitor-process-server-health"></a>Adım 1: İşlem sunucusu durumunu izleyin

Site Kurtarma, çoğaltılan verileri almak ve en iyi duruma getirmek ve Azure'a göndermek için [işlem sunucusunu](vmware-physical-azure-config-process-server-overview.md#process-server) kullanır.

Bağlı ve düzgün çalıştığından ve işlem sunucusuyla ilişkili kaynak makineler için çoğaltmanın ilerledikten emin olmak için portaldaki işlem sunucularının sistem durumunu izlemenizi öneririz.

- İşlem sunucularını izleme [hakkında bilgi edinin.](vmware-physical-azure-monitor-process-server.md)
- [En iyi uygulamaları inceleme](vmware-physical-azure-troubleshoot-process-server.md#best-practices-for-process-server-deployment)
- [Sorun giderme](vmware-physical-azure-troubleshoot-process-server.md#check-process-server-health) işlemi sunucu sağlığı.

## <a name="step-2-troubleshoot-connectivity-and-replication-issues"></a>Adım 2: Bağlantı ve çoğaltma sorunlarını giderme

İlk ve devam eden çoğaltma hataları genellikle kaynak sunucu ile işlem sunucusu arasındaki veya işlem sunucusu ile Azure arasındaki bağlantı sorunlarından kaynaklanır.

Bu sorunları çözmek [için, bağlantı ve çoğaltma sorun giderme.](vmware-physical-azure-troubleshoot-process-server.md#check-connectivity-and-replication)




## <a name="step-3-troubleshoot-source-machines-that-arent-available-for-replication"></a>Adım 3: Çoğaltma için kullanılamayan kaynak makineleri giderme

Site Kurtarma'yı kullanarak çoğaltmayı etkinleştirmek için kaynak makineyi seçmeye çalıştığınızda, makine aşağıdaki nedenlerden biriyle kullanılamayabilir:

* **Aynı örneku UUID'ye sahip iki sanal makine**: vCenter altında iki sanal makineaynı örneğin UUID'ye sahipse, yapılandırma sunucusu tarafından keşfedilen ilk sanal makine Azure portalında gösterilir. Bu sorunu gidermek için, iki sanal makinenin aynı örnek UUID'ye sahip olmadığından emin olun. Bu senaryo genellikle yedek VM'nin etkin hale geldiği ve keşif kayıtlarımıza günlüğe kaydedildiği durumlarda görülür. Azure [Site Kurtarma VMware'den Azure'a bakın: Çözümlemek için yinelenen veya eski girişleri nasıl temizlersin.](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx)
* **Yanlış vCenter kullanıcı kimlik bilgileri**: OVF şablonu veya birleşik kurulum kullanarak yapılandırma sunucusunu ayarlarken doğru vCenter kimlik bilgilerini eklediğinizden emin olun. Kurulum sırasında eklediğiniz kimlik bilgilerini doğrulamak [için](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery)bkz.
* **vCenter yetersiz ayrıcalıkları**: vCenter'a erişmek için sağlanan izinlerde gerekli izinler yoksa, sanal makinelerin bulunamaması oluşabilir. [Otomatik keşif için hesap hazırlama'da](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) açıklanan izinlerin vCenter kullanıcı hesabına eklenmesini sağlayın.
* **Azure Site Kurtarma yönetim sunucuları**: Sanal makine aşağıdaki rollerden biri veya birkaçı altında yönetim sunucusu olarak kullanılırsa - Yapılandırma sunucusu /ölçeklendirme işlem sunucusu / Ana hedef sunucu, o zaman sanal makineyi portaldan seçemeyeceksiniz. Yönetimler sunucuları çoğaltılamaz.
* **Azure Site Kurtarma hizmetleri aracılığıyla zaten korunmuş/başarısız olmuş**: Sanal makine Site Kurtarma aracılığıyla zaten korunuyorsa veya başarısız oluyorsa, sanal makine portalda korunmak için seçilemiyor. Portalda aradığınız sanal makinenin başka bir kullanıcı tarafından veya farklı bir abonelik altında zaten korunmadığından emin olun.
* **vCenter bağlı değil**: vCenter'ın bağlı durumda olup olmadığını denetleyin. Doğrulamak için, Kurtarma Hizmetleri kasasına > Site Kurtarma Altyapısı > Yapılandırma Sunucuları'na gidin > İlgili yapılandırma sunucusuna tıklayın > ilgili sunucuların ayrıntılarıyla birlikte sağınızda bir bıçak açılır. vCenter'ın bağlı olup olmadığını kontrol edin. "Bağlanmadı" durumundaysa, sorunu giderin ve ardından [portaldaki yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server) Bundan sonra, sanal makine portalda listelenecektir.
* **ESXi destekli kapalı**: Sanal makinenin bulunduğu ESXi ana bilgisayar kapalı durumdaysa, sanal makine listelenmez veya Azure portalında seçilemez. ESXi ana bilgisayarda güç, [portaldaki yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server) Bundan sonra, sanal makine portalda listelenecektir.
* **Bekleyen yeniden başlatma**: Sanal makinede bekleyen bir yeniden başlatma varsa, Azure portalında makineyi seçemeyeceksiniz. Bekleyen yeniden başlatma etkinliklerini tamamladığından emin olun, [yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server) Bundan sonra, sanal makine portalda listelenecektir.
* **IP bulunamadı**: Sanal makinenin bununla ilişkili geçerli bir IP adresi yoksa, azure portalında makineyi seçemezsiniz. Sanal makineye geçerli bir IP adresi atadığından emin olun, [yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server) Bundan sonra, sanal makine portalda listelenecektir.

### <a name="troubleshoot-protected-virtual-machines-greyed-out-in-the-portal"></a>Sorun giderme korumalı sanal makineler portalda soluk

Sistemde yinelenen girişler varsa, Site Kurtarma altında çoğaltılan sanal makineler Azure portalında kullanılamaz. Eski girişleri nasıl sildiğinizi ve sorunu nasıl çözeceğinizi öğrenmek için [Azure Site Kurtarma VMware'den Azure'a: Yinelenen veya eski girişleri nasıl temizleyeceğiniz konusuna](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx)bakın.

## <a name="no-crash-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Son 'XXX' dakikasında VM için kilitlenme tutarlı kurtarma noktası yok

En sık karşılaşılan sorunlardan bazıları aşağıda listelenmiştir

### <a name="initial-replication-issues-error-78169"></a>İlk çoğaltma sorunları [hata 78169]

Yukarıda, bağlantı, bant genişliği veya zaman eşitlemeyle ilgili sorunlar olmamasını sağlamak, şunları emin olun:

- Hiçbir virüsten koruma yazılımı Azure Site Kurtarma'yı engellenemez. Azure Site Kurtarma için gereken klasör hariç tutmalar hakkında [daha fazla](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) bilgi edinin.

### <a name="source-machines-with-high-churn-error-78188"></a>Yüksek çalkalama ile kaynak makineleri [hata 78188]

Olası Nedenler:
- Sanal makinenin listelenen disklerinde veri değiştirme oranı (bayt/sn yazma), çoğaltma hedef depolama hesabı türü için [Azure Site Kurtarma desteklenen sınırlarından](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) daha fazladır.
- Yükleme için bekleyen yüksek miktarda veri nedeniyle çalkalama hızında ani bir artış vardır.

Sorunu gidermek için:
- Hedef depolama hesabı türünün (Standart veya Premium) kaynaktaki çalkalama oranı gereksinimine göre sağlanmış olduğundan emin olun.
- Zaten Premium yönetilen bir diske (asrseeddisk türü) çoğalıyorsanız, diskboyutunun Site Kurtarma sınırlarına göre gözlenen karmaşa hızını desteklediğinden emin olun. Gerekirse asrseeddisk boyutunu artırabilirsiniz. Aşağıdaki adımları izleyin:
    - Etkilenen çoğaltılan makinenin Diskler bıçağına gidin ve çoğaltma disk adını kopyalayın
    - Bu yineleme yönetilen diske gidin
    - Genel Bakış bıçağında bir SAS URL'sinin oluşturulduğunu belirten bir afiş görebilirsiniz. Bu afişe tıklayın ve dışa aktarmayı iptal edin. Banner'ı görmüyorsanız bu adımı yoksayın.
    - SAS URL'si iptal edilir etmez Yönetilen Diskin Configuration blade'ine gidin ve ASR'nin kaynak diskte gözlenen karmaşa hızını destekleyecek şekilde boyutunu artırın
- Gözlenen karmaşa geçiciyse, bekleyen veri yüklemesinin yetişmesi ve kurtarma noktaları oluşturması için birkaç saat bekleyin.
- Disk geçici günlükler, test verileri vb. gibi kritik olmayan veriler içeriyorsa, bu verileri başka bir yere taşımayı düşünün veya bu diski çoğaltmadan tamamen hariç tutabilirsiniz
- Sorun devam ederse, çoğaltma yı planlamaya yardımcı olmak için Site Kurtarma [dağıtım planlayıcısını](site-recovery-deployment-planner.md#overview) kullanın.

### <a name="source-machines-with-no-heartbeat-error-78174"></a>Kalp atışı olmayan kaynak makineler [hata 78174]

Bu, Kaynak Makine'deki Azure Site Kurtarma Mobilite aracısı Configuration Server (CS) ile iletişim kurmadığında gerçekleşir.

Sorunu gidermek için, kaynak VM'den Config Server'a ağ bağlantısını doğrulamak için aşağıdaki adımları kullanın:

1. Kaynak Makine'nin çalıştığını doğrulayın.
2. Yönetici ayrıcalıklarına sahip bir hesap kullanarak Kaynak Makine'de oturum açın.
3. Aşağıdaki hizmetlerin çalıştığını ve hizmetleri yeniden başlatmadığını doğrulayın:
   - Svagents (InMage Scout VX Aracısı)
   - InMage Scout Uygulaması Hizmeti
4. Kaynak Makine'de, hata ayrıntıları için konumdaki günlükleri inceleyin:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log

### <a name="process-server-with-no-heartbeat-error-806"></a>Sinyal atamı olmayan işlem sunucusu [hata 806]
Process Server'dan (PS) sinyal atamı yoksa, aşağıdakileri kontrol edin:
1. PS VM çalışır durumda
2. Hata ayrıntıları için PS'deki aşağıdaki günlükleri kontrol edin:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="master-target-server-with-no-heartbeat-error-78022"></a>Sinyal atamı olmayan ana hedef sunucu [hata 78022]

Bu, Ana Hedef'teki Azure Site Kurtarma Mobilite aracısı Configuration Server ile iletişim kurmadığında gerçekleşir.

Sorunu gidermek için, hizmet durumunu doğrulamak için aşağıdaki adımları kullanın:

1. Ana Hedef VM'nin çalıştığını doğrulayın.
2. Yönetici ayrıcalıklarına sahip bir hesabı kullanarak Ana Hedef VM'de oturum açın.
    - Svagents hizmetinin çalıştığını doğrulayın. Çalışıyorsa, hizmeti yeniden başlatın
    - Hata ayrıntıları için konumdaki günlükleri kontrol edin:

          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
3. Ana hedefi yapılandırma sunucusuna kaydetmek için **%PROGRAMDATA%\ASR\Agent**klasörüne gidin ve komut isteminde aşağıdakileri çalıştırın:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="error-id-78144---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Hata Kimliği 78144 - Son 'XXX' dakikasında VM için uygulama tutarlı kurtarma noktası yok

VSS yükleme hatası davranışlarını işlemek için mobilite aracısı [9.23](vmware-physical-mobility-service-overview.md#mobility-service-agent-version-923-and-higher) & [9.27](site-recovery-whats-new.md#update-rollup-39) sürümlerinde geliştirmeler yapılmıştır. Sorun giderme VSS hataları hakkında en iyi rehberlik için en son sürümlerde olduğunuzdan emin olun.

En sık karşılaşılan sorunlardan bazıları aşağıda listelenmiştir

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>Neden 1: SQL sunucusunda bilinen sorun 2008/2008 R2
**Nasıl düzeltilir** : SQL server 2008/2008 R2 ile bilinen bir sorun vardır. Lütfen bu KB makalesine bakın [Azure Site Kurtarma Aracısı veya diğer bileşen dışı VSS yedekleme sql server 2008 R2 barındıran bir sunucu için başarısız](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-auto_close-dbs"></a>Neden 2: Azure Site Kurtarma işleri, AUTO_CLOSE DB'li SQL Server örneklerinin herhangi bir sürümünü barındıran sunucularda başarısız olur
**Nasıl düzeltilir** : Bkz. Kb [makale](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser)


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>Neden 3: SQL Server 2016 ve 2017'de bilinen sorun
**Nasıl düzeltilir** : Bkz. Kb [makale](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component)


### <a name="more-causes-due-to-vss-related-issues"></a>VSS ile ilgili sorunlar nedeniyle daha fazla neden:

Daha fazla sorun gidermek için, hata için tam hata kodunu almak için kaynak makinedeki dosyaları denetleyin:

    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Dosyadaki hataları bulma nasıl?
Bir editörde vacp.log dosyasını açarak "vacpError" dizesini arayın

    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

Yukarıdaki örnekte **2147754994** aşağıda gösterildiği gibi hata hakkında söyler hata kodudur

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS yazar yüklü değil - Hata 2147221164

*Nasıl düzeltilir*: Uygulama tutarlılığı etiketi oluşturmak için Azure Site Kurtarma Microsoft Birim Gölge kopyalama Hizmeti (VSS) kullanır. Uygulama tutarlılığı anlık görüntülerini almak için çalışması için bir VSS Sağlayıcısı yükler. Bu VSS Sağlayıcısı bir hizmet olarak yüklenir. VSS Sağlayıcı hizmetinin yüklenmemesi durumunda, uygulama tutarlılığı anlık oluşturma hata id 0x80040154 "Sınıf kayıtlı değil" ile başarısız olur. </br>
[VSS yazar yükleme sorun giderme için makaleye](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) bakın

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS yazar devre dışı bırakılır - Hata 2147943458

**Nasıl düzeltilir**: Uygulama tutarlılığı etiketi oluşturmak için Azure Site Kurtarma Microsoft Birim Gölge kopyalama Hizmeti (VSS) kullanır. Uygulama tutarlılığı anlık görüntülerini almak için çalışması için bir VSS Sağlayıcısı yükler. Bu VSS Sağlayıcısı bir hizmet olarak yüklenir. VSS Sağlayıcı hizmetidevre dışı bırakılırsa, uygulama tutarlılığı anlık oluşturma hata kimliği "Belirtilen hizmet devre dışı bırakılır ve başlatılamaz(0x80070422)" ile başarısız olur. </br>

- VSS devre dışı bırakılırsa,
    - VSS Sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlı olduğunu doğrulayın.
    - Aşağıdaki hizmetleri yeniden başlatın:
        - VSS hizmeti
        - Azure Site Kurtarma VSS Sağlayıcısı
        - VDS hizmeti

####  <a name="vss-provider-not_registered---error-2147754756"></a>VSS Provider NOT_REGISTERED - Hata 2147754756

**Nasıl düzeltilir**: Uygulama tutarlılığı etiketi oluşturmak için Azure Site Kurtarma Microsoft Birim Gölge kopyalama Hizmeti (VSS) kullanır.
Azure Site Kurtarma VSS Sağlayıcısı hizmetinin yüklü olup olmadığını denetleyin. </br>

- Sağlayıcı yüklemesini aşağıdaki komutları kullanarak yeniden deneyin:
- Varolan sağlayıcıyı kaldırın: C:\Program Files (x86)\Microsoft Azure Site Kurtarma\aracı\InMageVSSProvider_Uninstall.cmd
- Yeniden yükleme: C:\Program Files (x86)\Microsoft Azure Site Kurtarma\InMageVSSProvider_Install.cmd

VSS Sağlayıcı hizmetinin başlangıç türünün **Otomatik**olarak ayarlı olduğunu doğrulayın.
    - Aşağıdaki hizmetleri yeniden başlatın:
        - VSS hizmeti
        - Azure Site Kurtarma VSS Sağlayıcısı
        - VDS hizmeti

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuz Azure [Site Kurtarma forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)yayınlayın. Aktif bir topluluğumuz var ve mühendislerimizden biri size yardımcı olabilir.
