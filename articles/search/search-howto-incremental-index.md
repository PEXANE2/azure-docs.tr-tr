---
title: Önbellek ve artımlı zenginleştirme yapılandırma (Önizleme)
titleSuffix: Azure Cognitive Search
description: Bilişsel Beceri 'de denetimli işleme için önbelleğe alma ve koruma durumunu korumayı etkinleştirin. Bu özellik şu anda genel önizleme aşamasındadır.
author: vkurpad
manager: eladz
ms.author: vikurpad
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 6af9db5ed76ecb79f8891895eab52ff71bcab048
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86146876"
---
# <a name="how-to-configure-caching-for-incremental-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama artımlı zenginleştirme için önbelleğe alma yapılandırma

> [!IMPORTANT] 
> Artımlı zenginleştirme Şu anda genel önizlemededir. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [2019-05-06-Preview ve 2020-06-30-preview REST API sürümleri](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

Bu makalede, her seferinde yeniden derlemek zorunda kalmadan adımları artımlı olarak değiştirmek için bir zenginleştirme ardışık düzenine nasıl önbelleğe alma ekleyeceğiniz gösterilmektedir. Varsayılan olarak, bir beceri durum bilgisiz değildir ve kompozisyonunun herhangi bir bölümünü değiştirmek, dizin oluşturucunun tam yeniden çalıştırma işlemini gerektirir. Artımlı zenginleştirme ile, Dizin Oluşturucu, belge ağacının hangi bölümlerinin beceri veya Dizin Oluşturucu tanımlarında algılanan değişikliklere göre yenilenmesi gerektiğini belirleyebilir. Mevcut işlenen çıktı korunur ve mümkün olan yerlerde yeniden kullanılabilir. 

Önbelleğe alınan içerikler, sağladığınız hesap bilgileri kullanılarak Azure depolama alanına yerleştirilir. Adlı kapsayıcı, `ms-az-search-indexercache-<alpha-numerc-string>` Dizin oluşturucuyu çalıştırdığınızda oluşturulur. Arama hizmetiniz tarafından yönetilen bir iç bileşen olarak kabul edilmelidir ve değiştirilmemesi gerekir.

Dizinleyicilerle ilgili bilgi sahibi değilseniz, [dizin oluşturucuya genel bakış](search-indexer-overview.md) ile başlayın ve ardından zenginleştirme işlem hatları hakkında bilgi edinmek için [becerileri](cognitive-search-working-with-skillsets.md) 'ye devam edin. Temel kavramlar hakkında daha fazla arka plan için bkz. [artımlı zenginleştirme](cognitive-search-incremental-indexing-conceptual.md).

## <a name="enable-caching-on-an-existing-indexer"></a>Mevcut bir dizin oluşturucuda önbelleğe almayı etkinleştir

Zaten bir beceri olan mevcut bir dizin oluşturucunuz varsa, önbelleğe alma eklemek için bu bölümdeki adımları izleyin. Tek seferlik bir işlem olarak, artımlı işlemenin etkili olabilmesi için Dizin oluşturucuyu tam olarak sıfırlamanız ve yeniden çalıştırmanız gerekir.

> [!TIP]
> Kavram kanıtı olarak, gerekli nesneleri oluşturmak için bu [Portal Hızlı](cognitive-search-quickstart-blob.md) başlangıcı aracılığıyla çalışabilir ve sonra güncelleştirmelerinizi yapmak Için Postman veya Portal ' ı kullanabilirsiniz. Faturalanabilir bilişsel hizmetler kaynağı eklemek isteyebilirsiniz. Dizin oluşturucuyu birden çok kez çalıştırmak, tüm adımları tamamlamadan önce günlük ücretsiz ayırmayı tükecektir.

### <a name="step-1-get-the-indexer-definition"></a>1. Adım: Dizin Oluşturucu tanımını alın

Şu bileşenlere sahip geçerli, mevcut bir Dizin Oluşturucu ile başlayın: veri kaynağı, Beceri, dizin. Dizin oluşturucunun çalıştırılabilir olması gerekir. 

Bir API istemcisi kullanarak, dizin oluşturucunun geçerli yapılandırmasını almak için bir [Get Indexer isteği](https://docs.microsoft.com/rest/api/searchservice/get-indexer) oluşturun. Dizin oluşturucuyu al için Önizleme API sürümünü kullandığınızda, `cache` tanımlara null olarak ayarlanmış bir özellik eklenir.

```http
GET https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Dizin Oluşturucu tanımını yanıttan kopyalayın.

### <a name="step-2-modify-the-cache-property-in-the-indexer-definition"></a>2. Adım: Dizin Oluşturucu tanımındaki Cache özelliğini değiştirme

Varsayılan olarak, `cache` özelliği null olur. Önbellek yapılandırmasını ayarlamak için bir API istemcisi kullanın (Portal bu Particulate güncelleştirmesini desteklemez). 

Önbellek nesnesini aşağıdaki gerekli ve isteğe bağlı özellikleri içerecek şekilde değiştirin: 

+ `storageConnectionString`Gerekir ve bir Azure depolama bağlantı dizesine ayarlanmalıdır. 
+ `enableReprocessing`Boolean özelliği isteğe bağlıdır ( `true` Varsayılan olarak) ve artımlı zenginleştirme özelliğinin etkinleştirildiğini gösterir. Gerektiğinde, `false` yeni belgeler için dizin oluşturma gibi diğer kaynak yoğun işlemler çalışma ve daha sonra geri çevirme gibi diğer kaynak yoğunluklu işlemler devam ederken, bunu artımlı işlemeyi askıya almak için ayarlayabilirsiniz `true` .

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

### <a name="step-3-reset-the-indexer"></a>3. Adım: Dizin oluşturucuyu sıfırlama

Tüm belgelerin tutarlı bir durumda olduğundan emin olmak için, mevcut dizin oluşturucular için artımlı zenginleştirme ayarlanırken dizin oluşturucunun sıfırlanması gerekir. Bu görev için Portal 'ı veya bir API istemcisini ve [sıfırlama dizin oluşturucuyu REST API](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) kullanabilirsiniz.

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/reset?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

### <a name="step-4-save-the-updated-definition"></a>4. Adım: güncelleştirilmiş tanımı kaydetme

[Dizin oluşturucuyu](https://docs.microsoft.com/rest/api/searchservice/preview-api/update-indexer) bir put isteğiyle güncelleştirme, isteğin gövdesi Cache özelliğine sahip güncelleştirilmiş Dizin Oluşturucu tanımını içermelidir. 400 alırsanız, tüm gereksinimlerin karşılandığından emin olmak için Dizin Oluşturucu tanımını denetleyin (veri kaynağı, Beceri, dizin).

```http
PUT https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2020-06-30-Preview
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

Artık dizin oluşturucuda başka bir GET isteği verirseniz, hizmetten gelen yanıt `ID` Cache nesnesine bir özellik dahil edilir. Alfasayısal dize, tüm önbelleğe alınmış sonuçları ve bu Dizin Oluşturucu tarafından işlenen her bir belgenin ara durumunu içeren kapsayıcının adına eklenir. KIMLIĞI, blob depolamada önbelleği benzersiz şekilde adlandırmak için kullanılacaktır.

```http
    "cache": {
        "ID": "<ALPHA-NUMERIC STRING>",
        "enableReprocessing": true,
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
    }
```

### <a name="step-5-run-the-indexer"></a>5. Adım: Dizin oluşturucuyu çalıştırma

Dizin oluşturucuyu çalıştırmak için portalını veya API 'yi kullanabilirsiniz. Portalda, Dizin oluşturucular listesinden Dizin oluşturucuyu seçin ve **Çalıştır**' a tıklayın. Portalı kullanmanın bir avantajı, Dizin Oluşturucu durumunu izleyebilmeniz, işin süresini ve işlenen belge sayısını görebileceğinizi unutmayın. Portal sayfaları birkaç dakikada bir yenilenir.

Alternatif olarak, şu [Dizin oluşturucuyu çalıştırmak](https://docs.microsoft.com/rest/api/searchservice/run-indexer)için REST kullanabilirsiniz:

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/run?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Dizin Oluşturucu çalıştıktan sonra, önbelleği Azure Blob depolamada bulabilirsiniz. Kapsayıcı adı şu biçimdedir:`ms-az-search-indexercache-<YOUR-CACHE-ID>`

> [!NOTE]
> Dizin oluşturucunun sıfırlanması ve yeniden çalıştırılması, içeriğin önbelleğe alınabilmesi için tam yeniden oluşturma işlemine neden olur. Tüm bilişsel zenginler tüm belgelerde yeniden çalıştırılır.
>

### <a name="step-6-modify-a-skillset-and-confirm-incremental-enrichment"></a>6. Adım: bir beceri değiştirme ve artımlı zenginleştirme onaylama

Bir beceri değiştirmek için portalını veya API 'yi kullanabilirsiniz. Örneğin, metin çevirisi kullanıyorsanız, ' den veya başka bir dilde basit bir satır içi değişiklik, `en` `es` artımlı zenginleştirme kavram kanıtı testi için yeterlidir.

Dizin oluşturucuyu yeniden çalıştırın. Yalnızca zenginleştirilmiş belge ağacının bölümleri güncellenir. [Portal Hızlı](cognitive-search-quickstart-blob.md) başlangıcını kavram kanıtı olarak kullandıysanız ve metin çevirisi yetenini ' es ' olarak değiştirirken, özgün 14 yerine yalnızca 8 belge güncelleştirildiğini fark edersiniz. Çeviri işleminden etkilenen görüntü dosyaları önbellekten yeniden kullanılır.

## <a name="enable-caching-on-new-indexers"></a>Yeni Dizin oluşturucular üzerinde önbelleğe almayı etkinleştir

Yeni bir Dizin Oluşturucu için artımlı zenginleştirme ayarlamak için, tek yapmanız gereken, `cache` [Create Indexer (2020-06-30-Preview)](https://docs.microsoft.com/rest/api/searchservice/preview-api/create-indexer)çağrılırken Dizin Oluşturucu tanımı yüküne özelliği içerir. 


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

## <a name="checking-for-cached-output"></a>Önbelleğe alınan çıkış denetleniyor

Önbellek, Dizin Oluşturucu tarafından oluşturulur, kullanılır ve yönetilir ve içeriği insanlar tarafından okunabilen bir biçimde temsil edilmez. Önbelleğin kullanılıp kullanılmadığını belirlemenin en iyi yolu, Dizin oluşturucuyu çalıştırıp yürütme süresi ve belge sayıları için önceki ve sonraki ölçümleri karşılaştırın. 

Örneğin, taranmış belgelerin görüntü analizi ve optik karakter tanıma (OCR) ile başlayan bir beceri ve ardından sonuçta elde edilen metnin aşağı akış analizinden oluşan bir varsayın. Bir aşağı akış metin açıklamasını değiştirirseniz, Dizin Oluşturucu daha önce işlenmiş olan görüntünün ve OCR içeriğinin tümünü, düzenlemeleriniz tarafından belirtilen metinle ilgili değişikliklerden yalnızca, güncelleştirmeden ve işlemeden alabilir. Belge sayısında daha az belge görmeyi bekleyebilir (8/8 örneğin, özgün çalıştırmada 14/14 aksine), daha kısa yürütme süreleri ve faturanızda daha az ücret alabilirsiniz.

## <a name="working-with-the-cache"></a>Önbellek ile çalışma

Önbellek çalışmaya başladıktan sonra, mevcut çıkışın hangi bölümlerinin kullanılabileceğini görmek için Dizin [Oluşturucu](https://docs.microsoft.com/rest/api/searchservice/run-indexer) her çağrıldığında, Dizin oluşturucular önbelleği denetler. 

Aşağıdaki tabloda, çeşitli API 'Lerin önbellekle bağlantılı olduğu özetlenmektedir:

| API           | Önbellek etkisi     |
|---------------|------------------|
| [Dizin Oluşturucu oluştur (2020-06-30-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/preview-api/create-indexer) | Dizin Oluşturucu tanımınızda bir önbellek oluşturma da dahil olmak üzere ilk kullanımda bir dizin oluşturucu oluşturur ve çalıştırır. |
| [Dizin oluşturucuyu Çalıştır](https://docs.microsoft.com/rest/api/searchservice/run-indexer) | İsteğe bağlı olarak bir zenginleştirme işlem hattı yürütür. Bu API, varsa önbellekten okur veya güncelleştirilmiş bir Dizin Oluşturucu tanımına önbelleğe alma eklediyseniz bir önbellek oluşturur. Önbelleğe alma özelliği etkin olan bir Dizin Oluşturucu çalıştırdığınızda, önbelleğe alınmış çıkışın kullanılabilmesi için Dizin Oluşturucu adımları atlar. Bu API 'nin genel kullanıma açık veya önizleme API sürümünü kullanabilirsiniz.|
| [Dizin oluşturucuyu Sıfırla](https://docs.microsoft.com/rest/api/searchservice/reset-indexer)| Herhangi bir artımlı dizin oluşturma bilgisinin Dizin oluşturucuyu temizler. Sonraki bir Dizin Oluşturucu çalıştırması (isteğe bağlı veya zamanlama), tüm becerileri yeniden çalıştırmak ve önbelleği yeniden oluşturmak dahil olmak üzere sıfırdan tam işleme alır. Dizin oluşturucuyu silmek ve yeniden oluşturmak için işlevsel olarak eşdeğerdir. Bu API 'nin genel kullanıma açık veya önizleme API sürümünü kullanabilirsiniz.|
| [Becerileri sıfırlama](https://docs.microsoft.com/rest/api/searchservice/reset-skills) | Herhangi bir becerileri değiştirmemiş olsanız bile, bir sonraki Dizin Oluşturucu üzerinde hangi yeteneklerin yeniden çalıştırılacağını belirtir. Önbellek uygun şekilde güncelleştirilir. Bilgi deposu veya arama dizini gibi çıkışlar, önbellekteki yeniden kullanılabilir veriler ve güncelleştirilmiş yetenek başına yeni içerik kullanılarak yenilenir. |

Önbellekte ne olacağını denetleme hakkında daha fazla bilgi için bkz. [önbellek yönetimi](cognitive-search-incremental-indexing-conceptual.md#cache-management).

## <a name="next-steps"></a>Sonraki adımlar

Artımlı zenginleştirme, becerileri içeren dizin oluşturucular için geçerlidir. Sonraki adım olarak, kavramları ve kompozisyonu anlamak için beceri belgelerini ziyaret edin. 

Ayrıca, önbelleği etkinleştirdikten sonra, belirli davranışları geçersiz kılma veya zorlama dahil olmak üzere önbelleğe alma işleminde etken olan parametreler ve API 'Ler hakkında bilgi edinmek isteyeceksiniz. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

+ [Beceri kavramları ve kompozisyonu](cognitive-search-working-with-skillsets.md)
+ [Beceri oluşturma](cognitive-search-defining-skillset.md)
+ [Artımlı zenginleştirme ve önbelleğe alma bilgilerine giriş](cognitive-search-incremental-indexing-conceptual.md)
