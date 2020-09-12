---
title: Modeller
description: Bir modelin Azure uzaktan Işlemede ne olduğunu açıklar
author: jakrams
ms.author: jakras
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: e9c8c4a1209b8bb5be7af87ef22aeab0ffd90b79
ms.sourcegitcommit: 70ee014d1706e903b7d1e346ba866f5e08b22761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90023780"
---
# <a name="models"></a>Modeller

Azure uzaktan Işleme 'deki bir *model* , [varlıkların](entities.md) ve [bileşenlerden](components.md)oluşan tam bir nesne temsiline başvurur. Modeller, uzaktan işleme hizmetine özel veri almanın ana yoludur.

## <a name="model-structure"></a>Model yapısı

Bir modelin kök düğümü olarak tam olarak bir [varlığı](entities.md) vardır. Aşağıda, alt varlıkların rastgele bir hiyerarşisi olabilir. Bir model yüklerken, bu kök varlığa bir başvuru döndürülür.

Her varlıkta [bileşen](components.md) eklenmiş olabilir. En yaygın durumda, varlıkların, [kafes kaynaklarına](meshes.md)başvuran *meshcomponents*vardır.

## <a name="creating-models"></a>Modeller oluşturma

Çalışma zamanı için model oluşturma, FBX ve GLTF gibi dosya biçimlerinden [giriş modellerini dönüştürerek](../how-tos/conversion/model-conversion.md) elde edilir. Dönüştürme işlemi, dokular, malzemeler ve kafesler gibi tüm kaynakları ayıklar ve bunları iyileştirilmiş çalışma zamanı biçimlerine dönüştürür. Ayrıca yapısal bilgileri ayıklar ve bunu ARR 'nin varlık/bileşen grafik yapısına dönüştürür.

> [!IMPORTANT]
> [Model dönüştürme](../how-tos/conversion/model-conversion.md) , [kafeslerin](meshes.md)oluşturmanın tek yoludur. Kafesler, çalışma zamanında varlıklar arasında paylaşılabilecek olsa da, bir modeli yükleme dışında çalışma zamanına bir ağ almanın başka bir yolu yoktur.

## <a name="loading-models"></a>Modeller yükleniyor

Model dönüştürüldükten sonra, Azure Blob depolamadan çalışma zamanına yüklenebilir.

Varlığın BLOB depolama alanında ilgilenme yöntemiyle farklı olan iki farklı yükleme işlevi vardır:

* Model, SAS URI 'SI tarafından çözülebilir. İlgili yükleme işlevi `LoadModelFromSASAsync` parametresi ile `LoadModelFromSASParams` . [Yerleşik modeller](../samples/sample-model.md)yüklenirken de bu değişkeni kullanın.
* BLOB depolama, bu [Hesapla](../how-tos/create-an-account.md#link-storage-accounts)doğrudan BLOB depolama parametreleri ile çözülebilir. Bu durumda ilgili yükleme işlevi `LoadModelAsync` parametresiyle birlikte `LoadModelParams` .

Aşağıdaki kod parçacıkları, her iki işlevle de modellerin nasıl yükleneceğini gösterir. SAS URI 'sini kullanarak bir model yüklemek için aşağıdaki gibi bir kod kullanın:

```csharp
async void LoadModel(AzureSession session, Entity modelParent, string modelUri)
{
    // load a model that will be parented to modelParent
    var modelParams = new LoadModelFromSASParams(modelUri, modelParent);

    var loadOp = session.Actions.LoadModelFromSASAsync(modelParams);

    loadOp.ProgressUpdated += (float progress) =>
    {
        Debug.Log($"Loading: {progress * 100.0f}%");
    };

    await loadOp.AsTask();
}
```

```cpp
ApiHandle<LoadModelAsync> LoadModel(ApiHandle<AzureSession> session, ApiHandle<Entity> modelParent, std::string modelUri)
{
    LoadModelFromSASParams modelParams;
    modelParams.ModelUrl = modelUri;
    modelParams.Parent = modelParent;

    ApiHandle<LoadModelAsync> loadOp = *session->Actions()->LoadModelFromSASAsync(modelParams);

    loadOp->Completed([](const ApiHandle<LoadModelAsync>& async)
    {
        printf("Loading: finished.");
    });
    loadOp->ProgressUpdated([](float progress)
    {
        printf("Loading: %.1f%%", progress*100.f);
    });

    return loadOp;
}
```

Bir modeli doğrudan BLOB depolama parametrelerini kullanarak yüklemek istiyorsanız, aşağıdaki kod parçacığına benzer bir kod kullanın:

```csharp
async void LoadModel(AzureSession session, Entity modelParent, string storageAccount, string containerName, string assetFilePath)
{
    // load a model that will be parented to modelParent
    var modelParams = new LoadModelParams(
        storageAccount, // storage account name + '.blob.core.windows.net', e.g., 'mystorageaccount.blob.core.windows.net'
        containerName,  // name of the container in your storage account, e.g., 'mytestcontainer'
        assetFilePath,  // the file path to the asset within the container, e.g., 'path/to/file/myAsset.arrAsset'
        modelParent
    );

    var loadOp = session.Actions.LoadModelAsync(modelParams);

    // ... (identical to the SAS URI snippet above)
}
```

```cpp
ApiHandle<LoadModelAsync> LoadModel(ApiHandle<AzureSession> session, ApiHandle<Entity> modelParent, std::string storageAccount, std::string containerName, std::string assetFilePath)
{
    LoadModelParams modelParams;
    modelParams.Parent = modelParent;
    modelParams.Blob.StorageAccountName = std::move(storageAccount);
    modelParams.Blob.BlobContainerName = std::move(containerName);
    modelParams.Blob.AssetPath = std::move(assetFilePath);

    ApiHandle<LoadModelAsync> loadOp = *session->Actions()->LoadModelAsync(modelParams);
    // ... (identical to the SAS URI snippet above)
}
```

Daha sonra varlık hiyerarşisinde çapraz geçiş yapabilir ve varlıkları ve bileşenleri değiştirebilirsiniz. Aynı modeli birden çok kez yüklemek, her biri varlığın/bileşen yapısının kopyasıyla birlikte birden çok örnek oluşturur. Kafesler, malzemeler ve dokular [paylaşılan kaynaklar](../concepts/lifetime.md)olduğundan, verileri de yeniden yüklenmez. Bu nedenle, bir modelin birden çok kez örnekleniyor görece daha az bellek yükü doğurur.

> [!CAUTION]
> ARR 'deki tüm *zaman uyumsuz* işlevler zaman uyumsuz işlem nesneleri döndürüyor. İşlem tamamlanana kadar bu nesnelere bir başvuru depolamanız gerekir. Aksi halde, C# çöp toplayıcı işlemi erken silebilir ve hiçbir şekilde bitmeyebilir. Yukarıdaki örnek kodda, *await* öğesinin kullanımı, model yüklemesi tamamlanana kadar ' loadOp ' yerel değişkeninin bir başvuru bulundurduğunu garanti eder. Ancak, bunun yerine *tamamlanmış* olayını kullanıyorsanız, zaman uyumsuz işlemi bir üye değişkeninde depolamanız gerekir.

## <a name="api-documentation"></a>API belgeleri

* [C# RemoteManager. LoadModelAsync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadmodelasync)
* [C# RemoteManager. LoadModelFromSASAsync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadmodelfromsasasync)
* [C++ RemoteManager:: LoadModelAsync ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadmodelasync)
* [C++ RemoteManager:: LoadModelFromSASAsync ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadmodelfromsasasync)

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıklar](entities.md)
* [Ağ yapıları](meshes.md)
