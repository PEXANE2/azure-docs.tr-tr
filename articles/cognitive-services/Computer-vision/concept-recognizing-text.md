---
title: Resimlerden ve belgelerden metin okuma-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API'si kullanarak, yazdırma ve el yazısı metinler için, optik karakter tanıma (OCR) ve metin ile ilgili kavramlarla ilgili kavramlar.
services: cognitive-services
author: msbbonsu
manager: netahw
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: t-bebon
ms.custom: seodec18
ms.openlocfilehash: 65e1613eb8fda934899afe692f45a38fca04bff2
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85414049"
---
# <a name="read-text-from-images-and-documents"></a>Görüntülerden ve belgelerden metin okuma

Görüntü İşleme, görüntülerden ve PDF belgelerinden yazdırılmış veya el yazısı metin çıkaran yeni derin öğrenme tabanlı optik karakter tanıma (OCR) özellikleri içerir. Görüntü İşleme hem analog belgelerden (görüntüler, taranmış belgeler) hem de dijital belgelerde metin ayıklar. Sonuç olarak, belge-faturalar, senetler, finansal raporlar, makaleler ve daha fazlasını içeren, lisans levhalarının veya seri numaralarının bulunduğu kapsayıcıların fotoğraflarındaki resimlerden metin ayıklayabilirsiniz. Bu OCR işlevselliği, bulutta veya şirket içinde (kapsayıcılar) yönetilen hizmetin bir parçası olarak kullanılabilir. Ayrıca, kurumsal sınıf uyumluluk ve gizlilik ihtiyaçlarınızı karşılamak için sanal ağları ve özel uç noktaları destekler.

## <a name="read-api"></a>API 'YI oku 

Görüntü İşleme [okuma API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) , Microsoft 'un yazdırılan metni, el yazısı metin (yalnızca İngilizce), rakamları ve görüntü ve PDF belgelerinden gelen para birimi sembollerini ayıklayan en son OCR teknolojisidir. Joker karakter, görsel gürültü içeren görüntülerde metin, dijital veya taranan, dijital veya taranmış ve metin kalın görüntüleri çıkarmak için en iyi duruma getirilmiştir. Aynı görüntüde veya belgede yazdırılmış ve el yazısı metinleri (Ingilizce) ve karışık dilleri destekler. Desteklenen dillerin tam listesi [görüntü işleme Için dil desteği](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#text-recognition) sayfasında bulunabilir.


### <a name="how-it-works"></a>Nasıl çalışır?

[Okuma API 'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) zaman uyumsuzdur. İlk adım okuma işlemini çağırmalıdır. Okuma işlemi, girdi olarak bir görüntü veya PDF belgesi alır ve bir işlem KIMLIĞI döndürür. 

İkinci adım [sonuçları al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750) işlemini çağırmalıdır. Bu işlem, okuma işlemi tarafından oluşturulan işlem KIMLIĞINI alır. Daha sonra, resim veya belge içinden JSON biçiminde ayıklanan metin içeriğini döndürür. JSON yanıtı, tanınan sözcüklerin orijinal satır gruplandırmaları saklar. Ayıklanan metin çizgilerini ve bunların sınırlayıcı kutu koordinatlarını içerir. Her metin satırı, tüm ayıklanan kelimeleri ve bunların koordinatlarını ve güven puanlarını içerir.

Gerekirse, aşağıdaki çizimde görüldüğü gibi, okuma açısını yatay görüntü eksenine ilişkin derece cinsinden döndürerek, tanınan sayfanın dönüşünü düzeltir.

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/vision-overview-ocr-read.png)

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

Okuma API 'SI, yazdırılan metnin Ingilizce, Ispanyolca, Almanca, Fransızca, Italyanca, Portekizce ve Felemenkçe dillerde ayıklanmasından sonra desteklenir. Senaryonuz daha fazla dil desteklemelidir gerektiriyorsa, bu belgede OCR API 'sine genel bakış bölümüne bakın. [Desteklenen tüm dillerin](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#text-recognition) listesine bakın

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/supported-languages-example.png)

### <a name="mixed-languages-support"></a>Karışık diller desteği

Okuma API 'SI, genellikle karışık dil belgeleri olarak bilinen, birden çok dile sahip resimleri ve belgeleri destekler. Metin içeriğini Ayıklamadan önce belgedeki her metin satırını algılanan dile sınıflandırarak bunu yapar.

![Döndürülmekte olan bir görüntü ve metnin okunmakta ve ayırıcılarından biri](./Images/mixed-language-example.png)

### <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, okuma hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/trust-center/product-overview) ' nde bilişsel Hizmetler sayfasına bakın.

### <a name="deploy-on-premises"></a>Şirket içi dağıtma

Ayrıca, yeni OCR özelliklerini kendi ortamınızda dağıtmanıza olanak tanımak için bir Docker kapsayıcısı (Önizleme) olarak okuma de mevcuttur. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için harika. Bkz [. okuma kapsayıcıları nasıl yüklenir ve çalıştırılır.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)


## <a name="ocr-api"></a>OCR APı 'SI

[OCR API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) eski bir tanıma modeli kullanır. Yalnızca tek görüntüleri destekler, PDF 'Ler değil ve anında yanıt döndürür. Okuma API 'sinden [daha fazla dili](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#text-recognition) destekler.

## <a name="next-steps"></a>Sonraki adımlar

- [Okuma 3,0 REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005)hakkında bilgi edinin.
- REST API ile birlikte C#, Java, JavaScript veya Python kullanarak OCR uygulamak için [metin ayıklama](./QuickStarts/CSharp-hand-text.md) hızlı başlangıcını izleyin.
