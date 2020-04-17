---
title: Form Tanıma'daki yenilikler
titleSuffix: Azure Cognitive Services
description: Form Recognizer API'deki en son değişiklikleri anlayın.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 5f8c66db491b93278fedf1378d3df86e7ce5fdbf
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531091"
---
# <a name="whats-new-in-form-recognizer"></a>Form Tanıma'daki yenilikler

Form Recognizer hizmeti sürekli olarak güncelleştirilir. Özellik geliştirmeleri, düzeltmeler ve belge güncelleştirmeleri ile güncel kalmak için bu makaleyi kullanın.

> [!NOTE]
> Form Recognizer için hızlı başlangıçlar ve kılavuzlar belirtilmediği sürece HER zaman API'nin en son sürümünü kullanır.

## <a name="march-2020"></a>Mart 2020 

### <a name="new-features"></a>Yeni özellikler

* **Etiketleme için değer türleri** Artık Form Tanıyan örnek etiketleme aracıyla etiketlediğiniz değer türlerini belirtebilirsiniz. Aşağıdaki değer türleri ve varyasyonlar şu anda desteklenir:
  * `string`
    * varsayılan, `no-whitespaces``alphanumeric`
  * `number`
    * Varsayılan`currency`
  * `date` 
    * varsayılan, `dmy` `mdy`, ,`ymd`
  * `time`
  * `integer`

  Bu özelliğinasıl kullanacağınızı öğrenmek için [Örnek etiketleme aracı](./quickstarts/label-tool.md#specify-tag-value-types) kılavuzuna bakın.


* **Tablo görselleştirme** Örnek etiketleme aracı artık belgede tanınan tabloları görüntüler. Bu, etiketleme ve çözümlemeden önce belgeden tanınan ve çıkarılan tabloları görüntülemenize olanak tanır. Bu özellik katmanlar seçeneği kullanılarak açılabilir/kapatılabilir.

  Bu, tabloların nasıl tanındığı ve ayıklandırılınbir örneğidir:

  > [!div class="mx-imgBorder"]
  > ![Örnek etiketleme aracını kullanarak tablo görselleştirmesi](./media/whats-new/formre-table-viz.png)

    Ayıklanan tablolar JSON çıktısı altında `"pageResults"`mevcuttur.

  > [!IMPORTANT]
  > Etiketleme tabloları desteklenmez. Tablolar otomatik olarak tanınmıyor ve iade ediliyorsa, bunları yalnızca anahtar/değer çiftleri olarak etiketleyebilirsiniz. Tabloları anahtar/değer çiftleri olarak etiketlerken, her hücreyi benzersiz bir değer olarak etiketlendirin.

### <a name="extraction-enhancements"></a>Çıkarma geliştirmeleri

Bu sürüm, çıkarma geliştirmeleri ve doğruluk geliştirmeleri, özellikle, aynı metin satırında birden çok anahtar/değer çiftini etiketleme ve ayıklama özelliğini içerir. 
 
### <a name="sample-labeling-tool-is-now-open-source"></a>Örnek etiketleme aracı artık açık kaynak kodlu

Form Recognizer örnek etiketleme aracı artık açık kaynak projesi olarak kullanılabilir. Bunu çözümlerinize entegre edebilir ve ihtiyaçlarınızı karşılamak için müşteriye özel değişiklikler yapabilirsiniz.

Form Recognizer örnek etiketleme aracı hakkında daha fazla bilgi için [GitHub'da](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md)bulunan belgeleri inceleyin.

### <a name="tls-12-enforcement"></a>TLS 1.2 zorlaması

TLS 1.2 artık bu hizmete tüm HTTP istekleri için uygulanır. Daha fazla bilgi için Azure [Bilişsel Hizmetler güvenliği'ne](../cognitive-services-security.md)bakın.

## <a name="january-2020"></a>Ocak 2020

Bu sürüm Form Recognizer 2.0 (önizleme) tanıttı. Aşağıdaki bölümlerde, yeni özellikler, geliştirmeler ve değişiklikler hakkında daha fazla bilgi bulabilirsiniz. 

### <a name="new-features"></a>Yeni özellikler

* **Özel model**
  * **Etiketlerle tren** Artık el ile etiketlenmiş verilerle özel bir model eğitebilirsiniz. Bu, daha iyi performans gösteren modeller ile sonuçlanır ve anahtarsız değerler içeren karmaşık formlarla veya formlarla çalışan modeller üretebilir.
  * **Eşyokkron API** Büyük veri kümelerini ve dosyalarını eğitmek ve çözümlemek için async API çağrılarını kullanabilirsiniz.
  * **TIFF dosya desteği** Artık TIFF belgelerinden veri aktarabilir ve ayıklayabilirsiniz.
  * **Çıkarma doğruluğu iyileştirmeleri**

* **Önceden oluşturulmuş makbuz modeli**
  * **İpucu tutarları** Artık ipucu tutarlarını ve diğer el yazısı değerleri ayıklayabilirsiniz.
  * **Satır öğesi çıkarma** Makbuzlardan satır öğesi değerlerini ayıklayabilirsiniz.
  * **Güven değerleri** Çıkarılan her değer için modelin güvenini görüntüleyebilirsiniz.
  * **Çıkarma doğruluğu iyileştirmeleri**

* **Düzen çıkarma** Artık formlarınızdan metin verilerini ve tablo verilerini ayıklamak için Düzen API'sini kullanabilirsiniz.

### <a name="custom-model-api-changes"></a>Özel model API değişiklikleri

Özel modelleriçin tüm API'ler yeniden adlandırıldı ve bazı eşzamanlı yöntemler artık eşzamanlı. Aşağıdaki önemli değişiklikler şunlardır:

* Bir modeli eğitme süreci artık bir senkron. **/custom/models** API çağrısı ile eğitime başlatırsınız. Bu arama, eğitim sonuçlarını döndürmek için **özel/modellere/{modelID}** ye aktarabileceğiniz bir işlem kimliği döndürür.
* Anahtar/değer ayıklama şimdi **/custom/models/{modelID}/analyze** API çağrısı tarafından başlatılır. Bu arama, çıkarma sonuçlarını döndürmek için **özel/modellere/{modelID}/analyzeResults/{resultID}'e** geçebileceğiniz bir işlem kimliği döndürür.
* Tren işlemi için operasyon işlemi şimdi, **Operasyon** Konum üstbilgisinde değişiklik değişiklik altbilgisinde **Location** bulundu.

### <a name="receipt-api-changes"></a>Makbuz API değişiklikleri

Satış makbuzlarını okumak için API'ler yeniden adlandırıldı.

* Makbuz veri ayıklama şimdi **/prebuilt/receipt/analyze** API çağrısı tarafından başlatılır. Bu arama, çıkarma sonuçlarını döndürmek için **/prebuilt/receipt/analyzeResults/{resultID}** adresine geçirebileceğiniz bir işlem kimliği döndürür.

### <a name="output-format-changes"></a>Çıktı biçimi değişiklikleri

Tüm API çağrıları için JSON yanıtları yeni biçimlere sahiptir. Bazı anahtarlar ve değerler eklendi, kaldırıldı veya yeniden adlandırıldı. Geçerli JSON biçimleriörnekleri için hızlı başlangıçlara bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Form Recognizer API'leri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)ile başlamak için hızlı bir [başlangıç](quickstarts/curl-train-extract.md) tamamlayın.