---
title: Unity’de uzak varlıklarla çalışma
description: ARR varlıklarıyla çalışmayı gösteren öğretici.
author: florianborn71
ms.author: flborn
ms.date: 02/01/2020
ms.topic: tutorial
ms.openlocfilehash: 5d995e9a5cdb6fc18532e0c3533959e9feece908
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021254"
---
# <a name="tutorial-working-with-remote-entities-in-unity"></a>Öğretici: Unity 'de uzak varlıklarla çalışma

[Öğretici: sıfırdan bir Unity projesini ayarlama](project-setup.md) , yeni bir Unity projesinin Azure uzaktan işleme ile çalışması için nasıl yapılandırılacağını gösterdi. Bu öğreticide, her ARR kullanıcısına ihtiyacı olan en yaygın işlevselliğe göz atacağız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Işlarını kullanarak nesneleri seçin.
> * Renk tonu rengi, seçim durumu ve görünürlük gibi nesne durumlarını geçersiz kılın.
> * Uzak varlıkları silin.
> * Uzak varlıkları etrafında taşıyın.
> * Nesneleri içine bakmak için kesme düzlemleri kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici, öğreticinin en üstünde oluşturulur [: bir Unity projesini sıfırdan ayarlama](project-setup.md).

> [!TIP]
> [ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) , *Unity* klasöründe başvuru olarak kullanabileceğiniz tüm öğreticiler için hazırlanan Unity projelerini içerir.

## <a name="pick-objects"></a>Nesneleri Seç

Nesnelerle etkileşim kurmak istiyoruz, bu nedenle ihtiyaç duyduğumuz ilk şey, fare imleci altında nesne çekiyor.

**Remoteraycaster** adlı [Yeni bir betik](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve tüm içeriğini aşağıdaki kodla değiştirin:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;

[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRaycaster : MonoBehaviour
{
    public double MaxDistance = 30.0;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        arrService = GetComponent<ARRServiceUnity>();
    }

    private async Task<Entity> RemoteRayCast(Vector3 origin, Vector3 dir)
    {
        Entity entity = null;

        var raycast = new RayCast(origin.toRemotePos(), dir.toRemoteDir(), MaxDistance, HitCollectionPolicy.ClosestHit);

        var hits = await arrService.CurrentActiveSession.Actions.RayCastQueryAsync(raycast).AsTask();

        if (hits != null)
        {
            foreach (var hit in hits)
            {
                var hitEntity = hit.HitEntity;
                if (hitEntity == null)
                {
                    continue;
                }

                entity = hitEntity;
                break;
            }
        }

        return entity;
    }

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
        var ray = Camera.main.ScreenPointToRay(position);

        Raycast(ray.origin, ray.direction);
    }

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var entity = await RemoteRayCast(origin, direction);
        if (entity != null)
        {
            Debug.Log("Object Hit: " + entity.Name);
        }
    }
}
```

Bu bileşeni sahnenin *RemoteRendering* nesnesine ekleyin.

> [!WARNING]
>
> *Remoteraycaster* bileşeni, bir *ARRServiceUnity* bileşeninin aynı nesneye bağlı olmasını gerektirir. *ARRServiceUnity* , bazı ARR işlevlerine daha kolay erişmek için yardımcı bir sınıftır. Ancak, sahnede bu bileşenin yalnızca tek bir örneği olabilir. Bu nedenle, *ARRServiceUnity* gerektiren tüm bileşenleri aynı gameobject öğesine eklediğinizden emin olun.
> Birden çok oyun nesnesinden ARR işlevselliğine erişmek istiyorsanız, *ARRServiceUnity* bileşenini yalnızca bunlardan birine ekleyin ve diğer betiklerdeki başvuru ya da ARR işlevselliğine doğrudan erişin.

Oynat ' a basın, oturuma bağlanın ve model yükleyin. Şimdi sahnenin nesnelerine işaret edin ve konsol çıkışını izleyin. Üzerine geldiğinizde her parçanın nesne adını yazdırmalıdır.

## <a name="highlight-objects"></a>Nesneleri Vurgula

Bir sonraki adımda, bir modelin Kullanıcı tarafından işaret ettiği parçalarını görsel geri bildirimde bulunmak istiyoruz. Bunu başarmak için, çekdiğimiz varlığa bir [HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) ekler. Bu bileşen, bir nesnedeki çeşitli özellikleri etkinleştirmek veya devre dışı bırakmak için kullanılabilir. Burada, renk tonu ayarlamak ve [ana hat işlemeyi](../../overview/features/outlines.md)etkinleştirmek için kullanırız.

**Remotemodelentity** adlı başka bir betik dosyası oluşturun ve içeriğini şu kodla değiştirin:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

public class RemoteModelEntity : MonoBehaviour
{
    public Color HighlightColor = new Color(1.0f, 0.0f, 0.0f, 0.1f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;

    private RemoteEntitySyncObject localSyncObject = null;
    private ARRHierarchicalStateOverrideComponent localStateOverride = null;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();
        localStateOverride = GetComponent<ARRHierarchicalStateOverrideComponent>();

        if (localStateOverride == null)
        {
            localStateOverride = gameObject.AddComponent<ARRHierarchicalStateOverrideComponent>();
            var remoteStateOverride = localSyncObject.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

            if (remoteStateOverride == null)
            {
                // if there is no HierarchicalStateOverrideComponent on the remote side yet, create one
                localStateOverride.Create(RemoteManagerUnity.CurrentSession);
            }
            else
            {
                // otherwise, bind our local stateOverride component to the remote component
                localStateOverride.Bind(remoteStateOverride);
            }
        }

        localStateOverride.RemoteComponent.TintColor = HighlightColor.toRemote();
    }

    public void OnDisable()
    {
        SetStateOverride(false);

        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.Enabled = false;
            localStateOverride.enabled = false;
        }
    }

    private void SetStateOverride(bool on)
    {
        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.UseTintColorState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.SelectedState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);
    }
}
```
> [!CAUTION]
> Aşağıdaki kod tarafından programlı olarak atanabileceği için, bu betiği herhangi bir Unity oyun nesnesine atamayın.

