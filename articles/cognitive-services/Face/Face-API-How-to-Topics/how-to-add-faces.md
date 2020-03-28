---
title: 'Örnek: Kişi Grubuna yüz ekleme - Yüz'
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, Azure Bilişsel Hizmetler Yüz hizmetiyle bir PersonGroup nesnesine çok sayıda kişinin ve yüzün nasıl ekleyeceğini gösterir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 240905d538afc5c0f4b7f0e0bf400fac23c3183f
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169822"
---
# <a name="add-faces-to-a-persongroup"></a>Kişi Grubuna yüz ekleme

Bu kılavuz, bir PersonGroup nesnesine çok sayıda kişi ve yüz nasıl ekleyeceğinigösterir. Aynı strateji, LargePersonGroup, FaceList ve LargeFaceList nesneleri için de geçerlidir. Bu örnek, Azure Bilişsel Hizmetler Face .NET istemci kitaplığı kullanılarak C# olarak yazılır.

## <a name="step-1-initialization"></a>1. Adım: Başlatma

Aşağıdaki kod birkaç değişken bildirir ve yüz ekleme isteklerini zamanlamak için bir yardımcı işlev uygular:

- `PersonCount`, toplam kişi sayısıdır.
- `CallLimitPerSecond`, abonelik katmanına göre saniyedeki maksimum çağrı sayısıdır.
- `_timeStampQueue`, istek zaman damgalarını kaydetmek için kullanılan bir Kuyruktur.
- `await WaitCallLimitPerSecondAsync()`sonraki isteği göndermek için geçerli olana kadar bekler.

```csharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step-2-authorize-the-api-call"></a>2. Adım: API çağrısını yetkilendirme

İstemci kitaplığı kullandığınızda, abonelik anahtarınızı **FaceClient** sınıfının oluşturucusuna geçirmeniz gerekir. Örnek:

```csharp
private readonly IFaceClient faceClient = new FaceClient(
    new ApiKeyServiceClientCredentials("<SubscriptionKey>"),
    new System.Net.Http.DelegatingHandler[] { });
```

Abonelik anahtarını almak için Azure portalından Azure Marketi'ne gidin. Daha fazla bilgi için [Abonelikler'e](https://www.microsoft.com/cognitive-services/sign-up)bakın.

## <a name="step-3-create-the-persongroup"></a>3. Adım: PersonGroup oluşturma

Kişileri kaydetmek için "MyPersonGroup" adlı bir PersonGroup oluşturulur.
Genel doğrulama sağlamak için istek süresi, `_timeStampQueue` hedefinde kuyruğa alınır.

```csharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceClient.LargePersonGroup.CreateAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-for-the-persongroup"></a>Adım 4: PersonGroup için kişileri oluşturun

Kişiler aynı anda `await WaitCallLimitPerSecondAsync()` oluşturulur ve arama limitini aşmamak için de uygulanır.

```csharp
Person[] persons = new Person[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceClient.PersonGroupPerson.CreateAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>5. Adım: Kişilere yüz ekleme

Farklı kişilere eklenen yüzler aynı anda işlenir. Belirli bir kişi için eklenen yüzler sırayla işlenir.
Yine, `await WaitCallLimitPerSecondAsync()` istek sıklığı sınırlama kapsamında olduğundan emin olmak için çağrılır.

```csharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Özet

Bu kılavuzda, çok sayıda kişi ve yüze sahip bir Kişi Grubu oluşturma işlemini öğrendiniz. Bazı anımsatıcılar:

- Bu strateji, FaceLists ve LargePersonGroups için de geçerlidir.
- Farklı FaceLists'e veya LargePersonGroups'taki kişilere yüz ekleme veya silme işlemi aynı anda işlenir.
- Belirli bir FaceList'e veya LargePersonGroup'taki kişiye yüz ekleme veya silme sırayla yapılır.
- Basitlik için, olası bir özel durum nasıl işlenir bu kılavuzda atlanır. Daha sağlamlığı artırmak istiyorsanız, uygun yeniden deneme ilkesini uygulayın.

Aşağıdaki özellikler açıklanmış ve gösterilmiştir:

- [PersonGroup - CREATE](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API'yi kullanarak Kişi Grupları oluşturun.
- [PersonGroup Person - CREATE](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API kullanarak kişi oluşturun.
- [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API'yi kullanarak kişilere yüz ekleyin.

## <a name="related-topics"></a>İlgili konular

- [Görüntüdeki yüzleri tanımlama](HowtoIdentifyFacesinImage.md)
- [Bir görüntüdeki yüzleri algılama](HowtoDetectFacesinImage.md)
- [Büyük ölçek özelliğini kullanma](how-to-use-large-scale.md)
