---
title: Ortamınıza referans veri setleri ekleme - Azure Time Series Insights | Microsoft Dokümanlar
description: Bu makalede, Azure Zaman Serisi Öngörüleri ortamınızdaki verileri artırmak için bir referans veri kümesinin nasıl ekleyeceğiniz açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18~~~~
ms.openlocfilehash: 79628ed44753577023464ef6208027e1b7996d30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087252"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure portalını kullanarak Time Series Insights ortamınız için bir referans veri seti oluşturun

Bu makalede, Azure Zaman Serisi Öngörüleri ortamınıza bir referans veri kümesi nin nasıl ekleyeceğiniz açıklanmaktadır. Referans verileri, değerleri artırmak için kaynak verilerinize katılmak için yararlıdır.

Başvuru Veri Seti, olay kaynağınızdan olayları artıran öğeler topluluğudur. Time Series Insights giriş motoru, etkinlik kaynağınızdan gelen her olaya referans veri setinizdeki ilgili veri satırıyla katılır. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleştirme, referans veri setinizde tanımlanan Birincil Anahtar sütununa (lar) dayanır.

Başvuru verileri geriye dönük olarak birleştirilmeyecektir. Böylece, yalnızca geçerli ve gelecekteki giriş verileri eşleştirilir ve yapılandırıldıktan ve yüklendikten sonra başvuru tarihi kümesine katılır.

## <a name="video"></a>Video

### <a name="learn-about-time-series-insights-reference-data-modelbr"></a>Time Series Insight'ın referans veri modeli hakkında bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/Z0NuWQUMv1o]

