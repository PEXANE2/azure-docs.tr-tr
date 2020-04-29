---
title: StorSimple Sanal dizisi için yük devretme ve olağanüstü durum kurtarma
description: StorSimple Sanal dizinizi yük devretme hakkında daha fazla bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77467224"
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Azure portalı aracılığıyla StorSimple Sanal Diziniz için olağanüstü durum kurtarma ve cihaz yükü devretme

## <a name="overview"></a>Genel Bakış
Bu makalede, başka bir sanal diziye yük devretmek için ayrıntılı adımlar da dahil olmak üzere Microsoft Azure StorSimple Sanal diziniz için olağanüstü durum kurtarma açıklanmaktadır. Yük devretme, verilerinizi veri merkezindeki bir *kaynak* cihazdan *hedef* cihaza taşımanızı sağlar. Hedef cihaz aynı veya farklı bir coğrafi konumda bulunabilir. Cihaz yük devretmesi tüm cihaza yöneliktir. Yük devretme sırasında kaynak cihaz için bulut verileri, hedef cihazın sahipliğini değiştirir.

Bu makale yalnızca StorSimple Sanal dizileri için geçerlidir. 8000 serisi bir cihazın yükünü devretmek için, [StorSimple cihazınızın cihaz yük devretmesi ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md)bölümüne gidin.

## <a name="what-is-disaster-recovery-and-device-failover"></a>Olağanüstü durum kurtarma ve cihaz yük devretmesi nedir?

Olağanüstü durum kurtarma (DR) senaryosunda, birincil cihaz çalışmayı durduruyor. Bu senaryoda, başarısız cihazla ilişkili bulut verilerini başka bir cihaza taşıyabilirsiniz. Birincil cihazı *kaynak* olarak kullanabilir ve *hedef*olarak başka bir cihaz belirtebilirsiniz. Bu işlem, *Yük devretme*olarak adlandırılır. Yük devretme sırasında, kaynak cihazdaki tüm birimler veya Paylaşımlar sahipliği değiştirir ve hedef cihaza aktarılır. Verilerin filtrelemesine izin verilmez.

DR, ısı haritası tabanlı katmanlama ve izleme kullanılarak tam bir cihaz geri yüklemesi olarak modellenir. Bir ısı haritası, okuma ve yazma desenlerine göre verilere bir ısı değeri atanarak tanımlanır. Bu ısı haritası daha sonra en düşük ısı verisi öbeklerini önce buluta, yüksek ısı (en çok kullanılan) verilerini yerel katmanda parçalar. Bir DR sırasında StorSimple, verileri buluttan geri yüklemek ve yeniden oluşturmak için ısı haritasını kullanır. Cihaz, son son yedekteki tüm birimleri/paylaşımları (dahili olarak belirlendiği şekilde) getirir ve bu yedeklemeden geri yükleme gerçekleştirir. Sanal dizi tüm DR sürecini düzenler.

> [!IMPORTANT]
> Kaynak cihaz, cihaz yük devretmesinin sonunda silinir ve bu nedenle yeniden çalışma desteklenmez.
> 
> 

Olağanüstü durum kurtarma cihaz yük devretme özelliği aracılığıyla düzenlenir ve **cihazlar** dikey penceresinden başlatılır. Bu dikey pencere, StorSimple Aygıt Yöneticisi hizmetinize bağlı olan tüm StorSimple cihazlarını tablo halinde depolar. Her cihaz için kolay adı, durumu, sağlanan ve en yüksek kapasiteyi, türü ve modeli görebilirsiniz.

## <a name="prerequisites-for-device-failover"></a>Cihaz yük devretme önkoşulları

### <a name="prerequisites"></a>Ön koşullar

Bir cihaz yük devretmesi için aşağıdaki önkoşulların sağlandığından emin olun:

* Kaynak cihazın **devre dışı bırakılmış** bir durumda olması gerekir.
* Hedef cihazın Azure portal **ayarlanmaya** uygun olarak gösterilmesi gerekir. Aynı veya daha yüksek kapasiteye sahip bir hedef sanal dizi sağlayın. Hedef sanal diziyi yapılandırmak ve başarıyla kaydetmek için yerel Web Kullanıcı arabirimini kullanın.
  
  > [!IMPORTANT]
  > Kayıtlı sanal cihazı hizmet aracılığıyla yapılandırmaya çalışmayın. Hizmet aracılığıyla hiçbir cihaz yapılandırması gerçekleştirilmemelidir.
  > 
  > 
