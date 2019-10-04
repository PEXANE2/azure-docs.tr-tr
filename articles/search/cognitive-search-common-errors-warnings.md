---
title: Sık karşılaşılan hatalar ve uyarılar-Azure Search
description: Bu makalede, Azure Search ' de AI zenginleştirme sırasında karşılaşabileceğiniz yaygın hatalara ve uyarılara yönelik bilgi ve çözümler sağlanmaktadır.
services: search
manager: heidist
author: amotley
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: abmotley
ms.openlocfilehash: 18befbfb924129518ac32a7fdddaa9ee573840b0
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936481"
---
# <a name="common-errors-and-warnings-of-the-ai-enrichment-pipeline-in-azure-search"></a>Azure Search içindeki AI zenginleştirme işlem hattının ortak hataları ve uyarıları

Bu makalede, Azure Search ' de AI zenginleştirme sırasında karşılaşabileceğiniz yaygın hatalara ve uyarılara yönelik bilgi ve çözümler sağlanmaktadır.

## <a name="errors"></a>Hatalar
Hata sayısı [' maxfaileditems '](cognitive-search-concept-troubleshooting.md#tip-3-see-what-works-even-if-there-are-some-failures)aştığında dizin oluşturma durduruluyor. Aşağıdaki bölümler, dizin oluşturmanın devam etmesine izin veren hataları çözmenize yardımcı olabilir.

### <a name="could-not-read-document"></a>Belge okunamadı
Dizin Oluşturucu, veri kaynağından belgeyi okuyamadı. Bunun nedeni aşağıdakiler olabilir:

| Neden | Örnek | Eylem |
| --- | --- | --- |
| farklı belgeler genelinde tutarsız alan türleri | Değerin türü sütun türüyle eşleşmiyor. @No__t-0, yazarlar sütununda depolanamadı.  Beklenen tür JArray. | Her alanın türünün farklı belgeler arasında aynı olduğundan emin olun. Örneğin, ilk belge `'startTime'` alanı bir tarih saat ise ve ikinci belgede bir dize ise, bu hata olur. |
| veri kaynağının temelindeki hizmetten alınan hatalar | (Cosmos DB) `{"Errors":["Request rate is large"]}` | Sağlıklı olduğundan emin olmak için depolama örneğinizi denetleyin. Ölçeklendirmeyi/bölümlemeyi ayarlamanız gerekebilir. |
| geçici sorunlar | Sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu. (sağlayıcı: TCP sağlayıcısı, hata: 0-var olan bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı | Bazen beklenmedik bağlantı sorunları var. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

### <a name="could-not-extract-document-content"></a>Belge içeriği ayıklanamadı
Blob veri kaynağı olan Dizin Oluşturucu, içeriği belgeden (örneğin, bir PDF dosyası) ayıklayamadı. Bunun nedeni aşağıdakiler olabilir:

| Neden | Örnek | Eylem |
| --- | --- | --- |
| blob boyut sınırının üzerinde | Belge, geçerli hizmet katmanınız için belge ayıklama için en büyük boyut olan `'134217728'` bayt olan `'150441598'` bayttır. | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob desteklenmeyen içerik türüne sahip | Belgede desteklenmeyen içerik türü `'image/png'` | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob şifreli | Belge işlenemedi; şifrelenmiş veya parola korumalı olabilir. | [blob ayarları](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed) |
| geçici sorunlar | Blob işleme hatası: istek durduruldu: istek iptal edildi. | Bazen beklenmedik bağlantı sorunları var. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

### <a name="could-not-parse-document"></a>Belge ayrıştırılamadı
Dizin Oluşturucu veri kaynağından belgeyi okudu, ancak belge içeriği belirtilen alan eşleme şemasına dönüştürülürken bir sorun oluştu. Bunun nedeni aşağıdakiler olabilir:

| Neden | Örnek | Eylem |
| --- | --- | --- |
| Belge anahtarı eksik | Belge anahtarı eksik veya boş olamaz | Tüm belgelerin geçerli belge anahtarlarına sahip olduğundan emin olun |
| Belge anahtarı geçersiz | Belge anahtarı 1024 karakterden uzun olamaz | Belge anahtarını doğrulama gereksinimlerini karşılayacak şekilde değiştirin. |
| Alan eşlemesi bir alana uygulanamadı | Eşleme işlevi `'functionName'` `'fieldName'` alanına uygulanamadı. Dizi null olamaz. Parametre adı: bayt | Dizin oluşturucuda tanımlı [alan eşlemelerini](search-indexer-field-mappings.md) iki kez kontrol edin ve başarısız olan belgenin belirtilen alanındaki verilerle karşılaştırın. Alan eşlemelerini veya belge verilerini değiştirmek gerekebilir. |
| Alan değeri okunamadı | @No__t-1 dizinindeki `'fieldName'` sütununun değeri okunamadı. Sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu. (sağlayıcı: TCP sağlayıcısı, hata: 0-var olan bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı.) | Bu hatalar genellikle veri kaynağının temelindeki hizmet ile ilgili beklenmedik bağlantı sorunlarından kaynaklanır. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

### <a name="skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid"></a>' LanguageCode ' yetenek girişinde, en az bir tane geçersiz olan ' X, Y, Z ' dil kodları vardır.
Bir aşağı akış beceriye ilişkin isteğe bağlı `languageCode` girişine geçirilen bir veya daha fazla değer desteklenmiyor. Bu durum, [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) çıktısını sonraki becerilerle geçirmeniz durumunda meydana gelir ve çıktı bu aşağı akış becerilerinizi desteklenenden daha fazla dilde oluşur.

Veri ayarlamış olduğunuz tek bir dilde olduğunu biliyorsanız, [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) ve `languageCode` yetenek girişini kaldırmalı ve bu beceri için dilin desteklendiği varsayılarak, bu beceri için `defaultLanguageCode` yetenek parametresini kullanmanız gerekir.

Veri ayarlamış olduğunuz dilin birden çok dil içerdiğini ve bu nedenle [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) ve `languageCode` girdisine ihtiyacınız olduğunu biliyorsanız, şunu içine geçirmeden önce desteklenmeyen dillerle metni filtrelemek Için bir [conditionalbeceri](cognitive-search-skill-conditional.md) eklemeyi göz önünde bulundurun. aşağı akış beceriye metin.  İşte bunun Entityrecognitionbeceri için nasıl görünebileceğini bir örnek:

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

Bu hata iletisini oluşturabilecek yeteneklerin her biri için şu anda desteklenen dillere yönelik bazı başvurular aşağıda verilmiştir:
* [Desteklenen metin analizi diller](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) ( [KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md), [Entityrecognitionbeceri](cognitive-search-skill-entity-recognition.md)ve [sentimentbecerisi](cognitive-search-skill-sentiment.md)için)
* [Translator tarafından desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) ( [metin Için translationbeceri](cognitive-search-skill-text-translation.md)için)
* [Metin bölünmüş beceri](cognitive-search-skill-textsplit.md) Desteklenen Diller: `da, de, en, es, fi, fr, it, ko, pt`

### <a name="skill-did-not-execute-within-the-time-limit"></a>Yetenek zaman sınırı içinde yürütülemedi
Bu hata iletisiyle karşılaşacağınız iki durum vardır; bunların her biri farklı bir şekilde ele alınmalıdır. Lütfen bu hatayı sizin için hangi beceriye döndürdiğine bağlı olarak aşağıdaki yönergeleri izleyin.

#### <a name="built-in-cognitive-service-skills"></a>Yerleşik bilişsel hizmet becerileri
Dil algılama, varlık tanıma veya OCR gibi yerleşik bilişsel yeteneklerin çoğu bilişsel hizmet API uç noktası tarafından desteklenir. Bazen bu uç noktalarla geçici sorunlar vardır ve bir istek zaman aşımına uğrar. Geçici sorunlar için, beklemek ve yeniden denemek dışında bir çözüm yoktur. Risk azaltma olarak, Dizin Oluşturucularınızı [bir zamanlamaya göre çalışacak](search-howto-schedule-indexers.md)şekilde ayarlamayı düşünün. Zamanlanan dizin oluşturma, sol taraftaki yeri seçer. Geçici sorunların çözümlendiğini, dizin oluşturma ve Bilişsel Beceri işleme, bir sonraki zamanlanan çalıştırmaya devam edebilmelidir.

#### <a name="custom-skills"></a>Özel yetenekler
Oluşturduğunuz özel bir yeteneğe sahip bir zaman aşımı hatasıyla karşılaşırsanız, deneyebileceğiniz birkaç işlem vardır. İlk olarak, özel becerinizi gözden geçirin ve sonsuz bir döngüde takılı olmadığından ve sürekli olarak bir sonuç döndürmediğinden emin olun. Durum olduğunu onayladıktan sonra, becerinizde yürütme zamanının ne olduğunu saptayın. Özel beceri tanımınızda açık bir `timeout` değeri ayarlamadıysanız, varsayılan `timeout` 30 saniyedir. Becerinizi yürütmek için 30 saniye yeterince uzun değilse, özel beceri tanımınızda daha yüksek bir `timeout` değeri belirtebilirsiniz. Zaman aşımının 90 saniyeye ayarlandığı özel beceri tanımına bir örnek aşağıda verilmiştir:

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

@No__t-0 parametresi için ayarlayabileceğiniz maksimum değer 230 saniyedir.  Özel becerinizde sürekli olarak 230 saniye içinde yürütülemediğinde, tek bir yürütmede işlemek üzere daha az belge olması için özel becerinizdeki `batchSize` ' ı azaltmayı düşünebilirsiniz.  @No__t-0 ' ı zaten 1 olarak ayarladıysanız, bu yeteneği 230 saniye altında yürütebilmek için yeniden yazmanız gerekir veya herhangi bir özel yeteneğin yürütme süresi en fazla 230 saniyelik olacak şekilde birden çok özel beceriye bölebilirsiniz. Daha fazla bilgi için [özel beceri belgelerini](cognitive-search-custom-skill-web-api.md) gözden geçirin.

##  <a name="warnings"></a>Uyarılar
Uyarılar Dizin oluşturmayı durdurmaz, ancak beklenmedik sonuçlar oluşmasına neden olabilecek koşullar olduğunu gösterir. Eyleme ve senaryonuza bağlı olup olmadığına bakılmaksızın.

### <a name="skill-input-was-truncated"></a>Yetenek girişi kesildi
Bilişsel yetenekler, bir kerede çözümlenebilecek metin uzunluğuna yönelik sınırlara sahiptir. Bu yeteneklerin metin girişi bu sınırın üzerinde ise, sınırı karşılamak için metni kısaltacak ve sonra bu kesilen metinde zenginleştirme gerçekleştiririz. Bu, yeteneğin yürütüldüğü, ancak tüm verilerinizin üzerinde olmadığı anlamına gelir.

Aşağıdaki örnekte, `'text'` giriş alanı karakter sınırının üzerinde olursa bu uyarıyı tetikleyebilir. Beceri girişi sınırlarını [beceriler belgelerinde](cognitive-search-predefined-skills.md)bulabilirsiniz.

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

Tüm metinlerin çözümlendiğinden emin olmak istiyorsanız, [bölünmüş yeteneği](cognitive-search-skill-textsplit.md)kullanmayı göz önünde bulundurun.