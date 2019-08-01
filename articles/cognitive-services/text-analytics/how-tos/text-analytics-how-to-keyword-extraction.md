---
title: Metin Analizi REST API kullanarak anahtar tümceciği ayıklama
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'deki Metin Analizi REST API kullanarak anahtar tümceleri ayıklama.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/29/2019
ms.author: raymondl
ms.openlocfilehash: 1d94cff3eb3299692fc4172f5bb5211532ef1002
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697832"
---
# <a name="example-how-to-extract-key-phrases-using-text-analytics"></a>Örnek: Metin Analizi’ni kullanarak anahtar ifadeleri ayıklama

[Anahtar İfade Ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), yapılandırılmamış metni değerlendirir ve her bir JSON belgesi için bir anahtar ifade listesi döndürür.

Bir belge koleksiyonundaki ana noktaları hızlı şekilde belirlemeniz gerekiyorsa bu özellik kullanışlıdır. Örneğin, "The food was delicious and there were wonderful staff" (Yemek lezizdi ve müthiş personel hizmeti vardı) giriş metni olduğunda hizmet, "food" (yemek) ve "wonderful staff" (müthiş personel) ana konuşma noktalarını döndürür.

Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

> [!TIP]
> Metin Analizi Ayrıca, anahtar tümceciği ayıklama için Linux tabanlı bir Docker kapsayıcı görüntüsü sağlar, böylece Metin Analizi kapsayıcısını verilerinize yakın şekilde [yükleyip çalıştırabilirsiniz](text-analytics-how-to-install-containers.md) .

## <a name="preparation"></a>Hazırlık

Anahtar tümceciği ayıklama, üzerinde çalışmak için büyük miktarda metin verdiğinizde en iyi şekilde çalışır. Bu, daha küçük miktarlarda metin üzerinde daha iyi sonuç veren yaklaşım analizinden daha tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin, dil

Belge boyutu belge başına 5.120 veya daha az karakter olmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, anahtar ifade ayıklaması için gönderebileceğiniz içeriğin bir gösterimidir.

```json
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1\. adım: İsteği yapısı

İstek tanımı hakkında bilgi için bkz. [nasıl çağrılacağını Metin Analizi API'si](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Anahtar tümceleri API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6).

+ Azure 'da bir Metin Analizi kaynağı veya bir örneklenmiş [metin analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak anahtar tümceciği ayıklama için HTTP uç noktasını ayarlayın. `/keyPhrases` Kaynağı içermesi gerekir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`.

+ Metin Analizi işlemler için [erişim anahtarı](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) eklemek üzere bir istek üst bilgisi ayarlayın.

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2\. adım: İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakikada veya saniye başına göndereceğiniz isteklerin boyutu ve sayısı hakkında daha fazla bilgi için genel bakış konusunun [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3\. adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Anahtar tümceciği ayıklama çıkışının bir örneği burada gösterilmektedir:

```json
    {
        "documents": [
            {
                "keyPhrases": [
                    "year",
                    "trail",
                    "trip",
                    "views"
                ],
                "id": "1"
            },
            {
                "keyPhrases": [
                    "marked trails",
                    "Worst hike",
                    "goners"
                ],
                "id": "2"
            },
            {
                "keyPhrases": [
                    "trail",
                    "small children",
                    "family"
                ],
                "id": "3"
            },
            {
                "keyPhrases": [
                    "spectacular views",
                    "trail",
                    "area"
                ],
                "id": "4"
            },
            {
                "keyPhrases": [
                    "places",
                    "beautiful views",
                    "favorite trail"
                ],
                "id": "5"
            }
        ],
        "errors": []
    }
```

Belirtildiği gibi, çözümleyici, önemli olmayan sözcükleri bulur ve atar ve bir cümlenin konusu veya nesnesi olarak görünen tek terimleri veya tümceleri tutar.

## <a name="summary"></a>Özet

Bu makalede bilişsel hizmetler 'de Metin Analizi kullanarak anahtar tümceciği ayıklama için kavramları ve iş akışını öğrendiniz. Özet:

+ [Anahtar ifade ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), seçili dillerde kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak `/keyphrases` uç noktasına yapılır.
+ Her belge KIMLIĞI için anahtar sözcüklerden ve tümceciklerden oluşan yanıt çıkışı, Microsoft Office Excel ve Power BI dahil olmak üzere, birkaç kez ad vermek üzere JSON kabul eden herhangi bir uygulamaya akışla eklenebilir.

## <a name="see-also"></a>Ayrıca bkz.

 [Metin analizi genel bakış](../overview.md) [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API’si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6)
