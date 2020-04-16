---
title: Yüz Tanıma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler Yüz hizmeti, görüntülerdeki insan yüzlerini algılamak, tanımak ve analiz etmek için kullanılan algoritmalar sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 6714a0c4b967d80ad683ef023b5811423bdcb022
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403462"
---
# <a name="what-is-the-azure-face-service"></a>Azure Face hizmeti nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Bilişsel Hizmetler Yüz hizmeti, görüntülerdeki insan yüzlerini algılayan, tanıyan ve analiz eden algoritmalar sağlar. İnsan yüz bilgilerini işleme yeteneği birçok farklı yazılım senaryolarında önemlidir. Örnek senaryolar arasında güvenlik, doğal kullanıcı arabirimi, görüntü içeriği analizi ve yönetimi, mobil uygulamalar ve robotik sayılabilir.

Face hizmeti, her biri aşağıdaki bölümlerde özetlenen birkaç farklı işlev sağlar.

## <a name="face-detection"></a>Yüz algılama

Yüz hizmeti görüntüdeki insan yüzlerini algılar ve konumlarının dikdörtgen koordinatlarını döndürür. İsteğe bağlı olarak, yüz algılama yüzle ilgili bir dizi öznitelikleri ayıklayabilir. Örnekler baş poz, cinsiyet, yaş, duygu, yüz saç ve gözlük vardır.

> [!NOTE]
> Yüz algılama [özelliği, Computer Vision API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)üzerinden de kullanılabilir. Yüz verileriyle daha fazla işlem yapmak istiyorsanız, bu makalede tartışılan hizmet olan Face hizmetini kullanın.

![Yüzlerine, yaşlarına ve cinsiyetlerine çizilmiş dikdörtgenler ile bir kadın ve bir erkek görüntüsü](./Images/Face.detection.jpg)

Yüz algılama hakkında daha fazla bilgi için [Yüz algılama](concepts/face-detection.md) kavramları makalesine bakın. Ayrıca [api](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) başvuru belgelerini algıla.

## <a name="face-verification"></a>Yüz doğrulama

Doğrula API, algılanan iki yüze veya algılanan bir yüzden bir kişi nesnesine karşı kimlik doğrulaması yapar. Pratikte, iki yüzün aynı kişiye ait olup olmadığını değerlendirir. Bu özellik, güvenlik senaryolarında yararlı olabilir. Daha fazla bilgi için [Yüz tanıma](concepts/face-recognition.md) kavramları kılavuzuna veya API başvuru [belgelerini doğrula'ya](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) bakın.

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Benzer yüzü Bul API, hedef yüze benzeyen daha küçük bir yüz kümesi bulmak için hedef yüzü bir dizi aday yüzüyle karşılaştırır. MatchPerson ve matchFace olmak üzere iki çalışma modu desteklenir. MatchPerson modu, [API'yi doğrula'yı](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)kullanarak aynı kişi için filtre uygulamayaptıktan sonra benzer yüzleri döndürür. matchFace modu aynı kişi filtresini yok sayar. Aynı kişiye ait olabilecek veya ait olmayan benzer aday yüzlerinin listesini döndürür.

Aşağıdaki örnekte hedef yüz gösterilmektedir:

![Gülümseyen kadın](./Images/FaceFindSimilar.QueryFace.jpg)

Ve bu görüntüler aday yüzleri şunlardır:

![Beş tane gülümseyen kişi görüntüsü. A ve b görüntüleri aynı kişiyi gösterir.](./Images/FaceFindSimilar.Candidates.jpg)

Dört benzer yüz bulmak için matchPerson modu, hedef yüzle aynı kişiyi gösteren a ve b'yi döndürür. MatchFace modu, bazıları hedefle aynı&mdash;kişi olmasa veya düşük benzerliğe sahip olsa bile, a, b, c ve d tam olarak dört adayı döndürür. Daha fazla bilgi için [Yüz tanıma](concepts/face-recognition.md) kavramları kılavuzuna veya Benzer API başvuru belgelerini [bul'a](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) bakın.

## <a name="face-grouping"></a>Yüz gruplama

Gruplama API'si, bilinmeyen bir dizi yüzü benzerlik temelinde birkaç gruba ayırır. Her grup, özgün yüz kümesinin kopuk bir alt kümesidir. Bir gruptaki tüm yüzler büyük olasılıkla aynı kişiye ait olur. Tek bir kişi için birkaç farklı grup olabilir. Gruplar, örneğin ifade gibi başka bir faktörle ayırt edilir. Daha fazla bilgi için [Yüz tanıma](concepts/face-recognition.md) kavramları kılavuzuna veya Grup API başvuru [belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) bakın.

## <a name="person-identification"></a>Kişileri tanıma

Tanım API' si, algılanmış bir yüzü bir insan veritabanına karşı tanımlamak için kullanılır. Bu özellik, fotoğraf yönetimi yazılımında otomatik görüntü etiketleme için yararlı olabilir. Veritabanını önceden oluşturursunuz ve zaman içinde edinebilirsiniz.

Aşağıdaki resimde adlı bir veritabanı `"myfriends"`örneği gösterilmektedir. Her grup en fazla 1 milyon farklı kişi nesnesi içerebilir. Her kişi nesnesinde en fazla 248 kayıtlı yüz olabilir.

![Her biri üç satır yüz görüntüsü içeren, farklı kişiler için üç sütunlu bir ızgara](./Images/person.group.clare.jpg)

Bir veritabanı oluşturup eğitdikten sonra, algıya yeni bir yüzle gruba karşı kimlik belirleme yapabilirsiniz. Yüz, grupta bir kişi olarak belirlenirse kişi nesnesi döndürülür.

Kişi tanımlama hakkında daha fazla bilgi için [Yüz tanıma](concepts/face-recognition.md) kavramları kılavuzuna veya API başvuru [belgelerini](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) tanımla'ya bakın.

## <a name="containers"></a>Kapsayıcılar

Verilerinize daha yakın standart bir Docker kapsayıcısı yükleyerek yüzleri algılamak, tanımak ve tanımlamak için [Yüz kapsayıcısını kullanın.](face-how-to-install-containers.md)

## <a name="sample-apps"></a>Örnek uygulamalar

Aşağıdaki örnek uygulamalar Yüz hizmetini kullanmanın birkaç yolunu gösterir:

- [Yüz API'si: Windows İstemci Kitaplığı ve örnek yüz](https://github.com/Microsoft/Cognitive-Face-Windows) algılama, analiz ve tanımlama çeşitli senaryolar gösteren bir WPF uygulamasıdır.
- [FamilyNotes UWP uygulaması,](https://github.com/Microsoft/Windows-appsample-familynotes) aile notu paylaşım senaryosunda konuşma, Cortana, mürekkep ve kamerayla birlikte yüz tanımlaması kullanan evrensel bir Windows Platformu (UWP) uygulamasıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel Hizmetler kaynaklarında olduğu gibi, Yüz hizmetini kullanan geliştiriciler microsoft'un müşteri verilerine ilişkin ilkelerinden haberdar olmalıdır. Daha fazla bilgi için Microsoft Güven Merkezi'ndeki [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodda bir yüz algılama senaryosu uygulamak için hızlı bir başlangıç izleyin:

- [Hızlı başlatma: C# ile .NET SDK'yı kullanarak görüntüdeki yüzleri algıla.](quickstarts/csharp.md) Diğer diller mevcuttur.
