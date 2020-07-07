---
title: 'Örnek: görüntülerde yüzeyleri tanımla-yüz'
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda, bilinen kişilerden önceden oluşturulan PersonGroup nesneleri kullanılarak bilinmeyen yüzlerin nasıl tanımlandığı gösterilmektedir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 07/02/2020
ms.author: sbowles
ms.openlocfilehash: 607f67258c5d069590f934891c09ccada780c977
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85918739"
---
# <a name="example-identify-faces-in-images"></a>Örnek: görüntülerdeki yüzeyleri tanımla

Bu kılavuzda, bilinen kişilerden önceden oluşturulan PersonGroup nesneleri kullanılarak bilinmeyen yüzlerin nasıl tanımlandığı gösterilmektedir. Örnekler, Azure bilişsel hizmetler yüz istemci kitaplığı kullanılarak C# dilinde yazılır.

Bu örnek şunları gösterir:

- Bir kişilik grubu oluşturma. Bu PersonGroup, bilinen kişilerin bir listesini içerir.
- Her kişiye yüz atama. Bu yüzeyler, kişileri tanımlamak için temel olarak kullanılır. Yüzlerin açık ve tamamen görünümlerini kullanmanızı öneririz. Fotoğraf KIMLIĞI bir örnektir. İyi bir fotoğraf kümesi, farklı pozlar, giyme renkleriyle veya ince stillerde aynı kişinin yüzlerini içerir.

## <a name="prerequisites"></a>Önkoşullar
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın ve anahtarınızı kopyalayın.
* Tanımlı kişilerin yüzlerine sahip birkaç fotoğraf. Anna, Bill ve Clare için [örnek fotoğraflar indirin](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) .
* Bir dizi test fotoğrafı. Fotoğraflar, tanımlanan kişilerin yüzlerini içermeyebilir veya içermeyebilir. Örnek fotoğraflar bağlantısındaki bazı görüntüleri kullanın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `face-identify` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*. 

```dotnetcli
dotnet new console -n face-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="step-1-authorize-the-api-call"></a>1. Adım: API çağrısını yetkilendirme

Yüz Tanıma API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtar, bir sorgu dizesi parametresi aracılığıyla geçirilebilir veya istek üstbilgisinde belirtilebilir. Bir istemci kitaplığı kullandığınızda, abonelik anahtarı **Faceclient** sınıfının Oluşturucusu aracılığıyla geçirilir. Aşağıdaki kodu *program.cs* dosyasının **Main** yöntemine ekleyin.
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
    new ApiKeyServiceClientCredentials("<subscription key>"),
    new System.Net.Http.DelegatingHandler[] { });
```

## <a name="step-2-create-the-persongroup"></a>2. Adım: PersonGroup oluşturma

Bu adımda, "Myarkadaşlar" adlı bir kişi **grubu** , Anna, faturanızı ve Clare 'yi içerir. Her kişinin kayıtlı birkaç yüzü vardır. Görüntülerden yüzler algılanmalıdır. Tüm bu adımları tamamladıktan sonra aşağıdaki görüntüde olduğu gibi bir **Persono grubu** vardır:

