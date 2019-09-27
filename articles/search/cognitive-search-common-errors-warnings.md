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
ms.subservice: cognitive-search
ms.openlocfilehash: 4e31f818e96ae9f13e3ce8892e575318831848f6
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329389"
---
# <a name="common-errors-and-warnings-of-the-ai-enrichment-pipeline-in-azure-search"></a>Azure Search içindeki AI zenginleştirme işlem hattının ortak hataları ve uyarıları

Bu makalede, Azure Search ' de AI zenginleştirme sırasında karşılaşabileceğiniz yaygın hatalara ve uyarılara yönelik bilgi ve çözümler sağlanmaktadır.

## <a name="errors"></a>Hatalar
Hata sayısı [' maxfaileditems '](cognitive-search-concept-troubleshooting.md#tip-3-see-what-works-even-if-there-are-some-failures)aştığında dizin oluşturma durduruluyor. Aşağıdaki bölümler, dizin oluşturmanın devam etmesine izin veren hataları çözmenize yardımcı olabilir.

### <a name="could-not-read-document"></a>Belge okunamadı
Dizin Oluşturucu, veri kaynağından belgeyi okuyamadı. Bunun nedeni aşağıdakiler olabilir:

| Reason | Örnek | Action |
| --- | --- | --- |
| farklı belgeler genelinde tutarsız alan türleri | Değerin türü sütun türüyle eşleşmiyor. @No__t-0, yazarlar sütununda depolanamadı.  Beklenen tür JArray. | Her alanın türünün farklı belgeler arasında aynı olduğundan emin olun. Örneğin, ilk belge `'startTime'` alanı bir tarih saat ise ve ikinci belgede bir dize ise, bu hata olur. |
| veri kaynağının temelindeki hizmetten alınan hatalar | (Cosmos DB) `{"Errors":["Request rate is large"]}` | Sağlıklı olduğundan emin olmak için depolama örneğinizi denetleyin. Ölçeklendirmeyi/bölümlemeyi ayarlamanız gerekebilir. |
| geçici sorunlar | Sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu. sağlayıcısını TCP sağlayıcısı, hata: 0-mevcut bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı | Bazen beklenmedik bağlantı sorunları var. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

### <a name="could-not-extract-document-content"></a>Belge içeriği ayıklanamadı
Blob veri kaynağı olan Dizin Oluşturucu, içeriği belgeden (örneğin, bir PDF dosyası) ayıklayamadı. Bunun nedeni aşağıdakiler olabilir:

| Reason | Örnek | Action |
| --- | --- | --- |
| blob boyut sınırının üzerinde | Belge, geçerli hizmet katmanınız için belge ayıklama için en büyük boyut olan `'134217728'` bayt olan `'150441598'` bayttır. | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob desteklenmeyen içerik türüne sahip | Belgede desteklenmeyen içerik türü `'image/png'` | [blob dizin oluşturma hataları](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| blob şifreli | Belge işlenemedi; şifrelenmiş veya parola korumalı olabilir. | [blob ayarları](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed) |
| geçici sorunlar | Blob işleme hatası: İstek durduruldu: İstek iptal edildi. | Bazen beklenmedik bağlantı sorunları var. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

### <a name="could-not-parse-document"></a>Belge ayrıştırılamadı
Dizin Oluşturucu veri kaynağından belgeyi okudu, ancak belge içeriği belirtilen alan eşleme şemasına dönüştürülürken bir sorun oluştu. Bunun nedeni aşağıdakiler olabilir:

| Reason | Örnek | Action |
| --- | --- | --- |
| Belge anahtarı eksik | Belge anahtarı eksik veya boş olamaz | Tüm belgelerin geçerli belge anahtarlarına sahip olduğundan emin olun |
| Belge anahtarı geçersiz | Belge anahtarı 1024 karakterden uzun olamaz | Belge anahtarını doğrulama gereksinimlerini karşılayacak şekilde değiştirin. |
| Alan eşlemesi bir alana uygulanamadı | Eşleme işlevi `'functionName'` `'fieldName'` alanına uygulanamadı. Dizi null olamaz. Parametre adı: bayt | Dizin oluşturucuda tanımlı [alan eşlemelerini](search-indexer-field-mappings.md) iki kez kontrol edin ve başarısız olan belgenin belirtilen alanındaki verilerle karşılaştırın. Alan eşlemelerini veya belge verilerini değiştirmek gerekebilir. |
| Alan değeri okunamadı | @No__t-1 dizinindeki `'fieldName'` sütununun değeri okunamadı. Sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu. sağlayıcısını TCP sağlayıcısı, hata: 0-mevcut bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı.) | Bu hatalar genellikle veri kaynağının temelindeki hizmet ile ilgili beklenmedik bağlantı sorunlarından kaynaklanır. Belgeyi Dizin oluşturucudan daha sonra tekrar çalıştırmayı deneyin. |

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