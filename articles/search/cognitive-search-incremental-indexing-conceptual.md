---
title: Artımlı zenginleştirme (Önizleme)
titleSuffix: Azure Cognitive Search
description: Mevcut işlenen belgelerdeki yatırımları korumak için Azure depolama 'daki AI zenginleştirme ardışık düzeninde bulunan ara içeriği ve artımlı değişiklikleri önbelleğe alma. Bu özellik şu anda genel önizleme aşamasındadır.
manager: nitinme
author: Vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 285b3608bc57d88ca2e81ed14355923436ed9d8d
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028519"
---
# <a name="introduction-to-incremental-enrichment-and-caching-in-azure-cognitive-search"></a>Azure Bilişsel Arama artımlı zenginleştirme ve önbelleğe alma konusuna giriş

> [!IMPORTANT] 
> Artımlı zenginleştirme Şu anda genel önizlemededir. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

Artımlı zenginleştirme, bir zenginleştirme işlem hattına önbelleğe alma ve statefullik ekler ve yalnızca belirli bir değişiklikten etkilenen belgeleri değiştirirken, mevcut çıkışdaki yatırımınızı korur. Bu, yalnızca parasal yatırımınızın işleme (özellikle OCR ve görüntü işleme) yapılmamasını sağlar, ancak aynı zamanda daha verimli bir sistem için de geçerlidir. Yapılar ve içerik önbelleğe alındığında, bir Dizin Oluşturucu hangi yeteneklerin değiştirildiğini ve yalnızca değiştirilmiş olanları ve herhangi bir aşağı akış bağımlı becerileri tespit edebilir. 

## <a name="indexer-cache"></a>Dizin Oluşturucu önbelleği

Artımlı zenginleştirme, enzenginleştirme ardışık düzenine bir önbellek ekler. Dizin Oluşturucu sonuçları belge çözme işleminden ve her belge için her bir yeteneğin çıktılarından önbelleğe alır. Bir beceri güncelleştirilirken, yalnızca değiştirilen veya aşağı akış becerileri yeniden çalıştırılır. Güncelleştirilmiş sonuçlar önbelleğe yazılır ve belge arama dizininde veya bilgi deposunda güncelleştirilir.

Fiziksel olarak, önbellek, Azure Depolama hesabınızdaki bir blob kapsayıcısında depolanır. Bir arama hizmeti içindeki tüm dizinler, Dizin Oluşturucu önbelleğinin aynı depolama hesabını paylaşabilir. Her dizin oluşturucuya, kullandığı kapsayıcıya benzersiz ve sabit bir önbellek tanımlayıcısı atanır.

## <a name="cache-configuration"></a>Önbellek yapılandırması

Faydalanmasını, Dizin Oluşturucu üzerinde `cache` özelliğini artımlı zenginleştirme 'den başlatmak için ayarlamanız gerekir. Aşağıdaki örnek, önbelleğe alma etkin olan bir dizin oluşturucuyu gösterir. Bu yapılandırmanın belirli kısımları aşağıdaki bölümlerde açıklanmıştır. Daha fazla bilgi için bkz. [artımlı zenginleştirme ayarlama](search-howto-incremental-index.md).

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

Mevcut bir dizin oluşturucuda bu özelliğin ayarlanması, Dizin oluşturucuyu sıfırlamanıza ve yeniden çalıştırmanız gerekir, bu da veri kaynağınızdaki tüm belgelerin yeniden işlenmesini sağlar. Bu adım, önceki beceri sürümleri tarafından zenginleştirilmiş belgelerin ortadan kaldırılması için gereklidir. 

## <a name="cache-management"></a>Önbellek yönetimi

Önbelleğin yaşam döngüsü Dizin Oluşturucu tarafından yönetilir. Dizin oluşturucudaki `cache` özelliği null olarak ayarlandıysa veya bağlantı dizesi değiştirilirse, varolan önbellek bir sonraki Dizin Oluşturucu çalıştırmasında silinir. Önbellek yaşam döngüsü Ayrıca Dizin Oluşturucu yaşam döngüsüne de bağlıdır. Bir Dizin Oluşturucu silinirse, ilişkili önbellek de silinir.

