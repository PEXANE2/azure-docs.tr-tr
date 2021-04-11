---
title: Okuma API 'sini çağırma
titleSuffix: Azure Cognitive Services
description: Okuma API 'sini çağırma ve davranışını ayrıntılı olarak yapılandırma hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: pafarley
ms.openlocfilehash: c1f610a5fb3db41091ddc4d0d921bf1a0cd2cf34
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2021
ms.locfileid: "107012237"
---
# <a name="call-the-read-api"></a>Okuma API 'sini çağırma

Bu kılavuzda, görüntülerden metin ayıklamak için Read API 'sini çağırmayı öğreneceksiniz. Bu API 'nin davranışını gereksinimlerinizi karşılayacak şekilde yapılandırabileceğiniz farklı yollar öğreneceksiniz.

Bu kılavuzda, bir <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> görüntü işleme kaynağı oluşturma </a> ve bir abonelik anahtarı ve uç nokta URL 'si edindiğinizi bir görüntü işleme kaynağı oluşturmuş olduğunuz varsayılmaktadır. Yapmadıysanız, başlamak için bir [hızlı başlangıç](../quickstarts-sdk/client-library.md) izleyin.

## <a name="submit-data-to-the-service"></a>Verileri hizmete gönder

Okuma API 'sinin [okuma çağrısı](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) , bir görüntüyü ya da PDF belgesini girdi olarak alır ve zaman uyumsuz olarak ayıklar.

`https://{endpoint}/vision/v3.2-preview.3/read/analyze[?language][&pages][&readingOrder]`

Çağrı, adlı bir yanıt üst bilgisi alanı ile döndürülür `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak Işlem kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Örnek değer |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Faturalandırma** 
>
> [Görüntü işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) sayfası, okuma için fiyatlandırma katmanını içerir. Çözümlenen her resim veya sayfa bir işlemdir. İşlemi 100 sayfa içeren bir PDF veya TIFF belgesiyle çağırırsanız, okuma işlemi bunu 100 işlem olarak sayar ve 100 için faturalandırılırsınız. İşleme 50 çağrı yaptıysanız ve her çağrı 100 sayfalı bir belge gönderdiyseniz, 50 X 100 = 5000 işlemleri için faturalandırılırsınız.

## <a name="determine-how-to-process-the-data"></a>Verilerin nasıl işleyeceğini belirleme

### <a name="language-specification"></a>Dil belirtimi

[Okuma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) çağrısının dil için isteğe bağlı bir istek parametresi vardır. Okuma otomatik dil tanımlama ve çok dilli belgelerini destekler, bu nedenle belgeyi belirli bir dil olarak işlenmek üzere zorlamak istiyorsanız yalnızca bir dil kodu sağlayın.

### <a name="natural-reading-order-output-latin-languages-only"></a>Doğal okuma sırası çıkışı (yalnızca Latin dilleri)
[Okuma 3,2 Önizleme API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)ile, metin çizgilerinin hangi sırada sorgu parametresiyle çıkış olduğunu belirtebilirsiniz `readingOrder` . `natural`Aşağıdaki örnekte gösterildiği gibi, daha kolay okunabilir bir okuma sırası çıkışı için kullanın. Bu özellik yalnızca Latin dilleri için desteklenir.

:::image border type="content" source="../Images/ocr-reading-order-example.png" alt-text="OCR okuma düzeni örneği":::



### <a name="select-pages-or-page-ranges-for-text-extraction"></a>Metin ayıklama için sayfa (ler) veya sayfa aralıklarını seçin
[Okuma 3,2 Önizleme API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)ile, büyük çok sayfalı belgeler için, `pages` yalnızca bu sayfalardan metin Ayıklanacak sayfa numaralarını veya sayfa aralıklarını belirtmek üzere sorgu parametresini kullanın. Aşağıdaki örnek, her iki durumda da tüm sayfalar (1-10) ve seçili 3-6 Sayfalar için metin ayıklanarak 10 sayfalı bir belge gösterir.

:::image border type="content" source="../Images/ocr-select-pages.png" alt-text="Seçili sayfa çıkışı":::

## <a name="get-results-from-the-service"></a>Hizmetten sonuçları al

İkinci adım [okuma sonuçları al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750) işlemini çağırmalıdır. Bu işlem, okuma işlemi tarafından oluşturulan işlem KIMLIĞINI giriş olarak alır. 

`https://{endpoint}/vision/v3.2-preview.3/read/analyzeResults/{operationId}`

Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. 

|Değer | Anlamı |
|:-----|:----|
| `notStarted`| İşlem başlatılmamış. |
| `running`| İşlem işleniyor. |
| `failed`| İşlem başarısız oldu. |
| `succeeded`| İşlem başarılı oldu. |

**Başarılı** değerle döndürülünceye kadar bu işlemi tekrarlayarak çağırın. Saniye başına istek (RPS) oranını aşmamak için 1 ile 2 saniye arasında bir Aralık kullanın.

> [!NOTE]
> Ücretsiz katman, istek hızını dakika başına 20 çağrı ile sınırlandırır. Ücretli katman, istek üzerine artalabilecek saniye başına 10 istek (RPS) sağlar. Saniye başına istek (RPS) oranı istemek için Azure destek kanalı 'nı veya hesap takımınızı kullanın.

**Durum** alanı `succeeded` değeri olduğunda JSON yanıtı, görüntinizdeki veya belgenizdeki ayıklanan metin içeriğini içerir. JSON yanıtı, tanınan sözcüklerin orijinal satır gruplandırmaları saklar. Ayıklanan metin çizgilerini ve bunların sınırlayıcı kutu koordinatlarını içerir. Her metin satırı, tüm ayıklanan kelimeleri ve bunların koordinatlarını ve güvenirlik puanlarını içerir.

> [!NOTE]
> İşleme gönderilen veriler `Read` geçici olarak şifrelenir ve kısa bir süre için geri kalanında depolanır ve sonra silinir. Bu, uygulamalarınızın ayıklanan metni hizmet yanıtının bir parçası olarak almasına olanak sağlar.

### <a name="sample-json-output"></a>Örnek JSON çıkışı

Başarılı bir JSON yanıtı örneğine bakın:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### <a name="handwritten-classification-for-text-lines-latin-languages-only"></a>Metin satırları için el ile sınıflandırma (yalnızca Latin dilleri)
[Okuma 3,2 PREVIEW API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) yanıtı, her metin satırının el yazısı stili olup olmadığı sınıflandırıp bir güven puanı ile birlikte yer alır. Bu özellik yalnızca Latin dilleri için desteklenir. Aşağıdaki örnek görüntüdeki metin için el yazısı sınıflandırmasını gösterir.

:::image border type="content" source="../Images/ocr-handwriting-classification.png" alt-text="OCR el yazısı sınıflandırması örneği":::

## <a name="next-steps"></a>Sonraki adımlar

REST API denemek için [Read API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)gidin.