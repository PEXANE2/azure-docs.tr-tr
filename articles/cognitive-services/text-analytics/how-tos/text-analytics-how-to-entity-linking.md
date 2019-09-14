---
title: Metin Analizi API'si ile varlık tanımayı kullanın
titleSuffix: Azure Cognitive Services
description: Metin Analizi REST API kullanarak varlıkların nasıl tanınacağını öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: 642b21624ce3ffc993d5f29a413845044d703fd7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70984278"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Metin Analizi içinde adlandırılmış varlık tanımayı kullanma

[Adlandırılmış varlık tanıma API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) yapılandırılmamış metin alır ve her JSON belgesinde, Web üzerinde daha fazla bilgi için bağlantıları olan, ayırt edici varlıkların bir listesini döndürür (Vikipedi ve Bing).

## <a name="entity-linking-and-named-entity-recognition"></a>Varlık bağlama ve adlandırılmış varlık tanıma

Metin Analizi ' `entities` uç noktası hem adlandırılmış varlık tanıma (ner) hem de varlık bağlamayı destekler.

### <a name="entity-linking"></a>Varlık Bağlama
Varlık bağlama, metinde bulunan bir varlığın kimliğini belirleme ve ayırt etme olanağıdır (örneğin, "Mars" ın dünya olarak mı yoksa War 'nın roman gerçekleşen olarak mı kullanıldığını belirleme). Bu işlem, tanınan varlıkların bağlandığı bir Bilgi Bankası 'nın varolup olmadığını, `entities` uç nokta metin analizi için Bilgi Bankası olarak kullanılır.

### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)
Adlandırılmış varlık tanıma (NER), metinde farklı varlıkları belirleme ve bunları önceden tanımlanmış sınıflar halinde kategorilere ayırma olanağıdır. Desteklenen varlık sınıfları aşağıda listelenmiştir.

Metin Analizi [sürüm 2,1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)' de, hem varlık bağlama hem de adlandırılmış varlık tanıma (ner), çeşitli diller için kullanılabilir. Daha fazla bilgi için [dil desteği](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) makalesine bakın.

### <a name="language-support"></a>Dil desteği

Çeşitli dillerde varlık bağlamayı kullanmak için her dilde ilgili bir Bilgi Bankası kullanılması gerekir. Metin Analizi ' de varlık bağlama için bu, `entities` uç nokta tarafından desteklenen her dilin ilgili dilde ilgili Vises corpa bağlantısı olacağı anlamına gelir. Yapılar boyutu diller arasında farklılık gösterdiğinden, varlık bağlama işlevinin geri çağırma işlemi de değişir.

## <a name="supported-types-for-named-entity-recognition"></a>Adlandırılmış varlık tanıma için desteklenen türler

| Type  | SubType | Örnek |
|:-----------   |:------------- |:---------|
| Kişi        | YOK\*         | "Jeff", "Bill Gates"     |
| Location      | YOK\*         | "Redmond, Washington", "Paris"  |
| Kuruluş  | YOK\*         | “Microsoft”   |
| Miktar      | Sayı        | "6", "altı"     |
| Miktar      | Yüzde    | "%50", "yüzde elli"|
| Miktar      | Sıra       | "2.", "ikinci"     |
| Miktar      | Yaş           | "90 gün eski", "30 yıl eski"    |
| Miktar      | Currency      | “10,99 ABD Doları”     |
| Miktar      | Boyut     | "10 mil", "40 cm"     |
| Miktar      | Sıcaklık   | "32 derece"    |
| DateTime      | YOK\*         | “18:30 4 Şubat 2012”      |
| DateTime      | Date          | “2 Mayıs 2017”, “02.05.2017”   |
| DateTime      | Time          | "08:00", "8:00"  |
| DateTime      | Tarih Aralığı     | “2 - 5 Mayıs Arası”    |
| DateTime      | Saat Aralığı     | “18.00 ve 19.00 Arası”     |
| DateTime      | Duration      | "1 dakika 45 saniye"   |
| DateTime      | Ayarla           | "Her Salı"     |
| URL           | YOK\*         | "https:\//www.Bing.com"    |
| Email         | YOK\*         | "support@contoso.com" |

\*Girişe ve ayıklanan varlıklara bağlı olarak, belirli varlıklar seçeneğini atlayabilir `SubType`.  Listelenen tüm desteklenen varlık türleri yalnızca Ingilizce, Çince-Basitleştirilmiş, Fransızca, Almanca ve Ispanyolca dillerde kullanılabilir.



## <a name="preparation"></a>Hazırlık

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin, dil

Şu anda desteklenen diller için [Bu listeye](../text-analytics-supported-languages.md)bakın.

Belge boyutu belge başına 5.120 karakter altında olmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, varlık bağlama ucuna gönderebileceğiniz içerik çizimidir.

```json
    {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
            },
            {
                "id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1\. adım: İsteği yapısı

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Varlıklar API 'SI](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Azure 'da bir Metin Analizi kaynağı veya bir örneklenmiş [metin analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak anahtar tümceciği ayıklama için HTTP uç noktasını ayarlayın. Dahil `/text/analytics/v2.1/entities`etmeniz gerekir. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

+ Metin Analizi işlemler için [erişim anahtarı](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) eklemek üzere bir istek üst bilgisi ayarlayın.

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2\. adım: İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için isteklerin boyutu ve sayısı hakkında genel bakış konusundaki [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3\. adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Varlık bağlama çıkışının bir örneği aşağıda gösterilmiştir:

```json
    {
        "Documents": [
            {
                "Id": "1",
                "Entities": [
                    {
                        "Name": "Jeff",
                        "Matches": [
                            {
                                "Text": "Jeff",
                                "Offset": 0,
                                "Length": 4
                            }
                        ],
                        "Type": "Person"
                    },
                    {
                        "Name": "three dozen",
                        "Matches": [
                            {
                                "Text": "three dozen",
                                "Offset": 12,
                                "Length": 11
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50",
                        "Matches": [
                            {
                                "Text": "50",
                                "Offset": 49,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50%",
                        "Matches": [
                            {
                                "Text": "50%",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            },
            {
                "Id": "2",
                "Entities": [
                    {
                        "Name": "Great Depression",
                        "Matches": [
                            {
                                "Text": "The Great Depression",
                                "Offset": 0,
                                "Length": 20
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Great Depression",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                        "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                    },
                    {
                        "Name": "1929",
                        "Matches": [
                            {
                                "Text": "1929",
                                "Offset": 30,
                                "Length": 4
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "By 1933",
                        "Matches": [
                            {
                                "Text": "By 1933",
                                "Offset": 36,
                                "Length": 7
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "Gross domestic product",
                        "Matches": [
                            {
                                "Text": "GDP",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Gross domestic product",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                        "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                    },
                    {
                        "Name": "United States",
                        "Matches": [
                            {
                                "Text": "America",
                                "Offset": 56,
                                "Length": 7
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "United States",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                        "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                        "Type": "Location"
                    },
                    {
                        "Name": "25",
                        "Matches": [
                            {
                                "Text": "25",
                                "Offset": 72,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "25%",
                        "Matches": [
                            {
                                "Text": "25%",
                                "Offset": 72,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            }
        ],
        "Errors": []
    }
```

## <a name="summary"></a>Özet

Bu makalede, bilişsel hizmetler 'deki Metin Analizi kullanarak varlık bağlama kavramlarını ve iş akışını öğrendiniz. Özet:

+ [Varlıklar API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak `/entities` uç noktasına yapılır.
+ Bağlı varlıklardan oluşan yanıt çıkışı (her belge KIMLIĞI için güven puanları, uzaklıklar ve Web bağlantıları dahil) herhangi bir uygulamada kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Metin Analizine genel bakış](../overview.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
* [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712)