Daha sonra, yeni çekdiğimiz nesneye *Remotemodelentity* bileşeni eklemek için *Remoterdecaster* ' ı genişletireceğiz.

Aşağıdaki kodu **Remoteraycaster** uygulamasına ekleyin ve yinelenen işlevleri kaldırın:

```csharp
    private RemoteModelEntity focusedModel = null;

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var remoteEntity = await RemoteRayCast(origin, direction);

        if (focusedModel != null)
        {
            if (focusedModel.Entity == remoteEntity)
            {
                // picked the same object as before
                return;
            }

            ClearFocus();
        }

        if (remoteEntity == null)
        {
            // picked no object at all
            return;
        }

        // get the instance of a Unity GameObject for the ARR entity
        var entityGO = remoteEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the game object has the RemoteModelEntity component
        focusedModel = entityGO.GetComponent<RemoteModelEntity>();
        if (focusedModel == null)
        {
            focusedModel = entityGO.AddComponent<RemoteModelEntity>();
        }

        focusedModel.SetFocus(true);
    }

    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        focusedModel = null;
    }
```

Projenizi çalıştırın ve bir model üzerine gelin, kırmızı bir renk tonu ve beyaz seçim anahattı elde edip görmemeniz gerekir.

## <a name="isolate-the-selected-object"></a>Seçilen nesneyi ayır

[HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) 'in başka bir kullanımı, görünürlüğü geçersiz kılabilme olanağıdır. Bu, seçilen bir nesneyi modelin geri kalanından ayırmanıza olanak sağlar. **Remotemodelentity** betiğini açın, aşağıdaki kodu ekleyin ve yinelenen işlevleri kaldırın:

