---
title: Azure Site Kurtarma ile VMware olağanüstü durum kurtarma için plan kapasitesi
description: Bu makale, Azure Site Kurtarma'yı kullanarak VMware VM'lerin Azure'a olağanüstü kurtarma sını ayarladığınızda kapasite ve ölçekleme planlamanıza yardımcı olabilir.
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 4/9/2019
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 467c70a722b8a243be6ac2826188a4ba3459aa06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257621"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>VMware olağanüstü durum kurtarma için kapasite ve ölçekleme planlama

Şirket içi VMware VM'leri ve fiziksel sunucuları [Azure'da Azure'da Azure'da Azure'da Azure'da Azure'da Azure'da](site-recovery-overview.md)Azure'da azure'da çoğaltırken kapasite ve ölçekleme planlamak için bu makaleyi kullanın.

## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlamasına nasıl başlarım?

Azure Site Kurtarma altyapı gereksinimleri hakkında bilgi edinmek için, VMware çoğaltma için [Azure Site Kurtarma Dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner-doc) çalıştırarak çoğaltma ortamınız hakkında bilgi toplayın. Daha fazla bilgi için [VMware to Azure için Site Kurtarma Dağıtım Planlayıcısı hakkında](site-recovery-deployment-planner.md)bilgi içinbkz. 

Site Kurtarma Dağıtım Planlayıcısı, uyumlu ve uyumsuz VM'ler, VM başına diskler ve disk başına veri karmaşası hakkında tam bilgilere sahip bir rapor sağlar. Araç ayrıca, hedef RPO'yu ve başarılı çoğaltma ve test başarısızolması için gereken Azure altyapısını karşılamak için ağ bant genişliği gereksinimlerini de özetler.

## <a name="capacity-considerations"></a>Kapasiteye ilişkin önemli noktalar

Bileşen | Ayrıntılar
--- | ---
**Çoğaltma** | **Maksimum günlük değişim hızı**: Korumalı bir makine yalnızca bir işlem sunucusu kullanabilir. Tek bir işlem sunucusu günlük 2 TB'a kadar olan değişiklik oranını işleyebilir. Bu nedenle, 2 TB, korunan bir makine için desteklenen maksimum günlük veri değişim oranıdır.<br /><br /> **Maksimum iş verimi**: Çoğaltılan bir makine Azure'daki bir depolama hesabına ait olabilir. Standart bir Azure Depolama hesabı saniyede en fazla 20.000 istek işleyebilir. Bir kaynak makinedeki saniyede (IOPS) giriş/çıkış işlemleri sayısını 20.000 ile sınırlamanızı öneririz. Örneğin, beş diski olan bir kaynak makineniz varsa ve her disk kaynak makinede 120 IOPS (8 K boyutunda) oluşturuyorsa, kaynak makine 500'den oluşan Azure disk başına IOPS sınırı içindedir. (Gerekli depolama hesabı sayısı, IOPS'nin 20.000'e bölünen toplam kaynak makinesine eşittir.)
**Yapılandırma Sunucusu** | Yapılandırma sunucusu, korunan makinelerde çalışan tüm iş yüklerinde günlük değişim oranı kapasitesini işleyebilmelidir. Yapılandırma makinesinin verileri Azure Depolama'ya sürekli olarak çoğaltmak için yeterli bant genişliğine sahip olması gerekir.<br /><br /> En iyi yöntem, yapılandırma sunucusunu korumak istediğiniz makinelerle aynı ağa ve LAN segmentine yerleştirmektir. Yapılandırma sunucusunu farklı bir ağa yerleştirebilirsiniz, ancak korumak istediğiniz makinelerin katman 3 ağ görünürlüğüne sahip olması gerekir.<br /><br /> Yapılandırma sunucusu için boyut önerileri aşağıdaki bölümde tabloda özetlenmiştir.
**İşlem sunucusu** | İlk işlem sunucusu yapılandırma sunucusuna varsayılan olarak yüklenir. Ortamınızı ölçeklendirmek için ek işlem sunucuları dağıtabilirsiniz. <br /><br /> İşlem sunucusu, korumalı makinelerden çoğaltma verileri alır. İşlem sunucusu önbelleğe alma, sıkıştırma ve şifreleme kullanarak verileri en iyi duruma getirir. Ardından, işlem sunucusu verileri Azure'a gönderir. İşlem sunucusu makinesinin bu görevleri gerçekleştirmek için yeterli kaynağa sahip olması gerekir.<br /><br /> İşlem sunucusu disk tabanlı bir önbellek kullanır. Ağ darboğaz veya kesinti oluşursa depolanan veri değişikliklerini işlemek için 600 GB veya daha fazla ayrı bir önbellek diski kullanın.

