---
title: StorSimple 8000 serisi cihaz için arıza ve olağanüstü durum kurtarma
description: StorSimple cihazınızı kendisine, başka bir fiziksel aygıta veya bir bulut cihazına karşı nasıl başarısız oylayacaklarınızı öğrenin.
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
ms.openlocfilehash: 179bc5cdf982792f41e0dec209341f346959a31a
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397518"
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için yük devretme ve olağanüstü durum kurtarma

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple 8000 serisi aygıtlar için aygıt arıza özelliği ve bir felaket oluşursa Bu özelliğin StorSimple aygıtlarını kurtarmak için nasıl kullanılabilir. StorSimple, verileri veri merkezindeki bir kaynak aygıttan başka bir hedef aygıta geçirmek için aygıt hatasını kullanır. Bu makaledeki kılavuz, StorSimple 8000 serisi fiziksel aygıtlar ve yazılım sürümleri güncelleştirme 3 ve sonrası çalıştıran bulut cihazları için geçerlidir.

StorSimple, bir felaket durumunda aygıtın arıza özelliğini başlatmak için **Cihazlar** bıçağını kullanır. Bu bıçak, StorSimple Device Manager hizmetinize bağlı tüm StorSimple aygıtlarını listeler.

![Cihazlar bıçak](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Olağanüstü durum kurtarma (DR) ve cihaz arıza

Olağanüstü durum kurtarma (DR) senaryosunda, birincil aygıt çalışmayı durdurur. StorSimple birincil aygıtı _kaynak_ olarak kullanır ve ilişkili bulut verilerini başka bir _hedef_ aygıta taşır. Bu işlem *failover*olarak adlandırılır. Aşağıdaki grafik, başarısızlık işlemini göstermektedir.

![Cihaz arızasında ne olur?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Bir arıza için hedef cihaz fiziksel bir cihaz ya da bir bulut cihaz olabilir. Hedef aygıt, kaynak aygıttan aynı veya farklı bir coğrafi konumda bulunabilir.

Başarısız lık sırasında, geçiş için birim kapsayıcıları seçebilirsiniz. StorSimple daha sonra bu birim kapsayıcıların sahipliğini kaynak aygıttan hedef aygıta değiştirir. Birim kapsayıcılar sahipliğini değiştirdikten sonra, StorSimple bu kapsayıcıları kaynak aygıttan siler. Silme işlemi tamamlandıktan sonra, hedef aygıtı geri başarısız olabilirsiniz. _Failback_ sahipliği özgün kaynak aygıta geri aktarAr.

### <a name="cloud-snapshot-used-during-device-failover"></a>Aygıt arızası sırasında kullanılan bulut anlık görüntüsü

Dr'den sonra, verileri hedef aygıta geri yüklemek için en son bulut yedeklemesi kullanılır. Bulut anlık görüntüleri hakkında daha fazla bilgi için [bkz.](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup)

StorSimple 8000 serisinde, yedekleme ilkeleri yedeklemelerle ilişkilidir. Aynı birim için birden çok yedekleme ilkesi varsa, StorSimple en fazla birim sayısına sahip yedekleme ilkesini seçer. StorSimple daha sonra hedef aygıttaki verileri geri yüklemek için seçili yedekleme ilkesinden en son yedeklemeyi kullanır.

*VarsayılanPol* ve *customPol*olmak üzere iki yedekleme politikası olduğunu varsayalım:

* *defaultPol*: Bir birim, *vol1*, günlük olarak 22:30'dan itibaren çalışır.
* *customPol*: Dört cilt, *vol1,* *vol2*, *vol3*, *vol4*, günlük olarak 22:00'den itibaren çalışır.

Bu durumda, StorSimple kilitlenme tutarlılığı için öncelik verir ve daha fazla birim olduğu için *customPol* kullanır. Bu ilkenin en son yedeklemesi verileri geri yüklemek için kullanılır. Yedekleme ilkelerinin nasıl oluşturulup yönetilenhakkında daha fazla bilgi için, [yedekleme ilkelerini yönetmek için StorSimple Device Manager hizmetini kullanın'](storsimple-8000-manage-backup-policies-u2.md)a gidin.

## <a name="common-considerations-for-device-failover"></a>Cihaz arızaları için sık karşılaşılan hususlar

Bir aygıt üzerinde başarısız olmadan önce aşağıdaki bilgileri gözden geçirin:

* Aygıt başarısızlığını başlatmadan önce, birim kapsayıcıları içindeki tüm birimler çevrimdışı olmalıdır. Planlanmamış bir hatada, StotSimple birimleri otomatik olarak çevrimdışı olur. Ancak planlı bir hata yapıyorsanız (DR'yi test etmek için), tüm birimleri çevrimdışı na samalısınız.
* Yalnızca ilişkili bulut anlık görüntüsüne sahip birim kapsayıcıları DR için listelenir. Verileri kurtarmak için ilişkili bulut anlık görüntüsüne sahip en az bir birim kapsayıcı olmalıdır.
* Birden çok birim kapsayıcıya yayılan bulut anlık görüntüleri varsa, StorSimple bu birim kapsayıcıları üzerinde küme olarak başarısız olur. Nadir bir durumda, birden çok birim kapsayıcıya yayılan ancak ilişkili bulut anlık görüntüleri arasında yer alan yerel anlık görüntüler varsa, StorSimple yerel anlık görüntüler üzerinde başarısız olur ve yerel veriler DR'den sonra kaybolur.
* DR için kullanılabilir hedef aygıtlar, seçili birim kaplarını yerleştirmek için yeterli alana sahip aygıtlardır. Yeterli alana sahip olmayan aygıtlar hedef aygıtlar olarak listelenmez.
* Dr'den sonra (sınırlı bir süre için), aygıtın verilere buluttan erişmesi ve yerel olarak depolaması gerektiğinden, veri erişim performansı önemli ölçüde etkilenebilir.

#### <a name="device-failover-across-software-versions"></a>Yazılım sürümlerinde aygıt başarısız

Dağıtımdaki StorSimple Aygıt Yöneticisi hizmetinde, tümü farklı yazılım sürümleri çalıştıran fiziksel ve bulut olmak üzere birden çok aygıt olabilir.

Farklı bir yazılım sürümü çalıştıran başka bir aygıtta başarısız olup olmadığınızveya başarısız olup olmadığınız ve DR sırasında ses türünün nasıl çalıştığını belirlemek için aşağıdaki tabloyu kullanın.

#### <a name="failover-and-failback-across-software-versions"></a>Yazılım sürümlerinde failover ve failback

| Başarısız oldu/ Geri başarısız | Fiziksel cihaz | Bulut gereci |
| --- | --- | --- |
| 4'u Güncelleştirmek için 3'e Güncelleştir |Katmanlı birimler katmanlı olarak başarısız oldu. <br></br>Yerel olarak sabitlenmiş birimler, yerel olarak sabitlendikçe başarısız oldu. <br></br> Güncelleme 4 aygıtında anlık görüntü aldığınızda bir hata nın ardından, ısı haritası tabanlı izleme devreye [sayılsa.](storsimple-update4-release-notes.md#whats-new-in-update-4) |Yerel olarak sabitlenmiş birimler katmanlı olarak başarısız oldu. |
| 3'u Güncelleştirmek için 4'e Güncelleştir |Katmanlı birimler katmanlı olarak başarısız oldu. <br></br>Yerel olarak sabitlenmiş birimler, yerel olarak sabitlendikçe başarısız oldu. <br></br> Geri yüklemek için kullanılan yedeklemeler ısı haritası meta verilerini korur. <br></br>Heatmap tabanlı izleme, bir geri dönüş sonrasında Güncelleştirme 3'te kullanılamaz. |Yerel olarak sabitlenmiş birimler katmanlı olarak başarısız oldu. |


## <a name="device-failover-scenarios"></a>Aygıt arıza senaryoları

Bir felaket varsa, StorSimple cihazınız üzerinde başarısız olmayı seçebilirsiniz:

* [Fiziksel bir cihaza.](storsimple-8000-device-failover-physical-device.md)
* [Kendi kendine.](storsimple-8000-device-failover-same-device.md)
* [Bir bulut cihazına.](storsimple-8000-device-failover-cloud-appliance.md)

Yukarıdaki makaleler, yukarıdaki başarısız durumların her biri için ayrıntılı adımlar sağlar.


## <a name="failback"></a>Yeniden çalışma

Update 3 ve sonraki sürümler için StorSimple da failback destekler. Failback failover sadece tersi, hedef kaynak olur ve failover sırasında orijinal kaynak cihaz şimdi hedef cihaz olur. 

Geri alma sırasında StorSimple verileri birincil konuma yeniden senkronize eder, G/Ç ve uygulama etkinliğini durdurur ve özgün konuma geri döner.

Bir başarısızlık tamamlandıktan sonra, StorSimple aşağıdaki eylemleri gerçekleştirir:

* StorSimple, kaynak aygıttan başarısız olan birim kaplarını temizler.
* StorSimple, kaynak aygıtta birim kapsayıcı başına bir arka plan işi başlatır (üzerinde başarısız oldu). İş devam ederken başarısız olmaya çalışırsanız, bu yönde bir bildirim alırsınız. Failback başlatmak için iş tamamlanana kadar bekleyin.
* Birim kapsayıcılarının silinmesini tamamlamak için geçen süre, veri miktarı, verilerin yaşı, yedekleme sayısı ve işlem için kullanılabilen ağ bant genişliği gibi çeşitli etkenlere bağlıdır.

Test başarısızlarını veya test geri dönüşlerini planlıyorsanız, birim kapsayıcılarını daha az veriyle (Gbs) test etmeyi öneririz. Genellikle, failover tamamlandıktan 24 saat sonra failback başlayabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **DR başarısız olursa veya kısmi başarı yada olursa ne olur?**

A. DR başarısız olursa, yeniden denemenizi öneririz. İkinci aygıt başarısız iş ilk işin ilerleme farkında dır ve bu noktadan itibaren başlar.

S. **Aygıt arıza devam ederken bir aygıtı silebilir miyim?**

A. DR devam ederken aygıtı silemezsiniz. Cihazınızı yalnızca DR tamamlandıktan sonra silebilirsiniz. **İşler** bıçağındaki iş ilerlemesinin başarısız olduğunu izleyebilirsiniz.

S. **Kaynak aygıttaki yerel verilerin silinmesi için kaynak aygıtta çöp toplama ne zaman başlar?**

A. Çöp toplama, kaynak aygıtta yalnızca aygıt tamamen temizlendikten sonra etkinleştirilir. Temizleme, birim, yedekleme nesneleri (veri değil), birim kapsayıcılar ve ilkeler gibi kaynak aygıttan başarısız olan nesneleri temizlemeyi içerir.

S. **Kaynak aygıttaki birim kapsayıcılarıyla ilişkili silme işi başarısız olursa ne olur?**

A.  Silme işi başarısız olursa, birim kapsayıcılarını el ile silebilirsiniz. **Cihazlar** bıçağında, kaynak cihazınızı seçin ve **Birim kaplarını**tıklatın. Bıçağın üzerinde ve altında başarısız olduğunuz birim kaplarını seçin, **Sil'i**tıklatın. Kaynak aygıttaki tüm başarısız birim kapsayıcılarını sildikten sonra, geri alma geri yüklemesini başlatabilirsiniz. Daha fazla bilgi için [bir birim kapsayıcısil'e](storsimple-8000-manage-volume-containers.md#delete-a-volume-container)gidin.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği felaket kurtarma (BCDR)

Tüm Azure veri merkezi çalışmayı durdurduğunda bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu oluşur. Bu senaryo, StorSimple Aygıt Yöneticisi hizmetinizi ve ilişkili StorSimple aygıtlarını etkileyebilir.

Bir StorSimple aygıtı bir felaket meydana gelmeden hemen önce kaydedildiyse, bu aygıtın fabrika sıfırlamasına geçmesi gerekebilir. Felaketten sonra StorSimple aygıtı Azure portalında çevrimdışı olarak gösterilmektedir. Bu cihaz portaldan silinmelidir. Aygıtı fabrika varsayılanlarına sıfırlayın ve hizmete yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Aygıt başarısızlığı gerçekleştirmeye hazırsanız, ayrıntılı yönergeler için aşağıdaki senaryolardan birini seçin:

- [Başka bir fiziksel aygıtta başarısız](storsimple-8000-device-failover-physical-device.md)
- [Aynı cihazda başarısız](storsimple-8000-device-failover-same-device.md)
- [StorSimple Cloud Appliance üzerinde fail](storsimple-8000-device-failover-cloud-appliance.md)

Aygıtınız üzerinde başarısız olduysanız, aşağıdaki seçeneklerden birini seçin:

* [StorSimple cihazınızı devre dışı bırakın veya silin.](storsimple-8000-deactivate-and-delete-device.md)
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

