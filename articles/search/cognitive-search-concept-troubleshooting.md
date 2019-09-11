---
title: Bilişsel arama için sorun giderme ipuçları-Azure Search
description: Azure Search 'de bilişsel arama işlem hatlarını ayarlamaya yönelik ipuçları ve sorun giderme.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.subservice: cognitive-search
ms.workload: search
ms.topic: conceptual
ms.date: 02/02/2019
ms.author: luisca
ms.openlocfilehash: bc83a7c06c52ee26246329d6ca3177bce71c9de8
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186416"
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Bilişsel arama için sorun giderme ipuçları

Bu makale, Azure Search ' de bilişsel arama özellikleri ile çalışmaya başladığı sürece hareket ettirmeden elde edilecek ipuçları ve püf noktaları listesini içerir. 

Daha önce yapmadıysanız [öğreticide adım adım ilerleyin: Bilişsel arama API 'lerini](cognitive-search-quickstart-blob.md) , bir blob veri kaynağına bilişsel arama zenginleştirmelerinin uygulanmasıyla ilgili uygulama için nasıl çağıracağınızı öğrenin.

## <a name="tip-1-start-with-a-small-dataset"></a>İpucu 1: Küçük bir veri kümesiyle başlayın
Sorunları hızlı bir şekilde bulmanın en iyi yolu, sorunları giderebileceğiniz hızı artırmanızı sağlar. Dizin oluşturma süresini düşürmenin en iyi yolu, dizine eklenecek belgelerin sayısını azaltmaktır. 

Yalnızca bir belge/kayıt ile bir veri kaynağı oluşturarak başlayın. Belge örneğiniz, dizine eklenecek çeşitli belgelerin iyi bir gösterimi olmalıdır. 

Belge örneğinizi uçtan uca işlem hattı ile çalıştırın ve sonuçların ihtiyaçlarınızı karşılayıp karşılamadığını kontrol edin. Sonuçlardan memnun olduktan sonra veri kaynağınıza daha fazla dosya ekleyebilirsiniz.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>İpucu 2: Veri kaynağı kimlik bilgilerinizin doğru olduğundan emin olun
Veri kaynağı bağlantısı, onu kullanan bir Dizin Oluşturucu tanımlanana kadar doğrulanmaz. Dizin oluşturucunun verilere ulaşıma bir hata görürseniz, aşağıdakilerden emin olun:
- Bağlantı dizeniz doğru. Özel olarak SAS belirteçleri oluştururken Azure Search tarafından beklenen biçimi kullandığınızdan emin olun. Desteklenen [farklı biçimler hakkında bilgi edinmek](
https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) için bkz. kimlik bilgilerini belirtme bölümü.
- Dizin oluşturucudaki kapsayıcı adınız doğru.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>İpucu 3: Bazı sorunlar olsa bile bkz. ne işe yarar?
Bazen küçük bir hata, kendi izlemelerinin bir dizin oluşturucuyu durduruyor. Sorunları tek tek gidermeye çalışırsanız bu çok uygundur. Ancak, hangi akışların gerçekten çalıştığını görebilmeniz için dizin oluşturucunun devam etmesine izin vererek belirli bir hata türünü yoksaymak isteyebilirsiniz.

Bu durumda, Dizin oluşturucudan hataları yok saymasını söylemek isteyebilirsiniz. Bunu, Dizin Oluşturucu tanımının bir parçası olarak *maxFailedItems* ve *Maxfaileditemsperbatch* değerini-1 olarak ayarlayarak yapın.

```
{
  "// rest of your indexer definition
   "parameters":
   {
      "maxFailedItems":-1,
      "maxFailedItemsPerBatch":-1
   }
}
```
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>İpucu 4: En gelişmiş belgelerde bakıyor 
Zenginleştirilmiş belgeler, zenginleştirme sırasında oluşturulan geçici yapılardır ve sonra işlem tamamlandığında silinir.

Dizin oluşturma sırasında oluşturulan zenginleştirilmiş belgenin anlık görüntüsünü yakalamak için dizininize ```enriched``` adlı bir alan ekleyin. Dizin oluşturucu, otomatik olarak alana, o belgenin tüm zenginleştirmelerinin dize gösteriminin dökümünü alır.

```enriched``` alanı, JSON’da bellek içi zenginleştirilmiş belgenin mantıksal gösterimi olan bir dize içerir.  Ancak alan değeri geçerli bir JSON belgesidir. Tırnak işaretlerine kaçış karakteri eklenir, böylece belgeyi biçimlendirilmiş JSON olarak görüntülemek için `\"` öğesini `"` ile değiştirmeniz gerekir. 

Zenginleştirilmiş alan, ifadelerin değerlendirmekte olduğu içeriğin mantıksal şeklini anlamanıza yardımcı olmak için yalnızca hata ayıklama amacıyla hazırlanmıştır. Dizin oluşturma amacıyla bu alana bağlı kullanmamalısınız.

Hata ayıklama ```enriched``` amacıyla dizin tanımınızın bir parçası olarak bir alan ekleyin:

#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "fields": [
    // other fields go here.
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```

## <a name="tip-5-expected-content-fails-to-appear"></a>İpucu 5: Beklenen içerik görüntülenemez

Eksik içerik, dizin oluşturma sırasında bırakılan belgelerin sonucu olabilir. Ücretsiz ve temel katmanların belge boyutu üzerinde düşük limitleri vardır. Sınırı aşan herhangi bir dosya dizinleme sırasında bırakılır. Azure portal bırakılan belgeleri kontrol edebilirsiniz. Arama hizmeti panosunda Dizin oluşturucular kutucuğuna çift tıklayın. Dizin oluşturulan başarılı belgelerin oranını gözden geçirin. % 100 değilse, daha fazla ayrıntı almak için oranına tıklayabilirsiniz. 

Sorun dosya boyutuyla ilişkiliyse, şöyle bir hata görebilirsiniz: "Blob \<dosya adı >", geçerli hizmet katmanınız için \<belge ayıklama boyutu üst sınırını aşan dosya boyutu > baytlarının boyutuna sahiptir. " Dizin Oluşturucu sınırları hakkında daha fazla bilgi için bkz. [hizmet limitleri](search-limits-quotas-capacity.md).

İçeriğin görünmesi için ikinci bir neden ilgili giriş/çıkış eşleme hataları olabilir. Örneğin, çıkış hedefi adı "kişiler", ancak dizin alanı adı küçük harfli "insanlar" dır. Sistem, bir alanın boş olduğu durumlarda dizin oluşturma işleminin başarılı olduğunu düşünmenize olanak sağlamak için tüm işlem hattının 201 başarılı iletilerini döndürebilir. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>İpucu 6: İşlemeyi maksimum çalışma süresinin (24 saatlik pencere) ötesine Genişlet

Görüntü analizi, hatta basit durumlar için hesaplama açısından yoğun olduğundan, görüntüler özellikle büyük veya karmaşık olduğunda, işlem süreleri izin verilen en uzun süreyi aşabilir. 

Maksimum çalışma süresi katmana göre farklılık gösterir: Ücretsiz katmanda birkaç dakika, faturalandırılabilir katmanda 24 saat dizin oluşturma. İstek üzerine işlenmek üzere 24 saatlik bir dönemde işlem tamamlanamazsa, dizin oluşturucunun, kaldığınız yerden işlemeyi sağlamak için bir zamanlamaya geçin. 

Zamanlanan Dizin oluşturucular için, dizin oluşturma en son bilinen iyi belgede zamanlamaya devam eder. Yinelenen bir zamanlama kullanarak, Dizin Oluşturucu, tüm işlenmemiş görüntüler işlenene kadar bir saat veya gün boyunca görüntü biriktirme listesi aracılığıyla kendi yolunu kullanabilir. Zamanlama sözdizimi hakkında daha fazla bilgi için bkz [. Adım 3: Bir-Indexer](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer) oluşturun veya [Azure Search için dizin oluşturucularının zamanlamasını](search-howto-schedule-indexers.md)inceleyin.

> [!NOTE]
> Bir Dizin Oluşturucu belirli bir zamanlamaya göre ayarlanırsa ancak her çalıştırıldığında aynı belgede tekrar tekrar tekrar başarısız olursa, Dizin Oluşturucu bir süre önce devam edene kadar daha az sıklıkta (en fazla 24 saatte bir) çalışmaya başlar 'ndaki.  Dizin oluşturucunun belirli bir noktada takılmasına neden olan sorunu düzelttiğini düşünüyorsanız, dizin oluşturucunun isteğe bağlı olarak çalıştırılmasını gerçekleştirebilir ve başarıyla ilerleme yapıyorsa, Dizin Oluşturucu ayarlanan zamanlama aralığına yeniden döner.

Portal tabanlı dizin oluşturma için (hızlı başlangıç bölümünde açıklandığı gibi), "bir kez çalıştır" Dizin Oluşturucu seçeneği, işlemeyi 1 saat (`"maxRunTime": "PT1H"`) ile sınırlar. İşleme penceresini daha uzun bir şeye genişletmek isteyebilirsiniz.

## <a name="tip-7-increase-indexing-throughput"></a>İpucu 7: Dizin oluşturma verimini artırma

[Paralel dizin oluşturma](search-howto-large-index.md)için, verilerinizi aynı kapsayıcı içinde birden çok kapsayıcıya veya birden çok sanal klasöre yerleştirin. Ardından birden çok veri kaynağı ve Dizin Oluşturucu çifti oluşturun. Tüm Dizin oluşturucular aynı beceri kullanabilir ve aynı hedef arama dizinine yazabilir, bu nedenle arama uygulamanızın bu bölümlemeden haberdar olması gerekmez.
Daha fazla bilgi için bkz. [büyük veri kümelerini dizinleme](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Ayrıca bkz.
+ [Hızlı Başlangıç: Portalda bilişsel arama işlem hattı oluşturma](cognitive-search-quickstart-blob.md)
+ [Öğretici: Bilişsel arama REST API 'Lerini öğrenin](cognitive-search-tutorial-blob.md)
+ [Veri kaynağı kimlik bilgilerini belirtme](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Büyük veri kümelerini dizinleme](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Zenginleştirilmiş alanları bir dizinle eşleme](cognitive-search-output-field-mapping.md)
