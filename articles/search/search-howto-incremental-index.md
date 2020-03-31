---
title: Önbelleği ve artımlı zenginleştirmeyi yapılandırma (önizleme)
titleSuffix: Azure Cognitive Search
description: Bilişsel becerilerde kontrollü işleme için zenginleştirilmiş içeriğin önbelleğe alma durumunu etkinleştirin ve koruyun. Bu özellik şu anda genel önizlemede dir.
author: vkurpad
manager: eladz
ms.author: vikurpad
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 66bac2a063a3257a2101ca2f30e5946264adb9ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989561"
---
# <a name="how-to-configure-caching-for-incremental-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da artımlı zenginleştirme için önbelleğe alma nasıl yapılandırılmaz?

> [!IMPORTANT] 
> Artımlı zenginleştirme şu anda genel önizlemede. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

Bu makalede, her seferinde yeniden oluşturmak zorunda kalmadan adımları artımlı olarak değiştirebilmeniz için bir zenginleştirme ardışık hattına önbelleğe nasıl ekleyeceğinizi gösterir. Varsayılan olarak, bir skillset devletsizdir ve bileşiminin herhangi bir bölümünü değiştirmek dizinleyicinin tam bir yeniden çalışmasını gerektirir. Artımlı zenginleştirme ile dizinleyici, beceri veya dizinleyici tanımlarında algılanan değişikliklere göre belge ağacının hangi bölümlerinin yenilenmesi gerektiğini belirleyebilir. Mevcut işlenmiş çıktı korunur ve mümkün olan her yerde yeniden kullanılır. 

Önbelleğe alınan içerik, sağladığınız hesap bilgilerini kullanarak Azure Depolama'ya yerleştirilir. Dizin oluşturucuyu çalıştırdığınızda, adlı `ms-az-search-indexercache-<alpha-numerc-string>`kapsayıcı oluşturulur. Arama hizmetiniz tarafından yönetilen bir dahili bileşen olarak kabul edilmelidir ve değiştirilmemelidir.

Dizin oluşturma yı bilmiyorsanız, [dizin leyici genel bakışı](search-indexer-overview.md) yla başlayın ve ardından zenginleştirme ardışık hatları hakkında bilgi edinmek için beceri [kümelerine](cognitive-search-working-with-skillsets.md) devam edin. Anahtar kavramlar hakkında daha fazla arka plan için [artımlı zenginleştirme](cognitive-search-incremental-indexing-conceptual.md)bakın.

## <a name="enable-caching-on-an-existing-indexer"></a>Varolan bir dizin leyicide önbelleğe alma etkinleştirme

Zaten bir beceriye sahip varolan bir dizin leyiciniz varsa, önbelleğe alma eklemek için bu bölümdeki adımları izleyin. Tek seferlik bir işlem olarak, artımlı işlemenin etkili olması için dizin leyiciyi tam olarak sıfırlamanız ve yeniden çalıştırmanız gerekir.

> [!TIP]
> Kavram kanıtı olarak, gerekli nesneleri oluşturmak için bu [portalquickstart](cognitive-search-quickstart-blob.md) üzerinden çalıştırabilirsiniz ve daha sonra güncellemeler yapmak için Postacı veya portal ı kullanabilirsiniz. Faturalandırılabilir Bilişsel Hizmetler kaynağı eklemek isteyebilirsiniz. Tüm adımları tamamlamadan önce dizin leyiciyi birden çok kez çalıştırmak ücretsiz günlük ayırmayı tüketir.

### <a name="step-1-get-the-indexer-definition"></a>Adım 1: Dizinleyici tanımını alın

Veri kaynağı, skillset, index: Bu bileşenlere sahip geçerli, varolan bir dizinleyici ile başlayın. Dizin leyiciniz çalıştırılabilir olmalıdır. 

