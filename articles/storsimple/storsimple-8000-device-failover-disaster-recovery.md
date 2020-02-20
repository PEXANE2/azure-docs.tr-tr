---
title: StorSimple 8000 serisi cihazlar için yük devretme ve olağanüstü durum kurtarma
description: StorSimple cihazınızı, başka bir fiziksel cihaza veya bir bulut gerecine devretmek hakkında bilgi edinin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: c1acc084d5abe3385fe311873dfd64c9009e83f2
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468601"
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için yük devretme ve olağanüstü durum kurtarma

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple 8000 serisi cihazların cihaz yük devretme özelliği ve bir olağanüstü durum oluşursa StorSimple cihazlarını kurtarmak için bu özelliğin nasıl kullanılabileceği açıklanmaktadır. StorSimple, veri merkezindeki bir kaynak cihazdan başka bir hedef cihaza geçiş yapmak için cihaz yük devretmesini kullanır. Bu makaledeki kılavuz, StorSimple 8000 serisi fiziksel cihazlara ve güncelleştirme 3 ve üzeri yazılım sürümlerini çalıştıran bulut gereçlerine yöneliktir.

StorSimple, cihaz yük devretme özelliğini bir olağanüstü durum durumunda başlatmak için **cihazlar** dikey penceresini kullanır. Bu dikey pencere, StorSimple Aygıt Yöneticisi hizmetinize bağlı olan tüm StorSimple cihazlarını listeler.