Artımlı zenginleştirme, sizin bölümleriniz üzerinde müdahale olmadan değişiklikleri tespit etmek ve bunlara yanıt vermek üzere tasarlanırken, varsayılan davranışları geçersiz kılmak için kullanabileceğiniz parametreler vardır:

+ Yeni belgelerin önceliğini belirleme
+ Beceri denetimlerini atla
+ Veri kaynağı denetimlerini atla
+ Beceri değerlendirmesini zorla

### <a name="prioritize-new-documents"></a>Yeni belgelerin önceliğini belirleme

`enableReprocessing` özelliğini, önbellekte zaten temsil edilen gelen belgelerde işlemeyi denetlemek için ayarlayın. `true` (varsayılan) olduğunda, Dizin oluşturucuyu yeniden çalıştırdığınızda zaten önbellekte bulunan belgeler yeniden işlenir, böylece beceri güncelleştirmeniz bu belgeyi etkiler. 

`false`, mevcut belgeler yeniden işlenmediği zaman, yeni, gelen içeriğin var olan içerik üzerinde etkin bir şekilde önceliklerini. `enableReprocessing` yalnızca geçici olarak `false` ayarlamanız gerekir. Corpus genelinde tutarlılık sağlamak için, tüm belgelerin (hem yeni hem de mevcut) geçerli beceri tanımına göre geçerli olduğundan emin olmak için en fazla `true` `enableReprocessing` gerekir.

### <a name="bypass-skillset-evaluation"></a>Beceri değerlendirmesini atla

Bu beceri 'in bir beceri ve yeniden işlemesini değiştirmek genellikle el ile yapılır. Ancak, bir beceri üzerinde yapılan bazı değişiklikler yeniden işlemeye neden olmaz (örneğin, bir özel yeteneği yeni bir konuma veya yeni bir erişim anahtarıyla dağıtma). Büyük olasılıkla, Beceri 'in bir üyesi üzerinde orijinal etkisi olmayan çevresel değişiklikler vardır. 

Beceri bir değişikliğin gerçekten yararlanmayan olduğunu biliyorsanız, `disableCacheReprocessingChangeDetection` parametresini `true`olarak ayarlayarak beceri değerlendirmesini geçersiz kılmanız gerekir:

1. Update beceri 'i çağırın ve beceri tanımını değiştirin.
1. İsteğe `disableCacheReprocessingChangeDetection=true` parametresini ekleyin.
1. Değişikliği gönder.

Bu parametre ayarlandığında, yalnızca beceri tanımı güncelleştirmelerinin yürütülmesi ve değişikliğin mevcut Corpus üzerindeki etkiler için değerlendirilmemesi sağlanır.

Aşağıdaki örnek, parametresiyle bir Update beceri isteği gösterir:

```http
PUT https://customerdemos.search.windows.net/skillsets/callcenter-text-skillset?api-version=2019-05-06-Preview&disableCacheReprocessingChangeDetection=true
```

### <a name="bypass-data-source-validation-checks"></a>Veri kaynağı doğrulama denetimlerini atla

Bir veri kaynağı tanımında yapılan çoğu değişiklik önbelleği geçersiz kılar. Bununla birlikte, bir değişikliğin bir bağlantı dizesini değiştirme veya depolama hesabındaki anahtarı döndürme gibi bir değişikliğin önbelleği geçersiz kılamadı olduğunu bildiğiniz senaryolar için veri kaynağı güncelleştirmesinde`ignoreResetRequirement` parametresini ekleyin. Bu parametrenin `true` olarak ayarlanması, bir sıfırlama koşulunu tetiklemeden, tüm nesnelerin yeniden oluşturulmasına ve sıfırdan doldurulmasına neden olacak şekilde, yürütmeye izin verir.