Bir API istemcisi kullanarak, dizinleyicinin geçerli yapılandırmasını almak için bir [GET Indexer isteği](https://docs.microsoft.com/rest/api/searchservice/get-indexer) oluşturun. Önizleme API sürümünü GET dizinleyiciiçin kullandığınızda, tanımlara null olarak ayarlanmış bir `cache` özellik eklenir.

```http
GET https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Yanıttan dizinleyici tanımını kopyalayın.

### <a name="step-2-modify-the-cache-property-in-the-indexer-definition"></a>Adım 2: Dizinleyici tanımında önbellek özelliğini değiştirme

Varsayılan olarak `cache` özellik null. Önbellek yapılandırmasını ayarlamak için bir API istemcisi kullanın (portal bu partikül güncelleştirmesini desteklemez). 

Önbellek nesnesini aşağıdaki gerekli ve isteğe bağlı özellikleri içerecek şekilde değiştirin: 

+ Gerekli `storageConnectionString` olan ve bir Azure depolama bağlantı dizesi olarak ayarlanması gerekir. 
+ Boolean `enableReprocessing` özelliği isteğe`true` bağlıdır (varsayılan olarak) ve artımlı zenginleştirme etkin olduğunu gösterir. Gerektiğinde, yeni belgeleri dizine alma gibi diğer kaynak yoğun işlemler devam ederken artımlı işlemleri askıya alacak şekilde ayarlayabilir `false` ve sonra geri döndürebilirsiniz. `true`

```json
{
    "name": "<YOUR-INDEXER-NAME>",
    "targetIndexName": "<YOUR-INDEX-NAME>",
    "dataSourceName": "<YOUR-DATASOURCE-NAME>",
    "skillsetName": "<YOUR-SKILLSET-NAME>",
    "cache" : {
        "storageConnectionString" : "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
}
```

### <a name="step-3-reset-the-indexer"></a>Adım 3: Dizinleyiciyi sıfırlama

Tüm belgelerin tutarlı bir durumda olduğundan emin olmak için varolan dizinleyiciler için artımlı zenginleştirme ayarlarken dizinleyicinin sıfırlanması gerekir. Bu görev için portalı veya API istemcisini ve [Reset Indexer REST API'yi](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) kullanabilirsiniz.

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

### <a name="step-4-save-the-updated-definition"></a>Adım 4: Güncelleştirilmiş tanımı kaydetme

[Dizin leyiciyi](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/update-indexer) PUT isteğiyle güncelleştirin, isteğin gövdesi önbellek özelliğine sahip güncelleştirilmiş dizinleyici tanımını içermelidir. 400 alırsanız, tüm gereksinimlerin karşılandıkğınızdan emin olmak için dizinleyici tanımını kontrol edin (veri kaynağı, skillset, index).

```http
PUT https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
{
    "name" : "<YOUR-INDEXER-NAME>",
    ...
    "cache": {
        "storageConnectionString": "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    }
}
```

Şimdi dizinleyicide başka bir GET isteği verirseniz, hizmetten gelen yanıt önbellek nesnesinde bir `ID` özellik içerir. Alfasayısal dize, bu dizinleyici tarafından işlenen her belgenin tüm önbelleğe alınmış sonuçları ve ara durumunu içeren kapsayıcının adına eklenir. Kimlik, Blob depolama alanında önbelleğe benzersiz bir şekilde ad vermek için kullanılır.

    "cache": {
        "ID": "<ALPHA-NUMERIC STRING>",
        "enableReprocessing": true,
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
    }

### <a name="step-5-run-the-indexer"></a>Adım 5: Dizinleyiciyi çalıştırın

Dizinleyiciçalıştırmak için portalı veya API'yi kullanabilirsiniz. Portalda, dizinleyiciler listesinden dizinleyiciyi seçin ve **Çalıştır'ı**tıklatın. Portalı kullanmanın bir avantajı, dizinleyici durumunu izleyebilirsiniz, işin süresini ve kaç belgenin işlendiğini not edebilirsiniz. Portal sayfaları birkaç dakikada bir yenilenir.

Alternatif olarak, [dizinleyici çalıştırmak](https://docs.microsoft.com/rest/api/searchservice/run-indexer)için REST kullanabilirsiniz:

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/run?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Dizin leyici çalıştırdıktan sonra önbelleği Azure Blob depolama alanında bulabilirsiniz. Kapsayıcı adı aşağıdaki biçimdedir:`ms-az-search-indexercache-<YOUR-CACHE-ID>`

> [!NOTE]
> Dizin oluşturucunun sıfırlanıp yeniden çalıştırılması, içeriğin önbelleğe alınabilmesi için yeniden oluşturulmasına neden olur. Tüm bilişsel zenginleştirmeler tüm belgelerde yeniden yayınlanacaktır.
>

### <a name="step-6-modify-a-skillset-and-confirm-incremental-enrichment"></a>Adım 6: Bir beceri değiştirin ve artımlı zenginleştirme onaylamak

Bir beceriyi değiştirmek için portalı veya API'yi kullanabilirsiniz. Örneğin, metin çevirisi kullanıyorsanız, artımlı zenginleştirme kavram kanıtı testi için basit bir satır içi değişiklik `en` veya `es` başka bir dile geçiş yeterlidir.

Dizinleyiciyi yeniden çalıştırın. Yalnızca zenginleştirilmiş belge ağacının bölümleri güncelleştirilir. [Portalquickstart'ı](cognitive-search-quickstart-blob.md) kavram kanıtı olarak kullandıysanız, metin çeviri becerisini 'es' olarak değiştirdiyseniz, orijinal 14 yerine yalnızca 8 belgenin güncelleştirdiğini fark esiniz. Çeviri işleminden etkilenmeyen resim dosyaları önbellekten yeniden kullanılır.

## <a name="enable-caching-on-new-indexers"></a>Yeni dizin leyiciler üzerinde önbelleğe alma etkinleştirme

Yeni bir dizin oluşturucu için artımlı zenginleştirme ayarlamak için `cache` tek yapmanız [gereken, Dizin Oluştur'u (2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/create-indexer)ararken dizinleyici tanımı yüküne özelliği eklemektir. 


```json
{
    "name": "<YOUR-INDEXER-NAME>",
    "targetIndexName": "<YOUR-INDEX-NAME>",
    "dataSourceName": "<YOUR-DATASOURCE-NAME>",
    "skillsetName": "<YOUR-SKILLSET-NAME>",
    "cache" : {
        "storageConnectionString" : "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
    }
}
```

## <a name="checking-for-cached-output"></a>Önbelleğe alınmış çıktıyı denetleme

Önbellek dizin leyici tarafından oluşturulur, kullanılır ve yönetilir ve içeriği insan tarafından okunabilir bir biçimde temsil edilmez. Önbelleğin kullanılıp kullanılmadığını belirlemenin en iyi yolu dizinleyiciyi çalıştırmak ve yürütme süresi ve belge sayımları için önce ve sonra ölçümlerini karşılaştırmaktır. 

Örneğin, taranmış belgelerin görüntü analizi ve Optik Karakter Tanıma (OCR) ile başlayan bir beceri kellesini ve ardından ortaya çıkan metnin akış aşağı analizini varsayalım. Bir akış aşağı metin becerisini değiştirirseniz, dizinleyici önceden işlenmiş tüm görüntüyü ve OCR içeriğini önbellekten alabilir, düzenlemelerinizde belirtilen yalnızca metinle ilgili değişiklikleri güncelleyebilir ve işleyebilir. Belge sayısında daha az belge (örneğin, orijinal çalıştırmada 14/14 yerine 8/8), daha kısa yürütme süreleri ve faturanızda daha az ücret görmeyi bekleyebilirsiniz.

## <a name="working-with-the-cache"></a>Önbellekle çalışma

Önbellek çalışmaya başladıktan sonra, dizinleyiciler varolan çıktının hangi bölümlerinin kullanılabileceğini görmek için [Çalıştır Dizinleyici](https://docs.microsoft.com/rest/api/searchservice/run-indexer) çağrıldığında önbelleği denetler. 

Aşağıdaki tablo, çeşitli API'lerin önbellekle nasıl ilişkili olduğunu özetleyin:

| API           | Önbellek etkisi     |
|---------------|------------------|
| [Indexer Oluştur (2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/create-indexer) | Dizin leyici tanımınız bunu belirtiyorsa önbellek oluşturma da dahil olmak üzere ilk kullanımda bir dizin oluşturup çalıştırır. |
| [Çalıştır Dizinleyici](https://docs.microsoft.com/rest/api/searchservice/run-indexer) | Talep üzerine bir zenginleştirme boru hattı yürütür. Bu API varsa önbellekten okur veya güncelleştirilmiş bir dizin leyici tanımına önbelleğe alma eklediyseniz bir önbellek oluşturur. Önbelleğe alınmış bir dizin oluşturma etkin olduğunda, önbelleğe alınmış çıktı kullanılabilirse dizinleyici adımları atlar. Bu API'nin genel olarak kullanılabilir veya önizleme API sürümünü kullanabilirsiniz.|
| [Reset Indexer](https://docs.microsoft.com/rest/api/searchservice/reset-indexer)| Herhangi bir artımlı dizin bilgisi dizinleyici temizler. Bir sonraki dizin leyici çalışması (isteğe bağlı veya zamanlama) tüm becerileri yeniden çalıştırmak ve önbelleği yeniden oluşturmak da dahil olmak üzere sıfırdan tam yeniden işlemedir. İşlevsel olarak dizin oluşturucuyu silip yeniden oluşturmaya eşdeğerdir. Bu API'nin genel olarak kullanılabilir veya önizleme API sürümünü kullanabilirsiniz.|
| [Becerileri Sıfırlama (2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/reset-skills) | Herhangi bir beceri yideğmemiş olsanız bile, bir sonraki dizinleyici çalışmasında hangi becerileri yeniden çalıştırabileceğinizi belirtir. Önbellek buna göre güncelleştirilir. Bilgi deposu veya arama dizini gibi çıktılar, önbellekten yeniden kullanılabilir veriler ve güncelleştirilmiş beceri başına yeni içerik kullanılarak yenilenir. |

Önbelleğe ne olacağını denetleme hakkında daha fazla bilgi için [Önbellek yönetimine](cognitive-search-incremental-indexing-conceptual.md#cache-management)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Artımlı zenginleştirme, beceri içeren dizinleyicilerde uygulanabilir. Bir sonraki adım olarak, kavramları ve kompozisyonu anlamak için skillset dokümantasyonlarını ziyaret edin. 

Ayrıca, önbelleği etkinleştirdikten sonra, belirli davranışları nasıl geçersiz kakacağınız veya zorlayacağınız da dahil olmak üzere önbelleğe alma faktörü olan parametreler ve API'ler hakkında bilgi almak istersiniz. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

+ [Skillset kavramları ve kompozisyonu](cognitive-search-working-with-skillsets.md)
+ [Nasıl bir skillset oluşturmak için](cognitive-search-defining-skillset.md)
+ [Artımlı zenginleştirme ve önbelleğe giriş](cognitive-search-incremental-indexing-conceptual.md)