![Myarkadaşlar](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>Adım 2,1: PersonGroup için kişileri tanımlama
Kişi, temel bir tanımlama birimidir. Bir kişinin bir veya daha fazla bilinen yüzü kayıtlı olabilir. Bir **Persongroup** , bir kişi koleksiyonudur. Her kişi belirli bir **kişilik grubu**içinde tanımlanır. Kimlik bir **Persongroup 'a**göre yapılır. Görev, bir **Persongroup**oluşturmak ve ardından bu kişinin, örneğin Anna, Bill ve Clare gibi kişileri oluşturmak için kullanılır.

İlk olarak, [persongroup-Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API kullanarak yeni bir **persongroup** oluşturun. Karşılık gelen istemci kitaplığı API 'SI, **Faceclient** sınıfı için **CreatePersonGroupAsync** yöntemidir. Grubu oluşturmak için belirtilen grup KIMLIĞI her abonelik için benzersizdir. Ayrıca, diğer **kişiler** API 'lerini kullanarak da **persongroup**'ları alabilir, güncelleştirebilir veya silebilirsiniz. 

Bir grup tanımlandıktan sonra, kişinin içindeki kişileri, [Kişiskii grubu oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 'sini kullanarak tanımlayabilirsiniz. İstemci kitaplığı yöntemi **Createpersonasync**. Her kişiye, oluşturulduktan sonra bir yüz ekleyebilirsiniz.

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
### <a name="step-22-detect-faces-and-register-them-to-the-correct-person"></a><a name="step2-2"></a>Adım 2,2: yüzeyleri algılama ve bunları doğru kişiye kaydetme
HTTP istek gövdesinde görüntü dosyası ile [Yüz - Algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API’sine bir “POST” web isteği gönderilerek algılama gerçekleştirilir. İstemci kitaplığını kullandığınızda, algılama algılaması aracılığıyla yüz tanıma işlemi yapılır. FaceClient sınıfının zaman uyumsuz yöntemleri.

Algılanan her yüz için, [Persongroup kişisine](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) çağrı yapın ve doğru kişiye eklemek Için yüz ekleyin.

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
Görüntüde birden fazla yüz varsa yalnızca en büyük yüz eklenir. Kişiye başka yüzler ekleyebilirsiniz. "TargetFace = sol, üst, genişlik, yükseklik" biçiminde bir dizeyi [insana grubuna geçirin-yüz tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API 'Sinin targetface sorgu parametresini ekleyin. Diğer yüzeyleri eklemek için AddPersonFaceAsync yöntemi için targetFace isteğe bağlı parametresini de kullanabilirsiniz. Kişiye eklenen her yüz için benzersiz bir kalıcı yüz KIMLIĞI verilir. Bu KIMLIĞI [Kişiskii grubu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) içinde kullanabilirsiniz; yüz ve [yüzü Sil – tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>3. Adım: PersonGroup’u eğitme

Bir kimlik kullanılarak bir kimlik gerçekleştirilmeden önce **Persongroup** 'un eğitilmeli olması gerekir. Kişi ekledikten veya kaldırdıktan sonra ya da bir kişinin kayıtlı yüzünü düzenlerseniz, **Persongroup** 'un geri çekilmesi gerekir. Eğitim, [PersonGroup – Eğitim](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API’si tarafından gerçekleştirilir. İstemci kitaplığını kullandığınızda, **TrainPersonGroupAsync** yöntemine bir çağrıdir:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
Eğitim zaman uyumsuz bir işlemdir. **TrainPersonGroupAsync** yöntemi dönüşdikten sonra bile bitmeyebilir. Eğitim durumunu sorgulamanızı gerekebilir. İstemci kitaplığının [Persongroup-Get eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API 'Sini veya **Getpersongrouptraıningstatusasync** metodunu kullanın. Aşağıdaki kod, **Persongroup** eğitiminin tamamlanmasını beklerken basit bir mantığı göstermektedir:
 
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

Yüz hizmeti tanımlamaları yaptığında, bir grup içindeki tüm yüzler arasında bir test yüzünün benzerliğini hesaplar. Test yüzü için en çok karşılaştırılabilen kişileri döndürür. Bu işlem, istemci kitaplığının [yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) tanıma API 'Si veya ıdentıyasync yöntemi aracılığıyla yapılır.

Önceki adımlar kullanılarak test yüzü algılanmalıdır. Ardından, yüz KIMLIĞI kimlik API 'sine ikinci bir bağımsız değişken olarak geçirilir. Birden çok yüz kimliği, bir kerede tanımlanabilir. Sonuç, tüm tanımlanan sonuçları içerir. Varsayılan olarak, kimlik işlemi en iyi test yüzüyle eşleşen bir kişi döndürür. İsterseniz, kimlik işleminin daha fazla aday döndürmesini sağlamak için isteğe bağlı _maxNumOfCandidatesReturned_ parametresini belirtin.

Aşağıdaki kod, tanımlama işlemini göstermektedir:

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

Adımları tamamladıktan sonra farklı yüzeyleri belirlemeyi deneyin. Gamze algılama için karşıya yüklenen görüntülere, faturanızda veya Clare 'nin yüzlerine göre doğru şekilde tanımlanabilir olup olmadığını görün. Aşağıdaki örneklere bakın:

![Farklı yüzeyleri tanımla](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>5. Adım: büyük ölçekli Istek

Bir **Persongroup** , önceki tasarım sınırlamasını temel alan 10.000 kişiden fazlasını tutabilir. Milyon ölçekli senaryolar hakkında daha fazla bilgi için bkz. [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md).

## <a name="summary"></a>Özet

Bu kılavuzda, bir **Persongroup** oluşturma ve bir kişiyi tanımlama sürecini öğrendiniz. Aşağıdaki özellikler açıklanmıştı ve gösterilmiştir:

- [Yüz algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API 'sini kullanarak yüzeyleri tespit edin.
- [Persongroup-Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API ' i kullanarak kişilik grupları oluşturun.
- Kişiler [-oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API 'Sini kullanarak kişi oluşturun.
- [Persongroup – tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API 'sini kullanarak bir persongroup 'ı eğitme.
- [Yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) tanıma API 'sini kullanarak, persongroup 'a karşı bilinmeyen yüzeyleri tespit edin.

## <a name="related-topics"></a>İlgili konular

- [Yüz tanıma kavramları](../concepts/face-recognition.md)
- [Bir görüntüdeki yüzleri algılama](HowtoDetectFacesinImage.md)
- [Yüz ekleme](how-to-add-faces.md)
- [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md)
