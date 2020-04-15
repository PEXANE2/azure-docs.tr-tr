---
title: Unity’de uzak varlıklarla çalışma
description: ARR varlıklarıyla nasıl çalışılı şören öğretici.
author: florianborn71
ms.author: flborn
ms.date: 02/01/2020
ms.topic: tutorial
ms.openlocfilehash: db1f6a53121e05b29f7e3441af027985a141bc2e
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310202"
---
# <a name="tutorial-working-with-remote-entities-in-unity"></a>Öğretici: Unity içinde uzak varlıklarla çalışma

[Öğretici: Sıfırdan bir Unity projesi ayarlamak,](project-setup.md) Azure Uzaktan İşleme ile çalışmak üzere yeni bir Unity projesinin nasıl yapılandırılabildiğini gösterdi. Bu eğitimde, her ARR kullanıcısının ihtiyaç duyduğu en yaygın işlevselliğe bir göz atıyoruz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Ray dökümlerini kullanarak nesneleri seçin.
> * Renk rengi, seçim durumu ve görünürlük gibi nesne durumlarını geçersiz kılın.
> * Uzak varlıkları silin.
> * Uzak varlıkları hareket ettirin.
> * Nesnelerin içine bakmak için kesilmiş düzlemleri kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici Öğretici üstüne [oluşturur: sıfırdan bir Birlik projesi kurma](project-setup.md).

