---
title: Modeller
description: Azure Uzaktan İşleme'de bir modelin ne olduğunu açıklar
author: jakrams
ms.author: jakras
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 770c88ddfb44004b76633cbeb726d28e1626a72c
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681733"
---
# <a name="models"></a>Modeller

Azure Uzaktan İşleme'deki bir *model,* [varlıklar](entities.md) ve [bileşenlerden](components.md)oluşan tam nesne gösterimini ifade eder. Modeller, özel verileri uzaktan işleme hizmetine sokmanın ana yoludur.

## <a name="model-structure"></a>Model yapısı

Bir modelin kök düğümü olarak tam olarak bir [varlığı](entities.md) vardır. Aşağıda alt varlıklar rasgele bir hiyerarşi olabilir. Bir model yüklenirken, bu kök varlığa bir başvuru döndürülür.

Her varlığın [bileşenleri](components.md) eklenmiş olabilir. En yaygın durumda, varlıklar *MeshComponents*var , hangi başvuru [örgü kaynakları](meshes.md).

## <a name="creating-models"></a>Modeller oluşturma

Giriş modellerinin FBX ve GLTF gibi dosya biçimlerinden [dönüştürülmesiyle](../how-tos/conversion/model-conversion.md) çalışma süresi için modeller oluşturma sağlanır. Dönüştürme işlemi dokular, malzemeler ve çerçeveler gibi tüm kaynakları ayıklar ve bunları en iyi duruma getirilmiş çalışma zamanı biçimlerine dönüştürür. Ayrıca yapısal bilgileri ayıklayacak ve bunu ARR'ın varlık/bileşen grafik yapısına dönüştürecektir.

> [!IMPORTANT]
>
> [Model dönüştürme,](../how-tos/conversion/model-conversion.md) [meshes](meshes.md)oluşturmak için tek yoldur. Kafesler çalışma zamanında varlıklar arasında paylaşılabilir, ancak bir model yükleme dışında çalışma süresi içine bir mesh almak için başka bir yolu yoktur.

## <a name="loading-models"></a>Yükleme modelleri

Bir model dönüştürüldükten sonra, Azure blob depolamadan çalışma süresine yüklenebilir.

Kıymetin blob depolamada ele alınma şekline göre farklılık gösteren iki farklı yükleme işlevi vardır:

* Model, SAS URI ile ele alınabilir. İlgili yükleme `LoadModelFromSASAsync` işlevi parametre `LoadModelFromSASParams`ile. [Yerleşik modelleri](../samples/sample-model.md)yüklerken de bu varyantı kullanın.
* Model blob depolama parametreleri doğrudan, [blob depolama hesabına bağlı durumda](../how-tos/create-an-account.md#link-storage-accounts)ele alınabilir. Bu durumda ilgili yükleme `LoadModelAsync` işlevi `LoadModelParams`parametre ile.

Aşağıdaki kod parçacıkları, her iki işleve de sahip modellerin nasıl yüklenir olduğunu gösterir. SAS URI kullanarak bir model yüklemek için aşağıdaki gibi kodu kullanın:

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

Bir modeli doğrudan blob depolama parametrelerini kullanarak yüklemek istiyorsanız, aşağıdaki parçacıka benzer kod kullanın:

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

    // ... (identical to the SAS URI snippet above)
}
```

Daha sonra varlık hiyerarşisi çapraz ve varlıkları ve bileşenleri değiştirebilirsiniz. Aynı modelin birden çok kez yüklenmesi, her biri varlık/bileşen yapısının kendi kopyasına sahip birden çok örnek oluşturur. Ancak, meshes, malzemeler ve dokular [paylaşılan kaynaklar](../concepts/lifetime.md)olduğundan, verileri yine yüklenmez. Bu nedenle, bir modeli birden fazla kez anlık olarak anons etmek, nispeten az bellek yüküne neden olabilir.

> [!CAUTION]
> ARR'daki tüm *Async* işlevleri eşsenkronize işlem nesnelerini döndürer. İşlem tamamlanana kadar bu nesnelere bir başvuru depolamanız gerekir. Aksi takdirde C# çöp toplayıcı işlemi erken silebilir ve asla bitiremez. Bekleme *nin* yukarıdaki örnek kodunda, model yüklemesi bitene kadar yerel değişken 'loadOp'un referans tuttuğunu garanti eder. Ancak, bunun yerine *Tamamlanan* olayı kullanacaksanız, asynchronous işlemini bir üye değişkende depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıklar](entities.md)
* [Kafes](meshes.md)
