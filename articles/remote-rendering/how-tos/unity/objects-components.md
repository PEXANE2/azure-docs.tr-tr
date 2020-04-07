---
title: Unity oyun nesneleri ve bileşenleri
description: Uzaktan İşleme varlıkları ve bileşenleriyle çalışmak için Birliğe özgü yöntemleri açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/28/2020
ms.topic: how-to
ms.openlocfilehash: 872f0e60943a674e5febf269dcb5b09fd08d77c9
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681161"
---
# <a name="interact-with-unity-game-objects-and-components"></a>Unity oyun nesneleri ve bileşenleri ile etkileşim

Azure Uzaktan İşleme (ARR) çok sayıda nesne için optimize edilmiştir (bkz. [Sınırlar).](../../reference/limits.md) Ana bilgisayarda büyük ve karmaşık hiyerarşileri yönetmek mümkün olsa da, bunların hepsini düşük güçlü cihazlarda Unity'de çoğaltmak mümkün değildir.

Bu nedenle, bir model ana bilgisayara yüklendiğinde, Azure Uzaktan İşleme istemci aygıtındaki (ağ trafiğine neden olacak) model yapısı hakkındaki bilgileri yansıtmaktadır, ancak Unity'deki nesneleri ve bileşenleri çoğaltmaz. Bunun yerine, gerekli Unity oyun nesnelerini ve bileşenlerini el ile talep etmenizi bekler, böylelikle genel yükü gerçekte gerekenlerle sınırlandırabilirsiniz. Bu şekilde istemci tarafı performansı üzerinde daha fazla denetime sahip olabilirsiniz.

Sonuç olarak, Azure Uzaktan İşleme'nin Birlik tümleştirmesi, Uzaktan İşlem yapısını isteğe bağlı olarak çoğaltmak için ek işlevlerle birlikte gelir.

## <a name="load-a-model-in-unity"></a>Unity'de bir modeli yükleme

Bir modeli yüklediğinizde, yüklenen modelin kök nesnesine bir başvuru alırsınız. Bu başvuru bir Unity oyun nesnesi değildir, ancak uzantı yöntemini `Entity.GetOrCreateGameObject()`kullanarak bir nesneye dönüştürebilirsiniz. Bu işlev türünde `UnityCreationMode`bir argüman bekliyor. Geçerseniz, `CreateUnityComponents`yeni oluşturulan Unity oyun nesnesi ayrıca ana bilgisayarda bulunan tüm Uzaktan İşlembileşenleri için proxy bileşenleriyle doldurulur. Bu tavsiye edilir, ancak, tercih `DoNotCreateUnityComponents`etmek , havai minimum tutmak için.

### <a name="load-model-with-task"></a>Görevle birlikte yükleme modeli

```cs
LoadModelAsync _pendingLoadTask = null;
void LoadModelWithTask()
{
    _pendingLoadTask = RemoteManagerUnity.CurrentSession.Actions.LoadModelFromSASAsync(new LoadModelFromSASParams("builtin://Engine"));

    _pendingLoadTask.Completed += (LoadModelAsync res) =>
    {
        // turn the root object into a Unity game object
        var gameObject = res.Result.Root?.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
        _pendingLoadTask = null;
    };

    // also listen to progress updates:
    _pendingLoadTask.ProgressUpdated += (float progress) =>
    {
        // progress is a fraction in [0..1] range
        int percentage = (int)(progress * 100.0f);
        // do something...
        // Since the updates are triggered by the main thread, we may access unity objects here.
    };
}
```

### <a name="load-model-with-unity-coroutines"></a>Unity coroutines ile yük modeli

```cs
IEnumerator LoadModelWithCoroutine()
{
    LoadModelAsync task = RemoteManagerUnity.CurrentSession.Actions.LoadModelFromSASAsync(new LoadModelFromSASParams("builtin://Engine"));

    while (!task.IsCompleted)
    {
        int percentage = (int)(task.Progress * 100.0f);
        yield return null;
    }

    if (!task.IsFaulted)
    {
        var gameObject = task.Result.Root?.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
    }

    task = null;
}
```

### <a name="load-model-with-await-pattern"></a>Bekleme deseni ile yük modeli

```cs
async void LoadModelWithAwait()
{
    var result = await RemoteManagerUnity.CurrentSession.Actions.LoadModelFromSASAsync(new LoadModelFromSASParams("builtin://Engine")).AsTask();
    var gameObject = result.Root?.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
}
```

Yukarıdaki kod örnekleri, yerleşik model yüklendiğinden, SAS üzerinden model yükleme yolunu kullandınız. Blob kapları (kullanarak `LoadModelAsync` ve `LoadModelParams`) üzerinden model adresleme tamamen benzer çalışır.

## <a name="remoteentitysyncobject"></a>RemoteEntitySyncObject

Bir Unity oyun nesnesi `RemoteEntitySyncObject` oluşturmak dolaylı olarak oyun nesnesine bir bileşen ekler. Bu bileşen, varlık dönüşüm sunucuya eşitlemek için kullanılır. Varsayılan `RemoteEntitySyncObject` olarak, kullanıcının yerel `SyncToRemote()` Birlik durumunu sunucuya eşitlemek için açıkça aramasını gerektirir. Etkinleştirme `SyncEveryFrame` nesneyi otomatik olarak eşitler.

A'ya `RemoteEntitySyncObject` sahip nesneler, Uzak çocuklarınanlık bir şekilde, Çocukları **Göster** düğmesi aracılığıyla Unity düzenleyicisinde gösterilebilir.

![RemoteEntitySyncObject](media/remote-entity-sync-object.png)

## <a name="wrapper-components"></a>Sarma bileşenleri

Uzaktan İşleme varlıklarına bağlı [bileşenler](../../concepts/components.md) proxy `MonoBehavior`s aracılığıyla Unity'ye maruz kalır. Bu yakınlıklar Unity'deki uzak bileşeni temsil eder ve tüm değişiklikleri ana bilgisayara iletir.

Proxy Uzaktan İşleme bileşenleri oluşturmak için `GetOrCreateArrComponent`uzantı yöntemini kullanın:

```cs
var cutplane = gameObject.GetOrCreateArrComponent<ARRCutPlaneComponent>(RemoteManagerUnity.CurrentSession);
```

## <a name="coupled-lifetimes"></a>Birleştirilmiş ömürler

Uzak bir [varlığın](../../concepts/entities.md) ve Unity oyun nesnesinin ömrü birbirime geçerken bir `RemoteEntitySyncObject`.. Böyle bir `UnityEngine.Object.Destroy(...)` oyun nesnesi ile ararsanız, uzak varlık da kaldırılır.

Unity oyun nesnesini yok etmek için, uzak varlığı `Unbind()` etkilemeden, öncelikle `RemoteEntitySyncObject`.

Aynı tüm proxy bileşenleri için geçerlidir. Yalnızca istemci tarafı temsilini yok etmek `Unbind()` için önce proxy bileşenini aramanız gerekir:

```cs
var cutplane = gameObject.GetComponent<ARRCutPlaneComponent>();
if (cutplane != null)
{
    cutplane.Unbind();
    UnityEngine.Object.Destroy(cutplane);
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Birlik için Uzaktan İşleme'yi Ayarlama](unity-setup.md)
* [Öğretici: Unity içinde uzak varlıklarla çalışma](../../tutorials/unity/working-with-remote-entities.md)
