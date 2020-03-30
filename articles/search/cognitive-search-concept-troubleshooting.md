---
title: AI zenginleştirme tasarımı için ipuçları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da AI zenginleştirme ardışık noktalarını ayarlamanın ipuçları ve sorun giderme sorunları.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3fef5db90c3ae63a8fa48835646e09f9dfe6f023
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245492"
---
# <a name="tips-for-ai-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da AI zenginleştirme için ipuçları

Bu makalede, Azure Bilişsel Arama'da AI zenginleştirme özelliklerine başlarken hareket etmeye devam etmeniz için ipuçları ve püf noktaları bir liste bulunmaktadır. 

Bunu daha önce yapmadıysanız, Öğretici'ye adım atın: Bir blob veri kaynağına AI zenginleştirmeleri uygulama pratiği için [AI zenginleştirme API'lerini nasıl çağıracağınızı öğrenin.](cognitive-search-quickstart-blob.md)

## <a name="tip-1-start-with-a-small-dataset"></a>İpucu 1: Küçük bir veri kümesiyle başlayın
Sorunları hızlı bir şekilde bulmanın en iyi yolu, sorunları çözebileceğiniz hızı artırmaktır. Dizin oluşturma süresini azaltmanın en iyi yolu, dizine eklenecek belge sayısını azaltmaktır. 

Yalnızca bir avuç belge/kayıt içeren bir veri kaynağı oluşturarak başlayın. Belge örneğiniz, dizine eklenecek belge çeşitliliğinin iyi bir temsili olmalıdır. 

Belge örneğini uçtan uca ardışık ardışık ardışık ardışık ardışık olarak çalıştırın ve sonuçların gereksinimlerinizi karşılayıp karşılamadığını kontrol edin. Sonuçlardan memnun kaldıktan sonra, veri kaynağınıza daha fazla dosya ekleyebilirsiniz.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>İpucu 2: Veri kaynağı kimlik bilgilerinizin doğru olduğundan emin olun
Veri kaynağı bağlantısı, onu kullanan bir dizinleyici tanımlayana kadar doğrulanmaz. Dizinleyicinin verilere ulaşamayacağından bahsetmiş herhangi bir hata görürseniz, aşağıdakilerden emin olun:
- Bağlantı dizeniz doğru. Özellikle SAS belirteçleri oluştururken, Azure Bilişsel Arama tarafından beklenen biçimi kullandığınızdan emin olun. Desteklenen farklı biçimler hakkında bilgi edinmek için [kimlik bilgileri bölümünü nasıl belirtin.](
https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials)
- Dizinleyicideki kapsayıcı adınız doğru.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>İpucu 3: Bazı hatalar olsa bile nelerin işe yaradığına bakın
Bazen küçük bir hata, parçalarında bir dizinleyiciyi durdurur. Sorunları tek tek çözmeyi planlıyorsanız sorun yok. Ancak, dizinleyicinin hangi akışların gerçekte çalıştığını görebilmeniz için devam etmesine izin vererek belirli bir hata türünü yoksaymak isteyebilirsiniz.

Bu durumda, dizinleyiciye hataları yoksaymasını söylemek isteyebilirsiniz. Bunu, dizinleyici tanımının bir parçası olarak *maxFailedItems* ve *maxFailedItemsPerBatch'ı* -1 olarak ayarlayarak yapın.

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
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>İpucu 4: Kaputun altındaki zenginleştirilmiş belgelere bakmak 
Zenginleştirilmiş belgeler, zenginleştirme sırasında oluşturulan ve işleme tamamlandığında silinen geçici yapılardır.

Dizin oluşturma sırasında oluşturulan zenginleştirilmiş belgenin anlık görüntüsünü yakalamak için dizininize ```enriched``` adlı bir alan ekleyin. Dizin oluşturucu, otomatik olarak alana, o belgenin tüm zenginleştirmelerinin dize gösteriminin dökümünü alır.

```enriched``` alanı, JSON’da bellek içi zenginleştirilmiş belgenin mantıksal gösterimi olan bir dize içerir.  Ancak alan değeri geçerli bir JSON belgesidir. Tırnak işaretlerine kaçış karakteri eklenir, böylece belgeyi biçimlendirilmiş JSON olarak görüntülemek için `\"` öğesini `"` ile değiştirmeniz gerekir. 

Zenginleştirilmiş alan, ifadelerin karşı değerlendirildiği içeriğin mantıksal şeklini anlamanıza yardımcı olmak için yalnızca hata ayıklama amacıyla tasarlanmıştır. Dizin oluşturma amacıyla bu alana bağlı olmamalıdır.

Hata ```enriched``` ayıklama amacıyla dizin tanımınızın bir parçası olarak bir alan ekleyin:

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

## <a name="tip-5-expected-content-fails-to-appear"></a>İpucu 5: Beklenen içerik görüntülenmez

