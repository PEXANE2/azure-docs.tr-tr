---
title: Yüz verilerinizi abonelikler arasında geçirin - Face
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, depolanan yüz verilerinizi bir Yüz aboneliğinden diğerine nasıl geçirilen gösterir.
services: cognitive-services
author: lewlu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: lewlu
ms.openlocfilehash: e5ca51da7322e4eab4ea364ec5da086a1068fa9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76169806"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Yüz verilerinizi farklı bir Yüz aboneliğine geçirin

Bu kılavuz, yüzleri olan kayıtlı bir Kişi Grubu nesnesi gibi yüz verilerini nasıl farklı bir Azure Bilişsel Hizmetler Yüz aboneliğine taşıyabileceğinizi gösterir. Verileri taşımak için Anlık Görüntü özelliğini kullanırsınız. Bu şekilde, işlemlerinizi taşıdığınızda veya genişletdiğinizde bir PersonGroup veya FaceList nesnesini tekrar tekrar oluşturmak ve eğitmek zorunda kalmanızı önlersiniz. Örneğin, ücretsiz deneme aboneliği kullanarak bir PersonGroup nesnesi oluşturup şimdi ücretli aboneliğinize geçirmek isteyebilirsiniz. Veya büyük bir kurumsal işlem için yüz verilerini farklı bölgelerdeki abonelikler arasında eşitlemeniz gerekebilir.

Aynı geçiş stratejisi, LargePersonGroup ve LargeFaceList nesneleri için de geçerlidir. Bu kılavuzdaki kavramlara aşina değilseniz, [yüz tanıma kavramları](../concepts/face-recognition.md) kılavuzundaki tanımlarına bakın. Bu kılavuzda C# ile Yüz .NET istemci kitaplığı kullanır.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki öğelere ihtiyacınız vardır:

- İki Face abonelik anahtarı, biri varolan verilerle ve diğeri geçirilebis. Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)yönergeleri izleyin.
- Hedef aboneliğe karşılık gelen Face abonelik kimliği dizesi. Bulmak için Azure portalında **Genel Bakış'ı** seçin. 
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Bu kılavuz, yüz veri geçişçalıştırmak için basit bir konsol uygulaması kullanır. Tam bir uygulama için GitHub'daki [Yüz anlık görüntüsü örneğine](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) bakın.

1. Visual Studio'da yeni bir Konsol uygulaması .NET Framework projesi oluşturun. **FaceApiSnapshotSample adı.**
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesini seçin ve **ön sürüm ekle'yi**seçin. Aşağıdaki paketi bulun ve kurun:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-önizleme](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Yüz istemcileri oluşturma

*Program.cs'daki* **Ana** yöntemde, kaynak ve hedef abonelikleriniz için iki [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) örneği oluşturun. Bu örnek, hedef olarak kaynak olarak Doğu Asya bölgesinde bir Yüz aboneliği ve Batı ABD aboneliği kullanır. Bu örnek, verilerin bir Azure bölgesinden diğerine nasıl geçirilen olduğunu gösterir. 

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Kaynak ve hedef abonelikleriniz için abonelik anahtar değerlerini ve bitiş noktası URL'lerini doldurun.


## <a name="prepare-a-persongroup-for-migration"></a>Bir Kişi Grubu'nun geçiş için hazırlanması

Kaynak aboneliğinizdeki Kişi Grubu kimliğinin hedef aboneliğe geçirilebin. [PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet) yöntemini kullanarak PersonGroup nesnelerinizin listesini alın. Ardından [PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId) özelliğini alın. Bu işlem, sahip olduğunuz PersonGroup nesnelerine bağlı olarak farklı görünür. Bu kılavuzda, kaynak PersonGroup Kimliği `personGroupId`.

> [!NOTE]
> [Örnek kod,](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) yeni bir PersonGroup oluşturur ve geçiş için eğitir. Çoğu durumda, zaten kullanmak için bir PersonGroup olmalıdır.

## <a name="take-a-snapshot-of-a-persongroup"></a>PersonGroup'un anlık görüntüsünü alın

Anlık görüntü, belirli Yüz veri türleri için geçici uzak depolama dır. Bir abonelikten diğerine veri kopyalamak için bir tür pano işlevi görür. İlk olarak, kaynak abonelikteki verilerin anlık görüntüsünü alırsınız. Ardından, hedef abonelikteki yeni bir veri nesnesine uygularsınız.