* Hedef cihaz, kaynak cihazla aynı ada sahip olamaz.
* Kaynak ve hedef cihazın aynı türde olması gerekir. Yalnızca bir dosya sunucusu olarak yapılandırılmış bir sanal dizinin yükünü başka bir dosya sunucusuna devretmek için kullanabilirsiniz. Aynı değer bir Iscsı sunucusu için de geçerlidir.
* Bir dosya sunucusu DR için, hedef cihazı kaynak ile aynı etki alanına katmenizi öneririz. Bu yapılandırma, paylaşma izinlerinin otomatik olarak çözümlenmesini sağlar. Yalnızca aynı etki alanındaki bir hedef cihaza yük devretme desteklenir.
* DR için kullanılabilir hedef aygıtlar, kaynak cihazla karşılaştırıldığında aynı veya daha büyük kapasiteye sahip cihazlardır. Hizmetinize bağlı olan ancak yeterli alan ölçütlerini karşılamayan cihazlar hedef cihaz olarak kullanılamaz.

### <a name="other-considerations"></a>Diğer konular

* Planlı Yük devretme için:
  
  * Kaynak cihazdaki tüm birimleri veya paylaşımları çevrimdışına almanız önerilir.
  * Cihazın bir yedeğini alıp veri kaybını en aza indirmek için yük devretmeye devam etmenizi öneririz.
* Planlanmamış bir yük devretme için cihaz, verileri geri yüklemek için en son yedeği kullanır.

### <a name="device-failover-prechecks"></a>Cihaz yük devretme öndenetimleri

DR başlamadan önce cihaz önceden denetimler gerçekleştirir. Bu denetimler, DR geldiğinde herhangi bir hata oluşkullanılmadığından emin olmanıza yardımcı olur. Ön denetimler şunları içerir:

* Depolama hesabı doğrulanıyor.
* Azure ile bulut bağlantısı denetleniyor.
* Hedef cihazdaki kullanılabilir alan denetleniyor.
* Bir Iscsı sunucusu kaynak cihaz biriminde olup olmadığı denetleniyor
  
  * geçerli ACR adları.
  * geçerli ıQN (220 karakteri aşmıyor).
  * geçerli CHAP parolaları (12-16 karakter uzunluğunda).

Önceki ön denetimlerden herhangi biri başarısız olursa, DR ile devam edemezsiniz. Bu sorunları giderin ve ardından DR 'yi yeniden deneyin.

DR başarıyla tamamlandıktan sonra, kaynak cihazdaki bulut verilerinin sahipliği hedef cihaza aktarılır. Kaynak cihaz daha sonra portalda artık kullanılamaz. Kaynak cihazdaki tüm birimlere/paylaşımlara erişim engellenir ve hedef cihaz etkin hale gelir.

> [!IMPORTANT]
> Cihaz artık kullanılamıyor olsa da, konak sisteminde sağladığınız sanal makine halen kaynak tüketiyor. DR başarıyla tamamlandıktan sonra, bu sanal makineyi konak sisteminizden silebilirsiniz.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Sanal diziye yük devretme

Bu yordamı çalıştırmadan önce, StorSimple Aygıt Yöneticisi hizmetinize başka bir StorSimple Sanal dizisi sağlamanızı, yapılandırmanızı ve kaydetmenizi öneririz.

> [!IMPORTANT]
> 
> * StorSimple 8000 serisi cihazdan 1200 sanal cihazına yük devredemezsiniz.
> * Federal bilgi Işleme standardı (FIPS) etkinleştirilmiş bir sanal cihazdan başka bir FIPS etkin cihaza veya kamu portalında dağıtılan FIPS olmayan bir cihaza yük devreolursunuz.