![Cihazlar dikey penceresi](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Olağanüstü durum kurtarma (DR) ve cihaz yük devretmesi

Olağanüstü durum kurtarma (DR) senaryosunda, birincil cihaz çalışmayı durduruyor. StorSimple birincil cihazı _kaynak_ olarak kullanır ve ilişkili bulut verilerini başka bir _hedef cihaza taşıtir_ . Bu işlem, *Yük devretme*olarak adlandırılır. Aşağıdaki grafikte yük devretme süreci gösterilmektedir.

![Cihaz yük devretmesinde ne olur?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Yük devretme için hedef cihaz fiziksel bir cihaz veya hatta bir bulut gereci olabilir. Hedef cihaz aynı veya kaynak cihazdan farklı bir coğrafi konumda bulunabilir.

Yük devretme sırasında, geçiş için birim kapsayıcıları seçebilirsiniz. StorSimple daha sonra bu birim kapsayıcılarının sahipliğini kaynak cihazdan hedef cihaza değiştirir. Birim kapsayıcıları sahipliği değiştirdikten sonra StorSimple bu kapsayıcıları kaynak cihazdan siler. Silme işlemi tamamlandıktan sonra, hedef cihazın yükünü geri alabilirsiniz. Yeniden _çalışma_ , sahipliği özgün kaynak cihaza geri aktarır.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cihaz yük devretmesi sırasında kullanılan bulut anlık görüntüsü

Bir DR 'yi takip eden en son bulut yedeklemesi, verileri hedef cihaza geri yüklemek için kullanılır. Bulut anlık görüntüleri hakkında daha fazla bilgi için bkz. [StorSimple Aygıt Yöneticisi hizmetini kullanarak el ile yedekleme yapın](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

StorSimple 8000 serisinde yedekleme ilkeleri yedeklemelerle ilişkilendirilir. Aynı birim için birden çok yedekleme ilkesi varsa, StorSimple, en fazla birim sayısı ile yedekleme ilkesini seçer. StorSimple daha sonra hedef Cihazdaki verileri geri yüklemek için seçili yedekleme ilkesindeki en son yedeği kullanır.

*Defaultpol* ve *custompol*olmak üzere iki yedekleme ilkesi olduğunu varsayalım:

* *Defaultpol*: bir birim, *Vol1*, 10:30 pm itibariyle günlük olarak çalışır.
* *Custompol*: dört birim, *Vol1*, *VOL2*, *Vol3*, *VOL4*, 10:00 ile başlayarak günlük olarak çalışır.

Bu durumda, kilitlenme tutarlılığı için StorSimple önceliklendirir ve daha fazla birime sahip olan *Custompol* kullanır. Bu ilkeden en son yedekleme, verileri geri yüklemek için kullanılır. Yedekleme ilkeleri oluşturma ve yönetme hakkında daha fazla bilgi için, [yedekleme ilkelerini yönetmek üzere StorSimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md)bölümüne gidin.

## <a name="common-considerations-for-device-failover"></a>Cihaz yük devretmesi için genel hususlar

Bir cihazın yükünü devretmek için aşağıdaki bilgileri gözden geçirin:

* Cihaz yük devretmesi başlamadan önce, birim kapsayıcıları içindeki tüm birimlerin çevrimdışı olması gerekir. Planlanmamış bir yük devretmede, StotSimple birimleri otomatik olarak çevrimdışı duruma geçer. Ancak planlı bir yük devretme gerçekleştiriyorsanız (DR 'yi test etmek için), tüm birimleri çevrimdışına almanız gerekir.
* Yalnızca ilişkili bir bulut anlık görüntüsüne sahip birim kapsayıcıları DR için listelenir. Verileri kurtarmak için ilişkili bir bulut anlık görüntüsüne sahip en az bir birim kapsayıcısı olmalıdır.
* Birden çok birim kapsayıcılarından yayılan bulut anlık görüntüleri varsa, StorSimple, bu birim kapsayıcılarının kümesini küme olarak devreder. Nadir bir örnekte, birden çok birim kapsayıcılarından yayılan ancak ilişkili bulut anlık görüntüleri olmayan yerel anlık görüntüler varsa, StorSimple yerel anlık görüntüler üzerinden yük devreder ve yerel veriler DR sonrasında kaybedilir.
* DR için kullanılabilir hedef aygıtlar, seçilen birim kapsayıcılarına uyum sağlamak için yeterli alana sahip cihazlardır. Yeterli alana sahip olmayan cihazlar hedef cihaz olarak listelenmez.
* Bir DR 'den sonra (sınırlı bir süre için), cihazın buluttan verilere erişmesi ve yerel olarak depolaması gerektiğinden, veri erişim performansı önemli ölçüde etkilenebilir.

#### <a name="device-failover-across-software-versions"></a>Yazılım sürümleri arasında cihaz yük devretmesi

Bir dağıtımdaki StorSimple Aygıt Yöneticisi hizmeti, her ikisi de fiziksel ve bulut olan ve hepsi farklı yazılım sürümlerini çalıştıran birden fazla cihaza sahip olabilir.

Farklı bir yazılım sürümü çalıştıran başka bir cihaza yük devretmek veya geri dönüp yük devredebildiğini ve birim türlerinin DR sırasında nasıl davranacağını belirlemek için aşağıdaki tabloyu kullanın.

#### <a name="failover-and-failback-across-software-versions"></a>Yazılım sürümleri arasında yük devretme ve yeniden çalışma

| Yük devretme/geri dönme | Fiziksel cihaz | Bulut gereci |
| --- | --- | --- |
| Güncelleştirme 3-güncelleştirme 4 |Katmanlı birimler katmanlı olarak yük devreder. <br></br>Yerel olarak sabitlenmiş birimler yerel olarak sabitlenmiş olarak yük devreder. <br></br> Güncelleştirme 4 cihazında bir anlık görüntü aldığınızda bir yük devretmenin ardından içindeki [heatmap tabanlı izleme](storsimple-update4-release-notes.md#whats-new-in-update-4) noktaları. |Yerel olarak sabitlenmiş birimler katmanlı olarak yük devreder. |
| Güncelleştirme 4-güncelleştirme 3 |Katmanlı birimler katmanlı olarak yük devreder. <br></br>Yerel olarak sabitlenmiş birimler yerel olarak sabitlenmiş olarak yük devreder. <br></br> Geri yüklemek için kullanılan yedeklemeler, ısı haritasını meta verilerini korur. <br></br>Heatmap tabanlı izleme, yeniden çalışma sonrasında güncelleştirme 3 ' te kullanılamaz. |Yerel olarak sabitlenmiş birimler katmanlı olarak yük devreder. |


## <a name="device-failover-scenarios"></a>Cihaz yük devretme senaryoları

Bir olağanüstü durum varsa, StorSimple cihazınızın yükünü devretmek isteyebilirsiniz:

* [Fiziksel bir cihaza](storsimple-8000-device-failover-physical-device.md).
* [.](storsimple-8000-device-failover-same-device.md)
* [Bir bulut](storsimple-8000-device-failover-cloud-appliance.md)gerecine.

Yukarıdaki makaleler, yukarıdaki yük devretme durumlarının her biri için ayrıntılı adımlar sağlar.


## <a name="failback"></a>Yeniden çalışma

StorSimple, güncelleştirme 3 ve sonraki sürümlerinde yeniden çalışmayı da destekler. Yeniden çalışma, yalnızca yük devretme işlemini geri alır, hedef kaynak olur ve yük devretme sırasında özgün kaynak cihaz artık hedef cihaz haline gelir. 

Yeniden çalışma sırasında StorSimple, verileri birincil konuma geri eşitler, g/ç ve uygulama etkinliğini durdurur ve özgün konuma geri geçirir.

Yük devretme tamamlandıktan sonra StorSimple aşağıdaki eylemleri gerçekleştirir:

* StorSimple, kaynak cihazdan yük devredilen birim kapsayıcılarını temizler.
* StorSimple, kaynak cihazda birim kapsayıcısı başına (yük devredilme) bir arka plan işi başlatır. İş devam ederken yeniden oturum açmaya çalışırsanız, bu etkiye yönelik bir bildirim alırsınız. Yeniden çalışma işlemini başlatmak için iş tamamlanana kadar bekleyin.
* Birim kapsayıcılarını silmenin tamamlanma süresi, veri miktarı, verilerin yaşı, yedeklemelerin sayısı ve işlem için kullanılabilir ağ bant genişliği gibi çeşitli faktörlere bağlıdır.

Yük devretme testi veya yük devretme testi yapıyorsanız, birim kapsayıcılarını daha az veri (GB) ile test etmenizi öneririz. Genellikle, yük devretme tamamlandıktan sonra yeniden çalışmayı 24 saat sonra başlatabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **DR başarısız olursa veya kısmen başarılı olursa ne olur?**

A. DR başarısız olursa, yeniden denemeniz önerilir. İkinci cihaz yük devretme işi, ilk işin ilerlemesini algılar ve bu noktadan itibaren başlar.

S. **Cihaz yük devretmesi devam ederken bir cihazı silebilir miyim?**

A. Bir DR devam ederken bir cihazı silemezsiniz. Cihazınızı yalnızca DR tamamlandıktan sonra silebilirsiniz. **İşler** dikey penceresinde cihaz yük devretme işinin ilerlemesini izleyebilirsiniz.

S. **Kaynak cihazdaki yerel verilerin silinmesi için çöp toplama işlemi kaynak cihazda ne zaman başlar?**

A. Çöp toplama, kaynak cihazda yalnızca cihaz tamamen temizlendikten sonra etkinleştirilir. Temizleme, birim, yedekleme nesneleri (veri değil), birim kapsayıcıları ve ilkeler gibi kaynak cihazdan yük devredilen nesneleri temizleme işlemini içerir.

S. **Kaynak cihazdaki birim kapsayıcılarıyla ilişkili silme işi başarısız olursa ne olur?**

A.  Silme işi başarısız olursa, birim kapsayıcılarını el ile silebilirsiniz. **Cihazlar** dikey penceresinde, kaynak cihazınızı seçin ve **birim kapsayıcıları**' na tıklayın. Yük devretme işlemi yaptığınız birim kapsayıcılarını seçin ve dikey pencerenin alt kısmında **Sil**' e tıklayın. Kaynak cihazdaki yük devredilen tüm birim kapsayıcılarını sildikten sonra, yeniden çalışmayı başlatabilirsiniz. Daha fazla bilgi için [birim kapsayıcısını silme](storsimple-8000-manage-volume-containers.md#delete-a-volume-container)bölümüne gidin.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği olağanüstü durum kurtarma (BCDR)

Azure veri merkezinin tamamı çalışmayı durdurduğu zaman bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu oluşur. Bu senaryo, StorSimple Aygıt Yöneticisi hizmetinizi ve ilişkili StorSimple cihazlarını etkileyebilir.

Bir StorSimple cihazı bir olağanüstü durum olmadan hemen önce kaydedilmişse, bu cihazın bir fabrika sıfırlamasına ihtiyacı olabilir. Olağanüstü durum sonrasında, StorSimple cihazı Azure portal çevrimdışı olarak görünür. Bu cihazın portaldan silinmesi gerekir. Cihazı fabrika ayarlarına sıfırlayın ve hizmeti ile yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz yük devretmesini gerçekleştirmeye hazırsanız, ayrıntılı yönergeler için aşağıdaki senaryolarından birini seçin:

- [Başka bir fiziksel cihaza yük devretme](storsimple-8000-device-failover-physical-device.md)
- [Aynı cihaza yük devretme](storsimple-8000-device-failover-same-device.md)
- [StorSimple Cloud Appliance yük devretme](storsimple-8000-device-failover-cloud-appliance.md)

Cihazınızın yük devretme durumunda, aşağıdaki seçeneklerden birini seçin:

* [StorSimple cihazınızı devre dışı bırakın veya silin](storsimple-8000-deactivate-and-delete-device.md).
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanın](storsimple-8000-manager-service-administration.md).

