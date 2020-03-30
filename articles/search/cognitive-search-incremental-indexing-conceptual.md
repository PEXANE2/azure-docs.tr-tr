---
title: Artımlı zenginleştirme (önizleme)
titleSuffix: Azure Cognitive Search
description: Mevcut işlenmiş belgelerdeki yatırımları korumak için Azure Depolama'daki AI zenginleştirme ardışık alanından ara içeriği ve artımlı değişiklikleri önbelleğe al. Bu özellik şu anda genel önizlemede dir.
manager: nitinme
author: Vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 09003c26ead9108d07ae339fcf64235c246474a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024152"
---
# <a name="introduction-to-incremental-enrichment-and-caching-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da artımlı zenginleştirme ve önbelleğe alma ile giriş

> [!IMPORTANT] 
> Artımlı zenginleştirme şu anda genel önizlemede. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

Artımlı zenginleştirme, bir zenginleştirme boru hattına önbelleğe alma ve devlet niteliği ekleyerek mevcut çıktıya yatırımınızı korurken, yalnızca belirli değişikliklerden etkilenen belgeleri değiştirir. Bu sadece işleme (özellikle, OCR ve görüntü işleme) parasal yatırım korumak ama aynı zamanda daha verimli bir sistem için yapar. Yapılar ve içerik önbelleğe alındığında, dizinleyici hangi becerilerin değiştiğini belirleyebilir ve yalnızca değiştirilen becerilerin yanı sıra alt akışa bağlı becerileri de çalıştırabilir. 

## <a name="indexer-cache"></a>Dizinleyici önbellek

Artımlı zenginleştirme, zenginleştirme ardışık boru hattına bir önbellek ekler. Dizinleyici, belge kırılmasının sonuçlarını ve her belge için her becerinin çıktılarını önbelleğe alır. Bir skillset güncelleştirildiğinde, yalnızca değiştirilen veya alt akışta olan beceriler yeniden çalıştırılır. Güncelleştirilmiş sonuçlar önbelleğe yazılır ve belge arama dizininde veya bilgi deposunda güncelleştirilir.

Önbellek fiziksel olarak Azure Depolama hesabınızdaki bir blob kapsayıcısında depolanır. Önbellek, güncelleştirmeleri işlemenin dahili kaydı için tablo depolamasını da kullanır. Bir arama hizmetindeki tüm dizinler, dizinleyici önbelleği için aynı depolama hesabını paylaşabilir. Her dizinleyiciye, kullandığı kapsayıcıya benzersiz ve değişmez bir önbellek tanımlayıcısı atanır.

## <a name="cache-configuration"></a>Önbellek yapılandırması

Artımlı zenginleştirmeden `cache` yararlanmaya başlamak için özelliği dizinleyicide ayarlamanız gerekir. Aşağıdaki örnekte önbelleğe alma etkin bir dizinleyici gösterin. Bu yapılandırmanın belirli bölümleri aşağıdaki bölümlerde açıklanmıştır. Daha fazla bilgi için [bkz.](search-howto-incremental-index.md)

```json
{
    "name": "myIndexerName",
    "targetIndexName": "myIndex",
    "dataSourceName": "myDatasource",
    "skillsetName": "mySkillset",
    "cache" : {
        "storageConnectionString" : "Your storage account connection string",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
}
```

Bu özelliği varolan bir dizinleyiciüzerinde ayarlamak, veri kaynağınızdaki tüm belgelerin yeniden işlenmesine neden olacak dizin leyiciyi sıfırlamanızı ve yeniden çalıştırmanızı gerektirir. Bu adım skillset önceki sürümleri ile zenginleştirilmiş herhangi bir belge ortadan kaldırmak için gereklidir. 

## <a name="cache-management"></a>Önbellek yönetimi

Önbelleğin yaşam döngüsü dizinleyici tarafından yönetilir. Dizinleyicideki `cache` özellik null olarak ayarlanırsa veya bağlantı dizesi değiştirilirse, varolan önbellek bir sonraki dizinleyici çalışmasında silinir. Önbellek yaşam döngüsü de dizinleyici yaşam döngüsüne bağlıdır. Bir dizinleyici silinirse, ilişkili önbellek de silinir.

Artımlı zenginleştirme, değişiklikleri algılamak ve yanıtlamak için tasarlanmış olsa da, varsayılan davranışları geçersiz kılmak için kullanabileceğiniz parametreler vardır:

+ Yeni belgelere öncelik verin
+ Bypass skillset kontrolleri
+ Veri kaynağı denetimlerini atlama
+ Kuvvet becerileri değerlendirme

### <a name="prioritize-new-documents"></a>Yeni belgelere öncelik verin

