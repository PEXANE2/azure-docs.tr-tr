---
title: 'Örnek: Görüntülerdeki yüzleri belirleme - Yüz'
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, önceden bilinen kişilerden oluşturulan PersonGroup nesnelerini kullanarak bilinmeyen yüzlerinasıl tanımlayılabildiğini gösterir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 0b1cf99fe6e2aa4d7fcb12c3fb96b10b42c7c0b7
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169908"
---
# <a name="example-identify-faces-in-images"></a>Örnek: Resimlerdeki yüzleri tanımlama

Bu kılavuz, önceden bilinen kişilerden oluşturulan PersonGroup nesnelerini kullanarak bilinmeyen yüzlerinasıl tanımlayılabildiğini gösterir. Örnekler, Azure Bilişsel Hizmetler Yüz istemci kitaplığı kullanılarak C# olarak yazılır.

## <a name="preparation"></a>Hazırlık

Bu örnek şunları göstermektedir:

- PersonGroup nasıl oluşturulur. Bu Kişi Grubu, bilinen kişilerin listesini içerir.
- Her kişiye yüz atama. Bu yüzler, kişileri tanımlamak için temel olarak kullanılır. Yüzlerin açık ön görünümlerini kullanmanızı öneririz. Bir örnek bir fotoğraflı kimliktir. İyi bir fotoğraf seti, farklı pozlarda, renk renklerinde veya saç stillerinde aynı kişinin yüzlerini içerir.

Bu örneğin gösterisini gerçekleştirmek için aşağıdakileri hazırlayın:

- Kişinin yüzünü içeren birkaç fotoğraf. Anna, Bill ve Clare için [örnek fotoğrafları indirin.](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)
- Bir dizi test fotoğrafı. Fotoğraflar Anna, Bill veya Clare'in yüzlerini içerebilir veya içermeyebilir. Kimlik tespitini test etmek için kullanılırlar. Ayrıca, önceki bağlantıdan bazı örnek görüntüler seçin.

## <a name="step-1-authorize-the-api-call"></a>1. Adım: API çağrısını yetkilendirme