```http
PUT https://customerdemos.search.windows.net/datasources/callcenter-ds?api-version=2019-05-06-Preview&ignoreResetRequirement=true
```

### <a name="force-skillset-evaluation"></a>Beceri değerlendirmesini zorla

Önbelleğin amacı gereksiz işlemden kaçınmaktır, ancak dizin oluşturucunun algılamadığı bir yeteneğe değişiklik yapmayı (örneğin, özel bir beceri gibi harici koddaki bir şeyi değiştirme) varsayalım.

Bu durumda, bu yeteneğin çıktısına bağımlılığı olan tüm aşağı akış becerileri de dahil olmak üzere belirli bir yeteneğin yeniden işlenmesini zorlamak için [sıfırlama becerileri](preview-api-resetskills.md) kullanabilirsiniz. Bu API, yeniden işlenmek üzere geçersiz kılınmaları ve işaretlenmesi gereken yetenekler listesini içeren bir POST isteğini kabul eder. Yeteneklerini sıfırladıktan sonra, işlem hattını çağırmak için Dizin oluşturucuyu çalıştırın.

## <a name="change-detection"></a>Değişiklik algılama

Bir önbelleği etkinleştirdikten sonra, Dizin Oluşturucu, hangi içeriğin yeniden kullanılabilir olduğunu ve ne tekrar işlenmesi gerektiğini belirleyen işlem hattı kompozisyoninizdeki değişiklikleri değerlendirir. Bu bölümde, önbellek ve ardından artımlı işleme tetikleyen değişiklikler tarafından geçersiz kılan değişiklikler numaralandırılır. 

### <a name="changes-that-invalidate-the-cache"></a>Önbelleği geçersiz kılan değişiklikler

Geçersiz kılma değişikliği, önbelleğin tamamının artık geçerli olmadığı bir yerdir. Geçersiz kılma değişikliği örneği, veri kaynağınızın güncelleştirildiği bir örnektir. Önbelleğinizi geçersiz kılacak değişikliklerin tamamı aşağıda verilmiştir:

* Veri kaynağı türüne değiştirme
* Veri kaynağı kapsayıcısına geçiş
* Veri kaynağı kimlik bilgileri
* Veri kaynağı değişiklik algılama ilkesi
* Veri kaynakları algılama ilkesini silme
* Dizin Oluşturucu alan eşlemeleri
* Dizin Oluşturucu parametreleri
    * Ayrıştırma modu
    * Dışlanan dosya adı uzantıları
    * Dizinli dosya adı uzantıları
    * Yalnızca büyük belgeler için dizin depolama meta verileri
    * Sınırlandırılmış metin üstbilgileri
    * Sınırlandırılmış metin sınırlayıcısı
    * Belge kökü
    * Görüntü eylemi (görüntülerin ayıklandığı değişiklikler)

### <a name="changes-that-trigger-incremental-processing"></a>Artımlı işlemeyi tetikleyen değişiklikler

Artımlı işleme, Beceri tanımınızı değerlendirir ve hangi yeteneklerin yeniden çalıştırılacağını ve belge ağacının etkilenen bölümlerinin seçmeli olarak güncelleştirilmesini belirler. Artımlı zenginleştirme ile sonuçlanan tüm değişikliklerin listesi aşağıda verilmiştir:

* Beceri ' deki beceri farklı türde. Yeteneğin OData türü güncelleştirildi
* Beceriye özgü parametreler, örneğin URL, varsayılanlar veya diğer parametreler ile güncelleştirildi
* Beceri çıkışları değişiklikler, Beceri ek veya farklı çıktılar döndürüyor
* Yetenek güncelleştirmeleri sonucunda farklı bir aile, Beceri zinciri değişti yani Beceri girişleri
* Bu beceriye bir giriş sağlayan bir yetenek güncelleştirilirse herhangi bir yukarı akış yetenek doğrulaması
* Bilgi deposu projeksiyon konumundaki güncelleştirmeler, belgelerin yeniden yansıtımasına neden olur
* Bilgi deposu projeksiyonları üzerinde yapılan değişiklikler, belgelerin yeniden yansıtımasına neden olur
* Dizin Oluşturucu üzerinde değiştirilen çıkış alanı eşlemeleri, belgelerin dizine yeniden yansıtımasına neden olur

