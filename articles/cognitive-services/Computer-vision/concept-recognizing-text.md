---
title: Optik karakter tanıma (OCR)-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API'si kullanılarak yazdırılmış ve el yazısı metinle birlikte, resimlerin optik karakter tanıma (OCR) ile ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: e0247560afa8229f4fa5c25ec7dfbbca4f7defb2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102486107"
---
# <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Azure Görüntü İşleme API'si, görüntülerden yazdırılmış veya el yazısı metin çıkaran optik karakter tanıma (OCR) özelliklerini içerir. Belge-faturalar, senetler, finansal raporlar, makaleler ve daha fazlasını içeren, lisans levhalarının veya seri numaralarının bulunduğu kapsayıcıların fotoğraflarından metin ayıklayabilirsiniz.

## <a name="read-api"></a>API 'YI oku 

Görüntü İşleme [okuma API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) 'Si, Azure 'un en son OCR teknolojisidir[(yenilikler),](./whats-new.md)yazdırılan metni (bazı dillerde), el yazısı metin (yalnızca İngilizce), rakamları ve görüntü ve çok sayfalı PDF belgelerinden gelen para birimi sembollerini ayıklar. Metin açısından kalın görüntülerden ve çok sayfalı PDF belgelerinden karışık dillerle metin ayıklamak en iyi duruma getirilmiştir. Aynı görüntüde veya belgede hem yazdırılmış hem de el yazısı metnin algılanmasının aynısını destekler.

