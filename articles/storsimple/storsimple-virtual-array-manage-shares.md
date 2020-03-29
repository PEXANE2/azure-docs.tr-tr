---
title: StorSimple Virtual Array paylaşımlarını yönet | Microsoft Dokümanlar
description: StorSimple Aygıt Yöneticisi'ni açıklar ve StorSimple Virtual Array'inizdeki paylaşımları yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 82a6cdb6c9a39a0d196049a7ba662681ea06b36a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62116875"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a>StorSimple Sanal Dizisinde paylaşımları yönetmek için StorSimple Device Manager hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple Virtual Array'unuzda paylaşım oluşturmak ve yönetmek için StorSimple Device Manager hizmetini nasıl kullanacağınızı açıklar.

StorSimple Device Manager hizmeti, Azure portalında, StorSimple çözümünüzü tek bir web arabiriminden yönetmenize olanak tanıyan bir uzantıdır. Paylaşımları ve birimleri yönetmeye ek olarak, aygıtları görüntülemek ve yönetmek, uyarıları görüntülemek, yedekleme ilkelerini yönetmek ve yedekleme kataloğunu yönetmek için StorSimple Device Manager hizmetini kullanabilirsiniz.

## <a name="share-types"></a>Hisse Türleri

StorSimple paylaşımları şu olabilir:

* **Yerel olarak sabitlenmiş**: Bu paylaşımlarda bulunan veriler her zaman dizide kalır ve buluta dökülmez.
* **Katmanlı**: Bu paylaşımlarda bulunan veriler buluta dökülebilir. Katmanlı bir pay oluşturduğunuzda, alanın yaklaşık %10'u yerel katmanda ve alanın %90'ı bulutta karşılanır. Örneğin, 1 TB pay verdiyseniz, 100 GB yerel alanda kalır ve veri katmanları bulutta 900 GB kullanılır. Bu da, aygıttaki tüm yerel alanın biterse, katmanlı bir pay sağlayamadığınızanlamına gelir (çünkü yerel katmanda gereken %10'u kullanılamayacaktır).

### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her hisse türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcı** | **Sınırı** |
| --- | --- |
| Katmanlı bir payın minimum boyutu |500 GB |
| Katmanlı bir payın maksimum boyutu |20 TB |
| Yerel olarak sabitlenmiş bir payın minimum boyutu |50 GB |
| Yerel olarak sabitlenmiş bir payın maksimum boyutu |2 TB |

## <a name="the-shares-blade"></a>Hisse bıçağı

StorSimple hizmet özet bıçaknızdaki **Hisseler** menüsü, belirli bir StorSimple dizisindeki depolama paylaşımlarının listesini görüntüler ve bunları yönetmenize olanak tanır.

![Hisse bıçağı](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Bir paylaşım bir dizi özniteliklerden oluşur:

* **Pay Adı** – Benzersiz olması gereken ve paylaşımın tanımlanmasına yardımcı olan açıklayıcı bir ad.
* **Durum** – Çevrimiçi veya çevrimdışı olabilir. Bir paylaşım çevrimdışıysa, paylaşım kullanıcıları bu paylaşıma erişemez.
* **Tür** – Paylaşımın **Katmanlı** (varsayılan) veya **Yerel olarak sabitlenip sabitlenmediğini**gösterir.
* **Kapasite** – hissede depolanabilecek toplam veri miktarıyla karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Açıklama** – Paylaşımı açıklamaya yardımcı olan isteğe bağlı ayar.
* **İzinler** - NTFS, Windows Gezgini aracılığıyla yönetilebilen paylaşıma izin verir.
* **Yedekleme** – StorSimple Virtual Array durumunda, tüm paylaşımlar yedekleme için otomatik olarak etkinleştirilir.

![Ayrıntıları paylaşır](./media/storsimple-virtual-array-manage-shares/share-details.png)

Aşağıdaki görevleri gerçekleştirmek için bu öğreticideki yönergeleri kullanın:

* Paylaşım ekleme
* Bir paylaşımı değiştirme
* Bir paylaşımı çevrimdışına alma
* Paylaşımı silme

## <a name="add-a-share"></a>Paylaşım ekleme

1. StorSimple hizmet özet bıçağından komut çubuğundan **+ Pay Ekle'yi** tıklatın. Bu, **Ekle paylaşım** bıçağını açar.

    ![Paylaşım ekle](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. Ekle **paylaşım** bıçaklarında aşağıdakileri yapın:
   
   1. Paylaşım **adı** alanına, payınız için benzersiz bir ad girin. Ad, 3 ila 127 karakter içeren bir dize olmalıdır.

   2. Paylaşım için isteğe bağlı **bir Açıklama.** Açıklama, paylaşım sahiplerinin tanımlanmasına yardımcı olur.

   3. **Tür** açılır listesinde, **Katmanlı** veya Yerel olarak **sabitlenmiş** bir pay oluşturup oluşturmayacağımı belirtin. Yerel garantiler, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için **Yerel olarak sabitlenmiş payı**seçin. Diğer tüm veriler için **Katmanlı** paylaşım'ı seçin.

   4. **Kapasite** alanında, paylaşımın boyutunu belirtin. Katmanlı bir pay 500 GB ile 20 TB arasında, yerel olarak sabitlenmiş bir pay ise 50 GB ile 2 TB arasında olmalıdır.

   5. Varsayılan **alanı ayarla,,** izinleri kullanıcıya veya bu paylaşıma erişen gruba atayın. Biçimolarak _john@contoso.com_ kullanıcının veya kullanıcı grubunun adını belirtin. Yönetici ayrıcalıklarının bu paylaşımlara erişmesine izin vermek için bir kullanıcı grubu (tek bir kullanıcı yerine) kullanmanızı öneririz. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.
3. Payınızı yapılandırmayı tamamladığınızda **Oluştur'u**tıklatın. Belirtilen ayarlarla bir paylaşım oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, paylaşım için yedekleme etkinleştirilir.
4. Paylaşımın başarıyla oluşturulduğunu doğrulamak için **Shares** blade'e gidin. Listelenen paylaşımı görmelisiniz.
   
    ![Başarıyı paylaş](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Bir paylaşımı değiştirme

Paylaşımın açıklamasını değiştirmeniz gerektiğinde bir paylaşımı değiştirin. Paylaşım oluşturulduktan sonra başka hiçbir paylaşım özelliği değiştirilemez.

#### <a name="to-modify-a-share"></a>Bir paylaşımı değiştirmek için

1. StorSimple hizmet özet bıçak üzerindeki **Hisse** ayarından, değiştirmek istediğiniz paylaşımın bulunduğu sanal diziyi seçin.
2. Geçerli açıklamayı görüntülemek ve değiştirmek için payı **seçin.**
3. Kaydet komut çubuğunu tıklatarak değişikliklerinizi **kaydedin.** Belirtilen ayarlarınız uygulanır ve bir bildirim görürsünüz.
   
    ![ Paylaşımı nı edin](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Bir paylaşımı çevrimdışına alma

Bir hisseyi değiştirmeyi veya silmeyi planlarken bir payı çevrimdışı namına almanız gerekebilir. Bir paylaşım çevrimdışı olduğunda, okuma yazma erişimi için kullanılamaz. Aygıtın yanı sıra ana bilgisayardaki payı çevrimdışına almanız gerekir.

#### <a name="to-take-a-share-offline"></a>Bir paylaşımı çevrimdışına almak için

1. Söz konusu paylaşımın çevrimdışı olmadan önce kullanılmadığından emin olun.
2. Aşağıdaki adımları gerçekleştirerek dizideki paylaşımı çevrimdışı na alın:
   
    1. StorSimple hizmet özet bıçak üzerindeki **Hisse** ayarı'ndan, çevrimdışı nızı almak istediğiniz paylaşımın bulunduğu sanal diziyi seçin.

    2. **Select** Paylaşım ve Tıklayın **...** (dönüşümlü olarak bu satırda sağ tıklatın) ve bağlam menüsünden, **çevrimdışı al'ı**seçin.
     
        ![Çevrimdışı paylaşım](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. **Çevrimdışı al** bıçaktaki bilgileri gözden geçirin ve işlemi kabul edin. Paylaşımı çevrimdışına almak için **çevrimdışı kaldır'ı** tıklatın. Devam eden işlem bildirimini görürsünüz.

    4. Paylaşımın başarılı bir şekilde çevrimdışı hale alındığını doğrulamak için **Shares** blade'e gidin. Paylaşımın durumunu çevrimdışı olarak görmeniz gerekir.

## <a name="delete-a-share"></a>Paylaşımı silme

> [!IMPORTANT]
> Bir paylaşımı yalnızca çevrimdışıysa silebilirsiniz.


Bir paylaşımı silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-share"></a>Bir paylaşımı silmek için

1. StorSimple hizmet özet bıçak üzerindeki **Hisse** ayarından, silmek istediğiniz paylaşımın bulunduğu sanal diziyi seçin.
2. Paylaş'ı **seçin** ve **... 'ı** tıklatın (dönüşümlü olarak bu satırda sağ tıklatın) ve bağlam menüsünden **Sil'i**seçin.
   
    ![Paylaşımı silme](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Silmek istediğiniz paylaşımın durumunu denetleyin. Silmek istediğiniz paylaşım çevrimdışı değilse, önce çevrimdışı na alın. Bir paylaşımı [çevrimdışına alma](#take-a-share-offline)adımlarını izleyin.
4. **Sil** çubuğunda onay istendiğinde, onayı kabul edin ve **Sil'i**tıklatın. Paylaşım şimdi silinir ve **Paylaşımlar** bıçak sanal dizi içinde hisselerin güncelleştirilmiş listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple paylaşımını](storsimple-virtual-array-clone.md)nasıl klonlayacağını öğrenin.

