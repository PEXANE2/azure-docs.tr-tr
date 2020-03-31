---
title: Power BI ile bir bilgi deposuna (önizleme) bağlanın
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama bilgi deposu (önizleme) ile Power BI'yi analiz ve keşif için bağlayın.
author: HeidiSteen
ms.author: heidist
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: 4fd71a7f322cb2672eb485f17e4de2619a7c2d2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270039"
---
# <a name="connect-a-knowledge-store-with-power-bi"></a>Power BI ile bir bilgi deposubağın

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Bu makalede, Power BI Desktop uygulamasında Power Query'yi kullanarak bir bilgi deposuna nasıl bağlanıp keşfederiz öğrenin. Şablonlarla daha hızlı başlanabilir veya sıfırdan özel bir pano oluşturabilirsiniz.

+ [Azure portalında bir bilgi deposu oluştur](knowledge-store-create-portal.md) veya bu yol boyunca kullanılan örnek bilgi deposunu oluşturmak için [REST'i kullanarak bir Azure Bilişsel Arama bilgi deposu oluşturun](knowledge-store-create-rest.md) adımlarını izleyin. Ayrıca, bilgi deposunu oluşturmak için kullandığınız Azure Depolama hesabının adının yanı sıra Azure portalından erişim anahtarına da ihtiyacınız olacaktır.

+ [Power BI Desktop uygulamasını yükleme](https://powerbi.microsoft.com/downloads/)

## <a name="sample-power-bi-template---azure-portal-only"></a>Örnek Güç BI şablonu - Yalnızca Azure portalı

[Azure portalını kullanarak](knowledge-store-create-portal.md)bir bilgi deposu oluştururken, **İçe Aktar veri** sihirbazının ikinci sayfasında bir Power BI [şablonu](https://github.com/Azure-Samples/cognitive-search-templates) indirme seçeneğiniz vardır. Bu şablon, metin tabanlı içerik için WordCloud ve Network Navigator gibi çeşitli görselleştirmeler sağlar. 

Şablonu herkese açık GitHub konumundan almak ve indirmek için **Bilişsel Beceriler Ekle** sayfasında Güç BI **Şablonu Alın'ı** tıklatın. Sihirbaz, sihirbazda belirtilen bilgi deposu projeksiyonlarında yakalandığı gibi, şablonu verilerinizin şeklini barındıracak şekilde değiştirir. Bu nedenle, indirdiğiniz şablon, farklı veri girişleri ve beceri seçimleri varsayarak sihirbazı her çalıştırdığınızda değişir.

![Örnek Azure Bilişsel Arama Gücü BI Şablonu](media/knowledge-store-connect-power-bi/powerbi-sample-template-portal-only.png "Örnek Güç BI şablonu")

> [!NOTE]
> Şablon uçuş sırasında yken karşıdan yüklenmiş olsa da, kullanmadan önce bilgi deposunun Azure Tablo depolama alanında gerçekten oluşturulmasını beklemeniz gerekir.

## <a name="connect-with-power-bi"></a>Power BI ile bağlanma

1. Power BI Desktop'ı başlatın ve **veri alın'ı**tıklatın.

1. Veri **Al** penceresinde **Azure'u**seçin ve ardından **Azure Tablo Depolama'yı**seçin.

1. **Bağlan**'a tıklayın.

1. **Hesap Adı veya URL**için Azure Depolama hesap adınızı girin (tam URL sizin için oluşturulur).

1. İstenirse, depolama hesabı anahtarını girin.

1. Önceki gözden geçirmeler tarafından oluşturulan otel inceleme verilerini içeren tabloları seçin. 

   + Portal izbisi için, masa adları *hotelReviewsSsDocument,* *hotelReviewsSsEntities,* *hotelReviewsSsKeyPhrases*, ve *hotelReviewsSsPages*. 
   
   + REST walkthrough için, masa adları *hotelReviewsDocument*, *hotelReviewsPages*, *hotelReviewsKeyPhrases*, ve *otelReviewsSentiment*vardır .

1. **Yükle'yi**tıklatın.

1. Üst şeritte, Power Query Düzenleyicisi'ni açmak için **Sorguları Edit'i** tıklatın. **Power Query Editor**

   ![Güç Sorgusuaç](media/knowledge-store-connect-power-bi/powerbi-edit-queries.png "Güç Sorgusuaç")

1. *HotelReviewsSsDocument'ı*seçin ve ardından *PartitionKey,* *RowKey*ve *Timestamp* sütunlarını kaldırın. 
   ![Tabloları edin](media/knowledge-store-connect-power-bi/powerbi-edit-table.png "Tabloları edin")

1. *İçeriği*genişletmek için tablonun sağ üst tarafındaki karşıt okların yer alan simgesini tıklatın. Sütunlar listesi göründüğünde, tüm sütunları seçin ve ardından 'meta verilerle' başlayan sütunları seçin. Seçili sütunları göstermek için **Tamam'ı** tıklatın.

   ![Tabloları edin](media/knowledge-store-connect-power-bi/powerbi-expand-content-table.png "İçeriği genişletme")

1. Sütunun sol üst köşesindeki ABC-123 simgesine tıklayarak aşağıdaki sütunların veri türünü değiştirin.

   + *content.latitude* and *Content.longitude*için **Ondalık Sayı'yı**seçin.
   + *Content.reviews_date* ve *Content.reviews_dateAdded*için **Tarih/Saat'i**seçin.

   ![Veri türlerini değiştirme](media/knowledge-store-connect-power-bi/powerbi-change-type.png "Veri türlerini değiştirme")

1. *hotelReviewsSsPages'i*seçin ve ardından sütunları silmek ve *İçeriği*genişletmek için 9 ve 10 adımlarını tekrarlayın.
1. *Content.SentimentScore* için veri türünü **Ondalık Sayı**ile değiştirin.
1. *HotelReviewsSsKeyPhrases'i* seçin ve sütunları silmek ve *İçeriği*genişletmek için 9 ve 10 adımlarını tekrarlayın. Bu tablo için veri türü değişikliği yok.

1. Komut çubuğunda **Kapat ve Uygula'yı**tıklatın.

1. Sol gezinti bölmesindeki Model döşemesine tıklayın ve Power BI'nin üç tablo arasındaki ilişkileri gösterdiğini doğrulayın.

   ![İlişkileri doğrulama](media/knowledge-store-connect-power-bi/powerbi-relationships.png "İlişkileri doğrulama")

1. Her ilişkiyi çift tıklatın ve **Çapraz filtre yönünün** **her ikisine**de ayarlandığınızdan emin olun.  Bu, bir filtre uygulandığında görsellerinizin yenilenmesini sağlar.

1. Görselleştirmeler aracılığıyla verileri keşfetmek için sol gezinti bölmesindeki Rapor döşemesine tıklayın. Metin alanları için tablolar ve kartlar kullanışlı görselleştirmelerdir. Tabloyu veya kartı doldurmak için üç tablonun her birinden alan seçebilirsiniz. 

<!-- ## Try with larger data sets

We purposely kept the data set small to avoid charges for a demo walkthrough. For a more realistic experience, you can create and then attach a billable Cognitive Services resource to enable a larger number of transactions against the sentiment analyzer, keyphrase extraction, and language detector skills.

Create new containers in Azure Blob storage and upload each CSV file to its own container. Specify one of these containers in the data source creation step in Import data wizard.

| Description | Link |
|-------------|------|
| Free tier   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Small (500 Records) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Medium (6000 Records)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Large (Full dataset 35000 Records) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Be aware that very large data sets are expensive to process. This one costs roughly $1000 U.S dollars.|

In the enrichment step of the wizard, attach a billable [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) resource, created at the *S0* tier, in the same region as Azure Cognitive Search to use larger data sets. 

  ![Create a Cognitive Services resource](media/knowledge-store-connect-power-bi/create-cognitive-service.png "Create a Cognitive Services resource") -->

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Depolama Gezgini'ni kullanarak bu bilgi deposunu nasıl keşfederiz öğrenmek için aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Depolama Gezgini ile görüntüleme](knowledge-store-view-storage-explorer.md)