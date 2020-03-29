---
title: Tanıma modeli nasıl belirtilir - Yüz
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Yüz uygulamanızda hangi tanıma modelini kullanacağınızı nasıl seçeceğiniz gösterilecek.
services: cognitive-services
author: longli0
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: longl
ms.openlocfilehash: da9ad5576d146c007e45124668875e9681860ce6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76938834"
---
# <a name="specify-a-face-recognition-model"></a>Yüz tanıma modeli belirtme

Bu kılavuz, Azure Yüz hizmetini kullanarak yüz algılama, tanımlama ve benzerlik araması için yüz tanıma modelini nasıl belirtdiğinizi gösterir.

Face hizmeti, görüntülerdeki insan yüzleri ne zıddatları üzerinde işlem gerçekleştirmek için makine öğrenimi modellerini kullanır. Müşteri geri bildirimleri ve araştırmadaki gelişmelere dayanarak modellerimizin doğruluğunu geliştirmeye devam ediyoruz ve bu geliştirmeleri model güncellemeleri olarak sunuyoruz. Geliştiriciler, yüz tanıma modelinin hangi sürümünü kullanmak istediklerini belirtme seçeneğine sahiptir; kullanım durumlarına en uygun modeli seçebilirler.

Yeni bir kullanıcıysanız, en son modeli kullanmanızı öneririz. Model çakışmalarından kaçınırken farklı Yüz işlemlerinde nasıl belirtilen öğrenmek için okumaya devam edin. Gelişmiş bir kullanıcıysanız ve en son modele geçmeniz gerekip gerekmediğine emin değilseniz, yeni modeli değerlendirmek ve geçerli veri kümenizi kullanarak sonuçları karşılaştırmak için [farklı modelleri Değerlendir](#evaluate-different-models) bölümüne geçin.

## <a name="prerequisites"></a>Ön koşullar

AI yüz algılama ve tanımlama kavramlarına aşina olmalısınız. Değilseniz, önce şu nasıl yapılacağını anlatan kılavuzlara bakın:

* [Görüntüdeki yüzleri niçin algılarsın?](HowtoDetectFacesinImage.md)
* [Görüntüdeki yüzleri tanımlama](HowtoIdentifyFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Belirtilen modele sahip yüzleri algılama

Yüz algılama, insan yüzlerinin görsel simgelerini tanımlar ve sınırlayıcı kutusu konumlarını bulur. Ayrıca yüzün özelliklerini ayıklar ve kimlik kullanımı için saklar. Tüm bu bilgiler tek bir yüzün temsilini oluşturur.

Tanıma modeli, yüz özellikleri ayıklandığında kullanılır, böylece Algıla işlemi gerçekleştirirken bir model sürümü belirtebilirsiniz.

[Yüz - API'yi] algılayış ırken, `recognitionModel` model sürümünü parametreyle atayın. Kullanılabilir değerler şunlardır:

* `recognition_01`
* `recognition_02`

İsteğe bağlı olarak, _tanımaModeliyanıt_ olarak döndürülmelidir olup olmadığını belirtmek için _returnRecognitionModel_ parametre (varsayılan **yanlış)** belirtebilirsiniz. Yani, Yüz için bir istek URL [- Detect] REST API aşağıdaki gibi görünecektir:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel]&subscription-key=<Subscription key>`

İstemci kitaplığını kullanıyorsanız, sürümü temsil `recognitionModel` eden bir dize geçirerek değeri atayabilirsiniz.
Atanmamış olarak bırakırsanız, varsayılan model sürümü _(recognition_01)_ kullanılır. .NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, recognitionModel: "recognition_02", returnRecognitionModel: true);
```

## <a name="identify-faces-with-specified-model"></a>Belirtilen modele sahip yüzleri tanımlama

Yüz hizmeti, bir görüntüden yüz verilerini ayıklayabilir ve bir **Kişi** nesnesiyle ilişkilendirebilir (örneğin, [yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API ekle çağrısı aracılığıyla) ve birden çok **Kişi** nesnesi bir **PersonGroup'ta**birlikte depolanabilir. Daha sonra, yeni bir yüz bir **PersonGroup** [(Yüz ile] karşılaştırılabilir - Arama tanımlayın) ve bu grup içinde eşleşen kişi tanımlanabilir.

**Bir PersonGroup'un** tüm **Kişi**için tek bir benzersiz tanıma modeli `recognitionModel` olmalıdır ve grubu oluştururken bunu parametreyi kullanarak belirtebilirsiniz[(PersonGroup - Create] veya [LargePersonGroup - Create]). Bu parametreyi belirtmezseniz, `recognition_01` özgün model kullanılır. Bir grup her zaman oluşturulduğu tanıma modelini kullanır ve yeni yüzler bu modele eklendiğinde bu modelle ilişkilendirilir; bu, bir grubun oluşturulduktan sonra değiştirilemez. **Bir PersonGroup'un** hangi modelle yapılandırıldığı görmek için [PersonGroup ' u] kullanın - _ReturnRecognitionModel_ parametresi **doğru**olarak ayarlanmış API'yi alın.

.NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
// Create an empty PersonGroup with "recognition_02" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");
```

Bu kodda, **PersonGroup** kimliği `mypersongroupid` olan bir Kişi Grubu oluşturulur ve yüz özelliklerini ayıklamak için _recognition_02_ modelini kullanmak üzere ayarlanmıştır.

Buna bağlı olarak, bu **Kişi Grubu** [(Yüz - API algıla)] karşılaştırıldığında yüzleri algılarken hangi modeli kullanacağınızı belirtmeniz gerekir. Kullandığınız model her zaman **PersonGroup'un**yapılandırmasıyla tutarlı olmalıdır; aksi takdirde, işlem uyumsuz modeller nedeniyle başarısız olur.

Yüz'de değişiklik yok [- API'yi tanımla;] yalnızca algılama modeli sürümünü belirtmeniz gerekir.

## <a name="find-similar-faces-with-specified-model"></a>Belirtilen modele sahip benzer yüzleri bulma

Benzerlik araması için bir tanıma modeli de belirtebilirsiniz. [FaceList] ile yüz listesini `recognitionModel` oluştururken model sürümünü atayabilirsiniz - API oluştur veya [LargeFaceList - Oluştur]. Bu parametreyi belirtmezseniz, `recognition_01` özgün model kullanılır. Bir yüz listesi her zaman oluşturulduğu tanıma modelini kullanır ve yeni yüzler bu modele eklendiğinde bu modelle ilişkilendirilir; bu oluşturulduktan sonra değiştirilemez. Bir yüz listesinin hangi modelle yapılandırıldığı görmek [için, FaceList'i] kullanın - _ReturnRecognitionModel_ parametresi ile API'yi **doğru**olarak ayarlayın.

.NET istemci kitaplığı için aşağıdaki kod örneğine bakın.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");
```

Bu kod, özellik ayıklama için `My face collection` _recognition_02_ modelini kullanarak adlı bir yüz listesi oluşturur. Bu yüz listesini yeni algılanan bir yüze benzer yüzler için aradiğinizde, _bu yüzün recognition_02_ modeli kullanılarak algılanmış olması gerekir ([Yüz - Algılama]) Önceki bölümde olduğu gibi, model tutarlı olması gerekir.

Yüzde'de değişiklik yok [- Benzer API'yi bul;] yalnızca algılamada model sürümünü belirtirsiniz.

## <a name="verify-faces-with-specified-model"></a>Belirtilen modelle yüzleri doğrulama

[Yüz - ApI'yi doğrula,] iki yüzün aynı kişiye ait olup olmadığını denetler. Tanıma modelleri ile ilgili Olarak Doğrula API'sinde bir değişiklik yoktur, ancak yalnızca aynı modelle algılanan yüzleri karşılaştırabilirsiniz. Yani, iki yüz her ikisi de `recognition_01` kullanarak `recognition_02`tespit edilmiş olması gerekir ya da .

## <a name="evaluate-different-models"></a>Farklı modelleri değerlendirme

Verilerinizdeki _recognition_01_ ve _recognition_02_ modellerin performanslarını karşılaştırmak isterseniz, şunları yapmanız gerekir:

1. Sırasıyla _recognition_01_ ve _recognition_02_ iki **PersonGroup**s oluşturun.
1. Yüzleri algılamak ve bu iki **Person** **PersonGroup'un**kişilerine kaydetmek için görüntü verilerinizi kullanın ve [PersonGroup - Train] API ile eğitim sürecini tetikleyin.
1. Yüz le Test - Her iki **PersonGroup'ta**da [tanımlayın] ve sonuçları karşılaştırın.

Normalde bir güven eşiği (bir yüzü tanımlamak için modelin ne kadar emin olması gerektiğini belirleyen sıfır ile bir değer arasındaki değer) belirtirseniz, farklı modeller için farklı eşikler kullanmanız gerekebilir. Bir model için bir eşik başka bir modelle paylaşılmak için değildir ve mutlaka aynı sonuçları üretmek olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı Yüz hizmeti API'leri ile kullanılacak tanıma modelini nasıl belirtileceğinizi öğrendiniz. Ardından, yüz algılama kullanmaya başlamak için hızlı bir başlangıç izleyin.

* [Yüz .NET SDK](../Quickstarts/csharp-sdk.md)
* [Yüz Python SDK](../Quickstarts/python-sdk.md)
* [Yüz Git SDK](../Quickstarts/go-sdk.md)

[Yüz - Algılama]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Yüz - Benzer Bul]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Yüz - Belirleme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Yüz - Doğrula]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Alın]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Eğitme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Oluştur]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[LargeFaceList - Oluştur]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
