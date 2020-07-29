---
title: Optik karakter tanıma (OCR)-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API'si kullanarak, yazdırılmış ve el ile yazılmış metinlere sahip resimler ve belgelerden optik karakter tanıma (OCR) ile ilgili kavramlar.
services: cognitive-services
author: msbbonsu
manager: netahw
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: t-bebon
ms.custom: seodec18
ms.openlocfilehash: 83e76cd96e09b0e136d2bfbe2e5863b289724bdd
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284260"
---
# <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Microsoft Görüntü İşleme API'si, görüntülerden ve PDF belgelerinden yazdırılmış veya el yazısı metin çıkaran optik karakter tanıma (OCR) özelliklerini içerir. OCR API 'Leri, analog belgelerden (görüntüler, taranmış belgeler) ve dijital belgelerde metin ayıklar. Sonuç olarak, belge-faturalar, senetler, finansal raporlar, makaleler ve daha fazlasını içeren, lisans levhalarının veya seri numaralarının bulunduğu kapsayıcıların fotoğraflarındaki resimlerden metin ayıklayabilirsiniz. Yeni okuma OCR API 'SI, bulutta veya şirket içinde (kapsayıcılar) yönetilen hizmetin bir parçası olarak kullanılabilir. Ayrıca, kurumsal sınıf uyumluluk ve gizlilik ihtiyaçlarınızı karşılamak için sanal ağları ve özel uç noktaları destekler.

## <a name="read-api"></a>API 'YI oku 

Görüntü İşleme [okuma API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) , Microsoft 'un yazdırılan metni birkaç dilde çıkaran en son OCR teknolojisidir, resim ve çok sayfalı PDF belgelerinden el yazısı metin (yalnızca İngilizce), rakamlar ve para birimi sembolleri. Büyük-büyük ve çok sayfalı PDF belgelerinin karışık dillerde metin ayıklamak için en iyi duruma getirilmiştir. Aynı görüntüde veya belgede yazdırılmış ve el yazısı metinleri (Yalnızca Ingilizce) algılamanızı destekler. [OCR desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) sayfasının tam listesini inceleyin.

### <a name="how-ocr-works"></a>OCR nasıl kullanılır?

[Okuma API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) , 2000 sayfaya kadar olan metin ağır belgeleri destekler ve bu nedenle zaman uyumsuz olarak yürütülür. İlk adım okuma işlemini çağırmalıdır. Okuma işlemi, girdi olarak bir görüntü veya PDF belgesi alır ve bir işlem KIMLIĞI döndürür. 

İkinci adım [sonuçları al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750) işlemini çağırmalıdır. Bu işlem, okuma işlemi tarafından oluşturulan işlem KIMLIĞINI alır. Daha sonra, resim veya belge içinden JSON biçiminde ayıklanan metin içeriğini döndürür. JSON yanıtı, tanınan sözcüklerin orijinal satır gruplandırmaları saklar. Ayıklanan metin çizgilerini ve bunların sınırlayıcı kutu koordinatlarını içerir. Her metin satırı, tüm ayıklanan kelimeleri ve bunların koordinatlarını ve güvenirlik puanlarını içerir.

Gerekirse, aşağıdaki çizimde görüldüğü gibi, okuma açısını yatay görüntü eksenine ilişkin derece cinsinden döndürerek, tanınan sayfanın dönüşünü düzeltir.

![OCR, resimleri ve belgeleri ayıklanan metinle yapısal çıktıya dönüştürür](./Images/how-ocr-works.svg)

### <a name="sample-ocr-output"></a>Örnek OCR çıkışı

JSON biçiminde, aşağıdaki örnekte gösterildiği gibi başarılı bir yanıt döndürülür:

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

C# ve REST API kullanarak OCR uygulamak için, [yazdırılmış ve el ile yazılmış metin](./QuickStarts/CSharp-hand-text.md) hızlı başlangıcını izleyin.

### <a name="input-requirements"></a>Giriş gereksinimleri

Okuma API 'SI aşağıdaki girişleri alır:
* Desteklenen dosya biçimleri: JPEG, PNG, BMP, PDF ve TIFF
* PDF ve TIFF için en fazla 2000 sayfa işlenir. Ücretsiz katman aboneleri için yalnızca ilk iki sayfa işlenir.
* Dosya boyutu 50 MB 'tan az ve en az 50 x 50 piksel ve en fazla 10000 x 10000 piksel boyutunda olmalıdır.
* PDF boyutları, yasal veya A3 kağıt boyutlarına ve daha küçük boyutlara karşılık gelen en fazla 17 x 17 inç olmalıdır.

### <a name="text-from-images"></a>Görüntülerden metin

Aşağıdaki okuma API 'SI çıktısı, farklı açılarda, renklerde ve yazı tiplerinde metin içeren bir görüntüden ayıklanan metin çizgilerini ve kelimeleri gösterir

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>Belgelerden metin

Görüntülere ek olarak, okuma API 'SI bir PDF belgesini giriş olarak alır.

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/text-from-documents-example.png)


### <a name="handwritten-text-in-english"></a>Ingilizce dilinde el yazısı metin

Okuma işlemi şu anda yalnızca Ingilizce dilinde el ile yazılmış metinlerin ayıklanarak desteklenir.

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/handwritten-example.png)

### <a name="printed-text-in-supported-languages"></a>Desteklenen dillerde yazdırılmış metin

Read 3,0 API 'SI Ingilizce, Ispanyolca, Almanca, Fransızca, Italyanca, Portekizce ve Felemenkçe dillerde yazdırılmış metnin ayıklanmasından sonra desteklenir. [3,1-Preview ' i okuyun. 1 API genel önizlemesi](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005) Basitleştirilmiş Çince desteği ekler. Senaryonuz daha fazla dil desteklemelidir gerektiriyorsa, bu belgede OCR API 'sine genel bakış bölümüne bakın. [OCR tarafından desteklenen tüm dillerin](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) listesine bakın

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/supported-languages-example.png)

### <a name="mixed-languages-support"></a>Karışık diller desteği

Okuma API 'SI, genellikle karışık dil belgeleri olarak bilinen, birden çok dile sahip resimleri ve belgeleri destekler. Metin içeriğini Ayıklamadan önce belgedeki her metin satırını algılanan dile sınıflandırarak bunu yapar.

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/mixed-language-example.png)

### <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, okuma hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/trust-center/product-overview) ' nde bilişsel Hizmetler sayfasına bakın.

### <a name="containers-for-on-premise-deployment"></a>Şirket içi dağıtım için kapsayıcılar

Ayrıca, yeni OCR özelliklerini kendi ortamınızda dağıtmanıza olanak tanımak için bir Docker kapsayıcısı (Önizleme) olarak okuma de mevcuttur. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için harika. Bkz [. okuma kapsayıcıları nasıl yüklenir ve çalıştırılır.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)


## <a name="ocr-api"></a>OCR APı 'SI

[OCR API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) , daha eski bir tanıma modeli kullanır, yalnızca görüntüleri destekler ve algılanan metinle anında dönerek zaman uyumlu olarak yürütülür. OCR tarafından [desteklenen dillere](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) bkz. okuma API 'si.

## <a name="next-steps"></a>Sonraki adımlar

- [Okuma 3,0 REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005)hakkında bilgi edinin.
- Basitleştirilmiş Çince desteğiyle ilgili [REST API okuma 3,1-Önizleme. 1 ' i](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005) öğrenin.
- REST API ile birlikte C#, Java, JavaScript veya Python kullanarak OCR uygulamak için [metin ayıklama](./QuickStarts/CSharp-hand-text.md) hızlı başlangıcını izleyin.
