---
title: Text Analytics REST API kullanarak anahtar tümcecik çıkarma
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'den Text Analytics REST API'sini kullanarak temel tümcecikleri nasıl ayıklayabilirsiniz?
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/29/2019
ms.author: raymondl
ms.openlocfilehash: ec5ff756d7e732430675676868bc754627a2a4a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72429033"
---
# <a name="example-how-to-extract-key-phrases-using-text-analytics"></a>Örnek: Metin Analizi'ni kullanarak anahtar tümcecikler nasıl ayıklanır?

[Anahtar İfade Ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), yapılandırılmamış metni değerlendirir ve her bir JSON belgesi için bir anahtar ifade listesi döndürür.

Bir belge koleksiyonundaki ana noktaları hızlı şekilde belirlemeniz gerekiyorsa bu özellik kullanışlıdır. Örneğin, "The food was delicious and there were wonderful staff" (Yemek lezizdi ve müthiş personel hizmeti vardı) giriş metni olduğunda hizmet, "food" (yemek) ve "wonderful staff" (müthiş personel) ana konuşma noktalarını döndürür.

Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

> [!TIP]
> Text Analytics ayrıca anahtar tümcecik çıkarma için Linux tabanlı docker kapsayıcı görüntüsü sağlar, böylece [Text Analytics kapsayıcısını](text-analytics-how-to-install-containers.md) verilerinize yakın olarak yükleyebilir ve çalıştırabilirsiniz.

## <a name="preparation"></a>Hazırlık

Anahtar tümcecik ayıklama, üzerinde çalışmak için daha büyük miktarda metin verdiğinizde en iyi şekilde çalışır. Bu, daha küçük metin miktarlarında daha iyi performans gösteren duygu analizinin tam tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

Bu formatta JSON belgeleriniz olmalıdır: Kimlik, metin, dil

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

## <a name="step-1-structure-the-request"></a>1. Adım: İsteği yapılandırma

İstek tanımı hakkında bilgi için Metin [Analizi API'sini nasıl arayacağız](text-analytics-how-to-call-api.md)bölümüne bakın. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Anahtar Tümcecikler API.](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)

+ Azure'da bir Text Analytics kaynağı veya anında metin [analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak anahtar tümcecik ayıklama için HTTP bitiş noktasını ayarlayın. URL'ye `/text/analytics/v2.1/keyPhrases` eklemeniz gerekir. Örneğin: `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v2.1/keyPhrases`.

+ Metin Analizi işlemleri için [erişim anahtarını](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) içerecek şekilde bir istek üstbilgisini ayarlayın.

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)**API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2. Adım: İsteği gönderme

İstek alındığında analiz gerçekleştirilir. Dakikada veya saniyede gönderebileceğiniz isteklerin boyutu ve sayısı hakkında bilgi için genel bakışta [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3. Adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür. Döndürülen anahtar tümceciklerin sırası model tarafından dahili olarak belirlenir.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Anahtar tümcecik ayıklama için çıktı örneği burada gösterilmiştir:

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

Belirtildiği gibi, çözümleyici gerekli olmayan sözcükleri bulur ve atar ve bir cümlenin öznesi veya nesnesi gibi görünen tek terimleri veya tümcecikleri tutar.

## <a name="summary"></a>Özet

Bu makalede, Bilişsel Hizmetlerde Metin Analizi'ni kullanarak anahtar cümle çıkarma kavramlarını ve iş akışını öğrendiniz. Özet:

+ [Anahtar ifade ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), seçili dillerde kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir kimlik, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak `/keyphrases` uç noktasına yapılır.
+ Her belge kimliği için anahtar sözcükler ve tümceciklerden oluşan yanıt çıktısı, Microsoft Office Excel ve Power BI de dahil olmak üzere JSON'u kabul eden herhangi bir uygulamaya birkaç ını adlandırmak için aktarılabilir.

## <a name="see-also"></a>Ayrıca bkz.

 [Metin Analizine genel bakış](../overview.md) [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6)
