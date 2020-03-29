---
title: Yüz algılama ve öznitelikler kavramları
titleSuffix: Azure Cognitive Services
description: Yüz algılama, görüntüdeki insan yüzlerini bulma ve isteğe bağlı olarak yüzle ilgili farklı verileri döndürme eylemidir.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: 15e39eb9f5b8dd3556ea9ff8240bc2c9d252cd31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73743059"
---
# <a name="face-detection-and-attributes"></a>Yüz algılama ve özellikleri

Bu makalede, yüz algılama ve yüz öznitelik verileri kavramları açıklanmaktadır. Yüz algılama, görüntüdeki insan yüzlerini bulma ve isteğe bağlı olarak yüzle ilgili farklı verileri döndürme eylemidir.

Görüntüdeki yüzleri algılamak için [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) işlemini kullanırsınız. En azından, algılanan her yüz yanıtta bir faceRectangle alanına karşılık gelir. Sol, üst, genişlik ve yükseklik için bu piksel koordinatları kümesi bulunan yüzü işaretler. Bu koordinatları kullanarak, yüzün konumunu ve boyutunu alabilirsiniz. API yanıtında, yüzler en büyükten en küçülmeye kadar boyut sırasına göre listelenir.

## <a name="face-id"></a>Yüz Kimliği

Yüz kimliği, görüntüde algılanan her yüz için benzersiz bir tanımlayıcı dizedir. Yüzünüzde bir yüz kimliği isteyebilirsiniz - API aramasını [algıla.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)

## <a name="face-landmarks"></a>Yüz tanıma yer işaretleri

Yüz işaretleri, göz bebekleri veya burnun ucu gibi bir yüzde bulunması kolay noktalar kümesidir. Varsayılan olarak önceden tanımlanmış 27 yer işareti noktası vardır. Aşağıdaki şekil tüm 27 noktaları gösterir:

![27 simgenin tümündeki etiketli bir yüz diyagramı](../Images/landmarks.1.jpg)

Noktaların koordinatları piksel birimleri olarak döndürülür.

## <a name="attributes"></a>Öznitelikler

Öznitelikler, [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API tarafından isteğe bağlı olarak algılanabilen özellikler kümesidir. Aşağıdaki öznitelikler algılanabilir:

* **Yaş**. Belirli bir yüz yıl içinde tahmini yaş.
* **Bulanıklaştırma**. Görüntüdeki yüzün bulanıklığı. Bu öznitelik, sıfır ile bir arasında bir değer ve düşük, orta veya yüksek gayri resmi bir derecelendirme döndürür.
* **Duygu**. Verilen yüz için kendi algılama güven ile duyguların bir listesi. Güven puanları normale döndürülir ve tüm duygular arasındaki puanlar bire kadar eklenir. Döndürülen duygular mutluluk, üzüntü, tarafsız, öfke, hor görme, tiksinti, sürpriz ve korkudur.
* **Pozlama**. Görüntüdeki yüzün pozu. Bu öznitelik sıfır ve bir ve underExposure, goodExposure veya overExposure gayri resmi bir derecelendirme arasında bir değer döndürür.
* **Yüz kılları.** Tahmini yüz kılvarlığı ve verilen yüz için uzunluğu.
* **Cinsiyet**. Verilen yüzün tahmini cinsiyeti. Olası değerler erkek, kadın ve cinsiyetsizdir.
* **Gözlük.** Verilen yüzün gözlüklü olup olmadığı. Olası değerler NoGlasses, ReadingGlasses, Güneş Gözlüğü ve Yüzme Gözlüğü vardır.
* **Saç.** Yüzün saç tipi. Bu özellik, saçın görünür olup olmadığını, kelliğin algılanıp algılmadığını ve hangi saç renklerinin algılanıp algılmadığını gösterir.
* **Baş pozu**. 3D alanda yüzün oryantasyonu. Bu öznitelik derece olarak pitch, roll ve yaw açıları ile açıklanır. Değer aralıkları sırasıyla -90 derece ile 90 derece, -180 derece ile 180 derece ve 90 derece ile 90 derece arasındadır. Açı eşlemeleri için aşağıdaki diyagrama bakın:

    ![Pitch, rulo ve yaw eksenleri etiketli bir kafa](../Images/headpose.1.jpg)
* **Makyaj.** Yüzün makyajlı olup olmadığı. Bu özellik eyeMakeup ve lipMakeup için bir Boolean değeri döndürür.
* **Gürültü**. Yüz görüntüsünde algılanan görsel gürültü. Bu öznitelik, sıfır ile bir arasında bir değer ve düşük, orta veya yüksek gayri resmi bir derecelendirme döndürür.
* **Oklüzyon**. Yüzün bölümlerini engelleyen nesneler olup olmadığı. Bu öznitelik eyeOccluded, alnaOklü ve mouthOccluded için bir Boolean değeri döndürür.
* **Gülümseyin.** Verilen yüzün gülümseme ifadesi. Bu değer hiçbir gülümseme için sıfır ve net bir gülümseme için bir arasındadır.

> [!IMPORTANT]
> Yüz öznitelikleri istatistiksel algoritmalar kullanarak tahmin edilir. Her zaman doğru olmayabilir. Öznitelik verilerine dayalı kararlar alırken dikkatli olun.

## <a name="input-data"></a>Giriş verileri

Giriş görüntülerinizin en doğru algılama sonuçlarını verdiğinden emin olmak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş görüntü biçimleri ilk kare için JPEG, PNG, GIF ve BMP'dir.
* Görüntü dosyası boyutu 4 MB'dan büyük olmamalıdır.
* Tespit edilebilir yüz boyutu aralığı 36 x 36 ila 4096 x 4096 pikseldir. Bu aralığın dışındaki yüzler algılanmayacak.
* Bazı yüzler teknik zorluklar nedeniyle tespit edilemeyebilir. Aşırı yüz açıları (baş pozu) veya yüz tıkanıklığı (güneş gözlüğü veya yüzün bir kısmını engelleyen eller gibi nesneler) algılamayı etkileyebilir. Frontal ve yakın ön yüzler en iyi sonuçları verir.

Bir video akışındaki yüzleri algılıyorsanız, video kameranızdaki belirli ayarları ayarlayarak performansı nızı artırabilirsiniz:

* **Yumuşatma**: Birçok video kamera yumuşatma efekti uygular. Çerçeveler arasında bulanıklık oluşturduğundan ve netliği azalttığından, bunu kapatabiliyorsanız kapatmalısınız.
* **Deklanşör Hızı**: Daha hızlı deklanşör hızı çerçeveler arasındaki hareket miktarını azaltır ve her kareyi daha net hale getirir. 1/60 saniye veya daha hızlı deklanşör hızlarını öneririz.
* **Deklanşör Açısı**: Bazı kameralar deklanşör hızı yerine deklanşör açısını belirtir. Mümkünse daha düşük bir deklanşör açısı kullanmalısınız. Bu daha net video çerçeveleri neden olur.

    >[!NOTE]
    > Daha düşük deklanşör açısına sahip bir kamera her karede daha az ışık alır, böylece görüntü daha koyu olacaktır. Kullanmak için doğru düzeyi belirlemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Artık yüz algılama kavramlarına aşina olduğunuza göre, belirli bir görüntüdeki yüzleri algılayan bir komut dosyasının nasıl yazIleceğini öğrenin.

* [Bir görüntüdeki yüzleri algılama](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)