## <a name="size-recommendations-for-the-configuration-server-and-inbuilt-process-server"></a>Yapılandırma sunucusu ve dahili işlem sunucusu için boyut önerileri

İş yükünü korumak için dahili bir işlem sunucusu kullanan bir yapılandırma sunucusu, aşağıdaki yapılandırmaları temel alan en fazla 200 sanal makineyi işleyebilir:

CPU | Bellek | Önbellek disk boyutu | Veri değişim oranı | Korumalı makineler
--- | --- | --- | --- | ---
8 vCPUs (2 soket \@ * 4 çekirdek 2.5 GHz) | 16 GB | 300 GB | 500 GB veya daha az | 100'den az makineyi çoğaltmak için kullanın.
12 vCPUs (2 soket \@ * 6 çekirdek 2.5 GHz) | 18 GB | 600 GB | 501 GB - 1 TB | 100 ila 150 makineyi çoğaltmak için kullanın.
16 vCPUs (2 soket \@ * 8 çekirdek 2.5 GHz) | 32 GB | 1 TB | >1 TB - 2 TB | 151 ile 200 makineyi çoğaltmak için kullanın.
[OVF şablonu](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template)kullanarak başka bir yapılandırma sunucusudağıtın. | | | | 200'den fazla makineyi çoğaltıyorsanız yeni bir yapılandırma sunucusu dağıtın.
Başka bir [işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file)dağıtın. | | | >2 TB| Genel günlük veri değiştirme oranı 2 TB'den büyükse yeni bir ölçeklendirme işlem sunucusu dağıtın.

Bu yapılandırmalarda:

* Her kaynak makinenin her biri 100 GB'lık üç diski vardır.
* Önbellek disk ölçümleri için RAID 10 ile 10 K RPM'lik sekiz paylaşılan erişim imza sürücüsünün kıyaslama depolamasına neden olduk.

## <a name="size-recommendations-for-the-process-server"></a>İşlem sunucusu için boyut önerileri

İşlem sunucusu, Azure Site Kurtarma'da veri çoğaltma işlemini işleyen bileşendir. Günlük değişim oranı 2 TB'den büyükse, çoğaltma yükünü işlemek için ölçeklendirme işlem sunucuları eklemeniz gerekir. Ölçeklendirmek için şunları yapabilirsiniz:

* Ovf [şablonu](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template)kullanarak dağıtarak yapılandırma sunucularının sayısını artırın. Örneğin, iki yapılandırma sunucusu kullanarak en fazla 400 makineyi koruyabilirsiniz.
* [Ölçeklendirme işlem sunucuları](vmware-azure-set-up-process-server-scale.md#download-installation-file)ekleyin. Yapılandırma sunucusu yerine çoğaltma trafiğini işlemek için ölçeklendirme işlem sunucularını kullanın.

Aşağıdaki tabloda bu senaryo açıklanmaktadır:

* Bir ölçeklendirme işlem sunucusu ayarlarsınız.
* Ölçeklendirme işlem sunucusunu kullanmak için korumalı sanal makineleri yapılandırıldınız.
* Her korumalı kaynak makinenin her biri 100 GB'lık üç diski vardır.

Ek işlem sunucusu | Önbellek disk boyutu | Veri değişim oranı | Korumalı makineler
--- | --- | --- | ---
4 vCPUs (2 soket \@ * 2 çekirdek 2,5 GHz), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 veya daha az makineçoğaltmak için kullanın.
8 vCPUs (2 soket \@ * 4 çekirdek 2,5 GHz), 12 GB bellek | 600 GB | 251 GB - 1 TB | 86 ila 150 makineleri çoğaltmak için kullanın.
12 vCPUs (2 soket \@ * 6 çekirdek 2,5 GHz) 24 GB bellek | 1 TB | >1 TB - 2 TB | 151 ile 225 makineyi çoğaltmak için kullanın.

Sunucularınızı nasıl ölçeklendirdiğiniz, ölçeklendirme veya ölçeklendirme modeli tercihinize bağlıdır. Ölçeklendirmek için birkaç üst düzey yapılandırma sunucusu dağıtın ve sunucuları işle. Ölçeklendirmek için, daha az kaynağa sahip daha fazla sunucu dağıtın. Örneğin, 200 makineyi günlük veri değişim oranı 1,5 TB ile korumak istiyorsanız, aşağıdaki eylemlerden birini alabilirsiniz:

* Tek bir işlem sunucusu (16 vCPU, 24 GB RAM) ayarlayın.
* İki işlem sunucusu (2 x 8 vCPU, 2* 12 GB RAM) ayarlayın.

## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetleme

Çoğaltma için gereken bant genişliğini (ilk çoğaltma ve ardından delta) hesaplamak için [Site Kurtarma Dağıtım Planlayıcısı'nı](site-recovery-deployment-planner.md) kullandıktan sonra, çoğaltma için kullanılan bant genişliği miktarını denetlemek için birkaç seçeneğiniz vardır:

* **Throttle bant genişliği**: Azure'a çoğalan VMware trafiği belirli bir işlem sunucusundan geçer. İşlem sunucusu olarak çalışan makinelerde bant genişliğini daraltabilirsiniz.
* **Etki bant genişliği**: Birkaç kayıt defteri anahtarı kullanarak çoğaltma için kullanılan bant genişliğini etkileyebilirsiniz:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Çoğaltma\UploadThreadsPerVM** kayıt defteri değeri, bir diskin veri aktarımı (ilk veya delta çoğaltma) için kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer çoğaltma için kullanılan ağ bant genişliğini artırır.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Çoğaltma\DownloadThreadsPerVM** kayıt defteri değeri, failback sırasında veri aktarımı için kullanılan iş parçacığı sayısını belirtir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama

1. İşlem sunucusu olarak kullandığınız makinede Azure Yedekleme MMC snap-in'ini açın. Varsayılan olarak, Yedekleme için bir kısayol masaüstünde veya aşağıdaki klasörde kullanılabilir: C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\bin.
2. Snap-in'de **Özellikleri Değiştir'i**seçin.

    ![Özellikleri değiştirmek için Azure Yedekleme MMC snap-in seçeneğinin ekran görüntüsü](./media/site-recovery-vmware-to-azure/throttle1.png)
3. **Azaltma** sekmesinde, **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir'i**seçin. Çalışma ve çalışma dışı saatler için sınırları belirleyin. Geçerli aralıklar 512 Kbps ile 1.023 Mbps arasındadır.

    ![Azure Yedekleme Özellikleri iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bir örneği aşağıda verilmiştir:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="alter-the-network-bandwidth-for-a-vm"></a>VM için ağ bant genişliğini değiştirme

1. VM'nin kayıt defterinde, **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Çoğaltma'ya**gidin.
   * Çoğaltma diskindeki bant genişliği trafiğini değiştirmek için **UploadThreadsPerVM**değerini değiştirin. Yoksa anahtarı oluşturun.
   * Azure'dan gelen geri dönüş trafiği için bant genişliğini değiştirmek için **DownloadThreadsPerVM**değerini değiştirin.
2. Her anahtar için varsayılan **4**değer 4'dür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Kullanabileceğiniz maksimum değer **32'dir.** Değeri iyileştirmek için trafiği izleyin.

## <a name="set-up-the-site-recovery-infrastructure-to-protect-more-than-500-vms"></a>500'den fazla VM'yi korumak için Site Kurtarma altyapısını ayarlama

Site Kurtarma altyapısını ayarlamadan önce, aşağıdaki faktörleri ölçmek için ortama erişin: uyumlu sanal makineler, günlük veri değişim oranı, ulaşmak istediğiniz RPO için gerekli ağ bant genişliği, Site Kurtarma sayısı gereken bileşenler ve ilk çoğaltmayı tamamlamak için gereken süre. Gerekli bilgileri toplamak için aşağıdaki adımları tamamlayın:

1. Bu parametreleri ölçmek için, ortamınızda Site Kurtarma Dağıtım Planlayıcısı'ni çalıştırın. Yararlı yönergeler için, [VMware to Azure için Site Kurtarma Dağıtım Planlayıcısı hakkında'ya](site-recovery-deployment-planner.md)bakın.
2. Yapılandırma sunucusu için boyut önerilerini karşılayan bir [yapılandırma sunucusu dağıtın.](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) Üretim iş yükünüz 650 sanal makineyi aşıyorsa, başka bir yapılandırma sunucusu dağıtın.
3. Ölçülen günlük veri değişim hızına bağlı olarak, [ölçeklendirme işlem sunucularını](vmware-azure-set-up-process-server-scale.md#download-installation-file) [boyut yönergeleri](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server)yardımıyla dağıtın.
4. Bir disk sanal makinesinin veri değiştirme oranının 2 MBp'yi aşmasını bekliyorsanız, premium yönetilen diskler kullandığınızdan emin olun. Site Kurtarma Dağıtım Planlayıcısı belirli bir süre için çalışır. Diğer zamanlarda veri değişim hızındaki zirveler raporda yakalanabilir.
5. Ulaşmak istediğiniz RPO'ya göre [ağ bant genişliğini ayarlayın.](site-recovery-plan-capacity-vmware.md#control-network-bandwidth)
6. Altyapı ayarlandığında, iş yükünüz için olağanüstü durum kurtarmayı etkinleştirin. Nasıl yapılacağını öğrenmek [için vmware için kaynak ortamını Azure çoğaltmaya ayarlama konusuna](vmware-azure-set-up-source.md)bakın.

## <a name="deploy-additional-process-servers"></a>Ek işlem sunucularını dağıtma

Dağıtımınızı 200 kaynak makinenin ötesine ölçeklendiriyorsanız veya toplam günlük çalkalama oranınız 2 TB'den fazlaysa, trafik hacmini işlemek için işlem sunucuları eklemeniz gerekir. Ürünü 9.24 sürümünde geliştirtik ve ölçeklendirme li bir işlem sunucusu nun ne zaman ayarlanışlarına ilişkin [işlem sunucusu uyarıları sağladık.](vmware-physical-azure-monitor-process-server.md#process-server-alerts) Yeni kaynak makineleri korumak veya yükü [dengelemek](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load)için işlem [sunucusunu ayarlayın.](vmware-azure-set-up-process-server-scale.md)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Yeni işlem sunucusunu kullanmak için makineleri geçirin

1. **Ayarlar** > **Sitesi Kurtarma sunucularını**seçin. Yapılandırma sunucusunu seçin ve ardından **İşlem sunucularını**genişletin.

    ![İşlem Sunucusu iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Şu anda kullanılmakta olan işlem sunucusuna sağ tıklayın ve sonra **Değiştir'i**seçin.

    ![Configuration server iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. **Hedef işlem sunucusu seç'te,** kullanmak istediğiniz yeni işlem sunucusunu seçin. Ardından, sunucunun işleyeceğini sanal makineleri seçin. Sunucu hakkında bilgi almak için bilgi simgesini seçin. Yük kararları vermenize yardımcı olmak için, seçili her sanal makineyi yeni işlem sunucusuna çoğaltmak için gereken ortalama alan gösterilir. Yeni işlem sunucusunda çoğaltmaya başlamak için onay işaretini seçin.

## <a name="deploy-additional-master-target-servers"></a>Ek ana hedef sunucuları dağıtma

Aşağıdaki senaryolarda, birden fazla ana hedef sunucu gereklidir:

*   Linux tabanlı bir sanal makineyi korumak istiyorsunuz.
*   Yapılandırma sunucusunda bulunan ana hedef sunucunun VM veri deposuna erişimi yoktur.
*   Ana hedef sunucudaki toplam disk sayısı (sunucudaki yerel disk sayısı ve korunacak disk sayısı) 60 diskten büyüktür.

Linux tabanlı bir sanal makine için ana hedef sunucunun nasıl ekleyeceğiniöğrenmek [için](vmware-azure-install-linux-master-target.md)bkz.

Windows tabanlı bir sanal makine için ana hedef sunucu eklemek için:

1. Kurtarma **Hizmetleri Vault** > **Site Kurtarma Altyapı** > **Yapılandırma sunucularına**gidin.
2. Gerekli yapılandırma sunucusunu seçin ve ardından **Master Target Server'ı**seçin.

    ![Ana Hedef Sunucu Ekle düğmesini gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Birleşik kurulum dosyasını karşıdan yükleyin ve ana hedef sunucuyu kurmak için dosyayı VM'de çalıştırın.
4. **Ana hedefi Yükle'yi** > **Seçin İleri**.

    ![Ana hedef seçeneğini seçerek gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Varsayılan yükleme konumunu seçin ve sonra **Yükle'yi**seçin.

     ![Varsayılan yükleme konumunu gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Ana hedefi yapılandırma sunucusuna kaydetmek **için Yapılandırmaya Devam Et'i**seçin.

    ![Yapılandırmaya Devam düğmesini gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Yapılandırma sunucusunun IP adresini girin ve ardından parolayı girin. Parola oluşturmayı öğrenmek için [bkz.](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) 

    ![Yapılandırma sunucusu için IP adresinin ve parolanın nereye girilen yeri gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. **Kaydol**’u seçin. Kayıt **tamamlandığında, Bitiş'i**seçin.

Kayıt başarıyla bittiğinde, sunucu Yapılandırma sunucusunun ana hedef sunucularında **Recovery Services Vault** > **Site Recovery Infrastructure** > **Configuration sunucularında**Azure portalında listelenir.

 > [!NOTE]
 > Windows için ana [hedef sunucu birleşik kurulum dosyasının](https://aka.ms/latestmobsvc)en son sürümünü indirin.

## <a name="next-steps"></a>Sonraki adımlar

[Site Kurtarma Dağıtım Planlayıcısı'nı](https://aka.ms/asr-deployment-planner)indirin ve çalıştırın.