## <a name="api-reference-content-for-incremental-enrichment"></a>Artımlı zenginleştirme için API başvuru içeriği

REST `api-version=2019-05-06-Preview`, Dizin oluşturucular, becerileri ve veri kaynaklarına eklemeleri sayesinde artımlı zenginleştirme için API 'Ler sağlar. [Resmi başvuru belgeleri](https://docs.microsoft.com/rest/api/searchservice/) genel kullanıma açık API 'ler içindir ve Önizleme özelliklerini kapsamaz. Aşağıdaki bölümde, etkilenen API 'Ler için başvuru içeriği sağlanmaktadır.

Kullanım bilgileri ve örnekleri, artımlı bir zenginleştirme [için önbelleğe alma yapılandırma](search-howto-incremental-index.md)bölümünde bulunabilir.

### <a name="indexers"></a>Dizinleyiciler

[Create Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer) ve [Update Indexer](https://docs.microsoft.com/rest/api/searchservice/update-indexer) artık önbelleğiyle ilgili yeni özellikleri kullanıma sunacaktır:

+ `StorageAccountConnectionString`: ara sonuçları önbelleğe almak için kullanılacak depolama hesabına bağlantı dizesi.

+ `EnableReprocessing`: `true` olarak ayarla, `false`olarak ayarlandığında belgeler önbelleğe yazılmaya devam eder, ancak mevcut belgeler önbellek verilerine göre yeniden işlenir.

+ `ID` (salt okunurdur): `ID`, bu Dizin Oluşturucu için önbellek olarak kullanılacak `annotationCache` depolama hesabı içindeki kapsayıcının tanımlayıcısıdır. Bu önbellek, bu Dizin Oluşturucu için benzersiz olacak ve Dizin Oluşturucu silinip aynı adla yeniden oluşturulursa, `ID` yeniden oluşturulacaktır. `ID` ayarlanamaz, hizmet tarafından her zaman oluşturulur.

### <a name="skillsets"></a>Beceri kümeleri

+ [Güncelleştirme beceri](https://docs.microsoft.com/rest/api/searchservice/update-skillset) , istek üzerinde yeni bir parametreyi destekler: `disableCacheReprocessingChangeDetection`, geçerli eyleme göre mevcut belgelerde güncelleştirme yapmak istediğinizde `true` olarak ayarlanmalıdır.

+ [Yetenekleri sıfırlama](preview-api-resetskills.md) , bir beceri geçersiz kılmak için kullanılan yeni bir işlemdir.

### <a name="datasources"></a>Veri kaynakları

+ Bazı Dizin oluşturucular verileri sorgular aracılığıyla alır. Veri alan sorgular için [güncelleştirme veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/update-data-source) , güncelleştirme eyleminiz önbelleği geçersiz kılamadığında `true` olarak ayarlanması gereken `ignoreResetRequirement`bir istek üzerinde yeni bir parametreyi destekler.

Verilerinize kolayca algılanmayacak, istenmeyen tutarsızlığa yol açacağından, `ignoreResetRequirement` gelişigüzel bir şekilde kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Artımlı zenginleştirme, değişiklik izlemeyi becerileri ve AI zenginleştirme olarak genişleten güçlü bir özelliktir. Becerileri geliştikçe, artımlı zenginleştirme, belgelerinizi yine de nihai tutarlılığa yönlendirirken olası en az iş yapılmasını sağlar.

Var olan bir dizin oluşturucuya önbellek ekleyerek veya yeni bir Dizin Oluşturucu tanımlarken önbelleği ekleyerek başlayın.

> [!div class="nextstepaction"]
> [Artımlı zenginleştirme için önbelleğe almayı yapılandırın](search-howto-incremental-index.md)