> [!TIP]
> [ARR örnekleri deposu,](https://github.com/Azure/azure-remote-rendering) başvuru olarak kullanabileceğiniz *Unity* klasöründeki tüm öğreticiler için hazırlanmış Unity projeleri içerir.

## <a name="pick-objects"></a>Nesneleri seçme

Nesnelerle etkileşime gitmek istiyoruz, bu yüzden ihtiyacımız olan ilk şey, fare imlecinin altındaki nesneleri seçmektir.

**RemoteRaycaster** adında yeni bir [komut dosyası](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve tüm içeriğini aşağıdaki kodla değiştirin:

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

Bu bileşeni sahnenizdeki *RemoteRendering* nesnesine ekleyin.

> [!WARNING]
>
> *RemoteRaycaster* bileşeni, aynı nesneye bir *ARRServiceUnity* bileşeninin eklenmesini gerektirir. *ARRServiceUnity,* bazı ARR işlevlerine daha kolay erişmek için yardımcı bir sınıftır. Ancak, sahnede bu bileşenin yalnızca tek bir örneği olabilir. Bu nedenle, aynı *GameObject'e ARRServiceUnity* gerektiren tüm bileşenleri eklediğinizden emin olun.
> Birden çok oyun nesnesinden ARR işlevine erişmek istiyorsanız, *ARRServiceUnity* bileşenini yalnızca birine ekleyin ve diğer komut dosyalarında buna başvurun veya ARR işlevine doğrudan erişin.

Play tuşuna basın, oturuma bağlanın ve bir model yükleyin. Şimdi olay yerindeki nesneleri işaret edin ve konsol çıkışını izleyin. Üzerinde gezindiğiniz her parçanın nesne adını yazdırmalıdır.

## <a name="highlight-objects"></a>Nesneleri vurgulama

Bir sonraki adım olarak, kullanıcının işaret ettiği bir modelin hangi bölümlerini görsel geri bildirimvermek istiyoruz. Bunu başarmak için, seçtiğimiz varlığa [hiyerarşik Bir BiçimGeçersiz Bileşen](../../overview/features/override-hierarchical-state.md) ekliyoruz. Bu bileşen, bir nesneüzerindeki çeşitli özellikleri etkinleştirmek veya devre dışı bırakmak için kullanılabilir. Burada bir renk tonu ayarlamak ve [anahat işleme etkinleştirmek](../../overview/features/outlines.md)için kullanabilirsiniz.

**RemoteModelEntity** adlı başka bir komut dosyası dosyası oluşturun ve içeriğini aşağıdaki kodla değiştirin:

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
> Aşağıdaki kod tarafından programlı olarak atanacağı için bu komut dosyasını herhangi bir oyun nesnesine atamayın.

Sırada, RemoteRaycaster bileşenini az önce *RemoteModelEntity* seçtiğimiz nesneye eklemek için *RemoteRaycaster'ımızı* genişletmemiz gerekiyor.

**RemoteRaycaster** uygulamasına aşağıdaki kodu ekleyin ve yinelenen işlevleri kaldırın:

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

Projenizi çalıştırın ve bir modeli işaret edin, kırmızı bir renk tonu ve beyaz seçim anahattı elde görmeniz gerekir.

## <a name="isolate-the-selected-object"></a>Seçili nesneyi yalıtma

[Hiyerarşik StateOverrideBileşeni'nin](../../overview/features/override-hierarchical-state.md) bir diğer kullanımı da görünürlüğü geçersiz kılabilme yeteneğidir. Bu, seçili bir nesneyi modelin geri kalanından yalıtmanızı sağlar. **RemoteModelEntity** komut dosyasını açın, aşağıdaki kodu ekleyin ve yinelenen işlevleri kaldırın:

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

Bu kod, hiyerarşideki en üstteki nesnede bir durum geçersiz kılma bileşenine dayanır ve bu da tüm nesneleri görünmez kılar. Daha sonra, bir nesneyi görünür kılmak için seçili nesnedeki görünürlüğü yeniden geçersiz kılar. Bu nedenle, kök nesnesi bir durum geçersiz kılma bileşeni oluşturmamız gerekir.

**RemoteRendering** komut dosyasını açın ve *LoadModel* işlevinin üst kısmında aşağıdaki kodu ekleyin:

```csharp
    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();
        arrService.CurrentActiveSession.Actions.CreateComponent(ObjectType.HierarchicalStateOverrideComponent, modelEntity);

        ...
    }
```

Sonunda görünürlüğü geçişin bir yolunu bulmalıyız. **RemoteRaycaster** komut dosyasını açın ve *Güncelleştirme* işlevini değiştirin:

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

Kodu çalıştırın ve modelin bir bölümüne sağ tıklayın. Modelin geri kalanı kaybolur ve yalnızca vurgulanan parça görünür kalır.

## <a name="remove-gameobject-instances-of-remote-entities"></a>Uzak varlıkların GameObject örneklerini kaldırma

Kodun nesneleri oluşturmaya devam ettiğini fark etmiş olabilirsiniz, ancak bunları asla temizlemez. Bu nesne hiyerarşisi panelinde de görülebilir. Simülasyon sırasında uzak nesne hiyerarşisini genişletdiğinizde, modelin yeni bir bölümünün üzerinde her gezindiğinizde daha fazla nesnenin göründüğünü görebilirsiniz.

Bir sahnede çok sayıda nesneolması performansı olumsuz etkiler. Artık gerekli olmayan nesneleri her zaman temizlemeniz gerekir.

Aşağıdaki kodu **RemoteRaycaster** komut dosyasına ekleyin ve yinelenen işlevleri kaldırın:

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

## <a name="move-objects"></a>Nesneleri taşıma

Bir sonraki adım olarak, seçili bir nesneyi hareket ettirmek istiyoruz. **RemoteRaycaster** komut dosyasına bu kodu ekleyin ve yinelenen işlevi kaldırın:

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
> Bu kodu çalıştırdığınızda, hiçbir şey olmadığını fark edeceksiniz. Bunun nedeni, bir nesnenin dönüşümünü değiştirmenin, performans nedenleriyle durum değişikliğini sunucuya otomatik olarak eşitlemesidir. Bunun yerine, bu durum değişikliğini sunucuya el ile itmeniz veya *RemoteEntitySyncObject* bileşeninde **SyncEveryFrame'i** etkinleştirmeniz gerekir.

**RemoteModelEntity** komut dosyasını açın ve şu satırı ekleyin:

```csharp
    public void OnEnable()
    {
        ...

        localSyncObject.SyncEveryFrame = true;
    }
```

Kodu yeniden çalıştırdığınızda, bir nesneye sol tıklatıp sürükleyebilmelisiniz.

## <a name="add-a-cut-plane"></a>Kesme düzlemi ekleme

Biz bu öğretici denemek istediğiniz son özelliği, [kesme düzlemleri](../../overview/features/cut-planes.md)kullanıyor. Kesilmiş bir düzlem, işlenmiş nesnelerin parçalarını keser, çünkü bunların içine bakabilirsiniz.

**CutPlane**sahnesinde yeni bir GameObject oluşturun. Yeni bir komut dosyası oluşturun ve **RemoteCutPlane**adını alın. Bileşeni yeni GameObject'e ekleyin.

Komut dosyası dosyasını açın ve içeriğini aşağıdaki kodla değiştirin:

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

Kodunuzu şimdi çalıştırdığınızda, modelin düzlem tarafından nasıl açık kesildiğini görmeniz gerekir. *CutPlane* nesnesini seçebilir ve *Sahne* penceresinde taşıyıp döndürebilirsiniz. Kesilen düzlem nesnesini devre dışı bırakarak kesme düzlemini değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık uzak nesnelerle etkileşim kurmak için en önemli işlevselliği biliyorsunuz. Bir sonraki öğreticide, bir sahnenin görünümünü özelleştirmeye bir göz atacağız.

> [!div class="nextstepaction"]
> [Öğretici: Çevre ve malzeme değiştirme](changing-environment-and-materials.md)