```csharp
    private bool isolated = false;
    private HierarchicalStateOverrideComponent parentOverride = null;

    public void ToggleIsolate()
    {
        SetIsolated(!isolated);
    }

    public void SetIsolated(bool on)
    {
        if (localStateOverride == null || !localStateOverride.IsComponentValid || isolated == on)
        {
            return;
        }

        // find the top most parent object that has a HierarchicalStateOverrideComponent
        if (parentOverride == null)
        {
            var modelRoot = transform;

            while (modelRoot.parent != null)
            {
                modelRoot = modelRoot.parent;

                var parentSyncObject = modelRoot.GetComponent<RemoteEntitySyncObject>();

                var stateOverrideComp = parentSyncObject?.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

                if (stateOverrideComp != null)
                {
                    parentOverride = stateOverrideComp;
                }
            }
        }

        if (parentOverride != null)
        {
            isolated = on;

            parentOverride.HiddenState = isolated ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.HiddenState = isolated ? HierarchicalEnableState.ForceOff : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);

        if (!on)
        {
            SetIsolated(false);
        }
    }
```

Bu kod, hiyerarşideki en üstteki nesnede bir durum geçersiz kılma bileşenine sahip olmayı kullanır ve bu da tüm nesneleri görünmez hale getirir. Ardından, bir nesneyi görünür hale getirmek için seçili nesnede görünürlüğü yeniden geçersiz kılar. Bu nedenle, kök nesnesinde bir durum geçersiz kılma bileşeni oluşturuyoruz.

**RemoteRendering** betiğini açın ve aşağıdaki kodu *LoadModel* işlevinin üst kısmına ekleyin:

```csharp
    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();
        arrService.CurrentActiveSession.Actions.CreateComponent(ObjectType.HierarchicalStateOverrideComponent, modelEntity);

        ...
    }
```

Son olarak, görünürlüğü değiştirmek için bir yol gerekir. **Remoteraycaster** betiğini açın ve *Update* işlevini değiştirin:

```csharp
    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        var ray = Camera.main.ScreenPointToRay(Input.mousePosition);

        Raycast(ray.origin, ray.direction);

        if (Input.GetMouseButtonDown(1) && focusedModel != null)
        {
            focusedModel.ToggleIsolate();
        }
    }
```

Kodu çalıştırın ve modelin bir bölümüne sağ tıklayın. Modelin geri kalanı kaybolur ve yalnızca vurgulanan parça görünür olur.

## <a name="remove-gameobject-instances-of-remote-entities"></a>Uzak varlıkların GameObject örneklerini kaldırma

Kodun nesne oluşturma, ancak hiçbir şekilde temizlemenin devam etmeme olduğunu fark etmiş olabilirsiniz. Bu ayrıca nesne hiyerarşisi panelinde de görünür. Simülasyon sırasında uzak nesne hiyerarşisini genişlettikten sonra, modelin yeni bir bölümünün üzerine geldiğinizde daha fazla ve daha fazla nesne görünmesini sağlayabilirsiniz.

Sahnede birçok nesnenin olması, performansı olumsuz etkiler. Artık gerekmeyen nesneleri her zaman temizlemeniz gerekir.

Aşağıdaki kodu, **Remoteraycaster** betiğine ekleyin ve yinelenen işlevleri kaldırın:

```csharp
    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        CleanHierarchy(focusedModel.gameObject);
        focusedModel = null;
    }

    private void CleanHierarchy(GameObject focusedGO)
    {
        var sync = focusedGO?.GetComponent<RemoteEntitySyncObject>();
        if (sync == null || !sync.IsEntityValid)
        {
            return;
        }

        sync.Entity.DestroyGameObject(EntityExtensions.DestroyGameObjectFlags.DestroyEmptyParents | EntityExtensions.DestroyGameObjectFlags.KeepRemoteRoot);
    }
```

## <a name="move-objects"></a>Nesneleri taşı

Bir sonraki adım olarak, seçili bir nesneyi etrafında taşımak istiyoruz. **Remoteraycaster** betiğinden, bu kodu ekleyin ve yinelenen işlevi kaldırın:

```csharp
    private Vector3 lastPosition = Vector3.zero;

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        if (Input.GetMouseButton(0))
        {
            if (focusedModel)
            {
                // note: mousePosition is in 2D screen-space coordinates and has no relation with
                // the 3D object position. This just happens to work good enough for demonstration.
                var delta = Input.mousePosition - lastPosition;
                focusedModel.transform.position += delta * Time.deltaTime;
            }
        }
        else
        {
            Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
            var ray = Camera.main.ScreenPointToRay(position);

            Raycast(ray.origin, ray.direction);

            if (Input.GetMouseButtonDown(1) && focusedModel != null)
            {
                focusedModel.ToggleIsolate();
            }
        }

        lastPosition = Input.mousePosition;
    }
```

> [!IMPORTANT]
> Bu kodu çalıştırırsanız hiçbir şeyin gerçekleşmediğine dikkat edin. Bunun nedeni, bir nesnenin dönüşümünü değiştirmenin, performans nedenleriyle durum değişikliğini sunucu ile otomatik olarak eşitlememeleridir. Bunun yerine, bu durum değişikliğini sunucuya el ile göndermeniz ya da *Remoteentitysyncobject* bileşeninde **SyncEveryFrame** etkinleştirmeniz gerekir.

**Remotemodelentity** betiğini açın ve şu satırı ekleyin:

```csharp
    public void OnEnable()
    {
        ...

        localSyncObject.SyncEveryFrame = true;
    }
```

Kodu yeniden çalıştırmak için bir nesneye Sol tıklayıp sürükleyebilirsiniz.

## <a name="add-a-cut-plane"></a>Kesme düzlemi ekleme

Bu öğreticide denemek istediğimiz son özellik, [kesme düzlemleri](../../overview/features/cut-planes.md)kullanıyor. Kesilen bir düzlem, işlenen nesnelerin parçalarını, bunların içine bakabilmeniz için keser.

Sahnede yeni bir Oyunobject oluşturun **CutPlane**. Yeni bir betik oluşturun ve **RemoteCutPlane**çağırın. Bileşeni yeni Oyunobject öğesine ekleyin.

Komut dosyasını açın ve içeriğini şu kodla değiştirin:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteCutPlane : MonoBehaviour
{
    private ARRCutPlaneComponent localCutPlaneComponent = null;
    private RemoteEntitySyncObject remoteEntitySync = null;

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            // can't do anything while we are not connected
            return;
        }

        if (localCutPlaneComponent == null)
        {
            localCutPlaneComponent = gameObject.CreateArrComponent<ARRCutPlaneComponent>(RemoteManagerUnity.CurrentSession);
        }

        if (remoteEntitySync == null)
        {
            remoteEntitySync = gameObject.GetComponent<RemoteEntitySyncObject>();
            remoteEntitySync.SyncEveryFrame = true;
        }

        localCutPlaneComponent.RemoteComponent.Normal = Axis.X;
        localCutPlaneComponent.RemoteComponent.FadeLength = 0.025f;
        localCutPlaneComponent.RemoteComponent.FadeColor = new Color4Ub(255, 128, 0, 255);
        localCutPlaneComponent.RemoteComponent.Enabled = true;
    }

    void OnDisable()
    {
        if (localCutPlaneComponent && localCutPlaneComponent.IsComponentValid)
        {
            localCutPlaneComponent.RemoteComponent.Enabled = false;
        }

        if (remoteEntitySync && remoteEntitySync.IsEntityValid)
        {
            remoteEntitySync.SyncEveryFrame = false;
        }
    }
}
```

Kodunuzu şimdi çalıştırdığınızda, modelin düzlem tarafından nasıl açık olduğunu görmeniz gerekir. *CutPlane* nesnesini seçebilir ve *sahne* penceresinde taşıyabilir ve döndürebilirsiniz. Kesilen düzlemi nesnesini devre dışı bırakarak kesme düzlemini açıp kapatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık uzak nesnelerle etkileşimde bulunmak için en önemli işlevselliği öğrenmiş olursunuz. Bir sonraki öğreticide sahnenin görünümünü özelleştirmek için bir göz atacağız.

> [!div class="nextstepaction"]
> [Öğretici: ortamı ve malzemeleri değiştirme](changing-environment-and-materials.md)
