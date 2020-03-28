---
title: 'Örnek: Büyük Ölçekli özelliği kullanın - Yüz'
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, varolan PersonGroup ve FaceList nesnelerinden LargePersonGroup ve LargeFaceList nesnelerine nasıl ölçeklendirilene ilişkin bir makaledir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/01/2019
ms.author: sbowles
ms.openlocfilehash: dc0964e40e9214e414d865c06006f1d36e97eeb2
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169777"
---
# <a name="example-use-the-large-scale-feature"></a>Örnek: Büyük ölçekli özelliği kullanın

Bu kılavuz, varolan PersonGroup ve FaceList nesnelerinden sırasıyla LargePersonGroup ve LargeFaceList nesnelerine nasıl ölçeklendirilene ilişkin gelişmiş bir makaledir. Bu kılavuz, geçiş işlemini gösterir. PersonGroup ve FaceList nesneleri, [Tren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4) işlemi ve yüz tanıma işlevleri ile temel bir aşinalık varsayar. Bu konular hakkında daha fazla bilgi edinmek için [Yüz tanıma](../concepts/face-recognition.md) kavramsal kılavuzuna bakın.

LargePersonGroup ve LargeFaceList topluca büyük ölçekli işlemler olarak adlandırılır. LargePersonGroup, her biri en fazla 248 yüze sahip en fazla 1 milyon kişi içerebilir. LargeFaceList en fazla 1 milyon yüz içerebilir. Büyük ölçekli işlemler geleneksel PersonGroup ve FaceList benzer, ancak yeni mimari nedeniyle bazı farklılıklar var. 

Örnekler, Azure Bilişsel Hizmetler Yüz istemci kitaplığı kullanılarak C# olarak yazılır.

> [!NOTE]
> Büyük ölçekte Kimlik ve FindSimilar için Yüz arama performansını etkinleştirmek için, LargeFaceList ve LargePersonGroup'u önceden işlemek için bir Tren işlemi başlatın. Eğitim süresi, gerçek kapasiteye bağlı olarak saniyeile yaklaşık yarım saat arasında değişir. Eğitim süresi boyunca, daha önce başarılı bir eğitim icra edildiyse Kimlik ve FindSimilar'i gerçekleştirmek mümkündür. Dezavantajı, yeni eklenen kişi ve yüzleri büyük ölçekli eğitim için yeni bir post migration tamamlanana kadar sonuç görünmüyor olmasıdır.

## <a name="step-1-initialize-the-client-object"></a>Adım 1: İstemci nesnesini başlatma

Face istemci kitaplığını kullandığınızda, abonelik anahtarı ve abonelik bitiş noktası FaceClient sınıfının oluşturucusundan geçirilir. Örnek:

```csharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionEndpoint = "https://westus.api.cognitive.microsoft.com";
private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });
faceClient.Endpoint = SubscriptionEndpoint
```