## <a name="add-a-reference-data-set"></a>Referans veri seti ekleme

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Mevcut Azure Zaman Serisi Öngörüleri ortamınızı bulun. Azure portalının sol tarafındaki menüdeki **Tüm kaynakları** seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. Genel **Bakış** sayfasını seçin. **Zaman Serisi Öngörüler explorer URL'sini** bulmak ve bağlantıyı açmak için sayfanın üst kısmına yakın **Essentials** bölümünü genişletin.  

   [![Temel Bilgiler bölümünü genişletin](media/add-reference-data-set/essentials.png)](media/add-reference-data-set/essentials.png#lightbox)

   Zaman Serisi Öngörüleri ortamınız için explorer'ı görüntüleyin.

1. Zaman Serisi Öngörüler kaşifinde ortam seçicisini genişletin. Etkin ortamı seçin. Explorer sayfasında sağ üstteki başvuru veri simgesini seçin.

   [![Referans verisi ekleme](media/add-reference-data-set/tsi-select-environment-and-data-icons.png)](media/add-reference-data-set/tsi-select-environment-and-data-icons.png#lightbox)

1. Yeni **bir veri kümesi** eklemeye başlamak için + Veri kümesi ekle düğmesini seçin.

   [![Veri seti ekleme](media/add-reference-data-set/tsi-add-a-reference-data-set.png)](media/add-reference-data-set/tsi-add-a-reference-data-set.png#lightbox)

1. Yeni **başvuru veri seti** sayfasında, verilerin biçimini seçin:

   - Virgülle sınırlandırılmış veriler için **CSV'yi** seçin. İlk satır üstbilgi satırı olarak kabul edilir.
   - JavaScript nesne gösterimi (JSON) biçimlendirilmiş veriler için **JSON Dizisini** seçin.

   [![Veri biçimini seçin.](media/add-reference-data-set/tsi-select-data-upload-option.png)](media/add-reference-data-set/tsi-select-data-upload-option.png#lightbox)

1. İki yöntemden birini kullanarak verileri sağlayın:

   - Verileri metin düzenleyicisine yapıştırın. Ardından, **Parse başvuru veri düğmesini** seçin.
   - Yerel bir metin dosyasından veri eklemek için **Dosya'yı seçin** düğmesini seçin.

   Örneğin, CSV verilerini yapıştırın: [ ![Yapıştırılan CSV verileri](media/add-reference-data-set/select-csv-and-enter-data.png)](media/add-reference-data-set/select-csv-and-enter-data.png#lightbox)

   Örneğin, JSON dizi verilerini yapıştır: [ ![JSON verilerini yapıştır](media/add-reference-data-set/select-json-option-and-enter-data.png)](media/add-reference-data-set/select-json-option-and-enter-data.png#lightbox)

   Veri değerlerini ayrıştıran bir hata varsa, hata sayfanın alt kısmında kırmızı `CSV parsing error, no rows extracted`olarak görünür.

1. Veriler başarıyla ayrıştırıldıktan sonra, verileri temsil eden sütunları ve satırları gösteren bir veri ızgarası gösterilir. Doğruluğu sağlamak için veri ızgarasını gözden geçirin.

   [![Referans verilerini gözden geçirme](media/add-reference-data-set/review-displayed-data-grid.png)](media/add-reference-data-set/review-displayed-data-grid.png#lightbox)

1. Varsayılan veri türünü anlamak için her sütunu gözden geçirin ve gerekirse veri türünü değiştirin.  Sütun başlığındaki veri türü simgesini seçin: **#** çift (sayısal veri), **T| Boolean** için F, dize için **Abc.**

   [![Sütun başlıklarında veri türlerini seçin.](media/add-reference-data-set/select-column-types.png)](media/add-reference-data-set/select-column-types.png#lightbox)

1. Gerekirse sütun üstbilgisini yeniden adlandırın. Anahtar sütun adı olay kaynağınızda ilgili özelliği katılmak için gereklidir. 

   > [!IMPORTANT]
   > Başvuru veri sağlık sütun adlarının, vaka hassasiyeti de dahil olmak üzere gelen verilerinizle olay adıyla tam olarak çalıştığından emin olun. Anahtar olmayan sütun adları, gelen verileri karşılık gelen başvuru veri değerleriyle artırmak için kullanılır.

1. Gerektiğinde belirli satırları gözden geçirmek için satırları Filtre'ye bir değer **yazın...** alanı. Filtre verileri gözden geçirmek için yararlıdır, ancak verileri yüklerken uygulanmaz.

1. **Veri kümesinin** üzerindeki Veri kümesi ad alanını doldurarak veri kümesini adlandırın.

    [![Veri kümesini adlandırın.](media/add-reference-data-set/enter-reference-data-set-name.png)](media/add-reference-data-set/enter-reference-data-set-name.png#lightbox)

1. Veri ızgarasının üzerindeki açılır bırakmayı seçerek veri kümesindeki **Birincil Anahtar** sütununa sağlayın.

    [![Anahtar sütun(lar)ı seçin.](media/add-reference-data-set/select-primary-key-column.png)](media/add-reference-data-set/select-primary-key-column.png#lightbox)

    **(İsteğe bağlı)** Bileşik **+** birincil anahtar olarak ikincil bir anahtar sütunu eklemek için düğmeyi seçin. Seçimi geri almanız gerekiyorsa, ikincil anahtarı kaldırmak için açılır kaynaktan boş değeri seçin.

1. Verileri yüklemek için **Yükle satırları** düğmesini seçin.

    [![Satırları yükleyin ve verileri onaylayın.](media/add-reference-data-set/confirm-upload-reference-data.png)](media/add-reference-data-set/confirm-upload-reference-data.png#lightbox)

    Sayfa tamamlanan yüklemeyi onaylar ve İletiyi **başarıyla yüklenen veri kümesini**görüntüler.

    > [!WARNING]
    > Başvuru veri kümeleri arasında paylaşılan sütunlar veya özellikler **Yinelenen özellik adı** yükleme hatası görüntüler. Hata, başvuru veri kümelerinin başarılı bir şekilde yüklenmesini engellemez. Yinelenen özellik adını paylaşan satırları birleştirerek kaldırılabilir.

1. **Satır ekle,** **toplu içe aktarma satırları**veya gerektiğinde daha fazla başvuru veri değeri eklemek için sütun **ekleyin'i** seçin.

    [![Satır, Toplu alma satırları veya sütun ekleme ekleyin.](media/add-reference-data-set/add-row-or-bulk-upload.png)](media/add-reference-data-set/add-row-or-bulk-upload.png#lightbox)

   > [!IMPORTANT]
   > Benzersiz bir anahtarı başka bir satırla paylaşan herhangi bir satır, sütunlarını bu benzersiz anahtarı paylaşan son satıra eklenmiştir.

   > [!NOTE]
   > Eklenen **satırların** *dikdörtgen* olması gerekmez - başvuru veri kümesindeki diğer girişlerden daha az, daha büyük veya değişen sütunlara sahip olabilirler.

## <a name="next-steps"></a>Sonraki adımlar

* [Başvuru verilerini programlamayla yönetin](time-series-insights-manage-reference-data-csharp.md).

* Tam API başvurusu için [Başvuru Veri API belgesini](https://docs.microsoft.com/rest/api/time-series-insights/ga-reference-data-api) okuyun.
