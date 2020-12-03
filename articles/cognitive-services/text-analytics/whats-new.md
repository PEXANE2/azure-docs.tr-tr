---
title: Metin Analizi API'si yenilikler
titleSuffix: Text Analytics - Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler Metin Analizi yeni sürümler ve özellikler hakkında bilgi verilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: aahi
ms.custom: references_regions
ms.openlocfilehash: 157bef31b375db54f294284ca0a5d97cd7035b17
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96558972"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Metin Analizi API’sindeki yenilikler

Metin Analizi API'si, sürekli olarak güncelleştirilir. Son geliştirmelerin güncel kalması için, bu makalede yeni yayınlar ve özellikler hakkında bilgi verilmektedir.

## <a name="december-2020"></a>Aralık 2020

* Metin Analizi API'si için [güncelleştirilmiş fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) ayrıntıları

## <a name="november-2020"></a>Kasım 2020

* NER, PII ve anahtar tümceciği ayıklama işlemleri için toplu işlemeyi destekleyen yeni zaman uyumsuz [analiz API 'si](how-tos/text-analytics-how-to-call-api.md?tabs=analyze)için Metin Analizi API'si v 3.1-Preview. 3 içeren [Yeni bir uç nokta](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Analyze) .
* Toplu işleme desteğiyle [sistem durumu](how-tos/text-analytics-for-health.md) barındırılan API için yeni zaman uyumsuz Metin Analizi için Metin Analizi API'si v 3.1-Preview. 3 ile [Yeni bir uç nokta](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Health) .
* Yukarıda listelenen yeni özelliklerin her ikisi de yalnızca şu bölgelerde kullanılabilir: `West US 2` , `East US 2` , `Central US` , `North Europe` ve `West Europe` bölgeleri.
* Portekizce (Brezilya) `pt-BR` , model sürümü ile başlayarak [yaklaşım Analizi](how-tos/text-analytics-how-to-sentiment-analysis.md) v3. x sürümünde desteklenmektedir `2020-04-01` . `pt-PT`Portekizce için mevcut desteğe ekler.
* Zaman uyumsuz analiz ve sistem durumu işlemleri için Metin Analizi içeren güncelleştirilmiş istemci kitaplıkları. GitHub 'da örnekleri bulabilirsiniz:

    * [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
    * [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
    * [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)


> [!div class="nextstepaction"]
> [Metin Analizi API'si v 3.1-önizleme hakkında daha fazla bilgi edinin. 3](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Languages)

## <a name="october-2020"></a>Ekim 2020

* Model sürümünden itibaren Yaklaşım Analizi v3. x için Hintçe desteği `2020-04-01` . 
* `2020-09-01`Daha fazla dil algılama ve doğruluk iyileştirmeleri ekleyen v3/diller uç noktası için model sürümü.
* Orta Hindistan ve BAE Kuzey v3 kullanılabilirliği.

## <a name="september-2020"></a>Eylül 2020

### <a name="general-api-updates"></a>Genel API güncelleştirmeleri

* Aşağıdaki adlandırılmış varlık tanıma v3 uç noktalarında güncelleştirmeleri desteklemek üzere Metin Analizi v 3.1 Genel önizlemesi için yeni bir URL yayını: 
    * `/pii` uç nokta artık, `redactedText` Yanıt JSON öğesinde, giriş metninde ALGıLANAN PII varlıklarının bu varlıkların her bir karakteriyle değiştirildiği yeni özelliği içerir `*` .
    * `/linking` uç nokta artık `bingID` bağlantılı varlıklar için Response JSON içindeki özelliği içerir.
* Aşağıdaki Metin Analizi Preview API uç noktaları 4 Eylül 2020 ' de kullanımdan kaldırıldı:
    * v 2.1-Önizleme
    * v3.0-preview
    * v 3.0-Önizleme. 1
    
> [!div class="nextstepaction"]
> [Metin Analizi API'si v 3.1-önizleme hakkında daha fazla bilgi edinin. 2](quickstarts/text-analytics-sdk.md)

### <a name="text-analytics-for-health-container-updates"></a>Sistem durumu kapsayıcısı güncelleştirmeleri için Metin Analizi

Aşağıdaki güncelleştirmeler, yalnızca sistem durumu kapsayıcısı için Metin Analizi Eylül sürümüne özgüdür.
* `1.1.013530001-amd64-preview`Containerpreview deposuna yeni model sürümü olan etiketli yeni bir kapsayıcı görüntüsü `2020-09-03` yayımlandı. 
* Bu model sürümü, varlık tanıma, kısaltma algılama ve gecikme süresi geliştirmeleriyle ilgili iyileştirmeler sağlar.

> [!div class="nextstepaction"]
> [Sistem durumu Metin Analizi hakkında daha fazla bilgi edinin](how-tos/text-analytics-for-health.md)

## <a name="august-2020"></a>Ağustos 2020

### <a name="general-api-updates"></a>Genel API güncelleştirmeleri

* `2020-07-01`V3 için model sürümü `/keyphrases` ve şunları `/pii` `/languages` ekleyen uç noktalar:
    * Adlandırılmış varlık tanıma için ek kamu ve ülkeye özgü [varlık kategorileri](named-entity-types.md?tabs=personal) .
    * Yaklaşım Analizi v3 'de Norveç ve Türkçe desteği.
* Artık yayımlanan [veri sınırlarını](concepts/data-limits.md)aşan v3 API istekleri IÇIN bir HTTP 400 hatası döndürülür. 
* Bir sapmayı döndüren uç noktalar artık `stringIndexType` , döndürülen `offset` ve `length` değerleri desteklenen bir [dize Dizin düzeniyle](concepts/text-offsets.md)eşleşecek şekilde ayarlayan isteğe bağlı parametreyi destekler.

### <a name="text-analytics-for-health-container-updates"></a>Sistem durumu kapsayıcısı güncelleştirmeleri için Metin Analizi

Aşağıdaki güncelleştirmeler, yalnızca sistem durumu kapsayıcısı için Metin Analizi Ağustos sürümüne özgüdür.

* Sistem durumu için yeni model sürümü Metin Analizi: `2020-07-24`
* Sistem durumu istekleri için Metin Analizi göndermek üzere yeni URL: `http://<serverURL>:5000/text/analytics/v3.2-preview.1/entities/health` (Bu yeni kapsayıcı görüntüsünde bulunan demo Web uygulamasını kullanmak için bir tarayıcı önbelleği temizleme 'nin gerekli olacağını lütfen unutmayın)

JSON yanıtında aşağıdaki özellikler değiştirildi:

* `type`, `category` olarak yeniden adlandırıldı 
* `score`, `confidenceScore` olarak yeniden adlandırıldı
* `category`JSON çıktısının alanındaki varlıklar artık Pascal durumunda. Aşağıdaki varlıklar yeniden adlandırıldı:
    * `EXAMINATION_RELATION` , olarak yeniden adlandırıldı `RelationalOperator` .
    * `EXAMINATION_UNIT` , olarak yeniden adlandırıldı `MeasurementUnit` .
    * `EXAMINATION_VALUE` , olarak yeniden adlandırıldı `MeasurementValue` .
    * `ROUTE_OR_MODE` yeniden adlandırıldı `MedicationRoute` .
    * İlişkisel varlık `ROUTE_OR_MODE_OF_MEDICATION` olarak yeniden adlandırıldı `RouteOfMedication` .

Aşağıdaki varlıklar eklendi:

* HI
    * `AdministrativeEvent`
    * `CareEnvironment`
    * `HealthcareProfession`
    * `MedicationForm` 

* İlişki ayıklama
    * `DirectionOfCondition`
    * `DirectionOfExamination`
    * `DirectionOfTreatment`

> [!div class="nextstepaction"]
> [Sistem durumu kapsayıcısı için Metin Analizi hakkında daha fazla bilgi edinin](how-tos/text-analytics-for-health.md)

## <a name="july-2020"></a>Temmuz 2020 

### <a name="text-analytics-for-health-container---public-gated-preview"></a>Sistem durumu kapsayıcısı için Metin Analizi-genel geçişli Önizleme

Sistem durumu kapsayıcısı için Metin Analizi, artık genel geçitli önizlemededir ve bu, örneğin: hasta Intake formları, Doktor notları, araştırma kağıtları ve ücretsiz Özet özetleri gibi klinik belgelerde yapılandırılmamış Ingilizce dilindeki metinden bilgi ayıklamanıza olanak tanır. Şu anda, sistem durumu kapsayıcısı kullanımı için Metin Analizi faturalandırılmaz.

Kapsayıcı aşağıdaki özellikleri sunar:

* Adlandırılmış Varlık Tanıma
* İlişki ayıklama
* Varlık bağlama
* Negatif

## <a name="may-2020"></a>Mayıs 2020

### <a name="text-analytics-api-v3-general-availability"></a>Metin Analizi API'si v3 genel kullanılabilirliği

Metin analizi API v3, artık aşağıdaki güncelleştirmelerle genel kullanıma sunulmuştur:

* Model sürümü `2020-04-01`
* Her özellik için yeni [veri limitleri](concepts/data-limits.md)
* [Yaklaşım Analizi (SA) v3](how-tos/text-analytics-how-to-sentiment-analysis.md) için [dil desteği](language-support.md) güncelleştirildi
* Varlık bağlama için ayrı uç nokta 
* [Adlandırılmış varlık tanıma (ner) v3](how-tos/text-analytics-how-to-entity-linking.md)içindeki yeni "adres" varlık kategorisi.
* NER v3 içindeki yeni alt kategoriler:
   * Konum-coğrafi
   * Konum-yapısal
   * Kuruluş-stok alışverişi
   * Kuruluş-tıp
   * Kuruluş-spor
   * Olay-kültürel
   * Olay-doğal
   * Olay-spor

JSON yanıtında aşağıdaki özellikler eklendi:
   * `SentenceText` Yaklaşım Analizi
   * `Warnings` Her belge için 

JSON yanıtında aşağıdaki özelliklerin adları, uygun yerlerde değiştirilmiştir:

* `score`, `confidenceScore` olarak yeniden adlandırıldı
    * `confidenceScore` iki ondalık duyarlığa sahiptir. 
* `type`, `category` olarak yeniden adlandırıldı
* `subtype`, `subcategory` olarak yeniden adlandırıldı

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]

> [!div class="nextstepaction"]
> [Metin Analizi API'si v3 hakkında daha fazla bilgi edinin](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages)

### <a name="text-analytics-api-v31-public-preview"></a>Metin Analizi API'si v 3.1 genel önizleme
   * Yeni Yaklaşım Analizi özelliği- [görüşme madenciliği](how-tos/text-analytics-how-to-sentiment-analysis.md#opinion-mining)
   * `PII`Korunan sağlık bilgileri () için yeni kişisel () etki alanı filtresi `PHI` .

> [!div class="nextstepaction"]
> [Metin Analizi API'si v 3.1 Preview hakkında daha fazla bilgi edinin](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/Languages)

## <a name="february-2020"></a>Şubat 2020

### <a name="sdk-support-for-text-analytics-api-v3-public-preview"></a>Metin Analizi API'si v3 genel önizleme için SDK desteği

[Birleşik Azure SDK sürümünün](https://techcommunity.microsoft.com/t5/azure-sdk/january-2020-unified-azure-sdk-release/ba-p/1097290)bir parçası olarak, metın ANALIZI API'SI v3 SDK artık aşağıdaki programlama dilleri için genel önizleme olarak sunulmaktadır:
   * [C#](./quickstarts/text-analytics-sdk.md?pivots=programming-language-csharp&tabs=version-3)
   * [Python](./quickstarts/text-analytics-sdk.md?pivots=programming-language-python&tabs=version-3)
   * [JavaScript (Node.js)](./quickstarts/text-analytics-sdk.md?pivots=programming-language-javascript&tabs=version-3)
   * [Java](./quickstarts/text-analytics-sdk.md?pivots=programming-language-java&tabs=version-3)
   
> [!div class="nextstepaction"]
> [Metin Analizi API'si v3 SDK hakkında daha fazla bilgi edinin](./quickstarts/text-analytics-sdk.md?tabs=version-3)

### <a name="named-entity-recognition-v3-public-preview"></a>Adlandırılmış varlık tanıma v3 genel önizlemesi

Ek varlık türleri artık, metinde bulunan genel ve kişisel bilgi varlıklarının algılanmasını genişlettiğimiz için adlandırılmış varlık tanıma (NER) v3 genel önizleme hizmetinde kullanılabilir. Bu güncelleştirme aşağıdakileri içeren [model sürümünü](concepts/model-versioning.md) tanıtır `2020-02-01` :

* Aşağıdaki genel varlık türlerini tanıma (Yalnızca Ingilizce):
    * Kişilik türü
    * Ürün
    * Olay
    * Konum altında alt tür olarak geopolitik varlık (GPE)
    * İmde

* Aşağıdaki kişisel bilgi varlığı türlerini tanıma (Yalnızca Ingilizce):
    * Kişi
    * Kuruluş
    * Miktar altında alt tür olarak yaş
    * DateTime altındaki alt tür olarak Tarih
    * E-posta 
    * Telefon numarası (yalnızca ABD)
    * URL
    * IP Adresi

### <a name="october-2019"></a>Ekim 2019

#### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

* Kişisel bilgi varlık türlerini (Yalnızca Ingilizce) tanıma için [Yeni bir uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionPii)

* [Varlık tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionGeneral) ve [varlık bağlama](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesLinking)için ayrı uç noktalar.

* [Model sürümü](concepts/model-versioning.md) şunları `2019-10-01` içerir:
    * Metinde bulunan varlıkların genişletilmiş algılanması ve kategorilere ayrılması. 
    * Aşağıdaki yeni varlık türlerinin tanınması:
        * Telefon numarası
        * IP Adresi

Varlık bağlama, Ingilizce ve Ispanyolca 'yi destekler. NER dil desteği varlık türüne göre farklılık gösterir.

#### <a name="sentiment-analysis-v3-public-preview"></a>Yaklaşım Analizi v3 genel önizleme

* Yaklaşımı çözümlemek için [Yeni bir uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/Sentiment) .
* [Model sürümü](concepts/model-versioning.md) şunları `2019-10-01` içerir:

    * API 'nin metin kategorisi ve Puanlama açısından doğruluk ve ayrıntılarda önemli geliştirmeler.
    * Metindeki farklı sentiler için otomatik etiketleme.
    * Bir belge ve tümce düzeyinde yaklaşım Analizi ve çıktısı. 

İngilizce (), Japonca (), Çince Basitleştirilmiş (), Çince Geleneksel (), Fransızca (), İtalyanca (), İspanyolca (), Felemenkçe () `en` `ja` `zh-Hans` `zh-Hant` `fr` `it` `es` `nl` , Portekizce () ve `pt` Almanca ( `de` ) destekler ve şu bölgelerde kullanılabilir: `Australia East` , `Central Canada` , `Central US` , `East Asia` ,, `East US` `East US 2` , `North Europe` `Southeast Asia` `South Central US` `UK South` `West Europe` `West US 2` ,,,,, ve. 

> [!div class="nextstepaction"]
> [Yaklaşım Analizi v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi API'si nedir?](overview.md)  
* [Örnek kullanıcı senaryoları](text-analytics-user-scenarios.md)
* [Yaklaşım Analizi](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Dil algılama](how-tos/text-analytics-how-to-language-detection.md)
* [Varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md)
* [Anahtar ifade ayıklama](how-tos/text-analytics-how-to-keyword-extraction.md)
