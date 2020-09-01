---
title: Yüz Tanıma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler hizmet yüz hizmeti, görüntülerdeki insan yüzlerini algılamak, tanımak ve analiz etmek için kullanılan algoritmaları sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 9/01/2020
ms.author: pafarley
ms.openlocfilehash: e1b5ca8cfb9a6bc0fec7d85f609c411f38d1bbbc
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89280085"
---
# <a name="what-is-the-azure-face-service"></a>Azure yüz hizmeti nedir?
<!--
> [!IMPORTANT]
> On June 11, 2020, Microsoft announced that it will not sell facial recognition technology to police departments in the United States until strong regulation, grounded in human rights, has been enacted. As such, customers may not use facial recognition features or features included in Azure Services, such as Face or Video Indexer, if a customer is, or is allowing use of such services by or for, a police department in the United States.
-->
![Yasal Bildirim ](./media/overview/banner.png)

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure bilişsel hizmetler hizmet yüz hizmeti, görüntülerde insan yüzlerini algılayan, tanıyan ve çözümleyen algoritmalar sağlar. İnsan yüzü bilgilerini işleyebilme özelliği birçok farklı yazılım senaryosunda önemlidir. Örnek senaryolar arasında güvenlik, doğal Kullanıcı arabirimi, görüntü içeriği Analizi ve yönetimi, mobil uygulamalar ve Robotics bulunur.

Yüz hizmeti, her biri aşağıdaki bölümlerde özetlenen birkaç farklı işlev sağlar.

## <a name="face-detection"></a>Yüz algılama

Yüz hizmeti bir görüntüdeki insan yüzlerini algılar ve konumlarına ait dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama, yüz ile ilgili bir dizi özniteliği çıkarabilir. Örneğin baş poz, cinsiyet, yaş, duygu, yüz saç ve gözlük verilebilir.

> [!NOTE]
> Yüz algılama özelliği [görüntü işleme API'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)aracılığıyla da kullanılabilir. Yüz verileriyle daha fazla işlem yapmak istiyorsanız, bu makalede ele alınan hizmet olan yüz hizmetini kullanın.

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

Benzer dört yüzü bulmak için, matchPerson modu, hedef yüzü ile aynı kişiyi gösteren a ve b döndürür. Eşleme modu, &mdash; biri hedefle aynı kişi olmasa veya az benzerliğe sahip olsa bile, b, c ve d tam olarak dört aday döndürür. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [benzer API başvurusu bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) belgelerine bakın.

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Bir gruptaki tüm yüzler aynı kişiye ait olabilir. Tek bir kişi için birkaç farklı grup olabilir. Gruplar, örneğin ifadesi gibi başka bir faktörle farklılaştırılabilir. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [Grup API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) başvuru belgelerine bakın.

## <a name="person-identification"></a>Kişileri tanıma

Tanıma API 'SI, bir kişi veritabanında algılanan bir yüzü belirlemek için kullanılır. Bu özellik, fotoğraf yönetimi yazılımında otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturun ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki görüntüde adlı bir veritabanı örneği gösterilmektedir `"myfriends"` . Her grup, en fazla 1.000.000 farklı kişi nesnesi içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Her biri üç yüz görüntüsü içeren farklı kişiler için üç sütunlu bir kılavuz](./Images/person.group.clare.jpg)

Bir veritabanını oluşturup eğitdikten sonra, yeni bir algılanan yüz ile gruba göre kimlik tanımlayabilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi kimliği hakkında daha fazla bilgi için bkz. [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [API başvurusunu belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) belgeleri.

## <a name="containers"></a>Kapsayıcılar

Bir standartlaştırılmış Docker kapsayıcısını verilerinize yaklaştırarak yüzeyleri tespit etmek, tanımak ve tanımlamak için [yüz kapsayıcısını kullanın](face-how-to-install-containers.md) .

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalar yüz hizmetini kullanmanın birkaç yolunu göstermektedir:

- [Yüz Tanıma API'si: Windows Istemci kitaplığı ve örneği](https://github.com/Microsoft/Cognitive-Face-Windows) , yüz algılama, analiz ve tanımlama için çeşitli senaryolar gösteren bir WPF uygulamasıdır.
- [FAMILYNOTES UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes) , bir aile notunun yanı sıra konuşma, Cortana, mürekkep ve kamera ile birlikte yüz tanıma bilgilerini kullanan bir evrensel WINDOWS platformu (UWP) uygulamasıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetler kaynaklarında olduğu gibi, yüz hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine dikkat etmeniz gerekir. Daha fazla bilgi için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodda bir yüz algılama senaryosu uygulamak için hızlı başlangıcı izleyin:

- [Hızlı başlangıç: C# ile .NET SDK kullanarak görüntüdeki yüzeyleri algılayın](quickstarts/csharp.md). Diğer diller mevcuttur.
