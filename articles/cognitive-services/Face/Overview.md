---
title: Yüz Tanıma API’si nedir?
titleSuffix: Azure Cognitive Services
description: Azure bilişsel Hizmetler Yüz Tanıma API'si, görüntülerdeki insan yüzlerini algılamak, tanımak ve analiz etmek için kullanılan algoritmaları sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 75f2969562dd2de041ffaa146c394f1a18c7756b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74961548"
---
# <a name="what-is-the-azure-face-api"></a>Azure Yüz Tanıma API’si nedir?

Azure bilişsel Hizmetler Yüz Tanıma API'si, görüntülerdeki insan yüzlerini algılamak, tanımak ve analiz etmek için kullanılan algoritmaları sağlar. İnsan yüzü bilgilerini işleyebilme özelliği birçok farklı yazılım senaryosunda önemlidir. Güvenlik, doğal Kullanıcı arabirimi, görüntü içeriği Analizi ve yönetimi, mobil uygulamalar ve Robotics örnek senaryolar.

Yüz Tanıma API'si birkaç farklı işlev sağlar. Her işlev aşağıdaki bölümlerde özetlenmiştir. Hakkında daha fazla bilgi edinmek için okumaya devam edin.

## <a name="face-detection"></a>Yüz algılama

Yüz Tanıma API'si görüntüdeki insan yüzlerini algılar ve konumlarına ait dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama, yüz ile ilgili bir dizi özniteliği çıkarabilir. Örneğin baş poz, cinsiyet, yaş, duygu, yüz saç ve gözlük verilebilir.

> [!NOTE]
> Yüz algılama özelliği [görüntü işleme API'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)aracılığıyla da kullanılabilir. Yüz verileriyle daha fazla işlem yapmak istiyorsanız, bu makalede ele alınan Yüz Tanıma API'si kullanın.

![Kadın ve adam, yüz ve yaş ve cinsiyetlerinin etrafında çizilmiş dikdörtgenler](./Images/Face.detection.jpg)

Yüz algılama hakkında daha fazla bilgi için [yüz algılama](concepts/face-detection.md) kavramları makalesine bakın. Ayrıca bkz. [API başvurusunu Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) belgeleri.

## <a name="face-verification"></a>Yüz doğrulama

Verify API 'SI algılanan iki yüzde veya algılanan bir yüzden bir kişi nesnesine karşı bir kimlik doğrulaması yapar. Pratikte, iki yüzün aynı kişiye ait olup olmadığını değerlendirir. Bu özellik güvenlik senaryolarında faydalı olabilir. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya API başvurusunu [Doğrula](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) belgelerine bakın.

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Benzer bir API, hedef yüzle benzer şekilde daha küçük bir yüzey kümesi bulmak için bir aday yüzey kümesiyle hedef yüzü karşılaştırır. İki çalışma modu, matchPerson ve matchFace desteklenir. MatchPerson modu, [VERIFY API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)'sini kullanarak aynı kişi için filtreledikten sonra benzer yüzeyleri döndürür. MatchFace modu aynı kişi filtresini yoksayar. Aynı kişiye ait olan veya olmayan benzer aday yüzlerin bir listesini döndürür.

Aşağıdaki örnekte hedef yüz gösterilmektedir:

![Gülümseyen kadın](./Images/FaceFindSimilar.QueryFace.jpg)

Ve bu görüntüler aday yüzlerdir:

![Beş tane gülümseyen kişi görüntüsü. A ve b görüntüleri aynı kişiyi gösterir.](./Images/FaceFindSimilar.Candidates.jpg)

Benzer dört yüzü bulmak için, matchPerson modu, hedef yüzü ile aynı kişiyi gösteren a ve b döndürür. Eşleme modu, bir kişi hedefle aynı kişi olmasa ya da az benzerliğe sahip olsa bile, a, b, c ve d&mdash;tam olarak dört aday döndürür. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [benzer API başvurusu bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) belgelerine bakın.

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Bir gruptaki tüm yüzler aynı kişiye ait olabilir. Tek bir kişi için birkaç farklı grup olabilir. Gruplar, örneğin ifadesi gibi başka bir faktörle farklılaştırılabilir. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [Grup API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) başvuru belgelerine bakın.

## <a name="person-identification"></a>Kişileri tanıma

Tanıma API 'SI, bir kişi veritabanında algılanan bir yüzü belirlemek için kullanılır. Bu özellik, fotoğraf yönetimi yazılımında otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturun ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki görüntüde `"myfriends"`adlı bir veritabanı örneği gösterilmektedir. Her grup, en fazla 1.000.000 farklı kişi nesnesi içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Her biri üç yüz görüntüsü içeren farklı kişiler için üç sütunlu bir kılavuz](./Images/person.group.clare.jpg)

Bir veritabanını oluşturup eğitdikten sonra, yeni bir algılanan yüz ile gruba göre kimlik tanımlayabilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi kimliği hakkında daha fazla bilgi için bkz. [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [API başvurusunu belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) belgeleri.

## <a name="use-containers"></a>Kapsayıcıları kullanma

Bir standartlaştırılmış Docker kapsayıcısını verilerinize yaklaştırarak yüzeyleri tespit etmek, tanımak ve tanımlamak için [yüz kapsayıcısını kullanın](face-how-to-install-containers.md) .

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalar Yüz Tanıma API'si kullanmanın birkaç yolunu göstermektedir:

- [Microsoft yüz tanıma API'si: Windows Istemci kitaplığı ve örneği](https://github.com/Microsoft/Cognitive-Face-Windows) , yüz algılama, analiz ve tanımlama için çeşitli senaryolar gösteren bir WPF uygulamasıdır.
- [FAMILYNOTES UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes) , bir aile notunun yanı sıra konuşma, Cortana, mürekkep ve kamera ile birlikte yüz tanıma bilgilerini kullanan bir evrensel WINDOWS platformu (UWP) uygulamasıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetler kaynaklarında olduğu gibi, yüz hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine dikkat etmeniz gerekir. Daha fazla bilgi için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodda bir yüz algılama senaryosu uygulamak için hızlı başlangıcı izleyin:

- [Hızlı başlangıç: ile C#.NET SDK kullanarak bir görüntüdeki yüzeyleri tespit ](quickstarts/csharp.md)edin. Diğer diller mevcuttur.