Yüz Tanıma API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtar, sorgu dize parametresinden geçirilebilir veya istek üstbilgisinde belirtilebilir. Abonelik anahtarını sorgu dizesi üzerinden geçirmek için [Yüz - Algıla'nın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) istek URL'sine örnek olarak bakın:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Alternatif olarak, http istek üstbilgisinde abonelik **anahtarını belirtin &lt;ocp-apim-subscription-key: Abonelik Anahtarı&gt;**.
İstemci kitaplığı kullandığınızda, abonelik anahtarı FaceClient sınıfının oluşturucusundan geçirilir. Örnek:
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });
```
 
Abonelik anahtarını almak için Azure portalından Azure Marketi'ne gidin. Daha fazla bilgi için [Abonelikler'e](https://azure.microsoft.com/try/cognitive-services/)bakın.

## <a name="step-2-create-the-persongroup"></a>2. Adım: PersonGroup oluşturma

Bu adımda, "MyFriends" adlı bir PersonGroup Anna, Bill ve Clare içerir. Her kişinin kayıtlı birkaç yüzü vardır. Yüzler görüntülerden algılanmalıdır. Tüm bu adımlardan sonra, aşağıdaki görüntüye benzer bir PersonGroup öğeniz olur:

![Dostlarım](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>Adım 2.1: PersonGroup için kişileri tanımlayın
Kişi, temel bir tanımlama birimidir. Bir kişinin bir veya daha fazla bilinen yüzü kayıtlı olabilir. PersonGroup, bir insan topluluğudur. Her kişi belirli bir PersonGroup içinde tanımlanır. Kimlik tanımlaması bir PersonGroup'a karşı yapılır. Görev bir PersonGroup oluşturmak ve daha sonra anna, Bill ve Clare gibi içinde insanlar oluşturmaktır.

İlk olarak, [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API'yi kullanarak yeni bir Kişi Grubu oluşturun. Karşılık gelen istemci kitaplığı API, FaceClient sınıfı için CreatePersonGroupAsync yöntemidir. Grubu oluşturmak için belirtilen grup kimliği her abonelik için benzersizdir. Ayrıca, diğer PersonGroup API'lerini kullanarak Kişi Grupları alabilir, güncelleyebilir veya silebilirsiniz. 

Bir grup tanımlandıktan sonra, [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API'yi kullanarak içindeki kişileri tanımlayabilirsiniz. İstemci kitaplığı yöntemi, CreatePersonAsync yöntemidir. Oluşturulduktan sonra her kişiye bir yüz ekleyebilirsiniz.

```csharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceClient.PersonGroupPerson.CreateAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step-22-detect-faces-and-register-them-to-the-correct-person"></a><a name="step2-2"></a>Adım 2.2: Yüzleri algılama ve doğru kişiye kaydetme
HTTP istek gövdesinde görüntü dosyası ile [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API’sine bir “POST” web isteği gönderilerek algılama gerçekleştirilir. İstemci kitaplığını kullandığınızda, yüz algılama algıla... FaceClient sınıfının async yöntemleri.

Algılanan her yüz için [PersonGroup Person - Add Face'i](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) doğru kişiye eklemek için arayın.

Aşağıdaki kod, bir görüntüden yüzü algılama ve bu yüzü bir kişiye ekleme işlemini göstermektedir:

```csharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Görüntü birden fazla yüz içeriyorsa, yalnızca en büyük yüz eklenir. Kişiye başka yüzler ekleyebilirsiniz. [PersonGroup Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) - Face API'nın targetFace sorgu parametresine "targetFace = sol, üst, genişlik, yükseklik" biçiminde bir dize geçirin. AddPersonFaceAsync yöntemi için diğer yüzler eklemek için targetFace isteğe bağlı parametresini de kullanabilirsiniz. Kişiye eklenen her yüz, benzersiz bir kalıcı yüz kimliği verilir. Bu kimliği [PersonGroup Person – Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) and Face – Identify ['de](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)kullanabilirsiniz.

## <a name="step-3-train-the-persongroup"></a>3. Adım: PersonGroup’u eğitme

Kimlik kullanılarak bir tanımlama yapılabilmesi için PersonGroup'un eğitilmesi gerekir. PersonGroup, herhangi bir kişiyi ekledikten veya kaldırdıktan sonra veya bir kişinin kayıtlı yüzünü yeniden edindiğinizde yeniden eğitilmelidir. Eğitim, [PersonGroup – Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API’si tarafından gerçekleştirilir. İstemci kitaplığını kullandığınızda, TrainPersonGroupAsync yöntemine yapılan bir çağrıdır:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
Eğitim asynchronous bir süreçtir. TrainPersonGroupAsync yöntemi döndükten sonra bile tamamlanamayabilir. Eğitim durumunu sorgulamanız gerekebilir. [PersonGroup - Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API veya GetPersonGroupTrainingStatusAsync yöntemini istemci kitaplığı kullanın. Aşağıdaki kod, PersonGroup eğitiminin tamamlanmasını beklemenin basit bir mantığını gösterir:
 
```csharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceClient.PersonGroup.GetTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != TrainingStatusType.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>4. Adım: Tanımlı bir PersonGroup öğesine karşı yüzü belirleme

Face hizmeti tanımlamaları gerçekleştirdiğinde, bir grup içindeki tüm yüzler arasındaki test yüzünün benzerliğini hesaplar. Test yüzü için en karşılaştırılabilir kişileri döndürür. Bu işlem Yüz [- Tanım](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API veya istemci kitaplığın IdentifyAsync yöntemi üzerinden yapılır.

Test yüzü önceki adımlar kullanılarak algılanmalıdır. Daha sonra yüz kimliği ikinci bir bağımsız değişken olarak kimlik API'sine aktarılır. Aynı anda birden çok yüz kimliği tanımlanabilir. Sonuç, tanımlanan tüm sonuçları içerir. Varsayılan olarak, tanımlama işlemi test yüzle en iyi eşleşen yalnızca bir kişiyi döndürür. İstersenizin, tanımlama işleminin daha fazla aday alabilsin diye isteğe bağlı parametre maxNumOfCandidatesReturned'i belirtin.

Aşağıdaki kod tanımlama işlemini gösterir:

```csharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceClient.Face.DetectWithStreamAsync(s);
    var faceIds = faces.Select(face => face.FaceId.Value).ToArray();
 
    var results = await faceClient.Face.IdentifyAsync(faceIds, personGroupId);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceClient.PersonGroupPerson.GetAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Adımları bitirdikten sonra, farklı yüzleri tanımlamaya çalışın. Anna, Bill veya Clare'in yüzlerinin yüz algılama için yüklenen resimlere göre doğru bir şekilde tanımlanabiliyor mu diye bakın. Aşağıdaki örneklere bakın:

![Farklı yüzleri tanımlama](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Adım 5: Büyük ölçekli istek

Bir PersonGroup, önceki tasarım sınırlamasına bağlı olarak en fazla 10.000 kişiyi tutabilir.
Milyon ölçekli senaryolar hakkında daha fazla bilgi için bkz. [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md).

## <a name="summary"></a>Özet

Bu kılavuzda, bir Kişi Grubu oluşturma ve bir kişiyi tanımlama işlemini öğrendiniz. Aşağıdaki özellikler açıklanmış ve gösterilmiştir:

- Yüz - [ApI'yi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) algılayarak yüzleri algıla.
- [PersonGroup - CREATE](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API'yi kullanarak Kişi Grupları oluşturun.
- [PersonGroup Person - CREATE](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API kullanarak kişi oluşturun.
- PersonGroup – Train API'yi kullanarak bir [PersonGroup'u eğitin.](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
- [Yüz - ApI'yi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) Tanımla'yı kullanarak Kişi Grubu'na karşı bilinmeyen yüzleri tanımlayın.

## <a name="related-topics"></a>İlgili konular

- [Yüz tanıma kavramları](../concepts/face-recognition.md)
- [Bir görüntüdeki yüzleri algılama](HowtoDetectFacesinImage.md)
- [Yüz ekleme](how-to-add-faces.md)
- [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md)