Bir hedef StorSimple Sanal cihazına cihaz geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. [Cihaz yük devretmesi için önkoşulları](#prerequisites)karşılayan bir hedef cihaz sağlayın ve yapılandırın. Yerel Web Kullanıcı arabirimi aracılığıyla cihaz yapılandırmasını tamamlayıp StorSimple Aygıt Yöneticisi hizmetinize kaydedin. Bir dosya sunucusu oluşturuyorsanız, [dosya sunucusu olarak ayarlanan](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device)1. adıma gidin. Bir Iscsı sunucusu oluşturuyorsanız, [iSCSI sunucusu olarak ayarlanan](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device)1. adıma gidin.

2. Konakta birimleri/paylaşımları çevrimdışı duruma getirin. Birimleri/paylaşımları çevrimdışına almak için, ana bilgisayar için işletim sistemine özgü yönergelere bakın. Zaten çevrimdışı değilse, aşağıdakileri yaparak cihazdaki tüm birimleri/paylaşımları çevrimdışı duruma getirmeniz gerekir.
   
    1. **Cihazlar** dikey penceresine gidin ve cihazınızı seçin.
   
    2. Ayarlar ' a gidin **> > paylaşımlarını yönetin** (veya **> birimleri yönetme > ayarları**). 
   
    3. Bir Share/Volume seçin, sağ tıklayın ve **Çevrimdışına Al**' ı seçin. 
   
    4. Onay istendiğinde, **bu paylaşımdan çevrimdışı duruma getirmenin etkilerini anlıyorum.** 
   
    5. **Çevrimdışına Al**' a tıklayın.

3. StorSimple Aygıt Yöneticisi hizmetinizde **yönetim > cihazlar**' a gidin. **Cihazlar** dikey penceresinde, kaynak cihazınızı seçin ve tıklayın.

4. **Cihaz panosu** dikey penceresinde **devre dışı bırak**' a tıklayın.

5. **Devre dışı bırakma** dikey penceresinde onaylamanız istenir. Cihaz devre dışı bırakma işlemi geri alınamaz *kalıcı* bir işlemdir. Ayrıca, paylaşımlara/birimlerinizi konakta çevrimdışına almanız de anımsatılır. Onaylamak için cihaz adını yazın ve **devre dışı bırak**' a tıklayın.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. Devre dışı bırakma başlatılır. Devre dışı bırakma işlemi başarıyla tamamlandıktan sonra bir bildirim alırsınız.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Cihazlar sayfasında, cihaz durumu artık **devre dışı**olarak değiştirilir.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. **Cihazlar** dikey penceresinde, yük devretme için devre dışı bırakılmış kaynak cihazı seçin ve tıklayın. 
9. **Cihaz panosu** dikey penceresinde **Yük devret**' e tıklayın. 
10. Yük **devretme cihazı** dikey penceresinde aşağıdakileri yapın:
    
    1. Kaynak cihaz alanı otomatik olarak doldurulur. Kaynak cihaz için toplam veri boyutunu aklınızda yapın. Veri boyutu, hedef cihazdaki kullanılabilir kapasiteden daha az olmalıdır. Kaynak cihazla ilişkili cihaz adı, toplam kapasite ve yük devredilen paylaşımların adları gibi ayrıntıları gözden geçirin.

    2. Kullanılabilir cihazların açılan listesinden bir **hedef cihaz**seçin. Yalnızca yeterli kapasiteye sahip cihazlar açılan listede görüntülenir.

    3. **Bu işlemin hedef cihaza veri yük devredebildiğini anladım**. 

    4. **Yük devretme**' ye tıklayın.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Bir yük devretme işi başlatılır ve bir bildirim alırsınız. Yük devretmeyi izlemek için **cihazlara > işlere** gidin.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. **İşler** dikey penceresinde, kaynak cihaz için oluşturulmuş bir yük devretme işi görürsünüz. Bu iş, DR ön denetimleri gerçekleştirir.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     DR ön denetimleri başarılı olduktan sonra, yük devretme işi kaynak cihazınızda bulunan her bir paylaşıma/birime ait geri yükleme işlerini oluşturacak.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Yük devretme tamamlandıktan sonra **cihazlar** dikey penceresine gidin.
    
    1. Yük devretme işlemi için hedef cihaz olarak kullanılan StorSimple cihazını seçin ve tıklayın.
    2. **Ayarlar > yönetim > paylaşımlar** (veya iSCSI sunucusu varsa **birimler** ) bölümüne gidin. **Paylaşımlar** dikey penceresinde, eski cihazdan tüm paylaşımları (birimler) görüntüleyebilirsiniz.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Bağlanmaya çalışan tüm uygulamaların yeni cihaza yeniden yönlendirilmesi için [BIR DNS diğer adı oluşturmanız](https://support.microsoft.com/kb/168322) gerekecektir.

## <a name="errors-during-dr"></a>DR sırasında hatalar

**DR sırasında bulut bağlantısı kesintisi**

DR başladıktan sonra ve cihaz geri yükleme işlemi tamamlanmadan önce bulut bağlantısı kesintiye uğramışsa, DR başarısız olur. Bir hata bildirimi alırsınız. DR için hedef cihaz kullanılamıyor olarak işaretlendi *.* Gelecekteki DRs için aynı hedef cihazı kullanamazsınız.

**Uyumlu hedef cihaz yok**

Kullanılabilir hedef cihazlarda yeterli alan yoksa, uyumlu hedef cihaz bulunmadığını belirten bir hata görürsünüz.

**Ön denetim sorunları**

Öndenetimlerden biri karşılanmıyorsa, önceden denetim başarısızlıklarını görürsünüz.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği olağanüstü durum kurtarma (BCDR)

Azure veri merkezinin tamamı çalışmayı durdurduğu zaman bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu oluşur. Bu, StorSimple Aygıt Yöneticisi hizmetinizi ve ilişkili StorSimple cihazlarını etkileyebilir.

Yalnızca bir olağanüstü durum yaşanmadan önce kayıtlı olan StorSimple cihazları varsa, bu StorSimple cihazlarının silinmesi gerekebilir. Olağanüstü durum sonrasında bu cihazları yeniden oluşturabilir ve yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Yerel Web Kullanıcı arabirimini kullanarak StorSimple Sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

