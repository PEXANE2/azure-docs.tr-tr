---
title: Yayın notları - Yüz hizmeti
titleSuffix: Azure Cognitive Services
description: Face hizmetiiçin sürüm notları, çeşitli sürümler için sürüm değişikliklerinin geçmişini içerir.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: yluiu
ms.openlocfilehash: 767c9dec373a2bda806d75d602b194edde98c6b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76165861"
---
# <a name="face-release-notes"></a>Yüz serbest bırakma notları

Bu makale, Face hizmeti sürüm 1.0 ile ilgilidir.

### <a name="release-changes-in-june-2019"></a>Haziran 2019'daki sürüm değişiklikleri

* Küçük, yan görünümde, tıkanmış ve bulanık yüzler üzerinde geliştirilmiş doğrulukla yeni bir yüz algılama modeli eklendi. [Yüz - Algıla ,](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) [FaceList - Yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Person - Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ekle ve [LargePersonGroup Kişi -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42) Parametrede `detection_02` `detectionModel` yeni yüz algılama modeli adını belirterek Yüz Ekleyin. [Algılama modeli nin nasıl belirtilen](Face-API-How-to-Topics/specify-detection-model.md)hakkında daha fazla ayrıntı .

### <a name="release-changes-in-april-2019"></a>Nisan 2019'daki sürüm değişiklikleri

* Ve `age` `headPose` özniteliklerin genel doğruluğu artırıldı. Öznitelik, `headPose` şimdi etkinleştirilen `pitch` değerle de güncelleştirilir. Bu öznitelikleri Yüz `returnFaceAttributes` [- Algı](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi parametresinde belirterek kullanın. 

* Yüz geliştirilmiş hız [- Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Yüz ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Yüz ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Kişi - Yüz ve](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) [LargePersonGroup Kişi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)ekle - Yüz ekle .

### <a name="release-changes-in-march-2019"></a>Mart 2019'daki sürüm değişiklikleri

* Geliştirilmiş doğrulukla yeni bir yüz tanıma modeli eklendi. [Yüz - Algıla ,](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) [FaceList - Oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), Büyük Yüz Listesi - [Oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), Kişi Grubu [- Oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) ve [LargePersonGroup -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) Parametrede `recognition_02` `recognitionModel` yeni yüz tanıma modeli adını belirterek oluşturun. [Tanıma modeli nin nasıl belirtilen](Face-API-How-to-Topics/specify-recognition-model.md)daha fazla ayrıntı .

### <a name="release-changes-in-january-2019"></a>Ocak 2019'daki sürüm değişiklikleri

* Abonelikler arasında veri geçişini desteklemek için Anlık Görüntü özelliği eklendi: [Anlık Görüntü](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). [Yüz verilerinizi farklı bir Yüz aboneliğine geçirme](Face-API-How-to-Topics/how-to-migrate-face-data.md)hakkında daha fazla bilgi.

### <a name="release-changes-in-october-2018"></a>Ekim 2018'deki sürüm değişiklikleri

* Için rafine `status` `createdDateTime`açıklama `lastActionDateTime`, `lastSuccessfulTrainingDateTime` , , ve [PersonGroup - Eğitim Durumu alın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [LargePersonGroup - Eğitim Durumu alın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5), ve [LargeFaceList - Eğitim Durumu alın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf).

### <a name="release-changes-in-may-2018"></a>Mayıs 2018'de sürüm değişiklikleri

* Geliştirilmiş `gender` öznitelik önemli `age`ölçüde `glasses` `facialHair`ve `hair` `makeup` aynı zamanda geliştirilmiş , , , , öznitelikleri. [Yüz - Algı](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi ile kullanın. 

* Yüz 4 MB'dan 6 MB'a artırılmış giriş görüntü dosyası boyutu sınırı [- Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - FaceList -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) [LargeFaceList - Face Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Person - Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ve [LargePersonGroup Person ekle - Yüz ekle .](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)

### <a name="release-changes-in-march-2018"></a>Mart 2018'deki sürüm değişiklikleri

* Milyon Ölçekli Kapsayıcı eklendi: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) ve [LargePersonGroup.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) [Büyük ölçekli özelliğin nasıl kullanılacağı](Face-API-How-to-Topics/how-to-use-large-scale.md)hakkında daha fazla bilgi.

* Artırılmış Yüz - [1, 5] ile [1, 100] arasında parametre ve varsayılan 10'u [tanımlayın.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned`

### <a name="release-changes-in-may-2017"></a>Mayıs 2017'de sürüm değişiklikleri

* Face `hair` `makeup`' `accessory` `occlusion`de `blur` `exposure`eklenen `noise` , , , , , , ve öznitelikleri [- Algı](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi.

* Bir Kişi Grubu ve Yüz desteklenen 10K kişi [- Tanımlayın.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)

* PersonGroup Person destekli pagination - İsteğe `start` `top`bağlı parametreler ile [liste:](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) ve .

* PersonGroup'ta farklı FaceLists ve farklı kişilere karşı yüz ekleme/silme de desteklenen eşzamanlılık.

### <a name="release-changes-in-march-2017"></a>Mart 2017'deki sürüm değişiklikleri
* Face `emotion` 'e öznitelik eklendi [-](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` Algı parametresi.

* Yüzden döndürülen dikdörtgen ile yüzün yeniden algılanamaması [düzeltildi -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `targetFace` [FaceList'teki](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) gibi algıla - Yüz ve Kişi Grubu Kişi Ekle - Yüz [Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Kesinlikle 36x36 ile 4096x4096 piksel arasında olduğundan emin olmak için algılanabilir yüz boyutu düzeltildi.

### <a name="release-changes-in-november-2016"></a>Kasım 2016'da sürüm değişiklikleri
* [PersonGroup Person kullanırken](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ek kalıcı yüzleri depolamak için Yüz Depolama Standardı aboneliği eklendi - Yüz veya FaceList ekle - Kimlik veya benzerlik eşleştirme için [Yüz ekle.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) Depolanan görüntüler, 1000 yüz başına 0,5 $ üzerinden ücretlendirilir ve bu fiyat, günlere eşit olarak dağıtılır. Ücretsiz katman abonelikleri toplam 1.000 kişi ile sınırlı olmaya devam etmektedir.

### <a name="release-changes-in-october-2016"></a>Ekim 2016'da sürüm değişiklikleri
* HedefTeki birden fazla yüzün hata iletisini değiştiren Face,'Resimde birden fazla yüz vardır' ile 'Resimde birden fazla yüz var' [facelist](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ' - Yüz ve Kişi Grubu Kişi Ekle [- Yüz Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Temmuz 2016'da sürüm değişiklikleri
* Yüz Yüze Desteklenen Yüz kişi nesne kimlik doğrulaması [- Doğrula](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* İki `mode` çalışma modunun seçilmesini sağlayan isteğe `matchPerson` bağlı parametre eklendi: ve `matchFace` [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) and default ise. `matchPerson`

* Bir `confidenceThreshold` yüzün Yüz'deki bir Kişi nesnesine ait olup olmadığının eşiğini ayarlamak için kullanıcı için isteğe bağlı parametre eklendi [- Tanımlayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* PersonGroup'a isteğe bağlı `start` ve `top` parametreler eklendi - Kullanıcının başlangıç noktasını ve listeye ekleyecek toplam PersonGroups numarasını belirtmesini sağlamak için [listele.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248)

### <a name="v10-changes-from-v0"></a>V0'den V1.0 değişiklikleri
* Hizmet kökü bitiş ```https://westus.api.cognitive.microsoft.com/face/v0/``` noktasından ' a kadar ```https://westus.api.cognitive.microsoft.com/face/v1.0/```güncelleştirildi Uygulanan değişiklikler: [Yüz - Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Yüz - Tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Yüz - Benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve Yüz Bul [- Grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* En az tespit edilebilir yüz boyutunu 36x36 pikselolarak güncelledi. 36x36 pikselden küçük yüzler algılanmaz.

* Face V0'deki Kişi Grubu ve Kişi verilerini amortismana kattı. Bu verilere Face V1.0 hizmeti yle erişilemez.

* 30 Haziran 2016'da Face API'nin V0 bitiş noktası nı kaybetti.