PersonGroup'un anlık görüntüsünü almak için kaynak aboneliğin FaceClient örneğini kullanın. [TakeAsync'i](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet) PersonGroup Kimliği ve hedef aboneliğin kimliğiyle birlikte kullanın. Birden çok hedef aboneliğiniz varsa, bunları üçüncü parametreye dizi girişleri olarak ekleyin.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> Anlık görüntü alma ve uygulama işlemi, kaynak veya hedef PersonGroups veya FaceLists'e yapılan düzenli çağrıları engellemez. [Örneğin, FaceList yönetim çağrıları](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) veya [PersonGroup Train](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) çağrısı gibi kaynak nesneyi değiştiren eşzamanlı aramalar yapmayın. Anlık görüntü işlemi bu işlemlerden önce veya sonra çalışabilir veya hatalarla karşılaşabilir.

## <a name="retrieve-the-snapshot-id"></a>Anlık görüntü kimliğini alma

Anlık görüntü almak için kullanılan yöntem asynchronous, bu nedenle tamamlanmasını beklemeniz gerekir. Anlık görüntü işlemleri iptal edilemez. Bu kodda, `WaitForOperation` yöntem asynchronous çağrı izler. Her 100 ms'de bir durumu kontrol eder. İşlem bittikten sonra, `OperationLocation` alanı ayrıştarak bir işlem kimliği alın. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Tipik `OperationLocation` bir değer şuna benzer:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

`WaitForOperation` Yardımcı yöntemi burada:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Çalışma durumu gösterildikten `Succeeded`sonra, döndürülen OperationStatus örneğinin `ResourceLocation` alanını ayrıştarak anlık görüntü kimliğini alın.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Tipik `resourceLocation` bir değer şuna benzer:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-a-snapshot-to-a-target-subscription"></a>Hedef aboneye anlık görüntü uygulama

Ardından, rasgele oluşturulan bir kimlik kullanarak hedef abonelikte yeni PersonGroup oluşturun. Ardından anlık görüntünün bu PersonGroup'a uygulanması için hedef aboneliğin FaceClient örneğini kullanın. Anlık görüntü kimliğini ve yeni PersonGroup Kimliğini geçirin.

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Anlık Görüntü nesnesi yalnızca 48 saat için geçerlidir. Yalnızca kısa bir süre sonra veri geçişi için kullanmak istiyorsanız anlık görüntü alın.

Anlık görüntü uygulama isteği başka bir işlem kimliği döndürür. Bu kimliği almak için, `OperationLocation` döndürülen Uygulama SnapshotResult örneğinin alanını ayrıştırın. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

Anlık görüntü uygulama işlemi de asynchronous, bu yüzden tekrar bitirmek için beklemek için kullanın. `WaitForOperation`

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Veri geçişini test edin

Anlık görüntü uyguladıktan sonra, hedef abonelikteki yeni Kişi Grubu orijinal yüz verileriyle birlikte doldurulur. Varsayılan olarak, eğitim sonuçları da kopyalanır. Yeni PersonGroup, yeniden eğitime gerek kalmadan yüz tanımlama çağrıları için hazırdır.

Veri geçişini sınamak için aşağıdaki işlemleri çalıştırın ve yazdırdıkları sonuçları konsolla karşılaştırın:

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Aşağıdaki yardımcı yöntemleri kullanın:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

Artık hedef abonelikte yeni PersonGroup'u kullanabilirsiniz. 

Hedef PersonGroup'u gelecekte yeniden güncelleştirmek için anlık görüntüalmak için yeni bir Kişi Grubu oluşturun. Bunu yapmak için, bu kılavuzdaki adımları izleyin. Tek bir PersonGroup nesnesine yalnızca bir kez anlık görüntü uygulanabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yüz verilerini geçirmeyi tamamladıktan sonra anlık görüntü nesnesini el ile silin.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Sonraki adımlar

Ardından, ilgili API başvuru belgelerine bakın, Anlık Görüntü özelliğini kullanan örnek bir uygulamayı keşfedin veya burada belirtilen diğer API işlemlerini kullanmaya başlamak için nasıl yapılacağını gösteren kılavuzu izleyin:

- [Anlık görüntü başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Yüz anlık örneği](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Yüz ekleme](how-to-add-faces.md)
- [Bir görüntüdeki yüzleri algılama](HowtoDetectFacesinImage.md)
- [Görüntüdeki yüzleri tanımlama](HowtoIdentifyFacesinImage.md)
