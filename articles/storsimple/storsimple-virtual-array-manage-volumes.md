---
title: StorSimple Virtual Array'de birimleri yönetme | Microsoft Dokümanlar
description: StorSimple Aygıt Yöneticisi'ni açıklar ve StorSimple Virtual Array'inizdeki birimleri yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 2dbbe6bcd4957a108cc3eae4d41816b130cf8f07
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633877"
---
# <a name="use-storsimple-device-manager-service-to-manage-volumes-on-the-storsimple-virtual-array"></a>StorSimple Sanal Dizisinde birimleri yönetmek için StorSimple Device Manager hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple Virtual Array'unuzda hacim oluşturmak ve yönetmek için StorSimple Device Manager hizmetini nasıl kullanacağınızı açıklar.

StorSimple Device Manager hizmeti, Azure portalında, StorSimple çözümünüzü tek bir web arabiriminden yönetmenize olanak tanıyan bir uzantıdır. Paylaşımları ve birimleri yönetmeye ek olarak, aygıtları görüntülemek ve yönetmek, uyarıları görüntülemek ve yedekleme ilkelerini ve yedekleme kataloğunu görüntülemek ve yönetmek için StorSimple Device Manager hizmetini kullanabilirsiniz.

## <a name="volume-types"></a>Ses Türleri

StorBasit hacimler:

