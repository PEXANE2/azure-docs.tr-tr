---
title: Yüz tanıma kavramları
titleSuffix: Azure Cognitive Services
description: Bu makalede, yüz tanıma işlemleri ve temel veri yapıları doğrula, bul, grupla ve tanımla kavramları açıklanmaktadır.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: 164e5a8c107f445b376d26f9be7db92a7983b0d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73743067"
---
# <a name="face-recognition-concepts"></a>Yüz tanıma kavramları

Bu makalede, yüz tanıma işlemleri ve temel veri yapıları doğrula, bul, grupla ve tanımla kavramları açıklanmaktadır. Genel olarak, tanıma, benzer olup olmadıklarını veya aynı kişiye ait olup olmadıklarını belirlemek için iki farklı yüzü karşılaştırma işini açıklar.

## <a name="recognition-related-data-structures"></a>Tanıma ile ilgili veri yapıları

Tanıma işlemleri esas olarak aşağıdaki veri yapılarını kullanır. Bu nesneler bulutta depolanır ve kimlik dizeleri tarafından başvurulabilir. Kimlik dizeleri her zaman bir abonelik içinde benzersizdir. Ad alanları çoğaltılabilir.

|Adı|Açıklama|
|:--|:--|
|Algılanan Yüz| Bu tek yüz gösterimi [yüz algılama](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) işlemi ile alınır. Kimliği oluşturulduktan 24 saat sonra sona erer.|
|PersistedFace| AlgıYüz nesneleri FaceList veya Person gibi bir gruba eklendiğinde, Kalıcı Yüz nesneleri haline gelir. Bunlar herhangi bir zamanda [alınabilir](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) ve süresi dolmaz.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) veya [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Bu veri yapısı, PersistedFace nesnelerinin çeşitli bir listesidir. FaceList'in benzersiz bir kimliği, ad dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi vardır.|
|[Kişi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Bu veri yapısı, aynı kişiye ait Kalıcı Face nesnelerinin bir listesidir. Benzersiz bir kimliği, bir ad dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi vardır.|
|[Kişi Grubu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) veya [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Bu veri yapısı, Kişi nesnelerinin çeşitli bir listesidir. Benzersiz bir kimliği, bir ad dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi vardır. Bir PersonGroup tanıma işlemlerinde kullanılabilmesi için [eğitilmelidir.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)|

## <a name="recognition-operations"></a>Tanıma işlemleri

Bu bölümde, dört tanıma işleminin daha önce açıklanan veri yapılarını nasıl kullandığı ayrıntılı olarak açıklanmaktadır. Her tanıma işleminin geniş bir açıklaması için [bkz.](../Overview.md)

### <a name="verify"></a>Doğrulama

[Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) işlemi, Algıyüz veya Kalıcı Yüz'den bir yüz kimliği ve başka bir yüz kimliği veya bir Kişi nesnesi alır ve aynı kişiye ait olup olmadıklarını belirler. Bir Kişi nesnesinde geçerseniz, performansı artırmak için isteğe bağlı olarak o Kişinin ait olduğu bir Kişi Grubu'ndan geçebilirsiniz.

### <a name="find-similar"></a>Benzer Bul

[Benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) bul işlemi, DetectedFace veya PersistedFace'den bir yüz kimliği ve bir FaceList veya diğer yüz kimlikleri dizisi alır. Bir FaceList ile, verilen yüze benzer yüzlerin daha küçük bir FaceList'ini döndürür. Bir dizi yüz tonuyla, benzer şekilde daha küçük bir dizi döndürür.

### <a name="group"></a>Grup

[Grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) işlemi, DetectedFace veya PersistedFace'den çeşitli yüz teşekkülleri alır ve gruplanmış aynı teşekkülleri birkaç küçük diziye döndürür. Her "grup" dizisi benzer görünen yüz iT'leri içerir. Tek bir "messyGroup" dizisi, hiçbir benzerlik bulunmayan yüz işlikileri içerir.

### <a name="identify"></a>Tanımlamak

[Tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) işlemi, AlgıYüz veya Kalıcı Yüz'den bir veya birkaç yüz kimliği alır ve bir Kişi Grubu'ndan her yüzün ait olabileceği Kişi nesnelerinin listesini döndürür. Döndürülen Kişi nesneleri, tahmin güven değerine sahip Aday nesneler olarak sarılır.

## <a name="input-data"></a>Giriş verileri

Giriş görüntülerinizin en doğru tanıma sonuçlarını vermesini sağlamak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş görüntü biçimleri JPEG, PNG, GIF (ilk kare), BMP'dir.
* Resim dosyası boyutu 4 MB'dan büyük olmamalıdır.
* Kişi nesneleri oluştururken, farklı açı ve ışık türlerine sahip fotoğraflar kullanın.
* Bazı yüzler, örneğin teknik zorluklar nedeniyle tanınmayabilir:
  * Örneğin, aşırı aydınlatmalı görüntüler, şiddetli arkadan aydınlatma.
  * Bir ya da her iki gözü engelleyen engeller.
  * Saç tipi veya yüz saç farklılıkları.
  * Yaş agöre yüz görünümünde değişiklikler.
  * Aşırı yüz ifadeleri.

## <a name="next-steps"></a>Sonraki adımlar

Artık yüz tanıma kavramlarına aşina olduğunuza göre, eğitimli bir PersonGroup'a karşı yüzleri tanımlayan bir komut dosyası yazmayı öğrenin.

* [Görüntülerdeki yüzleri tanımlama](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)