`enableReprocessing` Önbellekte zaten temsil edilen gelen belgeler üzerinde işleme denetlemek için özelliği ayarlayın. (Varsayılan) olduğunda, `true` beceri güncelleştirmenizin bu dokümanı etkilediğini varsayarak dizinleyiciyi yeniden çalıştırdığınızda önbellekte bulunan belgeler yeniden işlenir. 

Varolan belgeler yeniden işlenmediğinde, `false`yeni ve gelen içeriğe varolan içeriğe göre etkili bir şekilde öncelik verilir. Yalnızca geçici `enableReprocessing` olarak `false` ayarlamalısınız. Corpus genelinde tutarlılık sağlamak `enableReprocessing` için, `true` tüm belgelerin, hem yeni hem de mevcut, geçerli skillset tanımına göre geçerli olmasını sağlamak, çoğu zaman olmalıdır.

### <a name="bypass-skillset-evaluation"></a>Bypass skillset değerlendirme

Bir skillset değiştirme ve bu skillset yeniden işleme genellikle el ele gidin. Ancak, bir skillset'te yapılan bazı değişiklikler yeniden işlemeyle sonuçlanmamalıdır (örneğin, özel bir beceriyi yeni bir konuma veya yeni bir erişim anahtarıyla dağıtma). Büyük olasılıkla, bu skillset kendisi madde üzerinde gerçek bir etkisi yoktur periferik değişiklikler vardır. 

Eğer skillset bir değişiklik gerçekten yüzeysel olduğunu biliyorsanız, `disableCacheReprocessingChangeDetection` parametre ayarlayarak skillset `true`değerlendirme geçersiz kılmak gerekir:

1. Update Skillset'i arayın ve skillset tanımını değiştirin.
1. İstek üzerine `disableCacheReprocessingChangeDetection=true` parametreyi ekle.
1. Değişikliği gönderin.

Bu parametrenin ayarlanması, yalnızca skillset tanımındaki güncelleştirmelerin işlenmesini ve değişikliğin varolan korpus üzerindeki etkileri açısından değerlendirilmemesini sağlar.

Aşağıdaki örnek, parametreile bir Güncelleştirme Skillset isteğini gösterir:

```http
PUT https://customerdemos.search.windows.net/skillsets/callcenter-text-skillset?api-version=2019-05-06-Preview&disableCacheReprocessingChangeDetection=true
```

### <a name="bypass-data-source-validation-checks"></a>Veri kaynağı doğrulama denetimlerini atlama

Veri kaynağı tanımındaki değişikliklerin çoğu önbelleği geçersiz kılınır. Ancak, bir değişikliğin önbelleği geçersiz kılmaması gerektiğini bildiğiniz senaryolarda (bağlantı dizesini değiştirme veya anahtarı depolama`ignoreResetRequirement` hesabında döndürme gibi) veri kaynağı güncelleştirmesindeki parametreyi ekleyin. Bu parametreyi, `true` tüm nesnelerin yeniden inşa edilmesine ve sıfırdan doldurulmasına neden olacak bir sıfırlama koşulu tetiklemeden, commit through geçmesine izin verir.

```http
PUT https://customerdemos.search.windows.net/datasources/callcenter-ds?api-version=2019-05-06-Preview&ignoreResetRequirement=true
```

### <a name="force-skillset-evaluation"></a>Kuvvet becerileri değerlendirme

Önbelleğin amacı gereksiz işlemlerden kaçınmaktır, ancak dizinleyicinin algılmadığı bir beceride değişiklik yaptığınızı varsayalım (örneğin, özel bir beceri gibi harici koddaki bir şeyi değiştirmek).

Bu durumda, sıfırlama [becerilerini,](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/reset-skills) söz konusu becerinin çıktısına bağımlı olan alt akış becerileri de dahil olmak üzere belirli bir becerinin yeniden işlenmesini zorlamak için kullanabilirsiniz. Bu API, geçersiz kılınması ve yeniden işleme için işaretlenmesi gereken becerilerin listesini içeren bir POST isteğini kabul eder. Becerileri Sıfırladıktan sonra, ardışık lığı çağırmak için dizinleyiciyi çalıştırın.

## <a name="change-detection"></a>Algılamayı değiştir

Önbelleği etkinleştirdikten sonra, dizinleyici, hangi içeriğin yeniden kullanılabileceğini ve hangilerinin yeniden işlenmesi gerektiğini belirlemek için dizi nida kompozisyonunuzdaki değişiklikleri değerlendirir. Bu bölümde, önbelleği geçersiz kılan değişiklikleri ve ardından artımlı işlemeyi tetikleyen değişiklikleri yazar. 