![OCR, resimleri ve belgeleri ayıklanan metinle yapısal çıktıya dönüştürür](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Giriş gereksinimleri
**Okuma** çağrısı görüntüleri ve belgeleri giriş olarak alır. Bunlar aşağıdaki gereksinimlere sahiptir:

* Desteklenen dosya biçimleri: JPEG, PNG, BMP, PDF ve TIFF
* PDF ve TIFF dosyaları için en fazla 2000 sayfa (ücretsiz katman için yalnızca ilk iki sayfa) işlenir.
* Dosya boyutu 50 MB 'tan az (ücretsiz katman için 4 MB) ve en az 50 x 50 piksel ve en fazla 10000 x 10000 piksel boyutunda olmalıdır. 
* PDF boyutları, yasal veya A3 kağıt boyutlarına ve daha küçük boyutlara karşılık gelen en fazla 17 x 17 inç olmalıdır.

> [!NOTE]
> **Dil girişi** 
>
> [Okuma çağrısının](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) dil için isteğe bağlı bir istek parametresi vardır. Okuma otomatik dil tanımlama ve çok dilli belgelerini destekler, bu nedenle belgeyi belirli bir dil olarak işlenmek üzere zorlamak istiyorsanız yalnızca bir dil kodu sağlayın.

## <a name="ocr-demo-examples"></a>OCR tanıtımı (örnekler)

![OCR gösterileri](./Images/ocr-demo.gif)

## <a name="step-1-the-read-operation"></a>1. Adım: okuma işlemi

Okuma API 'sinin [okuma çağrısı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) , bir görüntüyü ya da PDF belgesini girdi olarak alır ve zaman uyumsuz olarak ayıklar. Çağrı, adlı bir yanıt üst bilgisi alanı ile döndürülür `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak Işlem kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Sonuç URL 'SI |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Faturalandırma** 
>
> [Görüntü işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) sayfası, okuma için fiyatlandırma katmanını içerir. Çözümlenen her resim veya sayfa bir işlemdir. İşlemi 100 sayfa içeren bir PDF veya TIFF belgesiyle çağırırsanız, okuma işlemi bunu 100 işlem olarak sayar ve 100 için faturalandırılırsınız. İşleme 50 çağrı yaptıysanız ve her çağrı 100 sayfalı bir belge gönderdiyseniz, 50 X 100 = 5000 işlemleri için faturalandırılırsınız.

## <a name="step-2-the-get-read-results-operation"></a>2. Adım: okuma sonuçlarını al işlemi

İkinci adım [okuma sonuçları al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750) işlemini çağırmalıdır. Bu işlem, okuma işlemi tarafından oluşturulan işlem KIMLIĞINI giriş olarak alır. Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. **Başarılı** değerle döndürülünceye kadar bu işlemi tekrarlayarak çağırın. Saniye başına istek (RPS) oranını aşmamak için 1 ile 2 saniye arasında bir Aralık kullanın.

|Alan| Tür | Olası değerler |
|:-----|:----:|:----|
|durum | string | notStarted: işlem başlatılmamış. |
| |  | çalışıyor: işlem işleniyor. |
| |  | başarısız: işlem başarısız oldu. |
| |  | başarılı: işlem başarılı oldu. |

> [!NOTE]
> Ücretsiz katman, istek hızını dakika başına 20 çağrı ile sınırlandırır. Ücretli katman, istek üzerine artalabilecek saniye başına 10 istek (RPS) sağlar. Saniye başına istek (RPS) oranı istemek için Azure destek kanalı 'nı veya hesap takımınızı kullanın.

**Durum** alanı **başarılı** değerine sahip olduğunda JSON yanıtı, yansımanız veya belgenizdeki ayıklanan metin içeriğini içerir. JSON yanıtı, tanınan sözcüklerin orijinal satır gruplandırmaları saklar. Ayıklanan metin çizgilerini ve bunların sınırlayıcı kutu koordinatlarını içerir. Her metin satırı, tüm ayıklanan kelimeleri ve bunların koordinatlarını ve güvenirlik puanlarını içerir.

> [!NOTE]
> İşleme gönderilen veriler `Read` geçici olarak şifrelenir ve kısa bir süre için geri kalanında depolanır ve sonra silinir. Bu, uygulamalarınızın ayıklanan metni hizmet yanıtının bir parçası olarak almasına olanak sağlar.

## <a name="sample-json-output"></a>Örnek JSON çıkışı

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

## <a name="natural-reading-order-output-latin-only"></a>Doğal okuma sırası çıkışı (yalnızca Latin)
[Okuma 3,2 Önizleme API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)ile, metin çizgilerinin, sorgu parametresi ile çıkış sırasını belirtin `readingOrder` . `natural`Aşağıdaki örnekte gösterildiği gibi, daha kolay okunabilir bir okuma sırası çıkışı için kullanın. Bu özellik yalnızca Latin dilleri için desteklenir.

:::image border type="content" source="./Images/ocr-reading-order-example.png" alt-text="OCR okuma düzeni örneği":::

## <a name="handwritten-classification-for-text-lines-latin-only"></a>Metin satırları için el ile sınıflandırma (yalnızca Latin)
[Okuma 3,2 PREVIEW API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) yanıtı, her metin satırının el yazısı stili olup olmadığı sınıflandırıp bir güven puanı ile birlikte yer alır. Bu özellik yalnızca Latin dilleri için desteklenir. Aşağıdaki örnek görüntüdeki metin için el yazısı sınıflandırmasını gösterir.

:::image border type="content" source="./Images/ocr-handwriting-classification.png" alt-text="OCR el yazısı sınıflandırması örneği":::

## <a name="select-pages-or-page-ranges-for-text-extraction"></a>Metin ayıklama için sayfa (ler) veya sayfa aralıklarını seçin
[Okuma 3,2 Önizleme API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)ile, büyük çok sayfalı belgeler için, `pages` yalnızca bu sayfalardan metin Ayıklanacak sayfa numaralarını veya sayfa aralıklarını belirtmek üzere sorgu parametresini kullanın. Aşağıdaki örnek, her iki durumda da tüm sayfalar (1-10) ve seçili 3-6 Sayfalar için metin ayıklanarak 10 sayfalı bir belge gösterir.

:::image border type="content" source="./Images/ocr-select-pages.png" alt-text="Seçili sayfa çıkışı":::

## <a name="supported-languages"></a>Desteklenen diller
Okuma API 'Leri, yazdırma stili metni için toplam 73 dili destekler. [OCR tarafından desteklenen dillerin](./language-support.md#optical-character-recognition-ocr)tam listesine bakın. El ile yazılmış stil OCR yalnızca Ingilizce için desteklenir.

## <a name="use-the-cloud-api-or-deploy-on-premise"></a>Bulut API 'sini kullanma veya şirket içinde dağıtma
3. x bulut API 'Leri, çoğu müşteri için tercih edilen seçenektir ve bu, çok sayıda tümleştirme ve bu kutudan hızlı verimlilik açısından tercih edilir. Müşterilerinizin gereksinimlerine odaklanmanız durumunda Azure ve Görüntü İşleme Hizmeti tanıtıcı ölçeği, performansı, veri güvenliği ve uyumluluk gereksinimlerini karşılamanız gerekir.

Şirket içi dağıtım için, [okuma Docker kapsayıcısı (Önizleme)](./computer-vision-how-to-install-containers.md) , yeni OCR özelliklerini kendi yerel ortamınızda dağıtmanıza olanak sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır.

## <a name="ocr-api"></a>OCR APı 'SI

[OCR API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d) , daha eski bir tanıma modeli kullanır, yalnızca görüntüleri destekler ve algılanan metinle anında dönerek zaman uyumlu olarak yürütülür. [OCR desteklenen dillere](./language-support.md#optical-character-recognition-ocr) bakın ve ardından API 'yi okuyun.

> [!NOTE]
> Vison 2,0 RecognizeText işlemleri, bu makalede ele alınan yeni okuma API 'SI 'nin kullanım aşamasında kullanım sürecinde. Mevcut müşteriler [okuma işlemlerini kullanarak geçiş](upgrade-api-versions.md)yapması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntü İşleme REST API veya istemci kitaplığı hızlı](./quickstarts-sdk/client-library.md)başlangıçlarını kullanmaya başlayın.
- [Okuma 3,1 REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005)hakkında bilgi edinin.
- Toplam 73 dil desteğiyle ilgili [okuma 3,2 genel önizleme REST API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) hakkında bilgi edinin.
