---
title: Çevre ve malzeme nin değiştirilmesi
description: Birbirlik sahnesinde gökyüzü haritasını ve nesne materyallerini nasıl değiştirin için günü gÜ
author: jakrams
ms.author: jakras
ms.date: 02/03/2020
ms.topic: tutorial
ms.openlocfilehash: 1354ac3cf2c6fc716ac72ae339928fa49171893e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679621"
---
# <a name="tutorial-changing-the-environment-and-materials"></a>Öğretici: Çevre ve malzeme değiştirme

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Bir sahnenin ortam haritasını değiştirin.
> * Malzeme parametrelerini değiştirin.
> * Özel dokular yükleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu [öğretici, Öğretici](working-with-remote-entities.md)aşina olduğunuzu varsayar: Unity uzak varlıklar ile çalışma . Ancak, [Öğretici: Bir Birlik projesini sıfırdan ayarlama'da](project-setup.md)gösterildiği gibi, yalnızca oturumlara bağlanabileceğiniz ve bir modeli yükleyebileceğiniz bir Birlik projesine ihtiyacınız vardır.

> [!TIP]
> [ARR örnekleri deposu,](https://github.com/Azure/azure-remote-rendering) başvuru olarak kullanabileceğiniz *Unity* klasöründeki tüm öğreticiler için hazırlanmış Unity projeleri içerir.

## <a name="change-the-environment-map"></a>Çevre haritasını değiştirme

Azure Uzaktan İşleme, ortam aydınlatmasını simüle etmek için [gökyüzü kutularının](../../overview/features/sky.md) (bazen 'çevre haritaları' olarak da adlandırılır) kullanımını destekler. Bu, özellikle örnek modellerimizde olduğu gibi *[Fiziksel Tabanlı Görüntüleme'yi](../../overview/features/pbr-materials.md)* kullandığında kullanışlıdır. ARR ayrıca bu eğitimde kullanacağımız çeşitli yerleşik gökyüzü dokularıyla birlikte gelir.

**RemoteSky** adında yeni bir komut dosyası oluşturun ve yeni bir GameObject'e ekleyin. Komut dosyası dosyasını açın ve aşağıdaki kodla değiştirin:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteSky : MonoBehaviour
{
    private int skyIndex = 0;

    private LoadTextureFromSASParams[] builtIns =
    {
        new LoadTextureFromSASParams("builtin://Autoshop", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://BoilerRoom", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://ColorfulStudio", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Hangar", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://IndustrialPipeAndValve", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Lebombo", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SataraNight", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SunnyVondelpark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Syferfontein", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://TearsOfSteelBridge", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WhippleCreekRegionalPark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WinterRiver", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://DefaultSky", TextureType.CubeMap)
    };

    public async void ToggleSky()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        skyIndex = (skyIndex + 1) % builtIns.Length;

        var texture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(builtIns[skyIndex]).AsTask();

        Debug.Log($"Switching sky to: {builtIns[skyIndex].TextureId}");

        var settings = RemoteManagerUnity.CurrentSession.Actions.SkyReflectionSettings;

        settings.SkyReflectionTexture = texture;
    }

    private void OnGUI()
    {
        if (RemoteManagerUnity.IsConnected)
        {
            if (GUI.Button(new Rect(10, Screen.height - 50, 175, 30), "Toggle Sky"))
            {
                ToggleSky();
            }
        }
    }
}
```

Yerleşik dokular yüklendiğinden, varyantın `LoadTextureFromSASAsync` yukarıda kullanıldığını unutmayın. [Bağlantılı blob depolarından](../../how-tos/create-an-account.md#link-storage-accounts)yükleme durumunda, `LoadTextureAsync` varyantı kullanın. Bunun modeller için nasıl çalıştığına bir örnek [model yükleme bölümünde](../../concepts/models.md#loading-models)bulunabilir.

Kodu çalıştırdığınızda ve gökyüzü haritalarında geçiş yaptığınızda, modelinizde büyük ölçüde farklı ışıklandırmalar göreceksiniz. Ancak, arka plan siyah kalır ve gerçek gökyüzü dokusunu göremezsiniz. Bu kasıtlı, bir arka plan oluşturma bir Artırılmış Gerçeklik cihazı ile dikkat dağıtıcı olurdu gibi. Uygun bir uygulamada, nesnelerin daha gerçek görünmesine yardımcı olacağından, gerçek dünya çevrenize benzeyen gökyüzü dokularını kullanmalısınız.

## <a name="modify-materials"></a>Malzemeleri değiştirme

Önceki öğreticide, seçili nesnelerin renk rengini değiştirmek için [durum geçersiz kılma bileşenleri](../../overview/features/override-hierarchical-state.md) ni kullandık. Şimdi benzer bir etki elde etmek istiyorum, ama bunu bir nesnenin [malzeme](../../concepts/materials.md) değiştirerek.

İlk biz [ikinci öğretici](working-with-remote-entities.md)yaptığı gibi, nesneleri seçmek için bir komut dosyası gerekir. Henüz bir **RemoteRaycaster** komut dosyanız yoksa, şimdi oluşturun. İçeriğini aşağıdaki kodla değiştirin:

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
}
```

Bileşeni *RemoteRendering* oyun nesnenize ekleyin. Farenin altındaki nesneleri seçmekten ve seçilen nesnelere *RemoteModelEntity* bileşenleri eklemekten sorumludur. Bu bileşen sınıfı, gerçek malzeme değiştirme işlevini uyguladığımız yerdir.

Henüz bir **RemoteModelEntity** komut dosyanız yoksa, komut dosyasını oluşturun ve içeriğini bu kodla değiştirin:

```csharp
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

// to prevent namespace conflicts
using ARRTexture = Microsoft.Azure.RemoteRendering.Texture;

public class RemoteModelEntity : MonoBehaviour
{
    private Color HighlightColor = new Color(1.0f, 0.4f, 0.1f, 1.0f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;
    private RemoteEntitySyncObject localSyncObject = null;
    private ARRMeshComponent meshComponent = null;
    private Color4 originalColor;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();

        meshComponent = GetComponent<ARRMeshComponent>();
        if (meshComponent == null)
        {
            var mesh = localSyncObject.Entity.FindComponentOfType<MeshComponent>();
            if (mesh != null)
            {
                gameObject.BindArrComponent<ARRMeshComponent>(mesh);
                meshComponent = gameObject.GetComponent<ARRMeshComponent>();
            }
        }
        meshComponent.enabled = true;
    }

    public void SetFocus(bool on)
    {
        SetMaterialColor(on);
    }

    private void SetMaterialColor(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        // get the list of materials from the picked mesh
        // we only modify the first material; depending on how the model is authored, this may
        // work exactly as desired, or not ;-)
        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        // there are different types of materials, cast as necessary
        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
    }
}
```

Bu kodu çalıştırdığınızda, fareyle gezindiğiniz nesneler vurgulanır. Etkisi biz öğretici 2 ne benzer, ama elde yolu farklıdır. Burada seçilen nesne üzerinde malzemelerin listesini almak ve daha sonra farklı bir albedo renk için ilk bir değiştirin.

> [!IMPORTANT]
> Bu yöntemin bir modelin doğru bölümlerini vurgulayıp vurgulamadığının, bir modelin nasıl yazılmasına bağlı olduğunu lütfen unutmayın. Her nesne tam olarak bir malzeme kullanırsa, mükemmel çalışacaktır. Ancak, modelin parçalar ve malzemeler arasında 1:1 ilişkisi yoksa, yukarıdaki naif kod doğru olanı yapmaz.

## <a name="use-a-different-texture"></a>Farklı bir doku kullanma

[Dokular](../../concepts/textures.md) genellikle kaynak modelin bir parçasıdır. [Model dönüştürme](../../quickstarts/convert-model.md)sırasında, tüm dokular gerekli çalışma zamanı biçimine dönüştürülür ve son model dosyasına paketlenir. Bir dokuyu çalışma zamanında değiştirmek için, [dds dosya biçiminde](https://en.wikipedia.org/wiki/DirectDraw_Surface) kaydetmeniz ve Azure blob depolama alanına yüklemeniz gerekir. Azure blob kapsayıcısı oluşturmak için [bu hızlı başlangıç kılavuzuna](../../quickstarts/convert-model.md) başvurun. Bir blob kapsayıcınız olduğunda, azure depolama gezgininde açabilir ve dosyanızı sürükle ve bırak yoluyla yükleyebilirsiniz.

Çalışma zamanı tarafında, blob depolamabir doku varlık adrese iki farklı şekilde:

* SAS URI ile adres dokusu. Bunun için yüklenen dosyaya sağ tıklayın ve bağlam menüsünden "**Paylaşılan Erişim İmzasını Al...**" seçeneğini belirleyin. İşlev varyantı ile `LoadTextureFromSASAsync` bu SAS URI'yi kullanın (aşağıdaki örnek koduna bakın).
* Blob depolama hesabına bağlı olması durumunda, [blob](../../how-tos/create-an-account.md#link-storage-accounts)depolama parametreleri ile doğrudan doku adresi. Bu durumda ilgili yükleme `LoadTextureAsync`işlevi .

Şimdi **RemoteModelEntity** komut dosyasını açın, aşağıdaki kodu ekleyin ve yinelenen işlevleri kaldırın:

```csharp
    private string textureFile = ""; //<SAS URI for your texture>
    private ARRTexture originalTexture = null;

    private async void SetMaterialTexture(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        ARRTexture newTexture = originalTexture;

        if (on)
        {
            var textureParams = new LoadTextureFromSASParams(textureFile, TextureType.Texture2D);

            newTexture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(textureParams).AsTask();
        }

        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
    }

    public void SetFocus(bool on)
    {
        if (string.IsNullOrEmpty(textureFile))
        {
            SetMaterialColor(on);
        }
        else
        {
            SetMaterialTexture(on);
        }
    }
```

Bu kodu çalıştırın ve modelinizin üzerinde gezinin. Modelinizin uygun UV koordinatları varsa, doku görünmenizi görmeniz gerekir. Aksi takdirde, yalnızca bir renk değişikliği fark edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu, Azure Uzaktan İşleme'nin Unity ile nasıl kullanılacağı hakkındaki tanıtım serimizi sona erdiriyor. Bir sonraki adım olarak kendinizi ARR bazı temel kavramları aşina olmalıdır, [oturumları](../../concepts/sessions.md)gibi, [varlıklar](../../concepts/entities.md), ve [modeller](../../concepts/models.md) daha derin bir anlayış oluşturmak için. [Işıklar](../../overview/features/lights.md)gibi çeşitli özellikler de vardır , [anahat işleme](../../overview/features/outlines.md), [hiyerarşik durum geçersiz kılar](../../overview/features/override-hierarchical-state.md), ve daha ayrıntılı olarak keşfetmek gerekir [malzemeler.](../../concepts/materials.md)

> [!div class="nextstepaction"]
> [Unity oyun nesneleri ve bileşenleri](../../how-tos/unity/objects-components.md)
