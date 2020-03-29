---
title: Algılama modeli nasıl belirtilir - Yüz
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Yüz uygulamanızda hangi yüz algılama modelini kullanacağınızı nasıl seçeceğiniz gösterecektir.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: yluiu
ms.openlocfilehash: 40ca1dbf981c5a9025cf5a0bac6b007709d69a77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76934571"
---
# <a name="specify-a-face-detection-model"></a>Yüz algılama modeli belirtme

Bu kılavuz, Azure Yüz hizmeti için yüz algılama modelini nasıl belirtdiğinizi gösterir.

Face hizmeti, görüntülerdeki insan yüzleri ne zıddatları üzerinde işlem gerçekleştirmek için makine öğrenimi modellerini kullanır. Müşteri geri bildirimleri ve araştırmadaki gelişmelere dayanarak modellerimizin doğruluğunu geliştirmeye devam ediyoruz ve bu geliştirmeleri model güncellemeleri olarak sunuyoruz. Geliştiriciler, yüz algılama modelinin hangi sürümünü kullanmak istediklerini belirtme seçeneğine sahiptir; kullanım durumlarına en uygun modeli seçebilirler.

Belirli yüz operasyonlarında yüz algılama modelini nasıl belirtin öğrenin. Yüz hizmeti, bir yüzün görüntüsünü başka bir veri biçimine dönüştürdüğünde yüz algılama yı kullanır.

En son modeli kullanıp kullanmamanız gerektiğinden emin değilseniz, yeni modeli değerlendirmek ve geçerli veri kümenizi kullanarak sonuçları karşılaştırmak için [farklı modelleri Değerlendir](#evaluate-different-models) bölümüne gidin.

## <a name="prerequisites"></a>Ön koşullar

AI yüz algılama kavramına aşina olmalısınız. Bilmiyorsanız, yüz algılama kavramsal kılavuzuna veya nasıl yapılacağınız kılavuzuna bakın:

* [Yüz algılama kavramları](../concepts/face-detection.md)
* [Görüntüdeki yüzleri niçin algılarsın?](HowtoDetectFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Belirtilen modele sahip yüzleri algılama

Yüz algılama insan yüzlerinin sınırlayıcı kutusu konumlarını bulur ve görsel simgelerini tanımlar. Yüzün özelliklerini ayıklar ve [tanıma](../concepts/face-recognition.md) işlemlerinde daha sonra kullanmak üzere saklar.

Yüz - [Api] Algıla'yı kullandığınızda, model sürümünü `detectionModel` parametreyle atayabilirsiniz. Kullanılabilir değerler şunlardır:

* `detection_01`
* `detection_02`

Yüz için bir istek URL ' [si - REST] API'yi algıla şu şekilde görünecektir:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel][&detectionModel]&subscription-key=<Subscription key>`

İstemci kitaplığını kullanıyorsanız, uygun bir `detectionModel` dize geçirerek değeri atayabilirsiniz. Atanmamış olarak bırakırsanız, API varsayılan model sürümünü`detection_01`kullanır ( ). .NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, false, false, recognitionModel: "recognition_02", detectionModel: "detection_02");
```

## <a name="add-face-to-person-with-specified-model"></a>Belirtilen modelle Kişiye Yüz Ekleme

Face hizmeti, görüntüdeki yüz verilerini ayıklayabilir ve [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API aracılığıyla bir **Kişi** nesnesiyle ilişkilendirebilir. Bu API çağrısında, algılama modelini Yüz - [Algıla']daki gibi belirtebilirsiniz.

.NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
// Create a PersonGroup and add a person with face detected by "detection_02" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");

string personId = (await faceClient.PersonGroupPerson.CreateAsync(personGroupId, "My Person Name")).PersonId;

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.PersonGroupPerson.AddFaceFromUrlAsync(personGroupId, personId, imageUrl, detectionModel: "detection_02");
```

Bu kod, **PersonGroup** kimliği `mypersongroupid` olan bir Kişi Grubu oluşturur ve bu gruba bir **Kişi** ekler. Daha sonra `detection_02` modeli kullanarak bu **Kişiye** bir Yüz ekler. *AlgılamaModel* parametresini belirtmezseniz, API varsayılan modeli kullanır. `detection_01`

> [!NOTE]
> Bir **Kişi** nesnesindeki tüm yüzler için aynı algılama modelini kullanmanız gerekmez ve bir **Kişi** nesnesiyle karşılaştırmak için yeni yüzler algılarken aynı algılama modelini kullanmanız gerekmez (örneğin, Yüz [- API'yi tanımlayın).]

## <a name="add-face-to-facelist-with-specified-model"></a>Belirtilen modelle FaceList'e yüz ekleme

Varolan bir **FaceList** nesnesine yüz eklediğinizde bir algılama modeli de belirtebilirsiniz. .NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.FaceList.AddFaceFromUrlAsync(faceListId, imageUrl, detectionModel: "detection_02");
```

Bu kod, adı verilen `My face collection` bir **FaceList** oluşturur `detection_02` ve modelle birlikte bir Yüz ekler. *AlgılamaModel* parametresini belirtmezseniz, API varsayılan modeli kullanır. `detection_01`

> [!NOTE]
> Bir **FaceList** nesnesindeki tüm yüzler için aynı algılama modelini kullanmanız gerekmez ve **Bir FaceList** nesnesiyle karşılaştırmak için yeni yüzler algılarken aynı algılama modelini kullanmanız gerekmez.

## <a name="evaluate-different-models"></a>Farklı modelleri değerlendirme

Farklı yüz algılama modelleri farklı görevler için optimize edilebiyi sunar. Farklılıklara genel bakış için aşağıdaki tabloya bakın.

|**detection_01**  |**detection_02**  |
|---------|---------|
|Tüm yüz algılama işlemleri için varsayılan seçenek. | Mayıs 2019'da piyasaya sürüldü ve isteğe bağlı olarak tüm yüz algılama operasyonlarında kullanılabilir.
|Küçük, yan görünüm veya bulanık yüzler için optimize edilmedi.  | Küçük, yan görünümde ve bulanık yüzler üzerinde geliştirilmiş doğruluk. |
|Algılama çağrısında belirtilmişse yüz özniteliklerini (kafa pozu, yaş, duygu vb.) döndürür. |  Yüz öznitelikleri döndürmez.     |
|Algılama çağrısında belirtilmişse, yüz işaretlerini döndürür.   | Yüz işaretleri dönmez.  |

Ve `detection_02` modellerin performanslarını karşılaştırmanın `detection_01` en iyi yolu, bunları örnek bir veri kümesinde kullanmaktır. Her algılama modelini kullanarak [Yüz - Api'yi] çeşitli görüntülerde, özellikle de görülmesi zor birçok yüzün veya yüzlerin görüntülerinde api algılamanızı öneririz. Her modelin döndürdgösterdiği yüz sayısına dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı Yüz API'leri ile kullanılacak algılama modelini nasıl belirtileceğinizi öğrendiniz. Ardından, yüz algılama kullanmaya başlamak için hızlı bir başlangıç izleyin.

* [Yüz .NET SDK](../Quickstarts/csharp-sdk.md)
* [Yüz Python SDK](../Quickstarts/python-sdk.md)
* [Yüz Git SDK](../Quickstarts/go-sdk.md)

[Yüz - Algılama]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Yüz - Belirleme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[FaceList - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
