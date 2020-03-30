---
title: Azure Site Kurtarmayı İzle | Microsoft Dokümanlar
description: Portalı kullanarak Azure Site Kurtarma çoğaltma sorunlarını ve işlemlerini izleme ve sorun giderme
author: raynew
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: raynew
ms.openlocfilehash: aa9d776df50306ab1705426c923413b5a5d545a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68717361"
---
# <a name="monitor-site-recovery"></a>Site Recovery’yi izleme

Bu makalede, Site Kurtarma dahili izlemeyi kullanarak Azure [Site Kurtarma'yı](site-recovery-overview.md)nasıl izleyeceğinizi öğrenin.  Bunu izleyebilirsiniz:

- Site Kurtarma tarafından çoğaltılan makinelerin durumu ve durumu
- Makinelerin başarısız durumunu test edin.
- Yapılandırma ve çoğaltmayı etkileyen sorunlar ve hatalar.
- Şirket içi sunucular gibi altyapı bileşenleri.


## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce [sık karşılaşılan izleme sorularını](monitoring-common-questions.md) gözden geçirmek isteyebilirsiniz.

## <a name="monitor-in-the-dashboard"></a>Panodaki monitör

1. Kasada Genel **Bakış'ı**tıklatın. Kurtarma Hizmetleri panosu, kasanın tüm izleme bilgilerini tek bir konumda birleştirir. Hem Site Kurtarma hem de Azure Yedekleme hizmeti için sayfalar vardır ve bunlar arasında geçiş yapabilirsiniz.

    ![Site Kurtarma panosu](./media/site-recovery-monitor-and-troubleshoot/dashboard.png)

2. Gösterge panelinden farklı alanlara inin. 

    ![Site Kurtarma panosu](./media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png).

3. **Yinelenen öğelerde,** kasadaki tüm sunucuları görmek için **Tümünü Görüntüle'yi** tıklatın.
4. Detaya inmek için her bölümdeki durum ayrıntılarını tıklatın.
5. **Altyapı görünümünde,** izleme bilgilerini çoğaltma küçülen makine türüne göre sıralayın.

## <a name="monitor-replicated-items"></a>Çoğaltılan öğeleri izleme

**Çoğaltılan öğelerde,** çoğaltma etkin olan kasadaki tüm makinelerin durumunu izleyin.

**Durum** | **Şey**
--- | ---
Sağlam | Çoğaltma normal ilerliyor. Hata veya uyarı belirti algılanmadı.
Uyarı | Çoğaltmayı etkileyebilecek bir veya daha fazla uyarı belirti algılanır.
Kritik | Bir veya daha fazla kritik çoğaltma hatası belirtileri algılandı.<br/><br/> Bu hata belirtileri genellikle çoğaltma sıkışmış veya veri değiştirme hızı kadar hızlı ilerleme değil göstergeleridir.
Uygulanamaz | Şu anda çoğaltılması beklenmeyen sunucular. Bu, üzerinde başarısız olan makineleri içerebilir.

## <a name="monitor-test-failovers"></a>Test hatalarını izleme

**Failover testi başarısında,** kasadaki makinelerin arıza durumunu izleyin.

- Çoğaltılmış makinelerde en az altı ayda bir test başarısızlığı yapmanızı öneririz. Bu, üretim ortamınızı bozmadan, failover'ın beklendiği gibi çalıştığını denetlemenin bir yoludur. 
- Bir test başarısızmı, yalnızca başarısız ve başarısız sonrası temizleme başarıyla tamamlandıktan sonra başarılı olarak kabul edilir.

**Durum** | **Şey**
--- | ---
Önerilen test | Koruma etkinleştirildiğinden beri test başarısız olmadı makineleri.
Başarıyla gerçekleştirildi | Veya daha başarılı test başarısız olan makineler.
Uygulanamaz | Şu anda bir test başarısızlığına uygun olmayan makineler. Örneğin, üzerinde başarısız olan makineler, ilk çoğaltma/test failover/failover devam ediyor.

## <a name="monitor-configuration-issues"></a>Yapılandırma sorunlarını izleme

**Yapılandırma sorunlarınızda,** başarılı bir şekilde başarısız olma yeteneğinizi etkileyebilecek sorunları izleyin.

- Yapılandırma sorunları (yazılım güncelleştirme kullanılabilirliği hariç), varsayılan olarak her 12 saatte bir çalışan bir periyodik doğrulama işlemi tarafından algılanır. **Yapılandırma sorunları** bölümünün yanındaki yenileme simgesini tıklatarak doğrulayıcı işlemini hemen çalıştırmaya zorlayabilirsiniz.
- Daha fazla bilgi almak için bağlantıları tıklatın. Belirli makineleri etkileyen sorunlar için **Hedef yapılandırmaları** sütununda **dikkat edilmesi gereken leri** tıklatın. Ayrıntılar düzeltme önerileri içerir.

**Durum** | **Şey**
--- | ---
Eksik yapılandırmalar | Kurtarma ağı veya kaynak grubu gibi gerekli bir ayar eksik.
Eksik kaynaklar | Belirtilen bir kaynak bulunamaz veya abonelikte kullanılamaz. Örneğin, kaynak silindi veya geçirildi. İzlenen kaynaklar hedef kaynak grubu, hedef VNet/subnet, günlük/hedef depolama hesabı, hedef kullanılabilirlik kümesi, hedef IP adresi dahil.
Abonelik kotası |  Kullanılabilir abonelik kaynak kotabakiyesi, kasadaki tüm makinelerde başarısız olmak için gereken bakiyeyle karşılaştırılır.<br/><br/> Yeterli kaynak yoksa, yetersiz kota bakiyesi bildirilir.<br/><br/> Kotalar VM çekirdek sayısı, VM aile çekirdek sayısı, ağ arabirim kartı (NIC) sayısı için izleniyor.
Yazılım güncelleştirmeleri | Yeni yazılım güncelleştirmelerinin kullanılabilirliği ve süresi dolan yazılım sürümleri hakkında bilgi.


## <a name="monitor-errors"></a>Hataları izleme

**Hata özetinde,** kasadaki sunucuların çoğaltılandırmalarını etkileyebilecek şu anda etkin olan hata belirtilerini izleyin ve etkilenen makinelerin sayısını izleyin.

- Şirket içi altyapı bileşenlerini etkileyen hatalar bölümün başlangıcıdır. Örneğin, şirket içi yapılandırma sunucusundaki Azure Site Kurtarma Sağlayıcısı'ndan veya Hyper-V ana bilgisayardan sinyal alınmaması.
- Ardından, çoğaltılan sunucuları etkileyen çoğaltma hatası belirtileri gösterilir.
- Tablo girişleri, hata önem derecesinin sırasını azaltarak ve etkilenen makinelerin sayı sırasını azaltarak sıralanır.
- Etkilenen sunucu sayısı, altta yatan tek bir sorunun birden çok makineyi etkileyip etkilemediğini anlamanın yararlı bir yoludur. Örneğin, bir ağ hatası Azure'a çoğalan tüm makineleri etkileyebilir. 
- Tek bir sunucuda birden çok çoğaltma hatası oluşabilir. Bu durumda, her hata belirti, etkilenen sunucular listesindeki sunucuyu sayar. Sorun giderildikten sonra çoğaltma parametreleri gelişir ve hata makineden temizlenir.

## <a name="monitor-the-infrastructure"></a>Altyapıyı izleyin.

**Altyapı görünümünde,** çoğaltma yla ilgili altyapı bileşenlerini ve sunucular ve Azure hizmetleri arasındaki bağlantı durumunu izleyin.

- Yeşil çizgi, bağlantının sağlıklı olduğunu gösterir.
- Overlaid hata simgesi ile kırmızı bir çizgi bağlantı etkileyen bir veya daha fazla hata belirtileri varlığını gösterir.
-  Hatayı ve etkilenen varlıkların sayısını göstermek için fare işaretçisini hata simgesinin üzerine türün. Etkilenen varlıkların filtrelenmiş bir listesi için simgeyi tıklatın.

    ![Site Kurtarma altyapı görünümü (kasa)](./media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

### <a name="tips-for-monitoring-the-infrastructure"></a>Altyapıyı izlemek için ipuçları

- Şirket içi altyapı bileşenlerinin (yapılandırma sunucusu, işlem sunucuları, VMM sunucuları, Hyper-V ana bilgisayarları, VMware makineleri) Site Kurtarma Sağlayıcısı'nın ve/veya aracıların en son sürümlerini çalıştırdığından emin olun.
- Altyapı görünümündeki tüm özellikleri kullanmak için, bu bileşenler için [Güncelleştirme rollup 22'yi](https://support.microsoft.com/help/4072852) çalıştırıyor olmalısınız.
- Altyapı görünümünü kullanmak için ortamınızdaki uygun çoğaltma senaryosunu seçin. Daha fazla bilgi için görünümde ayrıntılı bilgi edinebilirsiniz. Aşağıdaki tabloda hangi senaryoların temsil edildiği gösterilmektedir.

    **Senaryo** | **Durum**  | **Görünüm kullanılabilir?**
    --- |--- | ---
    **Şirket içi siteler arasında çoğaltma** | Tüm eyaletler | Hayır 
    **Azure bölgeleri arasında Azure VM çoğaltma**  | Çoğaltma etkin/ilk çoğaltma devam ediyor | Evet
    **Azure bölgeleri arasında Azure VM çoğaltma** | Başarısız over/fail geri | Hayır   
    **Azure’a VMware çoğaltma** | Çoğaltma etkin/ilk çoğaltma devam ediyor | Evet     
    **Azure’a VMware çoğaltma** | Başarısız oldu/geri başarısız oldu | Hayır      
    **Azure'a Hyper-V çoğaltması** | Başarısız oldu/geri başarısız oldu | Hayır

- Kasa menüsünde tek bir çoğaltma makinesinin altyapı görünümünü görmek için **Yinelenen öğeler'i**tıklatın ve bir sunucu seçin.  




## <a name="monitor-recovery-plans"></a>Kurtarma planlarını izleme

**Kurtarma planlarında,** plan sayısını izleyin, yeni planlar oluşturun ve varolanplanları değiştirin.  

## <a name="monitor-jobs"></a>İşleri izleme

**İşler'de,** Site Kurtarma işlemlerinin durumunu izleyin.

- Azure Site Kurtarma'daki işlemlerin çoğu eşzamanlı olarak yürütülür ve bir izleme işi oluşturulur ve işlemin ilerlemesini izlemek için kullanılır. 
- İş nesnesi durumu ve işlemin ilerlemesini izlemek için gereken tüm bilgilere sahiptir. 

İşleri aşağıdaki gibi izleyin:

1. Pano > **İşler** bölümünde, son 24 saat içinde tamamlanmış, devam eden veya giriş bekleyen işlerin bir özetini görebilirsiniz. İlgili işler hakkında daha fazla bilgi almak için herhangi bir durumu tıklayabilirsiniz.
2. Son 24 saat içinde tüm işleri görmek için **Tümlerini** Görüntüle'yi tıklatın.

    > [!NOTE]
    > **Ayrıca, Site Kurtarma İşleri**> kasa menüsünden iş bilgilerine de erişebilirsiniz. 

2. Site **Kurtarma İşleri** listesinde, işlerin listesi görüntülenir. Üst menüde belirli işler için hata ayrıntıları alabilir, iş listesini belirli ölçütlere göre filtreleyebilir ve seçili iş ayrıntılarını Excel'e aktarabilirsiniz.
3. Bir işi tıklayarak delebilirsiniz. 

## <a name="monitor-virtual-machines"></a>Sanal makineleri izleme

**Çoğaltılan öğelerde,** çoğaltılan makinelerin listesini alın. 
    ![Site Kurtarma çoğaltılan öğeler liste görünümü](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

2. Bilgileri görüntüleyebilir ve filtreleyebilirsiniz. Üstteki eylem menüsünde, test hatası çalıştırma veya belirli hataları görüntüleme gibi belirli bir makine için eylemler gerçekleştirebilirsiniz.
3. Ek **Columns** sütunları göstermek için Sütunlar'ı tıklatın, Örneğin RPO'u, hedef yapılandırma sorunlarını ve çoğaltma hatalarını göstermek için.
4. Çoğaltma durumu veya belirli bir çoğaltma ilkesi gibi belirli parametreleri temel alan bilgileri görüntülemek için **Filtre'yi** tıklatın.
5. Test hatası gibi işlemleri başlatmak veya onunla ilişkili belirli hata ayrıntılarını görüntülemek için bir makineye sağ tıklayın.
6. Bunun için daha fazla ayrıntı yıkmak için bir makineyi tıklatın. Ayrıntılar şunlardır:
   - **Çoğaltma bilgileri**: Makinenin geçerli durumu ve durumu.
   - **RPO** (kurtarma noktası hedefi): Sanal makine için geçerli RPO ve RPO'nun en son hesaplandığı zaman.
   - **Kurtarma noktaları**: Makine için kullanılabilir en son kurtarma noktaları.
   - **Failover hazırlık**: Makine için bir test başarısızlığının çalışıp çalışmadığını, makinede çalışan aracı sürümünü (Mobilite hizmetini çalıştıran makineler için) ve yapılandırma sorunlarını gösterir.
   - **Hatalar**: Şu anda makinede gözlenen çoğaltma hatası belirtileri ve olası nedenleri/eylemleri listesi.
   - **Olaylar**: Makineyi etkileyen son olayların kronolojik listesi. Hata ayrıntıları şu anda gözlemlenebilir hata belirtilerini gösterirken, olaylar makineyi etkileyen sorunların tarihsel bir kaydıdır.
   - **Altyapı görünümü**: Makineler Azure'a çoğalırken senaryonun altyapı durumunu gösterir.

     ![Site Kurtarma çoğaltılmış öğe ayrıntıları/genel bakış](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

## <a name="subscribe-to-email-notifications"></a>E-posta bildirimlerine abone olun

Bu kritik olaylar için e-posta bildirimleri almak için abone olabilirsiniz:
 
- Çoğaltılan makine için kritik durum.
- Şirket içi altyapı bileşenleri ile Site Kurtarma hizmeti arasında bağlantı yoktur. Site Kurtarma ile kasada kayıtlı şirket içi sunucular arasındaki bağlantı, bir sinyal mekanizması kullanılarak algılanır.
- Failover hataları.

Aşağıdaki şekilde abone olabilirsiniz:

Tonoz > **İzleme** **bölümünde, Site Kurtarma Etkinlikleri'ni**tıklatın.
1. **E-posta bildirimleri**'ne tıklayın.
1. **E-posta bildiriminde,** bildirimleri açın ve kime göndereceğini belirtin. Tüm abonelik yöneticilerine bildirim ve isteğe bağlı olarak belirli e-posta adresleri gönderebilirsiniz.

    ![E-posta bildirimleri](./media/site-recovery-monitor-and-troubleshoot/email.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Monitor ile Site Kurtarma'yı izleme [hakkında bilgi edinin.](monitor-log-analytics.md)
