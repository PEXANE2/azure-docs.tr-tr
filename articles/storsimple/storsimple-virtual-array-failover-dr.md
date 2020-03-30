---
title: StorSimple Virtual Array için failover ve olağanüstü durum kurtarma
description: StorSimple Virtual Array'inizin nasıl başarısız olduğu hakkında daha fazla bilgi edinin.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 464fa05f658dd6e6e25d79f8840ceeb939383149
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77467224"
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Azure portalı aracılığıyla StorSimple Sanal Diziniz için olağanüstü durum kurtarma ve cihaz yükü devretme

## <a name="overview"></a>Genel Bakış
Bu makalede, microsoft Azure StorBasit Sanal Dizinizin olağanüstü durum kurtarma durumu, başka bir sanal dizide başarısız olmak için ayrıntılı adımları da içeren açıklanmaktadır. Bir failover, verilerinizi veri merkezindeki bir *kaynak* aygıttan *hedef* aygıta taşımanızı sağlar. Hedef aygıt aynı veya farklı bir coğrafi konumda bulunabilir. Cihaz arızası tüm aygıt içindir. Başarısız olma sırasında, kaynak aygıtın bulut verileri sahipliğini hedef aygıtın sahipliğini değiştirir.

Bu makale yalnızca StorSimple Sanal Diziler için geçerlidir. 8000 serisi bir cihazda başarısız olmak [için, StorSimple cihazınızın Aygıt arızave olağanüstü kurtarma bölümüne](storsimple-device-failover-disaster-recovery.md)gidin.

## <a name="what-is-disaster-recovery-and-device-failover"></a>Olağanüstü durum kurtarma ve aygıt arızası nedir?

Olağanüstü durum kurtarma (DR) senaryosunda, birincil aygıt çalışmayı durdurur. Bu senaryoda, başarısız aygıtla ilişkili bulut verilerini başka bir aygıta taşıyabilirsiniz. Birincil aygıtı *kaynak* olarak kullanabilir ve *hedef*olarak başka bir aygıt belirtebilirsiniz. Bu işlem *failover*olarak adlandırılır. Başarısız olma sırasında, kaynak aygıttaki tüm birimler veya hisseler sahipliği ni değiştirir ve hedef aygıta aktarılır. Verilerin filtrelanmasına izin verilmez.

DR, ısı haritası tabanlı katmanlama ve izleme kullanılarak tam bir cihaz geri yüklemesi olarak modellenmiştir. Isı haritası, okuma ve yazma desenlerine dayalı verilere Bir ısı değeri atayarak tanımlanır. Bu ısı haritası daha sonra yerel katmanda yüksek ısı (en çok kullanılan) veri öbek tutarken buluta ilk en düşük ısı veri parçaları katmanları. Dr sırasında StorSimple, buluttaki verileri geri yüklemek ve yeniden sulamak için ısı haritasını kullanır. Aygıt, son yedeklemedeki tüm birimleri/paylaşımları (dahili olarak belirlendiği gibi) getirir ve bu yedeklemeden geri yükleme gerçekleştirir. Sanal dizi tüm DR işlemini yönetir.

> [!IMPORTANT]
> Kaynak aygıt aygıt failover sonunda silinir ve bu nedenle bir failback desteklenmez.
> 
> 

Olağanüstü durum kurtarma, cihaz arıza özelliği ile planlanır ve **Cihazlar** bıçağından başlatılır. Bu bıçak, StorSimple Device Manager hizmetinize bağlı tüm StorSimple aygıtlarını titreştirer. Her aygıt için, dostu adı, durumu, verilen ve maksimum kapasiteyi, türünü ve modelini görebilirsiniz.

## <a name="prerequisites-for-device-failover"></a>Cihaz yük devretme önkoşulları

### <a name="prerequisites"></a>Ön koşullar

Bir aygıt başarısızlığı için, aşağıdaki ön koşulların karşılandığından emin olun:

* Kaynak aygıtın **devre dışı bırakılmış** durumda olması gerekir.
* Hedef aygıtın Azure portalında **kuruluma hazır** olarak gösterilmesi gerekir. Aynı veya daha yüksek kapasitede bir hedef sanal dizi sağlama. Hedef sanal diziyi yapılandırmak ve başarıyla kaydetmek için yerel web UI'sini kullanın.
  
  > [!IMPORTANT]
  > Kayıtlı sanal aygıtı hizmet aracılığıyla yapılandırmaya çalışmayın. Hizmet aracılığıyla hiçbir aygıt yapılandırması gerçekleştirilmemelidir.
  > 
  > 