* **Yerel olarak sabitlenmiş**: Bu birimlerdeki veriler her zaman dizide kalır ve buluta dökülmez.
* **Katmanlı**: Bu birimlerdeki veriler buluta dökülebilir. Katmanlı bir hacim oluşturduğunuzda, alanın yaklaşık %10'u yerel katmanda ve alanın %90'ı bulutta karşılanır. Örneğin, 1 TB birim sağlarsanız, 100 GB yerel alanda kalır ve veri katmanları bulutta 900 GB kullanılır. Bu da, aygıttaki tüm yerel alanın biterse, katmanlı bir hacim sağlayamadığınızanlamına gelir (çünkü yerel katmanda gereken %10'u kullanılamayacaktır).

### <a name="provisioned-capacity"></a>Sağlanan kapasite
Her birim türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcı**                                       | **Sınırı**     |
|------------------------------------------------------------|---------------|
| Katmanlı bir birimin minimum boyutu                            | 500 GB        |
| Katmanlı bir birimin maksimum boyutu                            | 5 TB          |
| Yerel olarak sabitlenmiş bir birimin minimum boyutu                    | 50 GB         |
| Yerel olarak sabitlenmiş bir birimin maksimum boyutu                    | 200 GB        |

## <a name="the-volumes-blade"></a>Hacimler bıçak
StorSimple hizmet özet bıçaknızdaki **Birimler** menüsü, belirli bir StorSimple dizisindeki depolama birimlerinin listesini görüntüler ve bunları yönetmenize olanak tanır.

![Hacimler bıçak](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Bir birim bir dizi özniteliklerden oluşur:

* **Birim Adı** – Benzersiz olması gereken ve birimin tanımlanmasına yardımcı olan açıklayıcı bir ad.
* **Durum** – Çevrimiçi veya çevrimdışı olabilir. Bir birim çevrimdışıysa, birimde erişime izin verilen başlatıcılar (sunucular) tarafından görülemez.
* **Tür** – Birimin **Katmanlı** (varsayılan) veya **Yerel olarak sabitlenip sabitlenmediğini**gösterir.
* **Kapasite** – başlatıcı (sunucu) tarafından depolanabilecek toplam veri miktarıyla karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Yedekleme** – StorSimple Virtual Array durumunda, tüm birimler yedekleme için otomatik olarak etkinleştirilir.
* **Bağlı ana bilgisayarlar** – Bu birimde erişime izin verilen başlatıcıları (sunucuları) belirtir.

![Birim ayrıntıları](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Aşağıdaki görevleri gerçekleştirmek için bu öğreticideki yönergeleri kullanın:

* Ses düzeyi ekleme
* Birim değiştirme
* Bir birimi çevrimdışı alma
* Bir birim silme

## <a name="add-a-volume"></a>Ses düzeyi ekleme

1. StorSimple hizmet özet bıçağından komut çubuğundan **+ Ses ekle'yi** tıklatın. Bu, **Ses Ekle** bıçağını açar.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. Ses **ekle** bıçağında aşağıdakileri yapın:
   
   * Birim **adı** alanına, biriminiz için benzersiz bir ad girin. Ad, 3 ila 127 karakter içeren bir dize olmalıdır.
   * **Tür** açılır listesinde, **Katmanlı** veya Yerel olarak **sabitlenmiş** bir birim oluşturup oluşturmayacağımı belirtin. Yerel garantiler, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için **Yerel olarak sabitlenmiş birimi**seçin. Diğer tüm veriler için **Katmanlı** birim'i seçin.
   * **Kapasite** alanında, birimin boyutunu belirtin. Katmanlı bir hacim 500 GB ile 5 TB arasında, yerel olarak sabitlenmiş bir hacim ise 50 GB ile 500 GB arasında olmalıdır.
   * * **Bağlı ana bilgisayarlara**tıklayın, bu birimde bağlanmak istediğiniz iSCSI başlatıcısına karşılık gelen bir erişim denetim kaydı (ACR) seçin ve sonra **Seç'i**tıklatın.
3. Yeni bir bağlı ana bilgisayar eklemek için **yeni ekle'yi**tıklatın, ana bilgisayar için bir ad girin ve iSCSI Kalifiye Adı (IQN) girin ve sonra **Ekle'yi**tıklatın.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Ses inizi yapılandırmayı tamamladığınızda **Oluştur'u**tıklatın. Belirtilen ayarlarla birlikte bir birim oluşturulur ve aynı ayarların başarılı bir şekilde oluşturulmasıyla ilgili bir bildirim görürsünüz. Varsayılan yedekleme ses düzeyi için etkinleştirilir.
5. Birimin başarıyla oluşturulduğunu doğrulamak için **Volumes** bıçağına gidin. Listelenen birim görmelisiniz.
   
    ![Hacim başarı yık](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Birim değiştirme

Ses birimine erişen ana bilgisayarları değiştirmeniz gerektiğinde bir birimi değiştirin. Birim oluşturulduktan sonra birimin diğer öznitelikleri değiştirilemez.

#### <a name="to-modify-a-volume"></a>Bir birimi değiştirmek için

1. StorSimple hizmet özet bıçağındaki **Birimler** ayarlarından, değiştirmek istediğiniz birimin bulunduğu sanal diziyi seçin.
2. Bağlı ana bilgisayarı görüntülemek ve farklı bir sunucuyla değiştirmek için ses düzeyini **seçin** ve **Bağlı ana** bilgisayarlarını tıklatın.
   
    ![Ses düzeyini edin](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Kaydet komut çubuğunu tıklatarak değişikliklerinizi **kaydedin.** Belirtilen ayarlarınız uygulanır ve bir bildirim görürsünüz.

## <a name="take-a-volume-offline"></a>Bir birimi çevrimdışı alma

Bir birimi değiştirmeyi veya silmeyi planlarken bir birimi çevrimdışı na mı götürmeniz gerekebilir. Bir birim çevrimdışı olduğunda, okuma yazma erişimi için kullanılamaz. Aygıtın yanı sıra ana bilgisayardaki ses düzeyini çevrimdışına almanız gerekir.

#### <a name="to-take-a-volume-offline"></a>Bir birimi çevrimdışı almak için

1. Çevrimdışı olmadan önce söz konusu birimin kullanılmadığından emin olun.
2. Önce ana bilgisayardaki ses düzeyini çevrimdışına alın. Bu, birimdeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için, ana bilgisayar işletim sisteminizin yönergelerine bakın.
3. Ana bilgisayardaki birim çevrimdışı olduktan sonra, aşağıdaki adımları gerçekleştirerek dizideki birimi çevrimdışı olarak alın:
   
   * StorSimple hizmet özet bıçağındaki **Birimler** ayarlarından, çevrimdışı almak istediğiniz birimin bulunduğu sanal diziyi seçin.
   * Ses düzeyini **seçin** ve **...** 'ı (dönüşümlü olarak bu satırda sağ tıklatın) ve bağlam menüsünden **çevrimdışı nı seç'i**seçin.
     
        ![Çevrimdışı birim](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * **Çevrimdışı al** bıçaktaki bilgileri gözden geçirin ve işlemi kabul edin. Birimi çevrimdışı na getirmek için **çevrimdışı kaldır'ı** tıklatın. Devam eden işlem bildirimini görürsünüz.
   * Birimin başarılı bir şekilde çevrimdışı namına alındığını doğrulamak için **Volumes** bıçağına gidin. Birimin durumunu çevrimdışı olarak görmeniz gerekir.
     
       ![Çevrimdışı ses onayı](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Bir birim silme

> [!IMPORTANT]
> Bir birimi yalnızca çevrimdışıysa silebilirsiniz.
> 
> 

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. StorSimple hizmet özet bıçağındaki **Birimler** ayarlarından, silmenizi istediğiniz birimin bulunduğu sanal diziyi seçin.
2. Ses düzeyini **seçin** ve **...** 'ı (dönüşümlü olarak bu satırda sağ tıklatın) ve bağlam menüsünden **Sil'i**seçin.
   
    ![Ses düzeyini silme](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Silmek istediğiniz birimin durumunu denetleyin. Silmek istediğiniz birim çevrimdışı değilse, [bir birim çevrimdışı al'daki](#take-a-volume-offline)adımları izleyerek önce çevrimdışı olarak alın.
4. **Sil** çubuğunda onay istendiğinde, onayı kabul edin ve **Sil'i**tıklatın. Birim şimdi silinir ve **Birimler** bıçak sanal dizi içinde birimlerin güncelleştirilmiş listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple birimini nasıl klonlamayı](storsimple-virtual-array-clone.md)öğrenin.

