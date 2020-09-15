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
ms.openlocfilehash: cbcfddcd02a3998b3b35b01d386816735c59ae7e
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90526417"
---
# <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Azure Görüntü İşleme API'si, görüntülerden yazdırılmış veya el yazısı metin çıkaran optik karakter tanıma (OCR) özelliklerini içerir. Belge-faturalar, senetler, finansal raporlar, makaleler ve daha fazlasını içeren, lisans levhalarının veya seri numaralarının bulunduğu kapsayıcıların fotoğraflarından metin ayıklayabilirsiniz.

## <a name="read-api"></a>API 'YI oku 

Görüntü İşleme [okuma API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) 'Si, Azure 'un en son OCR teknolojisidir[(yenilikler),](./whats-new.md)yazdırılan metni (bazı dillerde), el yazısı metin (yalnızca İngilizce), rakamları ve görüntü ve çok sayfalı PDF belgelerinden gelen para birimi sembollerini ayıklar. Metin açısından kalın görüntülerden ve çok sayfalı PDF belgelerinden karışık dillerle metin ayıklamak en iyi duruma getirilmiştir. Aynı görüntüde veya belgede hem yazdırılmış hem de el yazısı metnin algılanmasının aynısını destekler.

![OCR, resimleri ve belgeleri ayıklanan metinle yapısal çıktıya dönüştürür](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Giriş gereksinimleri
**Okuma** çağrısı görüntüleri ve belgeleri giriş olarak alır. Bunlar aşağıdaki gereksinimlere sahiptir:

* Desteklenen dosya biçimleri: JPEG, PNG, BMP, PDF ve TIFF
* PDF ve TIFF dosyaları için en fazla 2000 sayfa (ücretsiz katman için yalnızca ilk iki sayfa) işlenir.
* Dosya boyutu 50 MB 'tan az (ücretsiz katman için 4 MB) ve en az 50 x 50 piksel ve en fazla 10000 x 10000 piksel boyutunda olmalıdır. 
* PDF boyutları, yasal veya A3 kağıt boyutlarına ve daha küçük boyutlara karşılık gelen en fazla 17 x 17 inç olmalıdır.

### <a name="read-31-preview-allows-selecting-pages"></a>Okuma 3,1 önizlemesi, sayfa (ler) i seçmeyi sağlar
[Okuma 3,1 Önizleme API 'si](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005)ile, büyük çok sayfalı belgeler için, yalnızca bu sayfalardan metin ayıklamak üzere belirli sayfa numaralarını veya sayfa aralıklarını giriş parametresi olarak sağlayabilirsiniz. Bu, isteğe bağlı dil parametresine ek olarak yeni bir giriş parametresidir.

> [!NOTE]
> **Dil girişi** 
>
> [Okuma çağrısının](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) dil için isteğe bağlı bir istek parametresi vardır. Bu, belgedeki metnin BCP-47 dili kodudur. Okuma otomatik dil tanımlama ve çok dilli belgelerini destekler, bu nedenle belgeyi belirli bir dil olarak işlenmek üzere zorlamak istiyorsanız yalnızca bir dil kodu sağlayın.

## <a name="the-read-call"></a>Oku çağrısı

Okuma API 'sinin [okuma çağrısı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) , bir görüntüyü ya da PDF belgesini girdi olarak alır ve zaman uyumsuz olarak ayıklar. Çağrı, adlı bir yanıt üst bilgisi alanı ile döndürülür `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak Işlem kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Sonuç URL 'SI |
|:-----|:----|
|İşlem-konum | `https://cognitiveservice/vision/v3.0/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Faturalandırma** 
>
> [Görüntü işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) sayfası, okuma için fiyatlandırma katmanını içerir. Çözümlenen her resim veya sayfa bir işlemdir. İşlemi 100 sayfa içeren bir PDF veya TIFF belgesiyle çağırırsanız, okuma işlemi bunu 100 işlem olarak sayar ve 100 için faturalandırılırsınız. İşleme 50 çağrı yaptıysanız ve her çağrı 100 sayfalı bir belge gönderdiyseniz, 50 X 100 = 5000 işlemleri için faturalandırılırsınız.

## <a name="the-get-read-results-call"></a>Okuma sonuçları Al çağrısı

İkinci adım [okuma sonuçları al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750) işlemini çağırmalıdır. Bu işlem, okuma işlemi tarafından oluşturulan işlem KIMLIĞINI giriş olarak alır. Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. **Başarılı** değerle döndürülünceye kadar bu işlemi tekrarlayarak çağırın. Saniye başına istek (RPS) oranını aşmamak için 1 ile 2 saniye arasında bir Aralık kullanın.

|Alan| Tür | Olası değerler |
|:-----|:----:|:----|
|durum | string | notStarted: işlem başlatılmamış. |
| |  | çalışıyor: işlem işleniyor. |
| |  | başarısız: işlem başarısız oldu. |
| |  | başarılı: işlem başarılı oldu. |

> [!NOTE]
> Ücretsiz katman, istek hızını dakika başına 20 çağrı ile sınırlandırır. Ücretli katman, istek üzerine artalabilecek saniye başına 10 istek (RPS) sağlar. Saniye başına istek (RPS) oranı istemek için Azure destek kanalı 'nı veya hesap takımınızı kullanın.

**Durum** alanı **başarılı** değerine sahip olduğunda JSON yanıtı, yansımanız veya belgenizdeki ayıklanan metin içeriğini içerir. JSON yanıtı, tanınan sözcüklerin orijinal satır gruplandırmaları saklar. Ayıklanan metin çizgilerini ve bunların sınırlayıcı kutu koordinatlarını içerir. Her metin satırı, tüm ayıklanan kelimeleri ve bunların koordinatlarını ve güvenirlik puanlarını içerir.

## <a name="sample-json-output"></a>Örnek JSON çıkışı

Başarılı bir JSON yanıtı örneğine bakın:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "language": "en",
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
### <a name="read-31-preview-adds-text-line-style-latin-languages-only"></a>Okuma 3,1 Preview metin satırı stili ekler (yalnızca Latin dilleri)
[Okuma 3,1 Önizleme API 'si](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005) , her metin satırının yazdırma veya el yazısı stili olup olmadığı, Güvenirlik puanı ile birlikte bir **Görünüm** nesnesi verir. Bu özellik yalnızca Latin dilleri için desteklenir.

```json
  "appearance": {
              "style": "handwriting",
              "styleConfidence": 0.836
            }
```
[Görüntü işleme OCR SDK 'sı hızlı](./quickstarts-sdk/client-library.md) başlangıçlarını kullanmaya başlayın ve [okuma REST API hızlı](./QuickStarts/CSharp-hand-text.md) başlangıçlarını kullanarak, kuruluşunuzda OCR özelliklerini tümleştirmaya başlayabilirsiniz.

## <a name="supported-languages-for-print-text"></a>Yazdırma metni için desteklenen diller
[Read 3,0 API 'Si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) Ingilizce, Ispanyolca, Almanca, Fransızca, Italyanca, Portekizce ve Felemenkçe dillerde yazdırılmış metnin ayıklanmasından sonra desteklenir.

OCR destekli dillerin tam listesi için [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) bölümüne bakın.

### <a name="read-31-preview-adds-simplified-chinese-and-japanese"></a>Okuma 3,1 Preview Basitleştirilmiş Çince ve Japonca ekler
[Okuma 3,1 API genel önizlemesi](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005) , Basitleştirilmiş Çince ve Japonca desteği ekler. Senaryonuz daha fazla dil desteklemelidir gerektiriyorsa, [OCR API 'si](#ocr-api) bölümüne bakın. 

## <a name="supported-languages-for-handwritten-text"></a>El yazısı metin için desteklenen diller
Okuma işlemi şu anda yalnızca Ingilizce dilinde el yazısı metinleri ayıklamayı desteklemektedir.

## <a name="use-the-rest-api-and-sdk"></a>REST API ve SDK 'sını kullanma
[3. x okuma REST API](./QuickStarts/CSharp-hand-text.md) , çoğu müşteri için tercih edilen seçenektir ve bu, kullanıma hazır hale getirerek tümleştirme ve hızlı üretkenlik açısından çok daha hızlı bir şekilde sağlanır. Müşterilerinizin gereksinimlerine odaklanmanız durumunda Azure ve Görüntü İşleme Hizmeti tanıtıcı ölçeği, performansı, veri güvenliği ve uyumluluk gereksinimlerini karşılamanız gerekir.

## <a name="deploy-on-premise-with-docker-containers"></a>Docker kapsayıcılarıyla şirket içinde dağıtma
[Okuma 2,0 Docker kapsayıcısı (Önizleme)](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers) , yeni OCR özelliklerini kendi yerel ortamınızda dağıtmanıza olanak sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır.

## <a name="example-outputs"></a>Örnek çıktılar

### <a name="text-from-images"></a>Görüntülerden metin

Aşağıdaki okuma API 'SI çıktısı, farklı metin açılarına, renklere ve yazı tiplerine sahip bir görüntüden ayıklanan metni gösterir.

![Farklı renklerde ve açılarda, ayıklanan metinle listelenen birkaç sözcükten oluşan bir görüntü](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>Belgelerden metin

Okuma API 'SI, Ayrıca PDF belgelerini giriş olarak alabilir.

![Ayıklanan metinle listelenen bir fatura belgesi](./Images/text-from-pdf-example.png)

### <a name="handwritten-text"></a>El yazısı metin

Okuma işlemi görüntülerden el yazısı metinleri ayıklar (Şu anda yalnızca Ingilizce).

![El yazısı notunun bir görüntüsü, ayıklanan metinle listelendi](./Images/handwritten-example.png)

### <a name="printed-text"></a>Yazdırılan metin

Okuma işlemi, yazdırılmış metni birkaç farklı dilde ayıklayabilir.

![Çıkarılan metinle listelenen bir Ispanyolca textbook görüntüsü](./Images/supported-languages-example.png)

### <a name="mixed-language-documents"></a>Karışık dil belgeleri

Okuma API 'SI, genellikle karışık dil belgeleri olarak bilinen birden çok farklı dil içeren görüntüleri ve belgeleri destekler. Belgedeki her metin satırını, metin içeriğini Ayıklamadan önce algılanan dile sınıflandırarak işe yarar.

![Ayıklanan metinle listelenen çeşitli dillerdeki tümceciklerin görüntüsü](./Images/mixed-language-example.png)

## <a name="ocr-api"></a>OCR APı 'SI

[OCR API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) , daha eski bir tanıma modeli kullanır, yalnızca görüntüleri destekler ve algılanan metinle anında dönerek zaman uyumlu olarak yürütülür. [OCR desteklenen dillere](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) bakın ve ardından API 'yi okuyun.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, okuma/OCR hizmetlerini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için [Microsoft Güven Merkezi](https://www.microsoft.com/trust-center/product-overview) ' nde bilişsel Hizmetler sayfasına bakın.

> [!NOTE]
> Vison 2,0 RecognizeText işlemleri, bu makalede ele alınan yeni okuma API 'SI 'nin kullanım aşamasında kullanım sürecinde. Mevcut müşteriler [okuma işlemlerini kullanarak geçiş](upgrade-api-versions.md)yapması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- C#, Java, JavaScript veya Python 'da [görüntü işleme okuma 3,0 SDK hızlı](./quickstarts-sdk/client-library.md) başlangıçlarını kullanmaya başlayın.
- REST API 'Lerinin nasıl kullanılacağını öğrenmek için C#, Java, JavaScript veya Python 'da [3,0 REST API hızlı](./QuickStarts/CSharp-hand-text.md) başlangıçlarını kullanın.
- [Okuma 3,0 REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005)hakkında bilgi edinin.
- Basitleştirilmiş Çince ve Japonca desteğiyle ilgili [REST API 3,1 okuma genel önizlemesi](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005) hakkında bilgi edinin.
