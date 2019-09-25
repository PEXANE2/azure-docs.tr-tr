---
title: Power BI Azure Search bir bilgi deposuna bağlanma
description: Analiz ve araştırma için bir Azure Search bilgi deposu Power BI bağlayın.
author: lisaleib
services: search
ms.service: search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: de282213535a2e49f73bc30e476bae02d470fdb2
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265659"
---
# <a name="connect-a-knowledge-store-with-power-bi"></a>Bilgi deposunu Power BI bağlama

> [!Note]
> Bilgi deposu önizlemededir ve üretimde kullanılmamalıdır. [Azure Search REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>
Bu makalede, Power BI Desktop uygulamasındaki Power Query kullanarak bir bilgi deposunu nasıl bağlayacağınızı ve keşfedeceğinizi öğreneceksiniz. Bu kılavuzda kullanılan bilgi deposu örneğini oluşturmak için, bkz. [Azure Portal bilgi deposu oluşturma](knowledge-store-create-portal.md).

## <a name="prerequisites"></a>Önkoşullar

+ Bu kılavuzda kullanılan örnek bilgi deposunu oluşturmak için [Azure Portal bilgi deposu oluşturma](knowledge-store-create-portal.md) bölümündeki adımları izleyin. Ayrıca, bilgi deposunu oluşturmak için kullandığınız Azure depolama hesabının adına, Azure portal erişim anahtarıyla birlikte ihtiyacınız olacaktır.

+ [Power BI Desktop yüklensin](https://powerbi.microsoft.com/downloads/)

## <a name="connect-with-power-bi"></a>Power BI ile bağlanma

1. Power BI Desktop başlatın ve **veri al**' a tıklayın.

1. **Veri al** penceresinde **Azure**' ı seçin ve ardından **Azure Tablo depolama**' yı seçin.

1. **Bağlan**'a tıklayın.

1. **Hesap adı veya URL 'si**Için Azure depolama hesabınızın adını girin (sizin IÇIN tam URL oluşturulur).

1. İstenirse, depolama hesabı anahtarını girin.

1. *Hotelbelge\sdocument*, *HotelReviewsSsKeyPhrases*ve *Hotelbelge\spages* tablolarını seçin. Bu tablolar, otel 'nin Azure Tablo projeksiyonları, örnek verileri inceler ve bilgi deposu oluşturulduğunda seçilen bilişsel hizmet zenginleştirmelerini içerir.

1. **Yükle**' ye tıklayın.

1. **Power Query düzenleyicisini**açmak için üst şeritte **sorguları Düzenle** ' ye tıklayın.

   ![Power Query açın](media/knowledge-store-connect-power-bi/powerbi-edit-queries.png "Power Query açın")

1. *Hotelcode Ssındocument*' ı seçin ve ardından *partitionkey*, *rowkey*ve *timestamp* sütunlarını kaldırın. 

   ![Tabloları Düzenle](media/knowledge-store-connect-power-bi/powerbi-edit-table.png "Tabloları Düzenle")

1. *İçeriği*genişletmek için tablonun sağ üst tarafındaki karşıt oklu simgeye tıklayın. Sütun listesi göründüğünde, tüm sütunlar ' ı seçin ve ardından ' Metadata ' ile başlayan sütunların seçimini kaldırın. Seçilen sütunları göstermek için **Tamam** ' ı tıklatın.

   ![Tabloları Düzenle](media/knowledge-store-connect-power-bi/powerbi-expand-content-table.png "Içeriği Genişlet")

1. Sütunun sol üst kısmındaki ABC-123 simgesine tıklayarak aşağıdaki sütunlar için veri türünü değiştirin.

   + *Content. Enlem* ve *Content. Boylam*için **ondalık sayı**' yı seçin.
   + *Content. reviews_date* ve *Content. reviews_dateAdded*için **Tarih/saat**' i seçin.

   ![Veri türlerini değiştirme](media/knowledge-store-connect-power-bi/powerbi-change-type.png "Veri türlerini değiştirme")

1. *Hotelcode Ssspages*öğesini seçin ve ardından 9 ve 10. adımları yineleyerek sütunları silip *içeriği*genişletin.
1. *Content. Sentımentscore* için veri türünü **ondalık sayı**olarak değiştirin.
1. *HotelReviewsSsKeyPhrases* ' ı seçin ve sütunları silip *içeriği*genişletmek için 9. ve 10. adımları yineleyin. Bu tablo için veri türü değişikliği yok.

1. Komut çubuğunda **Kapat ve Uygula**' ya tıklayın.

1. Sol gezinti bölmesindeki model kutucuğuna tıklayın ve Power BI üç tablo arasındaki ilişkileri gösterir.

   ![Ilişkileri doğrula](media/knowledge-store-connect-power-bi/powerbi-relationships.png "Ilişkileri doğrula")

1. Her bir ilişkiye çift tıklayın ve **çapraz filtre yönünün** **her ikisine de**ayarlandığından emin olun.  Bu, bir filtre uygulandığında görsellerinizin yenilenmesini sağlar.

<!-- ## Try with larger data sets

We purposely kept the data set small to avoid charges for a demo walkthrough. For a more realistic experience, you can create and then attach a billable Cognitive Services resource to enable a larger number of transactions against the sentiment analyzer, keyphrase extraction, and language detector skills.

Create new containers in Azure Blob storage and upload each CSV file to its own container. Specify one of these containers in the data source creation step in Import data wizard.

| Description | Link |
|-------------|------|
| Free tier   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Small (500 Records) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Medium (6000 Records)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Large (Full dataset 35000 Records) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Be aware that very large data sets are expensive to process. This one costs roughly $1000 U.S dollars.|

In the enrichment step of the wizard, attach a billable [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) resource, created at the *S0* tier, in the same region as Azure Search to use larger data sets. 

  ![Create a Cognitive Services resource](media/knowledge-store-connect-power-bi/create-cognitive-service.png "Create a Cognitive Services resource") -->

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, sizin oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için bir projenin sonunda iyi bir fikir olur. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu bilgi deposunu Depolama Gezgini kullanarak nasıl keşfedeceğinizi öğrenmek için aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Depolama Gezgini ile görüntüle](knowledge-store-view-storage-explorer.md)

REST API 'Leri ve Postman kullanarak bilgi deposu oluşturmayı öğrenmek için aşağıdaki makaleye bakın.  

> [!div class="nextstepaction"]
> [REST 'te bilgi deposu oluşturma](knowledge-store-howto.md)
