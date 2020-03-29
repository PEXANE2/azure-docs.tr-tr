---
title: Dizin oluşturucu hataları ve uyarıları
titleSuffix: Azure Cognitive Search
description: Bu makalede, Azure Bilişsel Arama'da AI zenginleştirme sırasında karşılaşabileceğiniz sık karşılaşılan hatalara ve uyarılara bilgi ve çözümler sağlanmaktadır.
manager: nitinme
author: amotley
ms.author: abmotley
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 72bf08dce36d857c1fe91bbe9806336dfa185f7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671986"
---
# <a name="troubleshooting-common-indexer-errors-and-warnings-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sık karşılaşılan dizin oluşturma hatası ve uyarıları giderme

Bu makalede, Azure Bilişsel Arama'da dizin oluşturma ve AI zenginleştirme sırasında karşılaşabileceğiniz sık karşılaşılan hatalara ve uyarılara bilgi ve çözümler sağlanmaktadır.

Hata sayısı ['maxFailedItems'](cognitive-search-concept-troubleshooting.md#tip-3-see-what-works-even-if-there-are-some-failures)aşıldığında dizin oluşturma durur. 

Dizin leyicilerin bu hataları yoksaymasını (ve "başarısız olan `maxFailedItems` belgeleri" atlamasını) istiyorsanız, `maxFailedItemsPerBatch` [burada](https://docs.microsoft.com/rest/api/searchservice/create-indexer#general-parameters-for-all-indexers)açıklandığı gibi güncelleştirmeyi düşünün.

> [!NOTE]
> Her başarısız belge, belge anahtarıyla birlikte (varsa) dizin leyici yürütme durumunda bir hata olarak gösterecektir. Dizinleyiciyi hataları tolere etmek için ayarladıysanız, belgeleri daha sonraki bir noktada el ile yüklemek için [dizin api'sini](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) kullanabilirsiniz.

Bu makaledeki hata bilgileri, dizin oluşturmanın devam etmesine izin vererek hataları çözmenize yardımcı olabilir.

Uyarılar dizin oluşturmayı durdurmaz, ancak beklenmeyen sonuçlara neden olabilecek koşulları gösterir. Harekete geçip harekete geçmemeniz verilere ve senaryonuza bağlıdır.

API sürümüyle `2019-05-06`başlayarak, öğe düzeyinde Dizinleyici hataları ve uyarıları, nedenler ve sonraki adımlar etrafında daha fazla netlik sağlayacak şekilde yapılandırılmıştır. Aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Örnek |
| --- | --- | --- |
| anahtar | Hata veya uyarıdan etkilenen belgenin belge kimliği. | https:\//coromsearch.blob.core.windows.net/jfk-1k/docid-32112954.pdf |
| ad | Hatanın veya uyarının oluştuğu yeri açıklayan işlem adı. Bu aşağıdaki yapı tarafından oluşturulur: [kategori]. [alt kategori]. [kaynakTürü]. [resourceName] | DocumentExtraction.azureblob.myBlobContainerName Zenginleştirme.WebApiSkill.mySkillName Projection.SearchIndex.OutputFieldMapping.myOutputFieldName Projection.SearchIndex.MergeOrUpload.myIndexName Projeksiyon.KnowledgeStore.Table.myTableName |
| message | Hata veya uyarının üst düzey açıklaması. | Web Api isteği başarısız olduğu için beceri yürütülemedi. |
| Şey | Özel bir beceri yürütme başarısız olursa WebApi yanıtı gibi sorunu tanılamak için yararlı olabilecek ek ayrıntılar. | `link-cryptonyms-list - Error processing the request record : System.ArgumentNullException: Value cannot be null. Parameter name: source at System.Linq.Enumerable.All[TSource](IEnumerable`1 kaynak,`2 predicate) at Microsoft.CognitiveSearch.WebApiSkills.JfkWebApiSkills.` Func ... yığın iz geri kalanı ... |
| documentationLink | Hata ayıklama ve sorunu çözmek için ayrıntılı bilgileri içeren ilgili belgelere bağlantı. Bu bağlantı genellikle bu sayfadaki aşağıdaki bölümlerden birine işaret edecektir. | https://go.microsoft.com/fwlink/?linkid=2106475 |

<a name="could-not-read-document"/>

## <a name="error-could-not-read-document"></a>Hata: Belgeyi okuyamadı

Dizinleyici belgeyi veri kaynağından okuyamadı. Bu nedeniyle olabilir:

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Farklı belgeler arasında tutarsız alan türleri | "Değer türü sütun türü ile bir uyumsuzluk vardır. Yazarlar sütununda `'{47.6,-122.1}'` depolayamadım.  Beklenen türü JArray olduğunu."  "Hata float veri türü nvarchar dönüştürme."  "Nvarchar değerini '12 ay' veri türü int'e dönüştürürken dönüştürme başarısız oldu."  "Aritmetik taşma hatası ifadeyi veri türü int'e dönüştüren." | Her alanın türünün farklı belgelerarasında aynı olduğundan emin olun. Örneğin, ilk belge `'startTime'` alanı DateTime ise ve ikinci belgede bir dize ise, bu hata vurulur. |
| veri kaynağının temel hizmetinden gelen hatalar | (Cosmos DB'den)`{"Errors":["Request rate is large"]}` | Sağlıklı olduğundan emin olmak için depolama örneğini kontrol edin. Ölçekleme/bölümleme nizi ayarlamanız gerekebilir. |
| geçici sorunlar | Sunucudan sonuç alırken aktarım düzeyinde bir hata oluştu. (sağlayıcı: TCP Sağlayıcı, hata: 0 - Varolan bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı | Bazen beklenmeyen bağlantı sorunları vardır. Belgeyi daha sonra dizin leyiciniz arasında çalıştırmayı deneyin. |

<a name="could-not-extract-document-content"/>

## <a name="error-could-not-extract-content-or-metadata-from-your-document"></a>Hata: Belgenizden içerik veya meta veri ayıklayamadı
Blob veri kaynağıolan dizin leyici, belgeden (örneğin, bir PDF dosyası) içeriği veya meta verileri ayıklayamadı. Bu nedeniyle olabilir:

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| blob boyut sınırının üzerinde | `'150441598'` Belge, geçerli hizmet katmanınız için `'134217728'` belge ayıklama için maksimum boyutu aşan baytlardır. | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob desteklenmeyen içerik türü ne var | Belgenin desteklenmeyen içerik türü vardır`'image/png'` | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob şifrelenir | Belge işlenemedi - şifrelenebilir veya parola korunabilir. | Blob ayarları ile [blob](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed)atlayabilirsiniz. |
| geçici sorunlar | "Hata işleme blob: istek iptal edildi: İstek iptal edildi." "Belge işleme sırasında zamanlanmış." | Bazen beklenmeyen bağlantı sorunları vardır. Belgeyi daha sonra dizin leyiciniz arasında çalıştırmayı deneyin. |

<a name="could-not-parse-document"/>

## <a name="error-could-not-parse-document"></a>Hata: Belgeyi ayrışdıramadı
Dizinleyici belgeyi veri kaynağından okudu, ancak belge içeriğini belirtilen alan eşleme şemasına dönüştüren bir sorun vardı. Bu nedeniyle olabilir:

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Belge anahtarı eksik | Belge anahtarı eksik veya boş olamaz | Tüm belgelerin geçerli belge anahtarlarına sahip olduğundan emin olun |
| Belge anahtarı geçersiz | Belge anahtarı 1024 karakterden uzun olamaz | Doğrulama gereksinimlerini karşılamak için belge anahtarını değiştirin. |
| Alan eşlemesi bir alana uygulanamadı | Alana `'fieldName'`eşleme `'functionName'` işlevi uygulanamadı. Dizi null olamaz. Parametre adı: bayt | Dizinleyicide tanımlanan [alan eşlemelerini](search-indexer-field-mappings.md) çift olarak denetleyin ve başarısız belgenin belirtilen alanının verileriyle karşılaştırın. Alan eşlemelerini veya belge verilerini değiştirmek gerekebilir. |
| Alan değerini okuyamadı | Dizindeki `'fieldName'` `'fieldIndex'`sütunun değerini okuyamadı. Sunucudan sonuç alırken aktarım düzeyinde bir hata oluştu. (sağlayıcı: TCP Sağlayıcı, hata: 0 - Varolan bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı.) | Bu hatalar genellikle veri kaynağının temel hizmetiyle ilgili beklenmeyen bağlantı sorunlarından kaynaklanmaktadır. Belgeyi daha sonra dizin leyiciniz arasında çalıştırmayı deneyin. |

<a name="could-not-execute-skill"/>

## <a name="error-could-not-execute-skill"></a>Hata: Beceri yürütülemedi
Indexer skillset bir beceri çalıştırmak mümkün değildi.

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Geçici bağlantı sorunları | Geçici bir hata oluştu. Lütfen daha sonra tekrar deneyin. | Bazen beklenmeyen bağlantı sorunları vardır. Belgeyi daha sonra dizin leyiciniz arasında çalıştırmayı deneyin. |
| Olası ürün hatası | Beklenmeyen bir hata oluştu. | Bu, bilinmeyen bir hata sınıfını gösterir ve bir ürün hatası olduğu anlamına gelebilir. Lütfen yardım almak için bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin. |
| Yürütme sırasında bir hatayla karşılaşan bir beceri | (Birleştirme Becerisinden) Bir veya daha fazla ofset değerleri geçersizve ayrıştırılamadı. Öğeler metnin sonuna eklenmiştir | Sorunu gidermek için hata iletisindeki bilgileri kullanın. Bu tür bir hatanın çözülmesi için eylem gerekir. |

<a name="could-not-execute-skill-because-the-web-api-request-failed"/>

## <a name="error-could-not-execute-skill-because-the-web-api-request-failed"></a>Hata: Web API isteği başarısız olduğu için beceri yürütülemedi
Web API'ye yapılan çağrı başarısız olduğu için beceri yürütme başarısız oldu. Genellikle, bu hata sınıfı özel beceriler kullanıldığında oluşur ve bu durumda sorunu gidermek için özel kodunuzu ayıklamanız gerekir. Bunun yerine hata yerleşik bir beceriden geliyorsa, sorunu gidermede yardım için hata iletisine bakın.

<a name="could-not-execute-skill-because-web-api-skill-response-is-invalid"/>

## <a name="error-could-not-execute-skill-because-web-api-skill-response-is-invalid"></a>Hata: Web API beceri yanıtı geçersiz olduğu için beceri yürütülemedi
Web API'ye yapılan çağrı geçersiz bir yanıt döndürdüğünden beceri uygulama süre Genellikle, bu hata sınıfı özel beceriler kullanıldığında oluşur ve bu durumda sorunu gidermek için özel kodunuzu ayıklamanız gerekir. Bunun yerine başarısızlık yerleşik bir beceriden geliyorsa, lütfen yardım almak için bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin.

<a name="skill-did-not-execute-within-the-time-limit"/>

## <a name="error-skill-did-not-execute-within-the-time-limit"></a>Hata: Beceri zaman sınırı içinde yürütülmedi
Bu hata iletisiyle karşılaşabileceğiniz iki durum vardır ve bunların her biri farklı şekilde ele alınmalıdır. Lütfen bu hatayı sizin için hangi beceriye göre döndürdüne bağlı olarak aşağıdaki talimatları uygulayın.

### <a name="built-in-cognitive-service-skills"></a>Yerleşik Bilişsel Hizmet becerileri
Dil algılama, varlık tanıma veya OCR gibi yerleşik bilişsel becerilerin çoğu Bilişsel Hizmet API bitiş noktası tarafından desteklenen. Bazen bu uç noktalarla ilgili geçici sorunlar vardır ve istek zaman ları dolacak. Geçici sorunlar için, beklemek ve yeniden denemek dışında hiçbir çare yoktur. Azaltma olarak, dizin leyicinizi [bir zamanlamada çalışacak](search-howto-schedule-indexers.md)şekilde ayarlamayı düşünün. Zamanlanmış dizin oluşturma kaldığı yerden alır. Geçici sorunların çözüldüğünü varsayarsak, dizin oluşturma ve bilişsel beceri işleme bir sonraki zamanlanan çalıştırmada devam edebilmelidir.

Yerleşik bir bilişsel beceri için aynı belgede bu hatayı görmeye devam ederseniz, bu beklendiği gibi, yardım almak için lütfen bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin.

### <a name="custom-skills"></a>Özel beceriler
Oluşturduğunuz özel bir beceriile bir zaman alabilen bir hatayla karşılaşırsanız, deneyebileceğiniz birkaç şey vardır. İlk olarak, özel beceri gözden geçirin ve sonsuz bir döngü içinde sıkışmış değil ve sürekli bir sonuç dönüyor emin olun. Durumun böyle olduğunu doğruladıktan sonra, becerinizin yürütme zamanının ne olduğunu belirleyin. Özel beceri tanımınızda açıkça `timeout` bir değer belirlemediyseniz, varsayılan `timeout` değer 30 saniyedir. 30 saniye, becerinizin yürütmesi için yeterince uzun `timeout` değilse, özel beceri tanımınızda daha yüksek bir değer belirtebilirsiniz. Zaman anına 90 saniye olarak ayarlandığı özel bir beceri tanımıörneği aşağıda verilmiştir:

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "uri": "<your custom skill uri>",
        "batchSize": 1,
        "timeout": "PT90S",
        "context": "/document",
        "inputs": [
          {
            "name": "input",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "output",
            "targetName": "output"
          }
        ]
      }
```

`timeout` Parametre için ayarlayabildiğiniz maksimum değer 230 saniyedir.  Özel beceriniz 230 saniye içinde tutarlı bir şekilde yürütülemiyorsa, tek bir yürütme içinde işlemek için daha az belgeye sahip olacak şekilde özel becerinizi azaltmayı `batchSize` düşünebilirsiniz.  Zaten 1 olarak `batchSize` ayarladıysanız, 230 saniyenin altında yürütmek için beceri yi yeniden yazmanız veya başka bir şekilde birden fazla özel beceriye bölmeniz gerekir, böylece herhangi bir özel beceriiçin yürütme süresi en fazla 230 saniye dir. Daha fazla bilgi için [özel beceri belgelerini](cognitive-search-custom-skill-web-api.md) gözden geçirin.

<a name="could-not-mergeorupload--delete-document-to-the-search-index"/>

## <a name="error-could-not-mergeorupload--delete-document-to-the-search-index"></a>Hata: Could`MergeOrUpload`not ' | '`Delete`' arama dizinine belge

Belge okundu ve işlendi, ancak dizinleyici arama dizinine ekleyemedi. Bu nedeniyle olabilir:

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Alan çok büyük bir terim içerir | Belgenizdeki bir terim [32 KB sınırından](search-limits-quotas-capacity.md#api-request-limits) daha büyüktür | Alanın filtrelenebilir, değiştirilebilir veya sıralanabilir olarak yapılandırılmaması için bu kısıtlamayı önleyebilirsiniz.
| Belge dizine alınamayacak kadar büyük | Belge, [maksimum api istek boyutundan](search-limits-quotas-capacity.md#api-request-limits) daha büyüktür | [Büyük veri kümelerini dizinoluşturma](search-howto-large-index.md)
| Belge, koleksiyonda çok fazla nesne içerir | Belgenizdeki bir [koleksiyon, tüm karmaşık koleksiyonlar daki maksimum öğeleri aşıyor"](search-limits-quotas-capacity.md#index-limits) "Anahtarlı `'1000052'` belgenin koleksiyonlarında nesneler (JSON dizileri) vardır. `'4303'` Çoğu `'3000'` nesnenin belgenin tamamında koleksiyonlarda yer almasına izin verilir. Lütfen nesneleri koleksiyonlardan kaldırın ve belgeyi yeniden dizine silmeyi deneyin." | Belgedeki karmaşık koleksiyonun boyutunu sınırın altına indirmenizi ve yüksek depolama alanı kullanımından kaçınmanızı öneririz.
| Hizmet sorgulama veya dizin oluşturma gibi başka bir yük altında olduğundan hedef dizine bağlanmada sorun (yeniden denemelerden sonra devam eder). | Güncelleştirme dizini bağlantısı oluşturulamadı. Arama hizmeti ağır yük altındadır. | [Arama hizmetinizi ölçeklendirin](search-capacity-planning.md)
| Arama hizmeti hizmet güncelleştirmesi için yamalı veya topoloji yeniden yapılandırmasının ortasında. | Güncelleştirme dizini bağlantısı oluşturulamadı. Arama hizmeti şu anda düştü/Arama hizmeti bir geçiş sürecinden geçiyor. | [SLA belgeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/) başına %99,9 kullanılabilirlik için hizmeti en az 3 yinelemeyle yapılandırın
| Temel bilgi işlem/ağ kaynağındaki hata (nadir) | Güncelleştirme dizini bağlantısı oluşturulamadı. Bilinmeyen bir hata oluştu. | Dizin işaretçilerini, başarısız bir durumdan almak üzere [bir zamanlamada çalışacak](search-howto-schedule-indexers.md) şekilde yapılandırın.
| Hedef dizine yapılan dizin oluşturma isteği, ağ sorunları nedeniyle bir zaman dilimi içinde onaylanmadı. | Arama dizinine zamanında bağlantı kuramamadı. | Dizin işaretçilerini, başarısız bir durumdan almak üzere [bir zamanlamada çalışacak](search-howto-schedule-indexers.md) şekilde yapılandırın. Ayrıca, bu hata koşulu devam ederse dizinleyici [toplu iş boyutunu](https://docs.microsoft.com/rest/api/searchservice/create-indexer#parameters) düşürmeyi deneyin.

<a name="could-not-index-document-because-the-indexer-data-to-index-was-invalid"/>

## <a name="error-could-not-index-document-because-some-of-the-documents-data-was-not-valid"></a>Hata: Belgenin bazı verileri geçerli olmadığından belgeyi dizine ekleyen

Belge dizinleyici tarafından okundu ve işlendi, ancak dizin alanlarının yapılandırmasındaki bir uyuşmazlık ve dizinleyici tarafından ayıklanan ve işlenen veriler nedeniyle arama dizinine eklenemedi. Bu nedeniyle olabilir:

| Neden | Ayrıntılar/Örnek
| --- | ---
| Dizinleyici tarafından çıkarılan alan(lar)ın veri türü, ilgili hedef dizin alanının veri modeliyle uyumsuzdur. | '888' anahtarına sahip belgedeki_veri_alanının geçersiz bir değeri vardır 'Edm.String' türü.' Beklenen tür 'Collection(Edm.String)' idi. |
| Herhangi bir JSON varlığını bir dize değerinden ayıklamak için başarısız oldu. | 'Edm.String' türü 'Disi'nin değerini ayrıştamak olamazdı ' alan '_veri_' bir JSON nesnesi olarak. Hata:'Bir değeri ayrıştattıktan sonra beklenmeyen bir karakterle karşılaşıldı: ''. Yol '_yolu_', satır 1, pozisyon 3162.' |
| JSON varlıklarının bir koleksiyonunu bir dize değerinden ayıklamak için başarısız oldu.  | Bir JSON dizisi olarak alan '_veri_' türü 'Edm.String' değeri ayrıştını olamazdı. Hata:'Bir değeri ayrıştattıktan sonra beklenmeyen bir karakterle karşılaşıldı: ''. Yol '[0]', satır 1, pozisyon 27.' |
| Kaynak belgede bilinmeyen bir tür bulundu. | Bilinmeyen türü '_bilinmiyor_' dizine eklenemez |
| Kaynak belgede coğrafya noktaları için uyumsuz bir gösterim kullanılmıştır. | WKT POINT dize literals desteklenmez. Lütfen geojson nokta literals yerine kullanın |

Tüm bu durumlarda, dizin şemasını doğru oluşturduğunuzdan ve uygun [dizin oluşturucu alan eşlemeleri](search-indexer-field-mappings.md)ayarladığınızdan emin olmak [için dizin oluşturucular için](https://docs.microsoft.com/rest/api/searchservice/data-type-map-for-indexers-in-azure-search) Desteklenen [Veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) ve Veri türü eşlemesi'ne bakın. Hata iletisi, uyuşmazlışın kaynağını izlemeye yardımcı olabilecek ayrıntıları içerir.

## <a name="error-integrated-change-tracking-policy-cannot-be-used-because-table-has-a-composite-primary-key"></a>Hata: Tablo bileşik birincil anahtar adedi olduğundan tümleşik değişim izleme ilkesi kullanılamıyor

Bu, SQL tabloları için geçerlidir ve genellikle anahtar bileşik anahtar olarak tanımlandığında veya tablo benzersiz bir kümelenmiş dizin tanımladığında (Azure Arama dizini değil, SQL dizininde olduğu gibi) gerçekleşir. Ana nedeni, anahtar özniteliği benzersiz bir [kümelenmiş dizin](https://docs.microsoft.com/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-ver15)durumunda bileşik birincil anahtar olarak değiştirilir olmasıdır. Bu durumda, SQL tablonuzun benzersiz bir kümelenmiş dizini olmadığından veya anahtar alanını yinelenen değerlere sahip olmadığı garanti edilen bir alanla eşleştirdiğinizden emin olun.

<a name="could-not-process-document-within-indexer-max-run-time"/>

## <a name="error-could-not-process-document-within-indexer-max-run-time"></a>Hata: Belgeyi dizinleyici max çalışma süresi içinde işlenemedi

Dizinleyici, izin verilen yürütme süresi içinde veri kaynağından tek bir belgeyi işlemeyi tamamlayamadığında bu hata oluşur. Beceriler kullanıldığında [maksimum çalışma süresi](search-limits-quotas-capacity.md#indexer-limits) daha kısadır. Bu hata oluştuğunda, maxFailedItems 0'dan başka bir değere ayarlanmışsa, dizin oluşturmanın ilerleyebilmeleri için dizinleyici gelecek çalıştırmalarda belgeyi atlar. Herhangi bir belgeyi atlamaya gücünüz yetmiyorsa veya bu hatayı sürekli olarak görüyorsanız, tek bir dizin leyici yürütme içinde kısmi ilerleme kaydedilebilmek için belgeleri daha küçük belgelere bölmeyi düşünün.

<a name="could-not-project-document"/>

## <a name="error-could-not-project-document"></a>Hata: Belgeyi yansıtamadı

Bu hata, dizinleyici [verileri bir bilgi deposuna yansıtmaya](knowledge-store-projection-overview.md) çalışırken oluşur ve bunu yapma girişimimizde bir hata oluştu.  Bu hata tutarlı ve düzeltilebilir olabilir veya düzeltmek için beklemeniz ve yeniden denemeniz gerekebilecek projeksiyon çıkışı lavabosuyla geçici bir hata olabilir.  Bilinen hata durumları ve olası çözümler kümesi aşağıda veda eder.

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Kapsayıcıdaki projeksiyon blob'u `'blobUri'` güncellemedi`'containerName'` |Belirtilen kapsayıcı yok. | Dizin oluşturucu, belirtilen kapsayıcının daha önce oluşturulup oluşturulmamasını denetler ve gerekirse oluşturur, ancak bu denetimi yalnızca bir kez dizinleyici çalıştırıcı başına gerçekleştirir. Bu hata, bir şeyin bu adımdan sonra kapsayıcıyı sildiği anlamına gelir.  Bu hatayı gidermek için şunu deneyin: depolama hesabı bilgilerinizi rahat bırakın, dizinleyicinin bitmesini bekleyin ve ardından dizinleyiciyi yeniden çalıştırın. |
| Kapsayıcıdaki projeksiyon blob'u `'blobUri'` güncellemedi`'containerName'` |Aktarım bağlantısına veri yazılamıyor: Varolan bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı. | Bunun Azure Depolama ile geçici bir hata olması ve bu nedenle dizinleyiciyi yeniden çalıştırarak çözülmesi bekleniyor. Bu hatayla sürekli olarak karşılaşırsanız, daha fazla araştırılabilmek için lütfen bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin.  |
| Tablodaki satırı `'projectionRow'` güncelleştiremedi`'tableName'` | Sunucu meşgul. | Bunun Azure Depolama ile geçici bir hata olması ve bu nedenle dizinleyiciyi yeniden çalıştırarak çözülmesi bekleniyor. Bu hatayla sürekli olarak karşılaşırsanız, daha fazla araştırılabilmek için lütfen bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin.  |

<a name="could-not-execute-skill-because-a-skill-input-was-invalid"/>

## <a name="warning-skill-input-was-invalid"></a>Uyarı: Beceri girişi geçersiz oldu
Beceri girişi eksikti, yanlış türde veya başka bir şekilde geçersizdi. Uyarı iletisi etkisini gösterir:
1) Beceri yürütülemedi
2) Beceri yürütüldü ancak beklenmeyen sonuçlar doğurabilir

Bilişsel beceriler gerekli girdiler ve isteğe bağlı girdiler var. Örneğin [Anahtar tümcecik çıkarma becerisinin](cognitive-search-skill-keyphrases.md) `languageCode`iki gerekli girişi `text`vardır ve isteğe bağlı giriş yoktur. Özel beceri girişleri isteğe bağlı girişler olarak kabul edilir.

Gerekli girişler eksikse veya herhangi bir giriş doğru türde değilse, beceri atlanır ve bir uyarı oluşturur. Atlanan beceriler herhangi bir çıktı oluşturmaz, bu nedenle diğer beceriler atlanan beceriçıktılarını kullanırsa ek uyarılar oluşturabilirler.

İsteğe bağlı bir giriş eksikse, beceri yine de çalışır, ancak eksik giriş nedeniyle beklenmeyen çıktı üretebilir.

Her iki durumda da, verilerinizin şekli nedeniyle bu uyarı beklenebilir. Örneğin, `firstName`alanları `middleName`olan kişiler hakkında bilgi içeren bir belgeniz varsa ve `lastName`, ' için `middleName`girişi olmayan bazı belgelerinz olabilir. Eğer boru `middleName` hattında bir beceri için bir giriş olarak geçmek için, o zaman bu beceri girişi bazı zaman eksik olabilir bekleniyor. Bu uyarının sonucu olarak herhangi bir eylemin gerekli olup olmadığını belirlemek için verilerinizi ve senaryonuzu değerlendirmeniz gerekir.

Girişin eksik olması durumunda varsayılan bir değer sağlamak istiyorsanız, varsayılan değer oluşturmak için [Koşullu beceriyi](cognitive-search-skill-conditional.md) kullanabilir ve ardından [koşullu beceri](cognitive-search-skill-conditional.md) çıktısını beceri girişi olarak kullanabilirsiniz.


```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'en'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

| Neden | Ayrıntılar/Örnek | Çözüm |
| --- | --- | --- |
| Beceri girişi yanlış türüdür | "Gerekli beceri girişi beklenen tip `String`değildi. Adı: `text`, `/document/merged_content`Kaynak: ."  "Gerekli beceri girişi beklenen formatta değildi. Adı: `text`, `/document/merged_content`Kaynak: ."  "Dizi `/document/normalized_images/0/imageCelebrities/0/detail/celebrities`olmayan lar üzerinde tekrarlanamaz."  "Dizi `/document/normalized_images/0/imageCelebrities/0/detail/celebrities`dışı `0` seçileme " | Bazı beceriler belirli türde girdileri bekliyor, `text` örneğin Duygu [beceri](cognitive-search-skill-sentiment.md) bir dize olmasını bekliyor. Giriş dize olmayan bir değer belirtirse, beceri yürütülmüyor ve hiçbir çıktı üretmiyor. Veri setinizin giriş değerlerinin türünde tek tip olduğundan emin olun veya girişi önceden işlemek için [Özel Web API becerisi](cognitive-search-custom-skill-web-api.md) kullanın. Beceriyi bir dizi üzerinde yinelederseniz, beceri bağlamını ve `*` doğru konumlardaki girdiyi kontrol edin. Genellikle hem bağlam hem de giriş `*` kaynağı diziler için sona ermelidir. |
| Beceri girişi eksik | "Gerekli beceri girişi eksik. Adı: `text`, `/document/merged_content`Kaynak: " `/document/normalized_images/0/imageTags`"Eksik değer ."  "Uzunluk `0`dizisinde `0` `/document/pages` seçilemiyor." | Tüm belgeleriniz bu uyarıyı alıyorsa, büyük olasılıkla giriş yollarında bir yazım hatası vardır ve `*` yoldaki fazladan veya eksik özellik adı kasasını iki kez kontrol etmeli ve veri kaynağından gelen belgelerin gerekli girişleri sağladığından emin olmalısınız. |
| Beceri dili kodu girişi geçersiz | Beceri girişi `languageCode` aşağıdaki dil `X,Y,Z`kodlarına sahiptir ve en az biri geçersizdir. | Daha fazla ayrıntıyı [aşağıda](cognitive-search-common-errors-warnings.md#skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid) görebilirsiniz |

<a name="skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid"/>

## <a name="warning--skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid"></a>Uyarı: Beceri girişi 'languageCode' en az biri geçersiz olan 'X,Y,Z' dil kodlarına sahiptir.
Bir alt akış becerisinin isteğe bağlı `languageCode` girişine aktarılan değerlerden biri veya birkaçı desteklenmez. Bu durum, [LanguageDetectionSkill'in](cognitive-search-skill-language-detection.md) çıktısını sonraki becerilere aktarTıysanız oluşabilir ve çıktı, bu alt akım becerilerinde desteklenenden daha fazla dilden oluşur.

Veri setinizin tek bir dilde olduğunu biliyorsanız, [Dil Algılama Becerisini](cognitive-search-skill-language-detection.md) ve beceri girdisini `languageCode` kaldırmalı ve dilin bu beceri için desteklenmiş olduğunu varsayarak bu beceri için `defaultLanguageCode` beceri parametresini kullanmalısınız.

Veri setinizin birden çok dil içerdiğini ve bu nedenle `languageCode` [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) ve girişine ihtiyacınız olduğunu biliyorsanız, metni alt akış becerisine geçirmeden önce desteklenmeyen dillere sahip metinleri filtrelemek için bir [Koşullu Beceri](cognitive-search-skill-conditional.md) eklemeyi düşünün.  Burada Bu EntityRecognitionSkill için nasıl görünebileceğini bir örnektir:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'de' || $(/document/language) == 'en' || $(/document/language) == 'es' || $(/document/language) == 'fr' || $(/document/language) == 'it'" },
        { "name": "whenTrue", "source": "/document/content" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "supportedByEntityRecognitionSkill" } ]
}
```

Bu hata iletisini oluşturabilecek becerilerin her biri için şu anda desteklenen diller için bazı başvurular aşağıda veda edilmiştir:
* [Metin Analitiği Desteklenen Diller](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) [(KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md)için , [EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md), [SentimentSkill](cognitive-search-skill-sentiment.md), ve [PIIDetectionSkill](cognitive-search-skill-pii-detection.md))
* [Çevirmen Destekli Diller](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) [(Metin Çevirileri Için)](cognitive-search-skill-text-translation.md)
* [Metin Bölme Becerisi](cognitive-search-skill-textsplit.md) Desteklenen Diller:`da, de, en, es, fi, fr, it, ko, pt`

<a name="skill-input-was-truncated"/>

## <a name="warning-skill-input-was-truncated"></a>Uyarı: Beceri girişi kesildi
Bilişsel becerilerin, aynı anda analiz edilebilen metnin uzunluğuyla ilgili sınırları vardır. Bu becerilerin metin girişi bu sınırı aşarsa, metni sınırı karşılamak için kesilir ve sonra kesilen metinüzerindeki zenginleştirme gerçekleştiririz. Bu, becerinin yürütüldüğü, ancak verilerinizin tümünde değişmediği anlamına gelir.

Aşağıdaki "LanguageDetectionSkill" örneğinde, `'text'` giriş alanı karakter sınırını aşması durumunda bu uyarıyı tetikleyebilir. [Beceri belgelerinde](cognitive-search-predefined-skills.md)beceri giriş sınırlarını bulabilirsiniz.

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [...]
  }
```

Tüm metnin analiz edildiğinden emin olmak istiyorsanız, [Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

<a name="web-api-skill-response-contains-warnings"/>

## <a name="warning-web-api-skill-response-contains-warnings"></a>Uyarı: Web API beceri yanıtı uyarılar içerir
Indexer beceri becerisi nde bir beceri çalıştırabildi, ancak Web API isteğinin yanıtı yürütme sırasında uyarılar olduğunu gösterdi. Verilerinizin nasıl etkilendiğini ve eylem gerekip gerekmediğini anlamak için uyarıları gözden geçirin.

<a name="the-current-indexer-configuration-does-not-support-incremental-progress"/>

## <a name="warning-the-current-indexer-configuration-does-not-support-incremental-progress"></a>Uyarı: Geçerli dizinleyici yapılandırması artımlı ilerlemeyi desteklemez

Bu uyarı yalnızca Cosmos DB veri kaynakları için oluşur.

Dizin oluşturma sırasında artan ilerleme, dizinleyici yürütme geçici hatalar veya yürütme süresi sınırı tarafından kesilirse, dizinleyici sıfırdan tüm koleksiyonu yeniden dizine almak zorunda yerine, bir sonraki çalıştığında bıraktığı yerden alabilir sağlar. Bu, özellikle büyük koleksiyonları dizine ekrirken önemlidir.

Tamamlanmamış bir dizin oluşturma işine devam etme becerisi, `_ts` sütun tarafından sipariş edilen belgelerin olmasına bağlıdır. Dizinleyici, sonraki belgeyi almak için zaman damgası kullanır. `_ts` Sütun eksikse veya dizin leyici özel bir sorgu nun sıralanıp sıralanmadığını belirleyemezse, dizinleyici başlangıçta başlar ve bu uyarıyı görürsünüz.

Artımlı ilerlemeyi etkinleştirerek ve yapılandırma özelliğini kullanarak bu uyarıyı `assumeOrderByHighWatermarkColumn` bastırarak bu davranışı geçersiz kılmak mümkündür.

Daha fazla bilgi için [bkz.](search-howto-index-cosmosdb.md#IncrementalProgress)

<a name="some-data-was-lost-during projection-row-x-in-table-y-has-string-property-z-which-was-too-long"/>

## <a name="warning-some-data-was-lost-during-projection-row-x-in-table-y-has-string-property-z-which-was-too-long"></a>Uyarı: Bazı veriler projeksiyon sırasında kayboldu. 'Y' tablosundaki 'X' satırıçok uzun olan 'Z' dize özelliğine sahiptir.

[Tablo Depolama hizmeti,](https://azure.microsoft.com/services/storage/tables) varlık [özelliklerinin](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model#property-types) ne kadar büyük olabileceğine ilişkin sınırlara sahiptir. Dizeleri 32.000 karakter veya daha az olabilir. Dize özelliği 32.000 karakterden uzun olan bir satır önsürüyorsa, yalnızca ilk 32.000 karakter korunur. Bu sorunu çözmek için, dize özellikleri 32.000 karakterden daha uzun olan satırları yansıtmaktan kaçının.

<a name="truncated-extracted-text-to-x-characters"/>

## <a name="warning-truncated-extracted-text-to-x-characters"></a>Uyarı: X karakterleri için kesilen metin
Dizin işaretleyiciler, herhangi bir belgeden ne kadar metin çıkarılabilir sınırlayın. Bu sınır fiyatlandırma katmanına bağlıdır: Ücretsiz katman için 32.000 karakter, Temel için 64.000, Standart için 4 milyon, Standart S2 için 8 milyon ve Standart S3 için 16 milyon. Kesilen metin dizine eklenmez. Bu uyarıyı önlemek için, büyük miktarda metin içeren belgeleri birden çok, daha küçük belgelere ayırmayı deneyin. 

Daha fazla bilgi için [Indexer sınırlarına](search-limits-quotas-capacity.md#indexer-limits)bakın.

<a name="could-not-map-output-field-x-to-search-index"/>

## <a name="warning-could-not-map-output-field-x-to-search-index"></a>Uyarı: Çıkış alanını arama dizinine 'X' eşlenemedi
Var olmayan/null verilerine başvuran çıktı alanı eşlemeleri, her belge için uyarılar üretir ve boş bir dizin alanı yla sonuçlanır. Bu sorunu çözmek için, olası yazım hataları için çıktı alanı eşleme kaynak yollarınızı iki kez denetleyin veya [Koşullu beceriyi](cognitive-search-skill-conditional.md#sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist)kullanarak varsayılan bir değer ayarlayın.

<a name="the-data-change-detection-policy-is-configured-to-use-key-column-x"/>

## <a name="warning-the-data-change-detection-policy-is-configured-to-use-key-column-x"></a>Uyarı: Veri değişikliği algılama ilkesi 'X' anahtar sütunu kullanacak şekilde yapılandırılmıştır
[Veri değişikliği algılama ilkeleri,](https://docs.microsoft.com/rest/api/searchservice/create-data-source#data-change-detection-policies) değişikliği algılamak için kullandıkları sütunlar için özel gereksinimlere sahiptir. Bu gereksinimlerden biri, bu sütunun kaynak öğe her değiştirilmede güncelleştirilmesidir. Başka bir gereksinim, bu sütun için yeni değer önceki değerden daha büyük olmasıdır. Anahtar sütunlar, her güncelleştirmede değişmedikleri için bu gereksinimi karşılamaz. Bu sorunu çözmek için, değişiklik algılama ilkesi için farklı bir sütun seçin.

<a name="document-text-appears-to-be-utf-16-encoded-but-is-missing-a-byte-order-mark"/>

## <a name="warning-document-text-appears-to-be-utf-16-encoded-but-is-missing-a-byte-order-mark"></a>Uyarı: Belge metni UTF-16 kodlanmış gibi görünüyor, ancak bir bayt sipariş işareti eksik

[Dizin leyici ayrıştırma modlarının,](https://docs.microsoft.com/rest/api/searchservice/create-indexer#blob-configuration-parameters) ayrıştırmadan önce metnin nasıl kodlandığını bilmesi gerekir. Metni kodlamanın en yaygın iki yolu UTF-16 ve UTF-8'dir. UTF-8, her karakterin 1 bayt ile 4 bayt uzunluğunda olduğu değişken uzunlukta bir kodlamadır. UTF-16, her karakterin 2 bayt uzunluğunda olduğu sabit uzunlukta bir kodlamadır. UTF-16 iki farklı varyantları vardır, "büyük endian" ve "küçük endian". Metin kodlaması, metinden önce bir dizi bayt olan "bayt sıraişareti" ile belirlenir.

| Encoding | Bayt Sipariş İşareti |
| --- | --- |
| UTF-16 Büyük Endian | 0xFE 0xFF |
| UTF-16 Küçük Endian | 0xFF 0xFE |
| UTF-8 | 0xEF 0xBB 0xBF |

Bayt sipariş işareti yoksa, metnin UTF-8 olarak kodlanmış olduğu varsayılır.

Bu uyarıyı çözmek için, bu blob için metin kodlama sının ne olduğunu belirleyin ve uygun bayt sipariş işaretini ekleyin.

<a name="cosmos-db-collection-has-a-lazy-indexing-policy"/>

## <a name="warning-cosmos-db-collection-x-has-a-lazy-indexing-policy-some-data-may-be-lost"></a>Uyarı: Cosmos DB koleksiyonu 'X' bir Tembel dizinleme ilkesi vardır. Bazı veriler kaybolabilir

[Lazy](https://docs.microsoft.com/azure/cosmos-db/index-policy#indexing-mode) dizin oluşturma ilkelerine sahip koleksiyonlar tutarlı bir şekilde sorgulanamıyor ve bu da dizin leyicinizin eksik verilerine neden olur. Bu uyarıyı aşmak için dizin oluşturma ilkenizi Tutarlı olarak değiştirin.