Abonelik anahtarını ilgili bitiş noktasıyla almak için Azure portalından Azure Marketi'ne gidin.
Daha fazla bilgi için [Abonelikler'e](https://azure.microsoft.com/services/cognitive-services/directory/vision/)bakın.

## <a name="step-2-code-migration"></a>Adım 2: Kod geçişi

Bu bölümde, PersonGroup veya FaceList uygulamasının LargePersonGroup veya LargeFaceList'e nasıl geçirilene odaklanıştÝr. LargePersonGroup veya LargeFaceList tasarım ve dahili uygulamada PersonGroup veya FaceList'ten farklı olsa da, API arabirimleri geriye dönük uyumluluk için benzerdir.

Veri geçişi desteklenmez. Bunun yerine LargePersonGroup veya LargeFaceList'i yeniden oluşturursunuz.

### <a name="migrate-a-persongroup-to-a-largepersongroup"></a>Kişi Grubunu Bir LargePersonGroup'a geçirme

PersonGroup'tan LargePersonGroup'a geçiş yapmak kolaydır. Grup düzeyindeki işlemleri tam olarak paylaşırlar.

PersonGroup veya kişi ile ilgili uygulama için yalnızca API yollarını veya SDK sınıf/modülünü LargePersonGroup ve LargePersonGroup Person olarak değiştirmek gerekir.

PersonGroup'taki tüm yüzleri ve kişileri yeni LargePersonGroup'a ekleyin. Daha fazla bilgi için [bkz.](how-to-add-faces.md)

### <a name="migrate-a-facelist-to-a-largefacelist"></a>Bir FaceList'i LargeFaceList'e Geçirin

| FaceList API’leri | LargeFaceList API’leri |
|:---:|:---:|
| Oluşturma | Oluşturma |
| Sil | Sil |
| Al | Al |
| Liste | Liste |
| Güncelleştirme | Güncelleştirme |
| - | Eğitim |
| - | Eğitim Durumunu Alma |

Yukarıdaki tabloda, FaceList ile LargeFaceList arasındaki liste düzeyinde işlemlerin karşılaştırması yer almaktadır. Gösterildiği gibi, LargeFaceList facelist ile karşılaştırıldığında, yeni operasyonlar, Tren ve Eğitim Durumu alın ile birlikte gelir. LargeFaceList'i eğitmek [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işleminin bir ön koşuludur. FaceList için eğitim gerekmez. Aşağıdaki parçacık, Bir LargeFaceList'in eğitimini beklemek için yardımcı işlevdir:

```csharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceClient.LargeTrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await faceClient.LargeFaceList.TrainAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Daha önce, eklenen yüzleri ve FindSimilar ile FaceList tipik bir kullanımı aşağıdaki gibi görünüyordu:

```csharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
faceClient.FaceList.CreateAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await faceClient.FaceList.AddFaceFromStreamAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = faceClient.Face.DetectWithStreamAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await faceClient.Face.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

LargeFaceList'e geçirildiğinde aşağıdaki ler olur:

```csharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
faceClient.LargeFaceList.CreateAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await faceClient.LargeFaceList.AddFaceFromStreamAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = faceClient.Face.DetectWithStreamAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await faceClient.Face.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Daha önce gösterildiği gibi, veri yönetimi ve FindSimilar parçası hemen hemen aynıdır. Bunun tek istisnası, FindSimilar çalışmadan önce büyük yüz listesinde yeni bir ön işleme Tren işleminin tamamlanması gerektiğidir.

## <a name="step-3-train-suggestions"></a>Adım 3: Tren önerileri

Tren operasyonu [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [Identification'ı](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)hızlandırsa da, eğitim süresi, özellikle büyük ölçekli gelirken zarar görür. Farklı ölçeklerde tahmini eğitim süresi aşağıdaki tabloda listelenmiştir.

| Yüzler veya kişiler için ölçeklendirme | Tahmini eğitim süresi |
|:---:|:---:|
| 1000 | 1-2 sn |
| 10,000 | 5-10 sn |
| 100.000 | 1-2 dk |
| 1.000.000 | 10-30 dk |

Büyük ölçekli özelliği daha iyi kullanmak için aşağıdaki stratejileri öneririz.

### <a name="step-31-customize-time-interval"></a>Adım 3.1: Zaman aralığını özelleştirme

Gösterildiği `TrainLargeFaceList()`gibi, sonsuz eğitim durumu kontrol işlemini geciktirmek için milisaniye cinsinden bir zaman aralığı vardır. Daha fazla yüz içeren LargeFaceList için, büyük bir aralık kullanıldığında çağrı sayıları ve maliyeti azaltılır. Zaman aralığını LargeFaceList'in beklenen kapasitesine göre özelleştirin.

Aynı strateji LargePersonGroup için de geçerlidir. Örneğin, 1 milyon kişiiçeren bir LargePersonGroup `timeIntervalInMilliseconds` eğittiyseniz, 1 dakikalık bir aralık 60.000 olabilir.

### <a name="step-32-small-scale-buffer"></a>Adım 3.2: Küçük ölçekli arabellek

Bir LargePersonGroup veya LargeFaceList'teki kişiler veya yüzler, ancak eğitildikten sonra aranabilir. Dinamik bir senaryoda, yeni kişiler veya yüzler sürekli olarak eklenir ve hemen aranabilir olmalıdır, ancak eğitim istenenden daha uzun sürebilir. 

Bu sorunu azaltmak için, yalnızca yeni eklenen girişler için arabellek olarak fazladan küçük ölçekli bir LargePersonGroup veya LargeFaceList kullanın. Bu arabellek nedeniyle daha küçük boyutu eğitmek için daha kısa bir zaman alır. Bu geçici arabellekteki hemen arama özelliği çalışmalıdır. Bu arabelleği, usta LargePersonGroup veya LargeFaceList'teki eğitimle birlikte, usta eğitimini daha seyrek bir aralıkta çalıştırarak kullanın. Örnekler gece ve günlük gece yarısı vardır.

Örnek bir iş akışı:

1. Ana koleksiyon olan bir büyük LargePersonGroup veya LargeFaceList oluşturun. Arabellek koleksiyonu olan bir arabellek LargePersonGroup veya LargeFaceList oluşturun. Arabellek koleksiyonu yalnızca yeni eklenen kişiler veya yüzler içindir.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna yeni kişiler veya yüzler ekleyin.
1. Yeni eklenen girişlerin etkili olduğundan emin olmak için arabellek koleksiyonunu yalnızca kısa bir zaman aralığıyla eğitin.
1. Hem ana koleksiyona hem de arabellek koleksiyonuna karşı Kimlik veya FindSimilar'i arayın. Sonuçları birleştirin.
1. Arabellek toplama boyutu bir eşiğe veya sistem boşta yken arttığında, yeni bir arabellek koleksiyonu oluşturun. Ana koleksiyonda Tren işlemini tetikle.
1. Ana koleksiyonda Tren işlemi bittikten sonra eski arabellek koleksiyonunu silin.

### <a name="step-33-standalone-training"></a>Adım 3.3: Bağımsız eğitim

Nispeten uzun bir gecikme sonları kabul edilebilirse, yeni veri ekledikten hemen sonra Tren işlemini tetiklemeniz gerekmez. Bunun yerine Eğitim işlemi, ana mantıktan ayrılabilir ve düzenli olarak tetiklenebilir. Bu strateji, kabul edilebilir gecikme gecikmesi olan dinamik senaryolar için uygundur. Tren frekansını daha da azaltmak için statik senaryolara uygulanabilir.

Buna benzer bir `TrainLargePersonGroup` işlev `TrainLargeFaceList`olduğunu varsayalım. Bir LargePersonGroup'ta [`Timer`](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) sınıfı çağırarak bağımsız eğitimin tipik bir `System.Timers` uygulaması şudur:

```csharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    faceClient.LargePersonGroup.CreateAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Set up standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1-minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1-hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces, and identification, except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Veri yönetimi ve kimlikle ilgili uygulamalar hakkında daha [Identify faces in an image](HowtoIdentifyFacesinImage.md)fazla bilgi için [bkz.](how-to-add-faces.md)

## <a name="summary"></a>Özet

Bu kılavuzda, varolan Kişi Grubu veya FaceList kodunu niçin verileri değil, LargePersonGroup'a veya LargeFaceList'e nasıl geçirtileceğinizi öğrendiniz:

- LargePersonGroup ve LargeFaceList, Train işleminin LargeFaceList tarafından gerekli olması dışında Kişi Grubu veya FaceList'e benzer çalışır.
- Büyük ölçekli veri kümeleri için dinamik veri güncelleştirmesi için uygun Tren stratejisini alın.

## <a name="next-steps"></a>Sonraki adımlar

Bir Kişi Grubu'na nasıl yüz ekleyeceğinizi veya Bir Kişi Grubu'ndaki Tanım İşlemini nasıl yürüteceklerini öğrenmek için nasıl nasıl yapılacağınız kılavuzunu izleyin.

- [Yüz ekleme](how-to-add-faces.md)
- [Görüntüdeki yüzleri tanımlama](HowtoIdentifyFacesinImage.md)
