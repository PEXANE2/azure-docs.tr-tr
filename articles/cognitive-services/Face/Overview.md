---
title: Azure yüz hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Azure yüz hizmeti, görüntülerde insan yüzlerini algılamayı, tanımasını ve analiz etmek için kullandığınız AI algoritmalarını sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 9/17/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: yüz tanıma, yüz tanıma yazılımı, yüz analizi, yüz eşleştirme, yüz tanıma uygulaması, görüntüye göre arama, yüz tanıma arama
ms.openlocfilehash: 0a7e242add9fdaa9e169a4003e8ad8f39b1fb111
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262493"
---
# <a name="what-is-the-azure-face-service"></a>Azure yüz hizmeti nedir?

> [!WARNING]
> 11 Haziran 2020’de Microsoft, insan haklarına dayalı etkili düzenlemeler yapılmadığı sürece ABD’deki polis departmanlarına yüz tanıma teknolojisi satmayacağını duyurmuştur. Bu nedenle, müşteriler yüz tanıma özelliklerini ya da yüz veya Video Indexer gibi Azure hizmetlerinde yer alan işlevselliği veya bir müşteri varsa ya da için Birleşik Devletler bir polis departmanı olan bu hizmetlerin kullanılmasına izin vermeyi kullanamaz.

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure yüz hizmeti, görüntülerdeki insan yüzlerini algılayan, tanıyan ve çözümleyen AI algoritmaları sağlar. Yüz tanıma yazılımı, güvenlik, doğal Kullanıcı arabirimi, görüntü içeriği Analizi ve yönetimi, mobil uygulamalar ve Robotics gibi birçok farklı senaryoda önemlidir.

Yüz tanıma hizmeti, her biri aşağıdaki bölümlerde özetlenen birkaç farklı yüz Analizi işlevi sağlar.

## <a name="face-detection"></a>Yüz algılama

Yüz hizmeti bir görüntüdeki insan yüzlerini algılar ve konumlarına ait dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama, baş poz, cinsiyet, yaş, duygu, yüz artı ve gözlük gibi bir dizi yüz bağlantılı öznitelik ayıklayabilir.

> [!NOTE]
> Yüz algılama özelliği [görüntü işleme hizmeti](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)aracılığıyla da kullanılabilir. Ancak, yüz verileriyle daha fazla işlem yapmak istiyorsanız, bunun yerine bu hizmeti kullanmanız gerekir.

![Kadın ve adam, yüz ve yaş ve cinsiyetlerinin etrafında çizilmiş dikdörtgenler](./Images/Face.detection.jpg)

Yüz algılama hakkında daha fazla bilgi için [yüz algılama](concepts/face-detection.md) kavramları makalesine bakın. Ayrıca bkz. [API başvurusunu Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) belgeleri.

## <a name="face-verification"></a>Yüz doğrulama

Verify API 'SI algılanan iki yüzde veya algılanan bir yüzden bir kişi nesnesine karşı bir kimlik doğrulaması yapar. Pratikte, iki yüzün aynı kişiye ait olup olmadığını değerlendirir. Bu özellik güvenlik senaryolarında faydalı olabilir. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya API başvurusunu [Doğrula](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) belgelerine bakın.

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Benzer API 'yi bul, hedef yüzü ve aday yüzey kümesi arasında eşleşen bir yüzey kümesi, hedef yüztekine benzer daha küçük bir yüzey kümesi buluyor. Bu, görüntüye göre yüz arama yapmak için yararlıdır. 

İki çalışma modu, **Matchperson** ve **matchface**desteklenir. **Matchperson** modu, [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)'sini kullanarak aynı kişi için filtreledikten sonra benzer yüzler döndürür. **Matchface** modu aynı kişi filtresini yoksayar. Aynı kişiye ait olan veya olmayan benzer aday yüzlerin bir listesini döndürür.

Aşağıdaki örnekte hedef yüz gösterilmektedir:

![Gülümseyen kadın](./Images/FaceFindSimilar.QueryFace.jpg)

Ve bu görüntüler aday yüzlerdir:

![Beş tane gülümseyen kişi görüntüsü. A ve b görüntüleri aynı kişiyi gösterir.](./Images/FaceFindSimilar.Candidates.jpg)

Benzer dört yüzü bulmak için, **matchperson** modu, hedef yüzü ile aynı kişiyi gösteren a ve b döndürür. **Eşleme** modu, &mdash; biri hedefle aynı kişi olmasa veya az benzerliğe sahip olsa bile, b, c ve d tam olarak dört aday döndürür. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [benzer API başvurusu bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) belgelerine bakın.

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Bir gruptaki tüm yüzler aynı kişiye ait olabilir. Tek bir kişi için birkaç farklı grup olabilir. Gruplar, örneğin ifadesi gibi başka bir faktörle farklılaştırılabilir. Daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya [Grup API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) başvurusu belgelerine bakın.

## <a name="person-identification"></a>Kişileri tanıma

Tanıma API 'SI, bir kişi veritabanında algılanan bir yüzeyi belirlemek için kullanılır (yüz tanıma arama). Bu özellik, fotoğraf yönetimi yazılımında otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturun ve zaman içinde düzenleyebilirsiniz.

Aşağıdaki görüntüde adlı bir veritabanı örneği gösterilmektedir `"myfriends"` . Her grup, en fazla 1.000.000 farklı kişi nesnesi içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Her biri üç yüz görüntüsü içeren farklı kişiler için üç sütunlu bir kılavuz](./Images/person.group.clare.jpg)

Bir veritabanını oluşturup eğitdikten sonra, yeni bir algılanan yüz ile gruba göre kimlik tanımlayabilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi kimliği hakkında daha fazla bilgi için, [yüz tanıma](concepts/face-recognition.md) kavramları Kılavuzu veya API başvurusunu [Tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) belgelerine bakın.

## <a name="containers"></a>Kapsayıcılar

Bir standartlaştırılmış Docker kapsayıcısını verilerinize yaklaştırarak yüzeyleri tespit etmek, tanımak ve tanımlamak için [yüz kapsayıcısını kullanın](face-how-to-install-containers.md) .

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalar yüz hizmetini kullanmanın birkaç yolunu göstermektedir:

- [Yüz Tanıma API'si: Windows Istemci kitaplığı ve örneği](https://github.com/Microsoft/Cognitive-Face-Windows) , yüz algılama, analiz ve tanımlama için çeşitli senaryolar gösteren bir WPF uygulamasıdır.
- [FAMILYNOTES UWP uygulaması](https://github.com/Microsoft/Windows-appsample-familynotes) , bir aile notunun yanı sıra konuşma, Cortana, mürekkep ve kamera ile birlikte yüz tanıma bilgilerini kullanan bir evrensel WINDOWS platformu (UWP) uygulamasıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetler kaynaklarında olduğu gibi, yüz hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine dikkat etmeniz gerekir. Daha fazla bilgi için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bir yüz tanıma uygulamasının temel bileşenlerini tercih ettiğiniz dilde kodlayın ve hızlı başlangıcı izleyin.

- [İstemci kitaplığı hızlı başlangıç](quickstarts/client-libraries.md).