Eksik içerik, belgelerin dizin oluşturma sırasında düşmesisonucu olabilir. Ücretsiz ve Temel katmanlar belge boyutunda düşük sınırlara sahiptir. Dizin oluşturma sırasında sınırı aşan tüm dosya bırakılır. Azure portalında bırakılan belgeleri denetleyebilirsiniz. Arama hizmeti panosunda, Dizin leyiciler döşemesini çift tıklatın. Dizine eklenmiş başarılı belgelerin oranını gözden geçirin. %100 değilse, daha fazla ayrıntı almak için oranı tıklatabilirsiniz. 

Sorun dosya boyutuyla ilgiliyse, şu gibi bir hata görebilirsiniz: "Blob \<dosya adı \<>" dosya boyutuna> bayt boyutuna sahiptir ve bu da geçerli hizmet katmanınız için belge ayıklama için maksimum boyutu aşıyor. Dizinleyici sınırları hakkında daha fazla bilgi için [Hizmet sınırlarına](search-limits-quotas-capacity.md)bakın.

İçeriğin görünmemesinin ikinci nedeni, ilgili giriş/çıktı eşleme hataları olabilir. Örneğin, bir çıktı hedef adı "Kişiler" ancak dizin alan adı küçük harf "kişiler" dir. Sistem tüm boru hattı için 201 başarı iletileri döndürebilir, böylece dizin oluşturmanın başarılı olduğunu düşünürsunuz, aslında bir alan boşken. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>İpucu 6: İşlemi maksimum çalışma süresinin ötesine genişletme (24 saatlik pencere)

Görüntü analizi basit durumlar için bile hesaplama lı yoğundur, bu nedenle görüntüler özellikle büyük veya karmaşık olduğunda, işleme süreleri izin verilen maksimum süreyi aşabilir. 

Maksimum çalışma süresi katmana göre değişir: Serbest katmanda birkaç dakika, faturalandırılabilir katmanlarda 24 saat dizin oluşturma. İsteğe bağlı işleme için 24 saatlik bir süre içinde işlem tamamlanırsa, dizinleyicinin işlemeyi bıraktığı yerden almasını sağlamak için zamanlamaya geçin. 

Zamanlanmış dizinleyiciler için, dizin oluşturma, bilinen son iyi belgede zamanlamaya göre devam eder. Dizinleyici, yinelenen bir zamanlama kullanarak, işlenmemiş tüm görüntüler işlenene kadar bir dizi saat veya gün boyunca görüntü biriktirme listesinde çalışabilir. Zamanlama sözdizimi hakkında daha fazla bilgi için [bkz: Adım 3: Dizini oluştur](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer) oluşturma veya [Azure Bilişsel Arama için dizin oluşturucuları nasıl zamanlayabilirsiniz'a](search-howto-schedule-indexers.md)bakın.

> [!NOTE]
> Bir dizin leyici belirli bir zamanlamaya ayarlanır, ancak her çalıştığında aynı belgede tekrar tekrar başarısız olursa, dizinleyici başarılı bir şekilde ilerleme kaydedene kadar daha az sıklıkta (en az 24 saatte bir en fazla) çalışmaya başlar Tekrar.  Dizinleyicinin belirli bir noktada sıkışıp kalmasına neden olan sorunu düzelttettiğinizi düşünüyorsanız, dizinleyicinin isteğe bağlı çalışmasını gerçekleştirebilirsiniz ve bu başarıyla ilerleme kaydederse, dizinleyici yine ayarlanan zamanlama aralığına geri döner.

Portal tabanlı dizin oluşturma için (quickstart'ta açıklandığı gibi), "bir kez çalıştır"`"maxRunTime": "PT1H"`dizinleyici seçeneğini seçmek işlemeyi 1 saatle sınırlar ( ). İşlem eşi penceresini daha uzun bir süreye uzatmak isteyebilirsiniz.

## <a name="tip-7-increase-indexing-throughput"></a>İpucu 7: Dizin oluşturma iş buzunu artırma

[Paralel dizine için](search-howto-large-index.md)verilerinizi aynı kapsayıcının içine birden çok kapsayıcıya veya birden çok sanal klasöre yerleştirin. Ardından birden çok veri kaynağı ve dizinleyici çifti oluşturun. Tüm dizin leyiciler aynı skillset'i kullanabilir ve aynı hedef arama dizinine yazabilir, böylece arama uygulamanızın bu bölümlemeden haberdar olması gerekmez.
Daha fazla bilgi için [bkz.](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

## <a name="see-also"></a>Ayrıca bkz.
+ [Quickstart: Portalda bir AI zenginleştirme boru hattı oluşturun](cognitive-search-quickstart-blob.md)
+ [Öğretici: AI zenginleştirme REST API'leri öğrenin](cognitive-search-tutorial-blob.md)
+ [Veri kaynağı kimlik bilgilerini belirtme](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Büyük Veri Kümelerini Dizine Alma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Zenginleştirilmiş alanları bir dizine nasıl eşlersin?](cognitive-search-output-field-mapping.md)
