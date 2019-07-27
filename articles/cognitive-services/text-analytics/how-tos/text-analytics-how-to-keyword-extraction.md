---
title: Metin Analizi REST API kullanarak anahtar tümceciği ayıklama
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'den Metin Analizi REST API kullanarak anahtar tümceleri ayıklama.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 06/05/2019
ms.author: raymondl
ms.openlocfilehash: 58bfb889662a58aa02286c41a2e242e6a0e9a75c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562636"
---
# <a name="example-how-to-extract-key-phrases-using-text-analytics"></a>Örnek: Metin Analizi’ni kullanarak anahtar ifadeleri ayıklama

[Anahtar İfade Ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), yapılandırılmamış metni değerlendirir ve her bir JSON belgesi için bir anahtar ifade listesi döndürür. 

Bir belge koleksiyonundaki ana noktaları hızlı şekilde belirlemeniz gerekiyorsa bu özellik kullanışlıdır. Örneğin, "The food was delicious and there were wonderful staff" (Yemek lezizdi ve müthiş personel hizmeti vardı) giriş metni olduğunda hizmet, "food" (yemek) ve "wonderful staff" (müthiş personel) ana konuşma noktalarını döndürür.

Daha fazla bilgi için [desteklenen diller](../text-analytics-supported-languages.md) makalesine bakın. 

> [!TIP]
> Metin Analizi Ayrıca, anahtar tümceciği ayıklama için Linux tabanlı bir Docker kapsayıcı görüntüsü sağlar, böylece Metin Analizi kapsayıcısını verilerinize yakın şekilde [yükleyip çalıştırabilirsiniz](text-analytics-how-to-install-containers.md) .

## <a name="preparation"></a>Hazırlık

Anahtar tümceciği ayıklama, üzerinde çalışmak için büyük miktarda metin verdiğinizde en iyi şekilde çalışır. Bu, daha küçük miktarlarda metin üzerinde daha iyi sonuç veren yaklaşım analizinden daha tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

JSON belgeleri kimlik, metin, dil biçiminde olmalıdır.

Belge boyutu belge başına 5.120 karakter altında olmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, anahtar ifade ayıklaması için gönderebileceğiniz içeriğin bir gösterimidir.

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

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Anahtar tümceleri API 'SI](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)

+ Azure 'da bir Metin Analizi kaynağı veya bir örneklenmiş [metin analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak anahtar tümceciği ayıklama için HTTP uç noktasını ayarlayın. `/keyPhrases` kaynağını içermelidir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için bkz. [Uç noktaları ve erişim anahtarlarını bulma](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2\. adım: İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için isteklerin boyutu ve sayısı hakkında genel bakış konusundaki [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3\. adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Anahtar tümceciği ayıklama çıkışının bir örneği burada gösterilmektedir:

```json
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
```

Belirtildiği gibi çözümleyici, gerekli olmayan sözcükleri bulup atar ve bir cümlenin konusu veya nesnesi olabilecek tekli terimleri veya ifadeleri tutar. 

## <a name="summary"></a>Özet

Bu makalede, Bilişsel Hizmetler’de Metin Analizi’ni kullanarak anahtar ifade ayıklaması için kavramları ve iş akışını öğrendiniz. Özet:

+ [Anahtar ifade ayıklama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), seçili dillerde kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir kimlik, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) kullanılarak `/keyphrases` uç noktasına yapılır.
+ Her belge kimliği için anahtar sözcüklerden ve ifadelerden oluşan yanıt çıktısı, Excel ve Power BI da dahil olmak üzere JSON kabul eden tüm uygulamalarda akışa alınabilir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API’si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6)
