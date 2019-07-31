---
title: Power BI ile bilgi deposuna bağlanma Azure Search
description: Azure portal veri alma sihirbazını kullanarak bir bilgi deposu oluşturun ve ardından analiz ve araştırma için Power BI bağlanın.
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: heidist
ms.openlocfilehash: 04b04e8080590aa6e9fe1c17369e83fa5bb6b894
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668224"
---
# <a name="create-an-azure-search-knowledge-store-and-connect-using-power-bi"></a>Azure Search bilgi deposu oluşturma ve Power BI kullanarak bağlanma

> [!Note]
> Bilgi deposu önizlemededir ve üretimde kullanılmamalıdır. [Azure Search REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Bilgi deposu, sonraki analizler veya diğer aşağı akış işlemleri için bir AI zenginleştirme ardışık düzeninde çıktıyı sürekli olarak sürdüren Azure Search bir özelliktir. Bir AI zenginleştirme işlem hattı, desteklenen bir veri kaynağından görüntü dosyalarını veya yapılandırılmamış metin dosyalarını kabul eder, onu Azure Search dizin oluşturmaya gönderir, bilişsel hizmetlerden AI zenginleştirme uygular (görüntü analizi ve doğal dil işleme) ve sonuçları kaydeder Azure depolama 'daki bir bilgi deposuna. Bilgi mağazasından, sonuçları araştırmak için Power BI veya Depolama Gezgini gibi araçlar ekleyebilirsiniz.

Bu makalede, portalda bir bilgi deposu oluşturun ve Power BI Desktop Power Query kullanarak bağlanın ve inceleyin. 

Bu izlenecek yol, müşteri İncelemeleri ve derecelendirmeleri, bilişsel hizmetlerden gelen yaklaşım puanlarından oluşan bir veri kümesini kullanır ve ardından belgelerinizi sorgulamak için Power Query. Veri otelden kaynaklanan veriler, Kaggle.com üzerindeki verileri gözden geçirir. 

## <a name="prerequisites"></a>Önkoşullar

+ [Otel INCELEMELERI CSV dosyası (HotelReviews_Free. csv) indirin](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Bu veri kümesi, oteller hakkındaki müşteri geri bildirimlerine ait kayıtları içerir.

+ [Power BI Desktop](https://powerbi.microsoft.com/downloads/)

+ [Azure Search](search-create-service-portal.md). Bu izlenecek yol için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Azure Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Hesabın varsayılan veya *depolama alanı (genel amaçlı v1)* olan "genel amaçlı", *StorageV2 (genel amaçlı v2)* olduğundan emin olun. Azure Search aynı bölgeyi seçin.
 
## <a name="prepare-data"></a>Verileri hazırlama 

Bir Azure Search Indexer tarafından erişilebilmesi için. csv dosyasını Azure Blob depolamaya yükleyin.

1. [Azure Portal oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, Bloblar ' a tıklayınve ardından **+ Container**' a tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) : 

   1. Kapsayıcıyı `hotel-reviews`adlandırın. 
   
   1. Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayın. Varsayılan değer kullandık.

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **karşıya yükle** ' yi seçin.

1. **HotelReviews-Free. csv**dosyasını içeren klasöre gidin, dosyayı seçin ve **karşıya yükle**' ye tıklayın.

   ![. Csv dosyasını karşıya yükle](media/knowledge-store-howto-powerbi/hotel-reviews-blob-container.png ". Csv dosyasını karşıya yükle")

1. Azure depolarken bağlantı dizesini ve kapsayıcı adını alın.  Veri kaynağı nesnesi oluştururken bu dizelerin her ikisine de ihtiyacınız olacak:

   1. Genel Bakış sayfasında, **erişim anahtarları** ' na tıklayın ve *bağlantı dizesini*kopyalayın. İle `DefaultEndpointsProtocol=https;` başlar ve ile `EndpointSuffix=core.windows.net`sonlanır. Hesap adınız ve anahtarınız arasında bulunur. 

   1. Kapsayıcı adı `hotel-reviews` ya da atadığınız ad olmalıdır. 

## <a name="create-and-run-ai-enrichments"></a>AI zenginleştirme oluşturma ve çalıştırma

Bilgi deposu oluşturmak için veri alma Sihirbazı 'nı kullanın. Veri kümesini içeri aktarır, zenginler ' i seçin, bir bilgi deposu ve bir dizin yapılandırın ve ardından öğesini yürütün.

1. [Arama hizmetinizi Azure Portal bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) .

1. Komut çubuğunda **verileri Içeri aktar** ' a tıklayın.

1. Sihirbazın ilk sayfasında veri kaynağı nesnesini oluşturun.

   - **Azure Blob depolama**' yı seçin.

   - Veri kaynağına *otel-incelemeler-DS*gibi bir ad verin.

   - Veriler. csv olduğundan, ayrıştırma modu için **sınırlandırılmış metin** ' i seçin, **Ilk satırı başlık içerir**' i seçin ve sınırlayıcı karakterin virgül olduğundan emin olun.

   - Azure depolama hesabınıza yönelik bağlantı dizesi, portalda **erişim tuşları**altında elde edilebilir.

   - Kapsayıcı adı, portaldan Azure depolama Blobları listenizde de alınabilir.

      ![Veri kaynağı nesnesi oluşturma](media/knowledge-store-howto-powerbi/hotel-reviews-ds.png "Veri kaynağı nesnesi oluşturma")

1. Sihirbazın ikinci sayfasında, zenginleştirme ekleyin ve bir bilgi deposu yapılandırın.

   - Bilişsel **Hizmetler iliştirme**' de, günde 20 ' ye kadar Işlem sağlayan ücretsiz kaynağı kullanabilirsiniz. HotelReviews-Free. csv dosyasındaki kayıt sayısı 20 ' den küçük.

   - **Zenginler Ekle**' de, Beceri *otel-İnceleme-SS*' y i, *reviews_text* alanını seçin, bir ayrıntı düzeyi *sayfa (5000 karakter öbeği)* seçin ve ardından şu üç bilişsel becerileri seçin: *Anahtar tümceciklerini ayıklayın*, *dili algılayın*, yaklaşımı *tespit*edin.

      ![Beceri oluşturma](media/knowledge-store-howto-powerbi/hotel-reviews-ss.png "Beceri oluşturma")

   - Zenginleştirme bilgilerini **bilgi deposuna kaydet**bölümünde, genel amaçlı Azure depolama hesabınıza bağlantı dizesi sağlayın. Bu bölümdeki *Azure Tablo projeleri* seçeneklerini belirlediğinizde, Azure Tablo depolamada bir bilgi deposu oluşturulur.

      ![Bilgi deposunu yapılandırma](media/knowledge-store-howto-powerbi/hotel-reviews-ks.png "Bilgi deposunu yapılandırma")
   
   İleri **' ye tıklayın: Sonraki adıma devam** etmek için hedef dizini özelleştirin.

1. Azure Search içinde isteğe bağlı tam metin arama sorguları için bir dizin yapılandırın. Bu izlenecek yol, Azure Tablo depolamaya Power BI bağlı olsa da, **verileri Içeri aktarma** sihirbazı, Azure Search tam metin araması için kullanılan bir dizin de oluşturabilir. 

   Sihirbaz, alanları ve veri türlerini çıkarması için veri kaynağınızı örneklerimizden, tüm yapmanız gereken davranışları kullanmak için gerekli öznitelikleri seçin. Örneğin, açık bir şekilde alan içeriğinin hizmetten *alınabileceği anlamına gelir* , *arama yapılabilen* seçili alanlarda tam metin aramasına izin verebilir.

   - Dizine *otel-incelemeler-idx*gibi bir ad verin.
   - Tüm alanları **alınabilir**olarak ayarlayın.
   - *Şehir*, *ad*, *reviews_text*, *Language*, *keyphrases* ile **aranabilir**olarak ayarlayın.
   - Yaklaşım, *Language*, *Keyphrases* olarak **filtrelenebilir** ve çok **yönlü tablo** *olarak ayarlayın.* 
   
    Dizininiz aşağıdaki görüntüye benzer görünmelidir.

     ![Dizin yapılandırma](media/knowledge-store-howto-powerbi/hotel-reviews-idx.png "Dizin yapılandırma")

   İleri **' ye tıklayın: Bir sonraki adıma** devam etmek için bir Dizin Oluşturucu oluşturun.

1. Bir dizin Oluşturucuyu bir ad ve yürütme temposunda vererek yapılandırın. Bu izlenecek yol için, dizin oluşturucu adı olarak *otel-incelemeler-ıdxr* kullanın ve Dizin oluşturucuyu hemen çalıştırmak için varsayılan değeri **bir kez** kullanın.

   Dizin Oluşturucu yürütme, önceki tüm yapılandırmaların hareket halinde koyar. Bu adımda ayıklama, işleme ve bunların tümünü geri almaya yönelik tüm işlemler gerçekleşir.

1. Portaldaki bildirimler kuyruğunda Dizin oluşturmayı izleyin. Yürütmenin tamamlanabilmesi birkaç dakika sürer.

## <a name="connect-with-power-bi"></a>Power BI ile bağlanma

1. Power BI Desktop başlatın ve **veri al**' ı seçin.

1. Veri Al bölümünde **Azure** ' u ve ardından **Azure Tablo depolama**' yı seçin. **Bağlan**'a tıklayın.

1. Hesap adı veya URL 'de, Azure depolama hesabınızın adını (tam URL sizin için çözülür) yapıştırın.

1. Hesap anahtarını girin.

1. Belge, KeyPhrases ve sayfa seçin. Bilgi deposu yapılandırmasında aynı adlı öğeleri seçtiğinizde, verileri Içeri aktarma Sihirbazı tarafından oluşturulan tablolar şunlardır. **Yükle**' ye tıklayın.

1. **Sorguları Düzenle** komutuna tıklayarak Power Query açın.

   ![Power Query açın](media/knowledge-store-howto-powerbi/powerbi-edit-queries.png "Power Query açın")

1. Belgeler için:

   - Azure Tablo Depolaması tarafından oluşturulan PartitionKey, RowKey ve timestamp sütunlarını kaldırın. Bilgi deposu bu çözümlemede kullanılan ilişkileri sağlar.

   - İçerik sütununu genişletin.

     ![Tabloları Düzenle](media/knowledge-store-howto-powerbi/powerbi-edit-table.png "Tabloları Düzenle")

1. Tüm alanlar ' ı seçin ve ardından "meta veriler" den başlayarak seçimini kaldırın.

1. Her sütunda ABC-123 simgesini kullanarak aşağıdaki sütunlar için veri türünü düzeltin:

   - Tarih sütunları **DateTime** olmalıdır
   - *Enlem* **ondalık sayı** olmalıdır
   - *Boylam* **ondalık sayı** olmalıdır

1. KeyPhrases için önceki adımları tekrarlayın, PartitionKey ve diğer sütunları kaldırarak, içerik sütunlarını genişleterek, *Sentimentscore* öğesini **ondalık sayı**olarak ayarlama.

1. Sayfalar için yeniden tekrar tekrar, PartitionKey ve diğer sütunları kaldırarak içerik sütunları genişleterek. Bu tablo için veri türü değişikliği yok.

1. Kapat ' a tıklayın ve Power Query komut çubuğunun sol tarafında **Uygula** ' ya tıklayın.

1. Power BI, verileriniz içinde bilgi deposunun oluşturduğu ilişkileri tanıdığını doğrulayın. Sol gezinti bölmesindeki ilişkiler kutucuğuna tıklayın. Üç tablonun hepsi ilişkili olmalıdır.

   ![Ilişkileri doğrula](media/knowledge-store-howto-powerbi/powerbi-relationships.png "Ilişkileri doğrula")

## <a name="try-with-larger-data-sets"></a>Daha büyük veri kümeleriyle deneyin

Tanıtım anlatımına yönelik ücretlerden kaçınmak için veri kümesinin küçük bir şekilde tutulmasını sağlıyoruz. Daha gerçekçi bir deneyim için, yaklaşım Çözümleyicisi, keyphrase ayıklama ve dil algılayıcı becerileriyle daha fazla sayıda işlem sağlamak üzere faturalanabilir bir bilişsel hizmetler kaynağı oluşturup daha sonra ekleyebilirsiniz.

Azure Blob depolamada yeni kapsayıcılar oluşturun ve her CSV dosyasını kendi kapsayıcısına yükleyin. Veri alma Sihirbazı 'nda veri kaynağı oluşturma adımında bu kapsayıcılardan birini belirtin.

| Açıklama | Bağlantı |
|-------------|------|
| Ücretsiz katman   | [HotelReviews_Free. csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Küçük (500 kayıt) | [HotelReviews_Small. csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Orta (6000 kayıt)| [HotelReviews_Medium. csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Büyük (tam veri kümesi 35000 kayıtları) | [HotelReviews_Large. csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Çok büyük veri kümelerinin işleme göre pahalı olduğunu unutmayın. Bu bir maliyet kabaca $1000 ABD Doları.|

Sihirbazın zenginleştirme adımında, daha büyük veri kümeleri kullanmak için Azure Search *S0* katmanında oluşturulan faturalandırılabilir bilişsel [Hizmetler](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) kaynağını aynı bölgede kullanın. 

  Bilişsel ![Hizmetler kaynağı oluşturma] Bilişsel (media/knowledge-store-howto-powerbi/create-cognitive-service.png "Hizmetler kaynağı oluşturma")

## <a name="next-steps"></a>Sonraki adımlar

Bu alıştırmayı yinelemek veya başka bir AI zenginleştirme Kılavuzu yapmak istiyorsanız, az önce oluşturduğunuz *otel-incelemeler-IDX* Dizin oluşturucuyu silin. Dizin oluşturucunun silinmesi, ücretsiz günlük işlem sayacını yeniden sıfıra sıfırlar. 

Bilgi deposunu gösteren daha fazla adım adım izlenecek yol için, REST API 'Leri ve Postman kullanarak bilgi deposu oluşturmayı gösteren bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Bilgi deposu ile çalışmaya başlama](knowledge-store-howto.md)