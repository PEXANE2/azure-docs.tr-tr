---
title: Görüntü İşleme yenilikler nelerdir?
titleSuffix: Azure Cognitive Services
description: Bu makale Görüntü İşleme hakkındaki haberleri içerir.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 01/13/2021
ms.author: pafarley
ms.openlocfilehash: 78746e7623f58af5ae9df829b48245295dc39f01
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487109"
---
# <a name="whats-new-in-computer-vision"></a>Görüntü İşleme yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Bu öğeler sürüm notları, videolar, blog gönderileri ve diğer bilgi türleri olabilir. Hizmette güncel kalmak için bu sayfaya yer işareti ekleyin.

## <a name="march-2021"></a>Mart 2021

### <a name="computer-vision-32-public-preview-update"></a>Görüntü İşleme 3,2 genel önizleme güncelleştirmesi

Görüntü İşleme API'si v 3.2 genel önizlemesi güncelleştirildi. Önizleme sürümü, güncelleştirilmiş okuma ve analiz API 'Leri ile birlikte tüm Görüntü İşleme özelliklere sahiptir.

> [!div class="nextstepaction"]
> [Bkz. Görüntü İşleme v 3.2 genel önizleme 3](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)

## <a name="february-2021"></a>Şubat 2021

### <a name="read-api-v32-public-preview-with-ocr-support-for-73-languages"></a>73 dilleri için OCR desteğiyle API v 3.2 genel önizlemesini okuyun
Görüntü İşleme, bulut hizmeti ve Docker kapsayıcısı olarak sunulan Read API v 3.2 genel önizlemesi, şu güncelleştirmeleri içerir:
* Basitleştirilmiş ve geleneksel Çince, Japonca, Korece ve Latin dilleri gibi [73 dilleri Için OCR](./language-support.md#optical-character-recognition-ocr) .
* Metin satırı çıkışı için doğal okuma sırası (yalnızca Latin dilleri)
* Metin çizgileri için el yazısı stili sınıflandırması (yalnızca Latin dilleri).
* Yalnızca bir çok sayfalı belge için seçili sayfalar için metin ayıkla.
* Şirket içi dağıtım için [daha az bir kapsayıcı](./computer-vision-how-to-install-containers.md?tabs=version-3-2) olarak kullanılabilir.

Okuma API 'SI hakkında [daha fazla bilgi edinin](concept-recognizing-text.md) .

> [!div class="nextstepaction"]
> [Read API v 3.2 genel önizlemeyi kullanın](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)


## <a name="january-2021"></a>Ocak 2021

### <a name="spatial-analysis-container-update"></a>Uzamsal analiz kapsayıcısı güncelleştirmesi

Yeni bir özellik kümesiyle, [uzamsal analiz kapsayıcısının](spatial-analysis-container.md) yeni bir sürümü yayınlandı. Bu Docker kapsayıcısı, fiziksel ortamlar aracılığıyla insanlar ile hareketleri arasındaki uzamsal ilişkileri anlamak için gerçek zamanlı akış videosunu analiz etmenizi sağlar. 

* [Uzamsal analiz işlemleri](spatial-analysis-operations.md) , bir kişinin maske gibi bir koruyucu yüzeyi takmakta olup olmadığını algılamak için yapılandırılabilir. 
    * `personcount` `personcrossingline` `personcrossingpolygon` Parametresi yapılandırılarak, ve işlemleri için bir maske Sınıflandırıcısı etkinleştirilebilir `ENABLE_FACE_MASK_CLASSIFIER` .
    * Bu öznitelikler `face_mask` ve `face_noMask` video akışında algılanan her bir kişi için güvenirlik puanı olan meta veriler olarak döndürülecek
* *Personcrossingçokgen* işlemi, bir kişinin bir bölgede harcadığı süre sayısının hesaplanmasına izin verecek şekilde genişletildi. `type`İşlem Için bölge yapılandırmasındaki parametreyi `zonedwelltime` ve *Personzonedwelltimeevent* türünde yeni bir olay, `durationMs` kişinin bölgede harcadığı milisaniye sayısıyla doldurulmuş alanı içerir.
* **Son değişiklik**: *personzoneevent* olayı, *Personzoneenterexitevent* olarak yeniden adlandırıldı. Bu olay, bir kişi bölgeden girdiğinde ya da çıktığında ve çapraz bölgenin numaralı tarafıyla açıklamalı bilgi sağlayan, *personcrossingçokgen* işlemi tarafından tetiklenir.
* Video URL 'SI, tüm işlemlerde "özel parametre/karıştırılmış" olarak belirtilebilir. Gizleme şimdi isteğe bağlıdır ve yalnızca `KEY` ve `IV` ortam değişkenleri olarak sağlanmışsa çalışır.
* Ayar, tüm işlemler için varsayılan olarak etkindir. Öğesini `do_calibration: false` devre dışı bırakmak için ayarlayın.
* Parametresi aracılığıyla otomatik düzeltme (varsayılan olarak devre dışı) için destek eklendi `enable_recalibration` , Ayrıntılar için lütfen [uzamsal analiz işlemlerine](./spatial-analysis-operations.md) bakın
* İçin kamera ayarlama parametreleri `DETECTOR_NODE_CONFIG` . Ayrıntılar için [uzamsal analiz işlemlerine](./spatial-analysis-operations.md) bakın.


## <a name="october-2020"></a>Ekim 2020

### <a name="computer-vision-api-v31-ga"></a>Görüntü İşleme API'si v 3.1 GA

Genel kullanılabilirlik Görüntü İşleme API'si v 3.1 'e yükseltildi.

## <a name="september-2020"></a>Eylül 2020

### <a name="spatial-analysis-container-preview"></a>Uzamsal analiz kapsayıcısı önizlemesi

[Uzamsal analiz kapsayıcısı](spatial-analysis-container.md) artık önizlemededir. Görüntü İşleme uzamsal analiz özelliği, kişiler ve fiziksel ortamlar aracılığıyla taşınanlar arasındaki uzamsal ilişkileri anlamak için gerçek zamanlı akış videosunu analiz etmenizi sağlar. Uzamsal analiz, şirket içinde kullanabileceğiniz bir Docker kapsayıcısıdır. 

### <a name="read-api-v31-public-preview-adds-ocr-for-japanese"></a>Okuma API v 3.1 Genel Önizleme Japonca için OCR ekler
Görüntü İşleme Read API v 3.1 Genel önizlemesi şu özellikleri ekler:
* Japonca için OCR dili
* Her metin çizgisi için, görünümün el ile mi yoksa yazdırma stili mi olduğunu belirtin ve bir güven puanı (yalnızca Latin dilleri) ile birlikte görüntülenir.
* Birden çok sayfalı belge için yalnızca seçili sayfalar veya sayfa aralığı için metin ayıkla.

* Okuma API 'sinin bu önizleme sürümü Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Japonca, Portekizce, Basitleştirilmiş Çince ve Ispanyolca dilleri destekler.

Daha fazla bilgi için [okuma API 'sine genel bakış](concept-recognizing-text.md) bölümüne bakın.

> [!div class="nextstepaction"]
> [Okuma API v 3.1 genel önizleme 2 hakkında daha fazla bilgi edinin](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005)

## <a name="july-2020"></a>Temmuz 2020

### <a name="read-api-v31-public-preview-with-ocr-for-simplified-chinese"></a>Basitleştirilmiş Çince için OCR ile API v 3.1 Genel önizlemeyi okuyun
Görüntü İşleme okuma API v 3.1 genel önizleme, Basitleştirilmiş Çince desteği ekler.

* Okuma API 'sinin bu önizleme sürümü Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Portekizce, Basitleştirilmiş Çince ve Ispanyolca dilleri destekler.

Daha fazla bilgi için [okuma API 'sine genel bakış](concept-recognizing-text.md) bölümüne bakın.

> [!div class="nextstepaction"]
> [Okuma API v 3.1 Genel Önizleme 1 hakkında daha fazla bilgi edinin](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)

## <a name="may-2020"></a>Mayıs 2020
Görüntü İşleme API'si v 3.0 genel kullanılabilirliği ve [API 'Yi okumak](concept-recognizing-text.md)için güncelleştirmeler ile girilmiş:

* Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Portekizce ve Ispanyolca desteği
* İyileştirilmiş doğruluk
* Ayıklanan her sözcüğün Güvenirlik puanı
* Yeni çıkış biçimi

## <a name="march-2020"></a>Mart 2020

* TLS 1,2, bu hizmete yönelik tüm HTTP istekleri için de zorlanır. Daha fazla bilgi için bkz. Azure bilişsel [Hizmetler güvenliği](../cognitive-services-security.md).

## <a name="january-2020"></a>Ocak 2020

### <a name="read-api-30-public-preview"></a>API 3,0 genel önizlemeyi okuyun

Artık görüntülerden yazdırılmış veya el yazısı metin ayıklamak için Read API 'sinin 3,0 sürümünü kullanma seçeneğiniz vardır. Önceki sürümlere kıyasla 3,0 şunları sağlar:
* İyileştirilmiş doğruluk
* Yeni çıkış biçimi
* Ayıklanan her sözcüğün Güvenirlik puanı
* Ek dil parametresiyle hem Ispanyolca hem de Ingilizce diller için destek

3,0 API 'sini kullanmaya başlamak için [Extract metin hızlı](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/REST/CSharp-hand-text.md?tabs=version-3) başlangıcını izleyin.

## <a name="cognitive-service-updates"></a>Bilişsel hizmet güncelleştirmeleri

[Bilişsel hizmetler için Azure Update duyuruları](https://azure.microsoft.com/updates/?product=cognitive-services)