* Hedef aygıt kaynak aygıtla aynı ada sahip olamaz.
* Kaynak ve hedef aygıt aynı türde olmalıdır. Yalnızca başka bir dosya sunucusu için bir dosya sunucusu olarak yapılandırılan sanal bir dizi üzerinde başarısız olabilir. Aynı durum bir iSCSI sunucusu için de geçerlidir.
* Bir dosya sunucusu DR için, hedef aygıtı kaynakla aynı etki alanına katılmanızı öneririz. Bu yapılandırma, paylaşım izinlerinin otomatik olarak çözülmesini sağlar. Yalnızca aynı etki alanında bir hedef aygıta failover desteklenir.
* DR için kullanılabilir hedef aygıtlar, kaynak aygıtla karşılaştırıldığında aynı veya daha büyük kapasiteye sahip aygıtlardır. Hizmetinize bağlı olan ancak yeterli alan ölçütlerini karşılamayan aygıtlar hedef aygıtlar olarak kullanılamaz.

### <a name="other-considerations"></a>Diğer konular

* Planlı bir arıza için:
  
  * Kaynak aygıttaki tüm birimleri veya paylaşımları çevrimdışı almanızı öneririz.
  * Veri kaybını en aza indirmek için aygıtın yedeğini almanızı ve ardından başarısız olmaya devam etmenizi öneririz.
* Planlanmamış bir hata için aygıt, verileri geri yüklemek için en son yedeklemeyi kullanır.

### <a name="device-failover-prechecks"></a>Cihaz arıza ön kontrolleri

DR başlamadan önce, cihaz ön kontrolleri gerçekleştirir. Bu denetimler, DR başladığında hata oluşmamasını sağlamaya yardımcı olur. Ön kontroller şunlardır:

* Depolama hesabını doğruluyor.
* Azure'a bulut bağlantısını denetleme.
* Hedef aygıttaki kullanılabilir alanı kontrol etme.
* iSCSI sunucu kaynak aygıt biriminin
  
  * geçerli ACR adları.
  * geçerli IQN (220 karakteri geçmez).
  * geçerli CHAP şifreleri (12-16 karakter uzunluğunda).

Önceki ön kontrollerden herhangi biri başarısız olursa, DR ile devam edemezsiniz. Bu sorunları giderin ve dr yeniden deneyin.

DR başarıyla tamamlandıktan sonra, kaynak aygıttaki bulut verilerinin sahipliği hedef aygıta aktarılır. Kaynak aygıt artık portalda kullanılamaz. Kaynak aygıttaki tüm birimlere/paylaşımlara erişim engellenir ve hedef aygıt etkin hale gelir.

> [!IMPORTANT]
> Aygıt artık kullanılamasa da, ana bilgisayar sisteminde sağlanan sanal makine hala kaynak tüketiyor. DR başarıyla tamamlandıktan sonra, bu sanal makineyi ana bilgisayar sisteminizden silebilirsiniz.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Sanal bir dizide başarısız

Bu yordamı çalıştırmadan önce StorSimple Device Manager hizmetinize başka bir StorSimple Virtual Array sağlamanızı, yapılandırmanızı ve kaydetmenizi öneririz.

> [!IMPORTANT]
> 
> * StorSimple 8000 serisi bir cihazdan 1200 sanal aygıta geçemezsiniz.
> * Federal Bilgi İşlem Standardı (FIPS) etkinleştirilmiş bir sanal cihazdan başka bir FIPS etkinleştirilmiş aygıta veya Hükümet portalında dağıtılan FIPS olmayan bir aygıta geçebilirsiniz.


