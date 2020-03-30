---
title: Azure Site Kurtarma ile Hyper-V olağanüstü durum kurtarma sorunu
description: Azure Site Kurtarma'yı kullanarak Hyper-V'den Azure çoğaltmasına karşı olağanüstü durum kurtarma sorunlarını nasıl gidereceklerini açıklar
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/14/2019
ms.author: rajanaki
ms.openlocfilehash: 0a3e5c922009353e4ba9ccab12cf70ea2b5992da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73961476"
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Hyper-V’den Azure’a çoğaltma ve yük devretmeye ilişkin sorunları giderme

Bu makalede, Azure [Site Kurtarma'yı](site-recovery-overview.md)kullanarak şirket içi Hyper-V V VM'leri Azure'a kopyalarken karşılaşabileceğiniz sık karşılaşılan sorunlar açıklanmaktadır.

## <a name="enable-protection-issues"></a>Koruma sorunlarını etkinleştirme

Hyper-V V VM'ler için koruma yı etkinleştirdiğinizde sorunlarla karşılaşırsanız, aşağıdaki önerileri kontrol edin:

1. Hyper-V ana bilgisayarlarınızın ve VM'lerinizin tüm [gereksinimleri ve ön koşulları](hyper-v-azure-support-matrix.md)karşılayıp karşılamadığını kontrol edin.
2. Hyper-V sunucuları System Center Virtual Machine Manager (VMM) bulutlarında bulunuyorsa, [VMM sunucusunu](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional)hazırladığınızı doğrulayın.
3. Hyper-V Sanal Makine Yönetimi hizmetinin Hyper-V ana bilgisayarlarda çalıştığını kontrol edin.
4. Hyper-V-VMMS\Admin oturumlarında VM'de görünen sorunları denetleyin. Bu günlük Uygulamalar **ve Hizmetler Günlükleri** > **Microsoft** > **Windows**bulunur.
5. Konuk VM'de WMI'nin etkin ve erişilebilir olduğunu doğrulayın.
   - Temel WMI testleri [hakkında bilgi edinin.](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
   - [Sorun Giderme](https://aka.ms/WMiTshooting) Wmı.
   - WMI komut dosyaları ve hizmetleriyle ilgili sorunları [giderin.](https://technet.microsoft.com/library/ff406382.aspx#H22)
6. Konuk VM'de, Tümleştirme Hizmetlerinin en son sürümünün çalıştırdığından emin olun.
    - En son sürüme sahip olup olmadığınızı [kontrol edin.](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services)
    - [Tut](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) Entegrasyon Hizmetleri güncel.
    
## <a name="replication-issues"></a>Çoğaltma sorunları

İlk ve sürekli çoğaltma sorunlarını aşağıda gösterildiği gibi giderin:

1. Site Kurtarma hizmetlerinin [en son sürümünü](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) çalıştırdığınızdan emin olun.
2. Çoğaltmanın duraklatılıp duraklatılmadığını doğrulayın:
   - Hyper-V Manager konsolundaki VM sistem durumu durumunu kontrol edin.
   - Kritik durumdaysa, VM > **Çoğaltma** > **Görünümü Çoğaltma Durumu'na**sağ tıklayın.
   - Çoğaltma duraklatıldıysa, **Devam Çoğaltma'yı**tıklatın.
3. Gerekli hizmetlerin çalıştığını kontrol edin. Eğer değillerse, yeniden başlatın.
    - VMM olmadan Hyper-V'yi çoğaltıyorsanız, bu hizmetlerin Hyper-V ana bilgisayarda çalıştığını kontrol edin:
        - Sanal Makine Yönetim hizmeti
        - Microsoft Azure Kurtarma Hizmetleri Aracısı hizmeti
        - Microsoft Azure Site Recovery hizmeti
        - WMI Sağlayıcısı Ana Bilgisayar hizmeti
    - Ortamda VMM ile çoğalıyorsanız, bu hizmetlerin çalıştığını denetleyin:
        - Hyper-V ana bilgisayarda, Sanal Makine Yönetimi hizmeti, Microsoft Azure Kurtarma Hizmetleri Aracısı ve WMI Sağlayıcı Ana Bilgisayar hizmetinin çalışır durumda olup olmadığını kontrol edin.
        - VMM sunucusunda, System Center Virtual Machine Manager Service'in çalıştırdığından emin olun.
4. Hyper-V sunucusuyla Azure arasındaki bağlantıyı denetleyin. Bağlantıyı denetlemek için Hyper V ana bilgisayarda Görev Yöneticisi'ni açın. **Performans** sekmesinde **Kaynak İzle'yi Aç'ı**tıklatın. **Ağ** **Etkinliği ile > İşlem**sekmesinde, cbengine.exe'nin büyük hacimlerde (Mbs) etkin bir şekilde veri gönderip göndermediğini kontrol edin.
5. Hyper-V ana bilgisayarların Azure depolama blob URL'sine bağlanıp bağlanabıp bağlanamayabıp bağlanmayabileceğini kontrol edin. Ana bilgisayarların bağlanıp bağlanabıp bağlanabıp bağlanabıp bağlanabıp bağlanabıp bağlanmayabileceğini kontrol etmek için **cbengine.exe'yi**seçin ve kontrol edin. Ana bilgisayardan Azure depolama blob'una bağlantıyı doğrulamak için **TCP Bağlantıları'nı** görüntüleyin.
6. Aşağıda açıklandığı gibi performans sorunlarını kontrol edin.
    
### <a name="performance-issues"></a>Performans sorunları

Ağ bant genişliği sınırlamaları çoğaltmayı etkileyebilir. Sorun giderme sorunları aşağıdaki gibidir:

1. Ortamınızda bant genişliği veya daraltma kısıtlamaları olup olmadığını [denetleyin.](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage)
2. Dağıtım [Planlayıcısı profiloluşturucuçalıştırın.](hyper-v-deployment-planner-run.md)
3. Profil oluşturucuyu çalıştırdıktan sonra [bant genişliği](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) önerilerini izleyin.
4. [Veri karmaşası sınırlamalarını](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)denetleyin. VM'de yüksek veri karmaşası görüyorsanız, aşağıdakileri yapın:
   - VM'nizin yeniden eşitleme için işaretli olup olmadığını kontrol edin.
   - Karmaşanın kaynağını araştırmak için [aşağıdaki adımları](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) izleyin.
   - HrL günlük dosyaları kullanılabilir disk alanının %50'sini aştığında çalkalama oluşabilir. Sorun buysa, sorunun oluştuğu tüm VM'ler için daha fazla depolama alanı sağlar.
   - Çoğaltmanın duraklatmadığından emin değilim. Eğer varsa, artan boyutuna katkıda bulunabilir hrl dosyası, değişiklikleri yazmaya devam ediyor.
 

## <a name="critical-replication-state-issues"></a>Kritik çoğaltma durumu sorunları

1. Çoğaltma durumunu denetlemek için şirket içi Hyper-V Manager konsoluna bağlanın, VM'yi seçin ve sistem durumunu doğrulayın.

    ![Çoğaltma durumu](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Ayrıntıları görmek için **Çoğaltma Durumunu Görüntüle'yi** tıklatın:

    - Çoğaltma duraklatıldıysa, VM > **Çoğaltma** > **Devam çoğaltma'yı**sağ tıklatın.
    - Site Kurtarma'da yapılandırılan Bir Hyper-V ana bilgisayardaki bir VM aynı kümedeki farklı bir Hyper-V ana bilgisayara veya bağımsız bir makineye taşınırsa, VM için çoğaltma etkilenmez. Yeni Hyper-V ana bilgisayar sitesinin tüm ön koşulları karşılayıp karşılamadığını ve Site Kurtarma'da yapılandırıldığından kontrol edin.

## <a name="app-consistent-snapshot-issues"></a>Uygulama tutarlı anlık görüntü sorunları

Uygulama tutarlı anlık görüntüsü, VM içindeki uygulama verilerinin anlık görüntüsüdür. Birim Gölge Kopyalama Hizmeti (VSS), anlık görüntü alındığında VM'deki uygulamaların tutarlı bir durumda olmasını sağlar.  Bu bölümde karşılaşabileceğiniz bazı sık karşılaşılan sorunlar ayrıntıları.

### <a name="vss-failing-inside-the-vm"></a>VM içinde VSS başarısız

1. Tümleştirme hizmetlerinin en son sürümünün yüklü ve çalışır durumda olup olmadığını denetleyin.  Hyper-V ana bilgisayarda yüksek bir PowerShell isteminden aşağıdaki komutu çalıştırarak bir güncelleştirme nin kullanılabilir olup olmadığını kontrol edin: **get-vm | Select Name, State, IntegrationServicesState**.
2. VSS hizmetlerinin çalışır ve sağlıklı olup olmadığını kontrol edin:
   - Hizmetleri kontrol etmek için konuk VM'de oturum açın. Ardından bir yönetici komut istemi açın ve tüm VSS yazarlarının sağlıklı olup olmadığını kontrol etmek için aşağıdaki komutları çalıştırın.
       - **Vssadmin liste yazarları**
       - **Vssadmin liste gölgeleri**
       - **Vssadmin liste sağlayıcıları**
   - Çıktıyı kontrol et. Yazarlar başarısız bir durumdaysa, aşağıdakileri yapın:
       - VSS işlem hataları için VM'deki uygulama olay günlüğünü denetleyin.
   - Başarısız yazarla ilişkili bu hizmetleri yeniden başlatmayı deneyin:
     - Birim Gölge Kopyası
       - Azure Site Kurtarma VSS Sağlayıcısı
   - Bunu yaptıktan sonra, uygulama tutarlı anlık görüntülerinin başarıyla oluşturulıp oluşturulmadı sını görmek için birkaç saat bekleyin.
   - Son çare olarak VM yeniden başlatmayı deneyin. Bu, yanıt vermeyen durumda olan hizmetleri çözebilir.
3. VM'de dinamik disklerin yoksa kontrol edin. THis, uygulama tutarlı anlık görüntüler için desteklenmez. Disk Yönetimi'ni (diskmgmt.msc) iade edebilirsiniz.

    ![Dinamik disk](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. VM'ye bağlı bir iSCSI diskinin olup olmadığını kontrol edin. Bu özellik desteklenmez.
5. Yedekleme hizmetinin etkin olup olmadığını denetleyin. **Hyper-V ayarları** > **Tümleştirme Hizmetlerinde**etkinleştirildiğinden doğrulayın.
6. VSS anlık görüntülerini alan uygulamalarla çakışma olmadığından emin olun. Birden çok uygulama VSS anlık görüntülerini aynı anda almaya çalışıyorsa çakışmalar oluşabilir. Örneğin, Bir Yedekleme uygulaması, Site Kurtarma çoğaltma ilkeniz tarafından anlık görüntü almak üzere zamanlandığında VSS anlık görüntülerini alıyorsa.   
7. VM'de yüksek bir çalkalama oranı yaşanıp yaşanmadın:
    - Hyper-V ana bilgisayarda performans sayaçlarını kullanarak konuk VM'lerin günlük veri değişim oranını ölçebilirsiniz. Veri değişim oranını ölçmek için aşağıdaki sayacı etkinleştirin. VM karmaşasını elde etmek için bu değerin bir örneğini VM diskleri arasında 5-15 dakika toplayın.
        - Kategori: "Hyper-V Sanal Depolama Cihazı"
        - Sayaç: "Bayt / Sn yaz"</br>
        - Bu veri karmaşası oranı, VM'nin veya uygulamalarının ne kadar meşgul olduğuna bağlı olarak artacaktır veya yüksek bir düzeyde kalır.
        - Ortalama kaynak disk veri karmaşası, Site Kurtarma için standart depolama alanı için 2 MB/s'dir. [Daha fazlasını öğrenin](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Buna ek olarak [depolama ölçeklenebilirlik hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)doğrulayabilirsiniz.
8. Dağıtım [Planlayıcısını](hyper-v-deployment-planner-run.md)çalıştırın.
9. [Ağ](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input)için önerileri gözden geçirin.


### <a name="vss-failing-inside-the-hyper-v-host"></a>HYPER-V Ana Bilgisayar içinde VSS başarısız

1. VSS hataları ve önerileri için olay günlüklerini denetleyin:
    - Hyper-V ana bilgisayar sunucusunda, **Olay Görüntüleyicisi** > **Uygulamaları ve Hizmetleri Günlükleri** > **Microsoft** > **Windows** > **Hyper-V** > **Admin**Hyper-V Admin Hyper-V olay günlüğü açın.
    - Uygulama tutarlı anlık görüntü hatalarını gösteren olaylar olup olmadığını doğrulayın.
    - Tipik bir hata: "Hyper-V sanal makine 'XYZ' için VSS anlık ayar oluşturmak için başarısız oldu: Yazar geçici olmayan bir hata yaşadı. Hizmet yanıt vermiyorsa VSS hizmetini yeniden başlatmak sorunları çözebilir."

2. VM için VSS anlık görüntülerini oluşturmak için, Hyper-V Tümleştirme Hizmetlerinin VM'ye yüklü olup olmadığını ve Yedekleme (VSS) Tümleştirme Hizmetinin etkin olup olmadığını denetleyin.
    - Entegrasyon Hizmetleri VSS hizmetinin/daemonlarının konuk üzerinde çalıştığını ve **tamam** durumunda olduğundan emin olun.
    - **Get-VMIntegrationService -VMName VMName\<>-Name VSS** komutu ile Hyper-V ana bilgisayardaki yüksek bir PowerShell oturumundan bu bilgileri kontrol edebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - VM'deki Yedekleme/VSS tümleştirme Hizmetlerinin çalışıyor ve sağlıklı durumda olduğundan emin olun. Değilse, bu hizmetleri yeniden başlatın ve Hyper-V ana bilgisayar sunucusundaki Hyper-V Volume Shadow Copy istek veya hizmetini yeniden başlatın.

### <a name="common-errors"></a>Sık karşılaşılan hatalar

**Hata kodu** | **İleti** | **Şey**
--- | --- | ---
**0x800700EA** | "Hyper-V sanal makine için VSS anlık görüntü seti oluşturmak için başarısız oldu: Daha fazla veri kullanılabilir. (0x800700EA). Yedekleme işlemi devam ediyorsa VSS anlık görüntü kümesi oluşturma başarısız olabilir.<br/><br/> Sanal makine için çoğaltma işlemi başarısız oldu: Daha fazla veri kullanılabilir." | VM'nizin dinamik disk etkin olup olmadığını kontrol edin. Bu özellik desteklenmez.
**0x80070032** | "Hyper-V Volume Shadow Copy Requestor sanal makineye bağlanmayı başaramadı <./VMname> çünkü sürüm Hyper-V tarafından beklenen sürümle uyuşmuyor | En son Windows güncelleştirmelerinin yüklü olup olmadığını kontrol edin.<br/><br/> Tümleştirme Hizmetlerinin en son sürümüne [yükseltin.](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date)



## <a name="collect-replication-logs"></a>Çoğaltma günlüklerini toplama

Tüm Hyper-V çoğaltma olayı, **Uygulamalar ve Hizmetler Günlükleri** > **Microsoft** > **Windows'da**bulunan Hyper-V-VMMS\Admin günlüğünde günlüğe kaydedilir. Ayrıca, Hyper-V Sanal Makine Yönetimi Hizmeti için aşağıdaki gibi bir Analitik günlük etkinleştirebilirsiniz:

1. Olay Görüntüleyicisi'nde Analitik ve Hata Ayıklama günlüklerini görüntülenebilir hale getirin. Günlükleri kullanılabilir hale getirmek için Olay Görüntüleyicisi'nde **View Analytic** > **ve Hata Ayıklama Günlükleri'ni tıklatın.** Analytic günlüğü **Hyper-V-VMMS**altında görünür.
2. **Eylemler** bölmesinde **Günlük'u Etkinleştir'i**tıklatın. 

    ![Günlüğü etkinleştir](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Etkinleştirildikten sonra, Veri Toplayıcı Kümeleri altında Olay **İzleme Oturumu** olarak **Performans İzleyicisi'nde**görünür. **Data Collector Sets** 
4. Toplanan bilgileri görüntülemek için, günlüğü devre dışı bırakarak izleme oturumunu durdurun. Ardından günlüğü kaydedin ve Olay Görüntüleyicisi'nde yeniden açın veya gerektiğinde dönüştürmek için diğer araçları kullanın.


### <a name="event-log-locations"></a>Olay günlüğü konumları

**Olay günlüğü** | **Şey** |
--- | ---
**Uygulamalar ve Hizmet Günlükleri/Microsoft/VirtualMachineManager/Server/Admin** (VMM sunucusu) | VMM sorunlarını gidermek için günlükler.
**Uygulamalar ve Hizmet Günlükleri/MicrosoftAzureRecoveryServices/Replication** (Hyper-V ana bilgisayar) | Microsoft Azure Kurtarma Hizmetleri Aracısı sorunlarını gidermek için günlükler. 
**Uygulamalar ve Hizmet Günlükleri/Microsoft/Azure Site Kurtarma/Sağlayıcı/Operasyonel** (Hyper-V ana bilgisayar)| Microsoft Azure Site Kurtarma Hizmeti sorunlarını gidermek için günlükler.
**Uygulamalar ve Hizmet Günlükleri/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V ana bilgisayar) | Hyper-V VM yönetim sorunlarını gidermek için günlükler.

### <a name="log-collection-for-advanced-troubleshooting"></a>Gelişmiş sorun giderme için günlük koleksiyonu

Bu araçlar gelişmiş sorun giderme ile yardımcı olabilir:

-   VMM [için, Destek Tanılama Platformu (SDP) aracını](https://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx)kullanarak Site Kurtarma günlük koleksiyonunu gerçekleştirin.
-   VMM olmadan Hyper-V için, [bu aracı indirin](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab)ve günlükleri toplamak için Hyper-V ana bilgisayarda çalıştırın.