### <a name="changes-that-invalidate-the-cache"></a>Önbelleği geçersiz kılınan değişiklikler

Geçersiz bir değişiklik, önbelleğin tamamının artık geçerli olmadığı bir değişikliktir. Geçersiz bir değişikliğin bir örneği, veri kaynağınızın güncelleştirildiği bir değişikliktir. Önbelleğinizi geçersiz kacak değişikliklerin tam listesi aşağıda verilmiştir:

* Veri kaynağı türünüzde değişiklik
* Veri kaynağı kapsayıcısına geçiş
* Veri kaynağı kimlik bilgileri
* Veri kaynağı değişikliği algılama ilkesi
* Veri kaynakları algılama ilkesini siler
* Dizinleyici alan eşlemeleri
* Dizinleyici parametreleri
    * Ayrışma Modu
    * Hariç Dosya Adı Uzantıları
    * Dizinlenmiş Dosya Adı Uzantıları
    * Yalnızca büyük boyutlu belgeler için dizin depolama meta verileri
    * Sınırlı metin üstbilgi
    * Sınırlı metin delimiter
    * Belge Kökü
    * Görüntü Eylemi (Görüntülerin nasıl ayıklandığını değiştirir)

### <a name="changes-that-trigger-incremental-processing"></a>Artımlı işlemeyi tetikleyen değişiklikler

Artımlı işleme, beceri tanımınızı değerlendirir ve belge ağacının etkilenen bölümlerini seçen güncelleyerek hangi becerileri yeniden çalıştırılacaklarını belirler. Artımlı zenginleştirme yle sonuçlanan değişikliklerin tam listesi aşağıda verilmiştir:

* Beceri becerisi farklı türü vardır. Becerinin overi türü güncellenir
* Url, varsayılanlar veya diğer parametreler gibi becerilere özel parametreler güncelleştirildi
* Beceri çıktıları değişir, beceri ek veya farklı çıktıları döndürür
* Beceri güncellemeleri ortaya çıkan farklı ataları, beceri zincirleme yani değişti beceri girdileri
* Bu beceriye giriş sağlayan bir beceri güncellenirse, herhangi bir güncel beceri geçersizliği
* Bilgi deposu projeksiyon konumunda yapılan güncellemeler, belgelerin yeniden projeleilmesiyle sonuçlanır
* Bilgi deposu projeksiyonlarında yapılan değişiklikler, belgelerin yeniden projeleilmesiyle sonuçlanır
* Dizinleyici sonuçlarında değiştirilen çıktı alanı eşlemeleri, belgeleri dizin için yeniden yansıtma

## <a name="api-reference"></a>API başvurusu

REST API `2019-05-06-Preview` sürümü, dizin, beceri kümeleri ve veri kaynaklarındaki ek özellikler aracılığıyla artımlı zenginleştirme sağlar. Başvuru belgelerine ek olarak, API'lerin nasıl çağrılması yla ilgili ayrıntılar [için artımlı zenginleştirme için Yapılandırış'a](search-howto-incremental-index.md) bakın.

+ [Indexer Oluştur (api-version=2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/create-indexer) 

+ [Indexer'ı Güncelle (api-version=2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/update-indexer) 

+ [Skillset'i Güncelleştir (api-version=2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/update-skillset) (İstek üzerine yeni URI parametresi)

+ [Becerileri Sıfırlama (api-version=2019-05-06-Preview)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/reset-skills)

+ Veritabanı dizinleyicileri (Azure SQL, Cosmos DB). Bazı dizinleyiciler sorgular aracılığıyla veri alır. Veri alan sorgular için, [Veri Kaynağını Güncelleştir,](https://docs.microsoft.com/rest/api/searchservice/update-data-source) güncelleştirme eyleminizin önbelleği geçersiz `true` kılmaması gereken bir istek **yoksunması**üzerinde yeni bir parametreyi destekler. 

  Verilerinizde kolayca algılanmayacak istenmeyen tutarsızlıklara yol açabileceğiiçin **göz ardıEtmeGereksinimini** dikkatli kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Artımlı zenginleştirme, değişim izlemeyi beceri lere ve AI zenginleştirmeye kadar genişleten güçlü bir özelliktir. Aşamalı zenginleştirme, skillset tasarımı üzerinde yinelenirken varolan işlenmiş içeriğin yeniden kullanılmasını sağlar.

Bir sonraki adım olarak, varolan bir dizinleyici üzerinde önbelleğe etkinleştirin veya yeni bir dizinleyici tanımlarken bir önbellek ekleyin.

> [!div class="nextstepaction"]
> [Artımlı zenginleştirme için önbelleğe alma yapılandırılması](search-howto-incremental-index.md)
