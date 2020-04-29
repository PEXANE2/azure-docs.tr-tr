---
title: Ortamı ve malzemeleri değiştirme
description: Bir Unity sahnede gök eşlem ve nesne malzemelerinin nasıl değiştirileceğini gösteren öğretici.
author: jakrams
ms.author: jakras
ms.date: 02/03/2020
ms.topic: tutorial
ms.openlocfilehash: 1354ac3cf2c6fc716ac72ae339928fa49171893e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80679621"
---
# <a name="tutorial-changing-the-environment-and-materials"></a>Öğretici: ortamı ve malzemeleri değiştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir sahnenin ortam haritasını değiştirin.
> * Malzeme parametrelerini değiştirin.
> * Özel dokuları yükleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide [öğreticiyi bildiğiniz varsayılmaktadır: Unity 'de uzak varlıklarla çalışma](working-with-remote-entities.md). Bununla birlikte, yalnızca, oturumlara bağlanabildiğinizi ve bir modeli yüklemeyi sağlayan bir Unity projesine ihtiyacınız vardır ve bu, [öğreticide bir Unity projesi kurma: sıfırdan](project-setup.md)gösterildiği gibi.

> [!TIP]
> [ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) , *Unity* klasöründe başvuru olarak kullanabileceğiniz tüm öğreticiler için hazırlanan Unity projelerini içerir.

## <a name="change-the-environment-map"></a>Ortam haritasını değiştirme

Azure uzaktan Işleme, ortam aydınlatmasını taklit etmek için (bazen ' ortam haritaları ' olarak da adlandırılır) [gök kutularının](../../overview/features/sky.md) kullanımını destekler. Bu, özellikle nesneleriniz *[fiziksel tabanlı işleme](../../overview/features/pbr-materials.md)* kullanırken, örnek modellerimiz yaptığımızda yararlıdır. ARR Ayrıca, bu öğreticide kullanabilmemiz için çeşitli yerleşik gök dokularıyla birlikte gelir.

**Remotesky** adlı yeni bir betik oluşturun ve bunu yeni bir oyunobject öğesine ekleyin. Betik dosyasını açın ve aşağıdaki kodla değiştirin:

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

Yerleşik dokular yüklendiğinden `LoadTextureFromSASAsync` , bu değişkenin yukarıda kullanıldığını unutmayın. [Bağlı BLOB depolama](../../how-tos/create-an-account.md#link-storage-accounts)alanından yükleme durumunda, `LoadTextureAsync` türevini kullanın. [Model yükleme bölümünde](../../concepts/models.md#loading-models)bunun modeller için nasıl çalıştığı hakkında bir örnek bulabilirsiniz.

Kodu çalıştırıp gök haritaları boyunca geçiş yaptığınızda, modelinizde büyük ölçüde farklı aydınlatma fark edeceksiniz. Ancak arka plan siyah kalır ve gerçek gök dokusunu göremezsiniz. Bu bilerek, bir arka plan işleme, genişletilmiş gerçeklik cihazından dikkat dağıtıcı olacak şekilde yapılır. Uygun bir uygulamada, gerçek dünyaya benzer gök dokuları kullanmanız gerekir, bu da nesnelerin daha gerçekçi görünmesini sağlamaya yardımcı olur.

## <a name="modify-materials"></a>Malzemeleri değiştirme

Önceki öğreticide, seçili nesnelerin renk tonu rengini değiştirmek için [durum geçersiz kılma bileşenleri](../../overview/features/override-hierarchical-state.md) kullandık. Şimdi de benzer bir etkiye ulaşmak istiyoruz, ancak bunu bir nesnenin [malzemesini](../../concepts/materials.md) değiştirerek yapabilirsiniz.

İlk olarak, [ikinci öğreticide](working-with-remote-entities.md)yaptığımız gibi nesneleri seçmek için bir betiğe ihtiyacımız var. Henüz bir **Remoteraycaster** betiğinizi yoksa şimdi oluşturun. İçeriğini şu kodla değiştirin:

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

*RemoteRendering* Game nesnenizin bileşenini ekleyin. Bu, fare altındaki nesneleri çekmekten ve *Remotemodelentity* bileşenlerini çekilen nesnelere eklemekle sorumludur. Bu bileşen sınıfı, gerçek malzeme değişikliği işlevini uyguladığımız yerdir.

Henüz bir **Remotemodelentity** betiğinizin yoksa, oluşturun ve içeriğini şu kodla değiştirin:

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

Bu kodu çalıştırdığınızda, fare ile üzerine geldiğinizde vurgulanan nesneler vurgulanır. Bu efekt, öğretici 2 ' de yaptığımız yönteme benzerdir, ancak elde edilen yöntem farklıdır. Burada, çekilen nesne üzerindeki malzemelerin listesini alır ve ilk olanı farklı bir Albedo rengine sahip olacak şekilde değiştirirsiniz.

> [!IMPORTANT]
> Lütfen bu yöntemin bir modelin doğru bölümlerini vurgulamaları durumunda modelin nasıl yazıldığı hakkında dikkat edin. Her nesne tam olarak bir malzeme kullanıyorsa, bu işlem mükemmel bir şekilde çalışır. Ancak, modelin parçalar ve malzemeler arasında 1:1 ilişkisi yoksa yukarıdaki Naïve kodu doğru şeyi yapmaz.

## <a name="use-a-different-texture"></a>Farklı bir doku kullanın

[Dokular](../../concepts/textures.md) genellikle kaynak modelin bir parçasıdır. [Model dönüştürme](../../quickstarts/convert-model.md)sırasında tüm dokular gerekli çalışma zamanı biçimine dönüştürülüp son model dosyasına paketlenir. Çalışma zamanında bir dokuyu değiştirmek için, bu dosyayı [DDS dosya biçiminde](https://en.wikipedia.org/wiki/DirectDraw_Surface) kaydetmeniz ve Azure Blob depolama alanına yüklemeniz gerekir. Azure Blob kapsayıcısı oluşturmak için [Bu hızlı başlangıç kılavuzuna](../../quickstarts/convert-model.md) bakın. Bir blob Kapsayıcınız olduktan sonra, Azure Depolama Gezgini içinde açabilir ve sürükleyip bırakma yoluyla dosyanızı karşıya yükleyebilirsiniz.

Çalışma zamanı tarafında, blob depolamada bir doku varlığını iki farklı şekilde ele alabilirsiniz:

* Kendi SAS URI 'sine göre doku adresi. Bunun için karşıya yüklenen dosyaya sağ tıklayın ve bağlam menüsünden "**paylaşılan erişim Imzasını al...**" öğesini seçin. Bu SAS URI 'sini `LoadTextureFromSASAsync` işlev değişkeni ile birlikte kullanın (Aşağıdaki örnek koda bakın).
* [BLOB depolama alanı firmayla bağlantılı](../../how-tos/create-an-account.md#link-storage-accounts)olması durumunda, doğrudan BLOB depolama parametrelerine göre dokuyu çözün. Bu durumda ilgili yükleme işlevi `LoadTextureAsync`.

Şimdi **Remotemodelentity** betiğini açın, aşağıdaki kodu ekleyin ve yinelenen işlevleri kaldırın:

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

Bu kodu çalıştırın ve modelinizin üzerine gelin. Modelinize uygun UV koordinatları varsa, dokusunun göründüğünü görmeniz gerekir. Aksi takdirde, yalnızca bir renk değişikliği fark edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu, Azure uzaktan Işlemenin Unity ile nasıl kullanılacağına ilişkin giriş serimizi temel alır. Sonraki adım olarak, daha derin bir anlama oluşturmak için [Oturumlar](../../concepts/sessions.md), [varlıklar](../../concepts/entities.md)ve [modeller](../../concepts/models.md) gibi bazı temel kavramlara alışmanız gerekir. Ayrıca [ışıklar](../../overview/features/lights.md), [ana hat işleme](../../overview/features/outlines.md), [hiyerarşik durum geçersiz kılmaları](../../overview/features/override-hierarchical-state.md)ve daha ayrıntılı olarak araştırılacak [malzemeler](../../concepts/materials.md) gibi çeşitli özellikler de vardır.

> [!div class="nextstepaction"]
> [Unity oyun nesneleri ve bileşenleri](../../how-tos/unity/objects-components.md)
