---
title: Sürüm notları-yüz hizmeti
titleSuffix: Azure Cognitive Services
description: Yüz hizmeti için sürüm notları, çeşitli sürümler için sürüm değişikliklerinin geçmişini içerir.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: yluiu
ms.openlocfilehash: 767c9dec373a2bda806d75d602b194edde98c6b5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76165861"
---
# <a name="face-release-notes"></a>Yüz sürüm notları

Bu makale, yüz hizmeti 1,0 sürümü ile ilgilidir.

### <a name="release-changes-in-june-2019"></a>Haziran 2019 ' de yayın değişiklikleri

* Küçük, yan görünüm, kapatılmış ve bulanık yüzler üzerinde geliştirilmiş doğrulukla yeni bir yüz algılama modeli eklendi. Bu uygulamayı, [yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), çok [yönlü liste-ekle yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [Largeçok yönlü liste-ekleme yüzü](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [kişilik grubu kişi-ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) yüz ve [largepersongroup Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42) Ile, parametrede yeni yüz algılama modeli adını belirterek yüz `detection_02` ekleme `detectionModel` . [Algılama modeli belirtme hakkında](Face-API-How-to-Topics/specify-detection-model.md)daha fazla bilgi.

### <a name="release-changes-in-april-2019"></a>Nisan 2019 ' de yayın değişiklikleri

* Ve özniteliklerinin genel doğruluğu geliştirildi `age` `headPose` . `headPose`Özniteliği, `pitch` Şimdi etkinleştirilen değer ile de güncelleştirilir. Bu öznitelikleri, `returnFaceAttributes` [yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) parametresi parametresinde belirterek kullanın `returnFaceAttributes` . 

* [Yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), çok [yönlü liste-ekleme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [Largecemi listesi-ekleme yüzü](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [kişilik grubu kişi-yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ve [largepersongroup kişi ekleme-yüz ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2019"></a>Mart 2019 ' de yayın değişiklikleri

* İyileştirilmiş doğrulukla yeni bir yüz tanıma modeli eklendi. Parametre içinde yeni yüz tanıma modeli adını belirterek, [yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), çok [yönlü liste-oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [largecelist-Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [Persongroup-Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) ve [largepersongroup-](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) Create ile kullanın `recognition_02` `recognitionModel` . [Bir tanıma modeli belirtme hakkında](Face-API-How-to-Topics/specify-recognition-model.md)daha fazla bilgi.

### <a name="release-changes-in-january-2019"></a>Ocak 2019 ' de yayın değişiklikleri

* Abonelikler arasında veri geçişini desteklemek için anlık görüntü özelliği eklendi: [Snapshot](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). [Yüz verilerinizi farklı bir yüz aboneliğine geçirme hakkında](Face-API-How-to-Topics/how-to-migrate-face-data.md)daha fazla bilgi.

### <a name="release-changes-in-october-2018"></a>Ekim 2018 ' de yayın değişiklikleri

* ,,, Ve için,,, ve için bir arada `status` `createdDateTime` bulunan Açıklama `lastActionDateTime` `lastSuccessfulTrainingDateTime` [-eğitim durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [largepersongroup-Get eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5)ve [largecelıst-eğitim durumunu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf)al.

### <a name="release-changes-in-may-2018"></a>Mayıs 2018 ' de yayın değişiklikleri

* Geliştirilmiş `gender` öznitelik önemli ölçüde ve geliştirilmiş,,,, `age` `glasses` `facialHair` `hair` `makeup` öznitelikleri. Onları, [yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresiyle kullanın. 

* [Yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), çok [yönlü liste-ekleme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [Largecem listesi-ekleme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [kişiskii grup kişi-](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) yüz ve [largepersongroup kişi ekleme-yüz ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Mart 2018 ' de yayın değişiklikleri

* Million-Scale kapsayıcı eklendi: [Largecelist](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) ve [largepersongroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). [Büyük ölçekli özelliği kullanma hakkında](Face-API-How-to-Topics/how-to-use-large-scale.md)daha fazla ayrıntı.

* [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` [1, 5] ile [1, 100] arasında ve varsayılan olarak 10 ' a kadar artan yüz-parametre.

### <a name="release-changes-in-may-2017"></a>Mayıs 2017 ' de yayın değişiklikleri

* `hair` `makeup` `accessory` `occlusion` `blur` `exposure` `noise` [Yüz Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresinde,,,,, ve öznitelikleri eklendi.

* Bir kişilik grubundaki desteklenen 10.000 kişi ve [yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)tanıma.

* [Persongroup kişi listesindeki](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) desteklenen sayfalandırma isteğe bağlı parametrelerle: `start` ve `top` .

* Farklı çok yönlü listelerle ve PersonGroup 'taki farklı kişilerle yüz ekleme/silme için desteklenen eşzamanlılık.

### <a name="release-changes-in-march-2017"></a>Mart 2017 ' de yayın değişiklikleri
* `emotion` [Yüzü Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresinde öznitelik eklendi.

* Yüz, çok yönlü liste gibi yüz [algılamalarından](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) döndürülen dikdörtgenle yeniden algılanamadı `targetFace` . yüz ve [kişilik grubu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) [Ekle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) yüz

* Algılanamaz yüz boyutu düzeltildi çünkü tamamen 36x36 ila 4096x4096 piksel arasında olduğundan emin olun.

### <a name="release-changes-in-november-2016"></a>Kasım 2016 ' de yayın değişiklikleri
* [Persongroup kişisi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) kullanırken ek kalıcı yüzeyleri depolamak Için yüz depolama Standart abonelik eklendi-yüz veya çok [yönlü liste ekle-](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) tanımlama veya benzerlik eşleştirme için yüz ekleme. Depolanan görüntüler, 1000 yüz başına 0,5 $ üzerinden ücretlendirilir ve bu fiyat, günlere eşit olarak dağıtılır. Ücretsiz katman abonelikleri 1.000 toplam kişi ile sınırlı olmaya devam eder.

### <a name="release-changes-in-october-2016"></a>Ekim 2016 ' de yayın değişiklikleri
* TargetFace içindeki birden fazla yüzün hata iletisini "' görüntüde birden fazla yüz var", çok [yönlü liste-ekleme yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ve [kişiskii grup kişi-ekleme yüzeyi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)' nde birden fazla yüz var.

### <a name="release-changes-in-july-2016"></a>Temmuz 2016 ' de yayın değişiklikleri
* Yüz içinde nesne kimlik doğrulaması için desteklenen yüz [-doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* `mode`İki çalışma modunun seçilmesine olanak sağlayan isteğe bağlı parametre eklendi `matchPerson` : `matchFace` ve [yüz yüze, benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve varsayılan olarak bul `matchPerson` .

* `confidenceThreshold`Kullanıcı için isteğe bağlı parametresi, [yüz tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)' da bir kişinin bir kişi nesnesine ait olup olmadığı eşiğini ayarlamak için eklenmiştir.

* `start` `top` Kullanıcının başlangıç noktasını ve toplam persongroup sayısını listeye belirtmesini sağlamak Için, [persongroup-List](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) içinde isteğe bağlı ve parametreler eklendi.

### <a name="v10-changes-from-v0"></a>V0 'den v 1.0 değişiklikleri
* Service Root bitiş noktası sürümünden ' ```https://westus.api.cognitive.microsoft.com/face/v0/``` ye güncelleştirildi ```https://westus.api.cognitive.microsoft.com/face/v1.0/``` . Değişiklikler uygulandı: [yüz tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)tanıma, yüz tanıma, [benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [yüz grubunu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)bul.

* Minimum algılanabilir yüz boyutu 36x36 piksel olarak güncelleştirildi. 36x36 pikselden küçük yüzler algılanmayacak.

* Yüz v0 'daki Person grubu ve kişi verileri kullanım dışı bırakıldı. Yüz V 1.0 hizmetiyle bu verilere erişilemez.

* 30 Haziran 2016 tarihinde Yüz Tanıma API'si v0 uç noktası kullanımdan kaldırıldı.