Aygıtı hedef StorSimple sanal aygıtına geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. [Aygıt başarısızlığı için ön koşulları](#prerequisites)karşılayan bir hedef aygıtı sağlama ve yapılandırma. Yerel web kullanıcı aracı üzerinden aygıt yapılandırmasını tamamlayın ve StorSimple Device Manager hizmetinize kaydedin. Bir dosya sunucusu oluşturuyorsanız, dosya sunucusu olarak ayarlanan adım [1'e](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device)gidin. Bir iSCSI sunucusu oluşturuyorsanız, [iSCSI sunucusu olarak ayarlanan](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device)adım 1'e gidin.

2. Ana bilgisayarda birimleri/paylaşımları çevrimdışı alın. Birimleri/paylaşımları çevrimdışına almak için, ana bilgisayar için işletim sistemine özgü yönergelere bakın. Zaten çevrimdışı değilse, aşağıdakileri yaparak aygıttaki tüm birimleri/paylaşımları çevrimdışı almanız gerekir.
   
    1. **Cihazlar** blade gidin ve cihazınızı seçin.
   
    2. > **Paylaşımları Yönet > Ayarlar'a** gidin (veya > Birimleri > **Ayarları).** 
   
    3. Bir paylaşım/birim seçin, sağ tıklatın ve **çevrimdışı kaldır'ı**seçin. 
   
    4. Onay istendiğinde, bu paylaşımı çevrimdışı almanın etkisini anladığımı kontrol **edin.** 
   
    5. **Çevrimdışı Kaldır'ı**tıklatın.

3. StorSimple Device Manager hizmetinizde, **Yönetim > Cihazları'na**gidin. **Cihazlar** bıçağında kaynak cihazınızı seçin ve tıklatın.

4. **Cihazınızın pano bıyış** bıçağında **Devre dışı bırak'ı**tıklatın.

5. Devre **Dışı Bırakma** bıçağında, onay için istenir. Aygıt devre dışı bırakılsın, geri alınamayan *kalıcı* bir işlemdir. Ayrıca, paylaşımlarınızı/birimlerinizi ana bilgisayarda çevrimdışı almanız da hatırlatılır. Devre dışı'yı onaylamak için aygıt adını yazın ve **devre dışı bırakın'ı**tıklatın.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. Devre dışı bırakma başlar. Devre dışı bırakma başarıyla tamamlandıktan sonra bir bildirim alırsınız.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Aygıtlar sayfasında, aygıt durumu artık **Devre Dışı Bırakılmış**olarak değişecektir.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. **Cihazlar** bıçağında, devre dışı bırakılan kaynak aygıtı seçin ve tıklatın. 
9. Aygıt **pano** çubuğunda, **Üzerinde Başarısız'ı**tıklatın. 
10. Aygıt bıçak **üzerinde Fail'de** aşağıdakileri yapın:
    
    1. Kaynak aygıt alanı otomatik olarak doldurulur. Kaynak aygıtın toplam veri boyutuna dikkat edin. Veri boyutu, hedef aygıttaki kullanılabilir kapasiteden daha az olmalıdır. Aygıt adı, toplam kapasite ve üzerinde başarısız olan paylaşımların adları gibi kaynak aygıtla ilişkili ayrıntıları gözden geçirin.

    2. Kullanılabilir aygıtların açılır listesinden bir **Hedef aygıtı**seçin. Açılan listede yalnızca yeterli kapasiteye sahip aygıtlar görüntülenir.

    3. Bu **işlemin hedef aygıta veri üzerinde başarısız olacağını anladığımı**kontrol edin. 

    4. **Üzerinden Başarısız'ı**tıklatın.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Başarısız bir iş başlatır ve bir bildirim alırsınız. Failover'ı izlemek için **Aygıtlar > İşler'e** gidin.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. **İşler** bıçağında, kaynak aygıt için oluşturulan bir başarısız iş görürsünüz. Bu iş DR ön kontrollerini gerçekleştirir.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     DR ön denetimleri başarılı olduktan sonra, başarısız iş kaynak cihazınızda bulunan her paylaşım/birim için geri yükleme işleri yumurtlar.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Arıza tamamlandıktan sonra **Cihazlar** bıçağına gidin.
    
    1. Başarısızlık işlemi için hedef aygıt olarak kullanılan StorSimple aygıtını seçin ve tıklatın.
    2. **Paylaşımlar > Ayarlar > Yönetimi'ne** (veya iSCSI sunucusuvarsa **Birimlere)** gidin. **Paylaşımlar** bıçağında, eski cihazdaki tüm paylaşımları (birimleri) görüntüleyebilirsiniz.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Bağlanmaya çalışan tüm uygulamaların yeni aygıta yönlendirilebilmeleri için [bir DNS takma adı oluşturmanız](https://support.microsoft.com/kb/168322) gerekir.

## <a name="errors-during-dr"></a>DR sırasında hatalar

**DR sırasında bulut bağlantısı kesintisi**

DR başladıktan sonra ve aygıt geri yüklemesi tamamlanmadan önce bulut bağlantısı bozulursa, DR başarısız olur. Bir hata bildirimi alırsınız. DR için hedef aygıt *kullanılamaz* olarak işaretlenir. Gelecekteki DR'ler için aynı hedef aygıtı kullanamazsınız.

**Uyumlu hedef aygıtlar yok**

Kullanılabilir hedef aygıtların yeterli alanı yoksa, uyumlu hedef aygıtlar olmadığı nda bir hata görürsünüz.

**Ön kontrol hataları**

Ön denetimlerden biri karşılanmadıysa, ön kontrol hataları görürsünüz.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği felaket kurtarma (BCDR)

Tüm Azure veri merkezi çalışmayı durdurduğunda bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu oluşur. Bu, StorSimple Aygıt Yöneticisi hizmetinizi ve ilişkili StorSimple aygıtlarını etkileyebilir.

Bir felaket meydana gelmeden hemen önce kaydedilmiş StorSimple aygıtları varsa, bu StorSimple aygıtlarının silinmesi gerekebilir. Felaketten sonra, bu aygıtları yeniden oluşturabilir ve yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yerel web Kullanıcı Birliğini kullanarak nasıl yöneteceğimiz](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

