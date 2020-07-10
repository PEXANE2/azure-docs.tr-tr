---
title: Malzemeleri, ışıkları ve efektleri iyileştirme
description: Model malzemelerini ve aydınlatmayı değiştirin. Ana hat oluşturma ve düzlemi kesme gibi ek efektler ekleyin.
author: florianborn71
ms.author: flborn
ms.date: 06/15/2020
ms.topic: tutorial
ms.openlocfilehash: 39560966b542999b2c0c3472075c7497f2146455
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207341"
---
# <a name="tutorial-refining-materials-lighting-and-effects"></a>Öğretici: malzemeleri, aydınlatmayı ve etkileri Iyileştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Modelleri ve model bileşenlerini vurgulayın ve özetler
> * Modellere farklı malzemeler uygulama
> * Kesme düzlemiyle modeller arasında dilimle
> * Uzaktan işlenen nesneler için basit animasyonlar ekleme

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici [öğretici: modelleri düzenleme](..\manipulate-models\manipulate-models.md).

## <a name="highlighting-and-outlining"></a>Vurgulama ve anahat oluşturma

Kullanıcıya görsel geri bildirim sağlanması, herhangi bir uygulamadaki kullanıcı deneyiminin önemli bir parçasıdır. Azure uzaktan Işleme, [hiyerarşik durum geçersiz kılma işlemleri](../../../overview/features/override-hierarchical-state.md)aracılığıyla görsel geri bildirim mekanizmaları sağlar. Hiyerarşik durum geçersiz kılmaları, yerel model örneklerine eklenen bileşenlerle uygulanır. [Uzak nesne grafiğini Unity hiyerarşisinde eşitlerken](../manipulate-models/manipulate-models.md#synchronizing-the-remote-object-graph-into-the-unity-hierarchy)bu yerel örneklerin nasıl oluşturulacağını öğrendik.

İlk olarak, [**HierarchicalStateOverrideComponent**](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.hierarchicalstateoverridecomponent) bileşeni etrafında bir sarmalayıcı oluşturacağız. **HierarchicalStateOverrideComponent** , uzak varlıktaki geçersiz kılmaları denetleyen yerel betiktir. [**Öğretici varlıkları**](../custom-models/custom-models.md#import-assets-used-by-this-tutorial) , sarmalayıcı oluşturmak için Genişletireceğiz **BaseEntityOverrideController**adlı bir soyut temel sınıf içerir.

1. **EntityOverrideController** adlı yeni bir betik oluşturun ve içeriğini şu kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using UnityEngine;

    public class EntityOverrideController : BaseEntityOverrideController
    {
        public override event Action<HierarchicalStates> FeatureOverrideChange;

        private ARRHierarchicalStateOverrideComponent localOverride;
        public override ARRHierarchicalStateOverrideComponent LocalOverride
        {
            get
            {
                if (localOverride == null)
                {
                    localOverride = gameObject.GetComponent<ARRHierarchicalStateOverrideComponent>();
                    if (localOverride == null)
                    {
                        localOverride = gameObject.AddComponent<ARRHierarchicalStateOverrideComponent>();
                    }

                    var remoteStateOverride = TargetEntity.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

                    if (remoteStateOverride == null)
                    {
                        // if there is no HierarchicalStateOverrideComponent on the remote side yet, create one
                        localOverride.Create(RemoteManagerUnity.CurrentSession);
                    }
                    else
                    {
                        // otherwise, bind our local stateOverride component to the remote component
                        localOverride.Bind(remoteStateOverride);

                    }
                }
                return localOverride;
            }
        }

        private RemoteEntitySyncObject targetEntity;
        public override RemoteEntitySyncObject TargetEntity
        {
            get
            {
                if (targetEntity == null)
                    targetEntity = gameObject.GetComponent<RemoteEntitySyncObject>();
                return targetEntity;
            }
        }

        private HierarchicalEnableState ToggleState(HierarchicalStates feature)
        {
            HierarchicalEnableState setToState = HierarchicalEnableState.InheritFromParent;
            switch (LocalOverride.RemoteComponent.GetState(feature))
            {
                case HierarchicalEnableState.ForceOff:
                case HierarchicalEnableState.InheritFromParent:
                    setToState = HierarchicalEnableState.ForceOn;
                    break;
                case HierarchicalEnableState.ForceOn:
                    setToState = HierarchicalEnableState.InheritFromParent;
                    break;
            }

            return SetState(feature, setToState);
        }

        private HierarchicalEnableState SetState(HierarchicalStates feature, HierarchicalEnableState enableState)
        {
            if (GetState(feature) != enableState) //if this is actually different from the current state, act on it
            {
                LocalOverride.RemoteComponent.SetState(feature, enableState);
                FeatureOverrideChange?.Invoke(feature);
            }

            return enableState;
        }

        public override HierarchicalEnableState GetState(HierarchicalStates feature) => LocalOverride.RemoteComponent.GetState(feature);

        public override void ToggleHidden() => ToggleState(HierarchicalStates.Hidden);

        public override void ToggleSelect() => ToggleState(HierarchicalStates.Selected);

        public override void ToggleSeeThrough() => ToggleState(HierarchicalStates.SeeThrough);

        public override void ToggleTint(Color tintColor = default)
        {
            if (tintColor != default) LocalOverride.RemoteComponent.TintColor = tintColor.toRemote();
            ToggleState(HierarchicalStates.UseTintColor);
        }

        public override void ToggleDisabledCollision() => ToggleState(HierarchicalStates.DisableCollision);

        public override void RemoveOverride()
        {
            var remoteStateOverride = TargetEntity.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();
            if (remoteStateOverride != null)
            {
                remoteStateOverride.Destroy();
            }

            if (localOverride == null)
                localOverride = gameObject.GetComponent<ARRHierarchicalStateOverrideComponent>();

            if (localOverride != null)
            {
                Destroy(localOverride);
            }
        }
    }
    ```

**Localoverride**'ın ana işi kendisiyle arasında bir bağlantı oluşturmaktır `RemoteComponent` . **Localoverride** daha sonra, uzak varlığa bağlı olan yerel bileşende durum bayraklarını ayarlayammızı sağlar. Geçersiz kılmalar ve durumları, [hiyerarşik durum geçersiz kılmaları](../../../overview/features/override-hierarchical-state.md) sayfasında açıklanmaktadır. 

Bu uygulama aynı anda tek bir duruma geçiş yapar. Ancak, tek varlıklarda birden fazla geçersiz kılmayı birleştirmek ve hiyerarşideki farklı düzeylerde birleşimler oluşturmak tamamen mümkündür. Örneğin, `Selected` ve `SeeThrough` tek bir bileşeni birleştirmek, aynı zamanda saydam hale getirerek bir ana hat verir. Ya da bir alt varlığın geçersiz kılınmasına izin vermekle birlikte, kök varlık geçersiz kılmayı, `Hidden` `ForceOn` `Hidden` `ForceOff` alt öğe dışındaki her şeyi gizleyecek şekilde ayarlamak.

Varlıklara durum uygulamak için daha önce oluşturulan **Remoteentityhelper** 'ı değiştirebiliriz.

1. **Baseremoteentityhelper** soyut sınıfını uygulamak Için **remoteentityhelper** sınıfını değiştirin. Bu değişiklik, **öğretici varlıklarda**sunulan bir görünüm denetleyicisinin kullanılmasına izin verir. Bu, değiştirildiğinde şöyle görünmelidir:

    ```csharp
    public class RemoteEntityHelper : BaseRemoteEntityHelper
    ```

2. Aşağıdaki kodu kullanarak soyut yöntemleri geçersiz kılın:

    ```csharp
    public override BaseEntityOverrideController EnsureOverrideComponent(Entity entity)
    {
        var entityGameObject = entity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
        var overrideComponent = entityGameObject.GetComponent<EntityOverrideController>();
        if (overrideComponent == null)
            overrideComponent = entityGameObject.AddComponent<EntityOverrideController>();
        return overrideComponent;
    }

    public override HierarchicalEnableState GetState(Entity entity, HierarchicalStates feature)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        return overrideComponent.GetState(feature);
    }

    public override void ToggleHidden(Entity entity)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        overrideComponent.ToggleHidden();
    }

    public override void ToggleSelect(Entity entity)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        overrideComponent.ToggleSelect();
    }

    public override void ToggleSeeThrough(Entity entity)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        overrideComponent.ToggleSeeThrough();
    }

    public Color TintColor = new Color(0.0f, 1.0f, 0.0f, 0.1f);
    public override void ToggleTint(Entity entity)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        overrideComponent.ToggleTint(TintColor);
    }

    public override void ToggleDisableCollision(Entity entity)
    {
        var overrideComponent = EnsureOverrideComponent(entity);
        overrideComponent.ToggleHidden();
    }

    public override void RemoveOverrides(Entity entity)
    {
        var entityGameObject = entity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
        var overrideComponent = entityGameObject.GetComponent<EntityOverrideController>();
        if (overrideComponent != null)
        {
            overrideComponent.RemoveOverride();
            Destroy(overrideComponent);
        }
    }
    ```

Bu kod, hedef varlığa bir **EntityOverrideController** bileşeninin eklenmesini sağlar ve sonra iki durumlu yöntemden birini çağırır. İsterseniz, **Testmodel** oyunobject üzerinde bu yardımcı yöntemleri **çağırmak, bir geri çağırma olarak,** `OnRemoteEntityClicked` **Remotersıayakyon pointerhandler** bileşenindeki bir geri çağrı olarak eklenerek yapılabilir.

![İşaretçi geri çağırmaları](./media/pointer-event-callbacks.png)

Artık bu betikler modele eklendiğine göre, çalışma zamanına bağlandıktan sonra **Appmenu** görünüm denetleyicisi, **EntityOverrideController** betiği ile etkileşim kurmak için ek arabirimlerin etkinleştirilmiş olması gerekir. Kilidi açılmış görünüm denetleyicilerini görmek için **model araçları** menüsüne göz atın.

Bu noktada, **Testmodel** gameobject bileşenleri şuna benzer şekilde görünmelidir:

![Ek betiklerle test modeli](./media/test-model-updated.png)

Tek bir varlıkta yığınlama geçersiz kılmalarının bir örneği aşağıda verilmiştir. Yalnızca `Select` `Tint` bir ana hat ve renklendirmeyi sağlamak için kullanılır:

![Test modeli renk tonu seçme](./media/select-tint-test-model.png)

## <a name="cut-planes"></a>Düzlemleri kesme

[Kesin düzlemler](../../../overview/features/cut-planes.md) , herhangi bir uzak varlığa eklenebilen bir özelliktir. En yaygın olarak, kesilmiş düzlemi bileşenini tutmak için herhangi bir ağ verileriyle ilişkilendirilmemiş yeni bir uzak varlık oluşturursunuz. Kesilen düzlemin konumu ve yönü, eklendiği uzak varlığın konumu ve yönüne göre belirlenir.

Otomatik olarak uzak bir varlık oluşturan, bir düz düzlem bileşeni ekleyen ve bir yerel nesnenin dönüşümünü, düz bir nesne ile eşitlenen bir komut dosyası oluşturacağız. Daha sonra, **CutPlaneViewController** 'yi, onu işlememizi sağlayacak bir arabirimdeki kesme düzlemini kaydırabileceğiniz şekilde kullanabiliriz.

1. **RemoteCutPlane** adlı yeni bir betik oluşturun ve kodunu aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using UnityEngine;

    public class RemoteCutPlane : BaseRemoteCutPlane
    {
        public Color SliceColor = new Color(0.5f, 0f, 0f, .5f);
        public float FadeLength = 0.01f;
        public Axis SliceNormal = Axis.Y_Neg;

        public bool AutomaticallyCreate = true;

        private CutPlaneComponent remoteCutPlaneComponent;
        private bool cutPlaneReady = false;

        public override bool CutPlaneReady 
        { 
            get => cutPlaneReady;
            set 
            { 
                cutPlaneReady = value;
                CutPlaneReadyChanged?.Invoke(cutPlaneReady);
            }
        }

        public override event Action<bool> CutPlaneReadyChanged;

        public UnityBoolEvent OnCutPlaneReadyChanged = new UnityBoolEvent();

        public void Start()
        {
            // Hook up the event to the Unity event
            CutPlaneReadyChanged += (ready) => OnCutPlaneReadyChanged?.Invoke(ready);

            RemoteRenderingCoordinator.CoordinatorStateChange += RemoteRenderingCoordinator_CoordinatorStateChange;
            RemoteRenderingCoordinator_CoordinatorStateChange(RemoteRenderingCoordinator.instance.CurrentCoordinatorState);
        }

        private void RemoteRenderingCoordinator_CoordinatorStateChange(RemoteRenderingCoordinator.RemoteRenderingState state)
        {
            switch (state)
            {
                case RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected:
                    if (AutomaticallyCreate)
                        CreateCutPlane();
                    break;
                default:
                    DestroyCutPlane();
                    break;
            }
        }

        public override void CreateCutPlane()
        {
            //Implement me
        }

        public override void DestroyCutPlane()
        {
            //Implement me
        }
    }
    ```

    Bu kod, **öğretici varlıklarına**dahil olan **Baseremotecutdüzlemi** sınıfını genişletir. Uzaktan işlenen modele benzer şekilde, bu betik `RemoteRenderingState` uzak düzenleyiciyle ilgili değişiklikleri iliştirir ve dinler. Düzenleyici duruma ulaştığında, gerekiyorsa `RuntimeConnected` otomatik olarak bağlanmaya çalışır. Ayrıca, `CutPlaneComponent` izlemekte olduğumuz bir değişken de var. Bu, Uzak oturumdaki kesme düzlemi ile eşitlenen Azure uzaktan Işleme bileşenidir. Kesilen düzlemi oluşturmak için nelere ihtiyacım olduğuna göz atalım.

2. `CreateCutPlane()`Yöntemini aşağıdaki tamamlanmış sürümle değiştirin:

    ```csharp
    public override void CreateCutPlane()
    {
        if (remoteCutPlaneComponent != null)
            return; //Nothing to do!

        //Create a root object for the cut plane
        var cutEntity = RemoteRenderingCoordinator.CurrentSession.Actions.CreateEntity();

        //Bind the remote entity to this game object
        cutEntity.BindToUnityGameObject(this.gameObject);

        //Sync the transform of this object so we can move the cut plane
        var syncComponent = this.gameObject.GetComponent<RemoteEntitySyncObject>();
        syncComponent.SyncEveryFrame = true;

        //Add a cut plane to the entity
        remoteCutPlaneComponent = RemoteRenderingCoordinator.CurrentSession.Actions.CreateComponent(ObjectType.CutPlaneComponent, cutEntity) as CutPlaneComponent;

        //Configure the cut plane
        remoteCutPlaneComponent.Normal = SliceNormal;
        remoteCutPlaneComponent.FadeColor = SliceColor.toRemote();
        remoteCutPlaneComponent.FadeLength = FadeLength;
        CutPlaneReady = true;
    }
    ```

    Burada, uzak bir varlık oluşturuyor ve bunu yerel bir oyun nesnesine bağlamamız yapıyoruz. Uzak varlığın dönüşümünü, ayarına göre yerel dönüşümle eşitlenmiş olacak şekilde sağladığınızdan emin veriyoruz `SyncEveryFrame` `true` . Ardından, `CreateComponent` uzak nesneye bir eklemek için çağrısını kullanırız `CutPlaneComponent` . Son olarak, kes düzlemini Monodavranış üst kısmında tanımlanan ayarlarla yapılandırdık. Yöntemi uygulayarak kesme düzleminin temizlenme sürecini görelim `DestroyCutPlane()` .

3. `DestroyCutPlane()`Yöntemini aşağıdaki tamamlanmış sürümle değiştirin:

    ```csharp
    public override void DestroyCutPlane()
    {
        if (remoteCutPlaneComponent == null)
            return; //Nothing to do!

        remoteCutPlaneComponent.Owner.Destroy();
        remoteCutPlaneComponent = null;
        CutPlaneReady = false;
    }
    ```

Uzak nesne oldukça basittir ve yalnızca uzak ucu temizliyoruz (ve yerel nesnemizi koruyarak), `Destroy` uzak nesneyi çağırmak kolaydır ve buna yönelik başvurumuzu temizler.

**Appmenu** , kesme düzlemine otomatik olarak eklenecek ve bununla etkileşime girebilen bir görünüm denetleyicisi içerir. **Appmenu** veya görünüm denetleyicilerinden herhangi birini kullanmanız gerekmez, ancak daha iyi bir deneyim için bu şekilde yapılır. Artık kesilmiş düzlemi ve görünüm denetleyicisini test edin.

1. Sahnede yeni, boş bir GameObject oluşturun ve **CutPlane**olarak adlandırın.
1. **RemoteCutPlane** bileşenini **CutPlane** gameobject öğesine ekleyin.

   ![Düzlem bileşen yapılandırmasını kes](./media/cut-plane-config.png)

1. Bir uzak oturuma yüklemek ve bağlanmak için Unity düzenleyicisinde oynat ' a basın.
1. MRTK 'nın BT simülasyonu kullanılarak (döndürmek için CTRL tuşunu basılı tutun) CutPlane sahneyi etrafında taşıyın. İç bileşenleri göstermek için BT dilimini **Testmodel** 'e izleyin.

![Düzlemi kes örneği](./media/cut-plane-example-engine.png)

## <a name="configuring-the-remote-lighting"></a>Uzak aydınlatmayı yapılandırma

Uzaktan işleme oturumu tam bir [aydınlatma seçenekleri](../../../overview/features/lights.md)yelpazesi destekler. [Gök dokusu](../../../overview/features/sky.md) için betikler ve uzaktan işlemede kullanılacak iki Unity hafif türü için basit bir harita oluşturacağız.

### <a name="sky-texture"></a>Gök dokusu

Gök dokusunu değiştirirken arasından seçim yapabileceğiniz bir dizi yerleşik Cubemaps vardır. Bunlar, oturuma yüklenir ve gök dokusunu uygulanır. Ayrıca, gök ışığı olarak kullanmak üzere [kendi dokularınızı yüklemek](../../../concepts/textures.md) de mümkündür.

Yük parametreleri biçiminde yerleşik kullanılabilir Cubemaps listesini içeren bir **Remotesky** betiği oluşturacağız. Ardından, kullanıcının seçeneklerden birini seçmesine ve yüklemesine izin vereceğiz.

1. **Remotesky** adlı yeni bir betik oluşturun ve tüm içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using System;
    using System.Collections.Generic;
    using UnityEngine;

    public class RemoteSky : BaseRemoteSky
    {
        public override Dictionary<string, LoadTextureFromSASParams> AvailableCubemaps => builtInTextures;

        private bool canSetSky;
        public override bool CanSetSky
        {
            get => canSetSky;
            set
            {
                canSetSky = value;
                CanSetSkyChanged?.Invoke(canSetSky);
            }
        }

        private string currentSky = "DefaultSky";
        public override string CurrentSky
        {
            get => currentSky;
            protected set
            {
                currentSky = value;
                SkyChanged?.Invoke(value);
            }
        }

        private Dictionary<string, LoadTextureFromSASParams> builtInTextures = new Dictionary<string, LoadTextureFromSASParams>()
        {
            {"Autoshop",new LoadTextureFromSASParams("builtin://Autoshop", TextureType.CubeMap)},
            {"BoilerRoom",new LoadTextureFromSASParams("builtin://BoilerRoom", TextureType.CubeMap)},
            {"ColorfulStudio",new LoadTextureFromSASParams("builtin://ColorfulStudio", TextureType.CubeMap)},
            {"Hangar",new LoadTextureFromSASParams("builtin://Hangar", TextureType.CubeMap)},
            {"IndustrialPipeAndValve",new LoadTextureFromSASParams("builtin://IndustrialPipeAndValve", TextureType.CubeMap)},
            {"Lebombo",new LoadTextureFromSASParams("builtin://Lebombo", TextureType.CubeMap)},
            {"SataraNight",new LoadTextureFromSASParams("builtin://SataraNight", TextureType.CubeMap)},
            {"SunnyVondelpark",new LoadTextureFromSASParams("builtin://SunnyVondelpark", TextureType.CubeMap)},
            {"Syferfontein",new LoadTextureFromSASParams("builtin://Syferfontein", TextureType.CubeMap)},
            {"TearsOfSteelBridge",new LoadTextureFromSASParams("builtin://TearsOfSteelBridge", TextureType.CubeMap)},
            {"VeniceSunset",new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap)},
            {"WhippleCreekRegionalPark",new LoadTextureFromSASParams("builtin://WhippleCreekRegionalPark", TextureType.CubeMap)},
            {"WinterRiver",new LoadTextureFromSASParams("builtin://WinterRiver", TextureType.CubeMap)},
            {"DefaultSky",new LoadTextureFromSASParams("builtin://DefaultSky", TextureType.CubeMap)}
        };

        public UnityBoolEvent OnCanSetSkyChanged;
        public override event Action<bool> CanSetSkyChanged;

        public UnityStringEvent OnSkyChanged;
        public override event Action<string> SkyChanged;

        public void Start()
        {
            // Hook up the event to the Unity event
            CanSetSkyChanged += (canSet) => OnCanSetSkyChanged?.Invoke(canSet);
            SkyChanged += (key) => OnSkyChanged?.Invoke(key);

            RemoteRenderingCoordinator.CoordinatorStateChange += ApplyStateToView;
            ApplyStateToView(RemoteRenderingCoordinator.instance.CurrentCoordinatorState);
        }

        private void ApplyStateToView(RemoteRenderingCoordinator.RemoteRenderingState state)
        {
            switch (state)
            {
                case RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected:
                    CanSetSky = true;
                    break;
                default:
                    CanSetSky = false;
                    break;
            }
        }

        public override async void SetSky(string skyKey)
        {
            if (!CanSetSky)
            {
                Debug.Log("Unable to set sky right now");
                return;
            }

            if (AvailableCubemaps.ContainsKey(skyKey))
            {
                Debug.Log("Setting sky to " + skyKey);
                //Load the texture into the session
                var texture = await RemoteRenderingCoordinator.CurrentSession.Actions.LoadTextureFromSASAsync(AvailableCubemaps[skyKey]).AsTask();

                //Apply the texture to the SkyReflectionSettings
                RemoteRenderingCoordinator.CurrentSession.Actions.SkyReflectionSettings.SkyReflectionTexture = texture;
                SkyChanged?.Invoke(skyKey);
            }
            else
            {
                Debug.Log("Invalid sky key");
            }
        }
    }
    ```

    Bu kodun en önemli bölümü yalnızca birkaç satır olacaktır:

    ```csharp
    //Load the texture into the session
    var texture = await RemoteRenderingCoordinator.CurrentSession.Actions.LoadTextureFromSASAsync(AvailableCubemaps[skyKey]).AsTask();

    //Apply the texture to the SkyReflectionSettings
    RemoteRenderingCoordinator.CurrentSession.Actions.SkyReflectionSettings.SkyReflectionTexture = texture;
    ```

    Burada, yerleşik blob depolamadan oturum içine yükleyerek kullanılacak dokuya bir başvuru alırız. Daha sonra, bu dokuyu uygulamak için yalnızca oturum öğesine atamanız gerekir `SkyReflectionTexture` .

1. Sahnede boş bir oyun nesnesi oluşturun ve bunu **ufuk ışığı**olarak adlandırın.

1. Bir **Remotesky** betiğini, **ufuk** ve oyun nesnenizin içine ekleyin.

    Sky ışıkları arasında geçiş yapmak, `SetSky` içinde tanımlanan dize anahtarlarından biri ile çağırarak yapılabilir `AvailableCubemaps` . **Appmenu** içinde yerleşik olarak bulunan görünüm denetleyicisi otomatik olarak düğme oluşturur ve kendi olaylarını `SetSky` ilgili anahtar ile çağırmak üzere takar.
1. Unity düzenleyicisinde oynat ' a basın ve bir bağlantı verin.
1. Yerel çalışma zamanını uzak bir oturuma bağladıktan sonra, farklı gök seçeneklerini araştırmak ve **Testmodelini**nasıl etkileyeceğini görmek Için, **Appmenu > oturum araçları-> uzak çatına** gidin.

### <a name="scene-lights"></a>Sahne ışıkları

Uzak sahne ışıkları şunlardır: nokta, nokta ve yön. Yukarıda oluşturduğumuz kesme düzlemine benzer şekilde, bu sahne ışıkları, bileşenlere ekli olan uzak varlıklardır. Uzak sahsitenizin ışıklandırma, yerel sahninizdeki aydınlatmayı eşleştirmeye çalışırken önemli bir noktadır. HoloLens 2 ' nin birçok Unity uygulaması yerel olarak işlenmiş nesneler için fiziksel tabanlı işleme kullanmadığından, bu strateji her zaman mümkün değildir. Bununla birlikte, belirli bir düzeyde, Unity 'nin daha basit bir varsayılan aydınlatma benzetimi yapabilirsiniz.

1. **Remotelight** adlı yeni bir betik oluşturun ve kodunu aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using UnityEngine;

    [RequireComponent(typeof(Light))]
    public class RemoteLight : BaseRemoteLight
    {
        public bool AutomaticallyCreate = true;

        private bool lightReady = false;
        public override bool LightReady 
        {
            get => lightReady;
            set
            {
                lightReady = value;
                LightReadyChanged?.Invoke(lightReady);
            }
        }

        private ObjectType remoteLightType = ObjectType.Invalid;
        public override ObjectType RemoteLightType => remoteLightType;

        public UnityBoolEvent OnLightReadyChanged;

        public override event Action<bool> LightReadyChanged;

        private Light localLight; //Unity Light

        private Entity lightEntity;
        private LightComponentBase remoteLightComponent; //Remote Rendering Light

        private void Awake()
        {
            localLight = GetComponent<Light>();
            switch (localLight.type)
            {
                case LightType.Directional:
                    remoteLightType = ObjectType.DirectionalLightComponent;
                    break;
                case LightType.Point:
                    remoteLightType = ObjectType.PointLightComponent;
                    break;
                case LightType.Spot:
                case LightType.Area:
                    //Not supported in tutorial
                case LightType.Disc:
                    // No direct analog in remote rendering
                    remoteLightType = ObjectType.Invalid;
                    break;
            }
        }

        public void Start()
        {
            // Hook up the event to the Unity event
            LightReadyChanged += (ready) => OnLightReadyChanged?.Invoke(ready);

            RemoteRenderingCoordinator.CoordinatorStateChange += RemoteRenderingCoordinator_CoordinatorStateChange;
            RemoteRenderingCoordinator_CoordinatorStateChange(RemoteRenderingCoordinator.instance.CurrentCoordinatorState);
        }

        public void OnDestroy()
        {
            lightEntity?.Destroy();
        }

        private void RemoteRenderingCoordinator_CoordinatorStateChange(RemoteRenderingCoordinator.RemoteRenderingState state)
        {
            switch (state)
            {
                case RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected:
                    if (AutomaticallyCreate)
                        CreateLight();
                    break;
                default:
                    DestroyLight();
                    break;
            }
        }

        public override void CreateLight()
        {
            if (remoteLightComponent != null)
                return; //Nothing to do!

            //Create a root object for the light
            if(lightEntity == null)
                lightEntity = RemoteRenderingCoordinator.CurrentSession.Actions.CreateEntity();

            //Bind the remote entity to this game object
            lightEntity.BindToUnityGameObject(this.gameObject);

            //Sync the transform of this object so we can move the light
            var syncComponent = this.gameObject.GetComponent<RemoteEntitySyncObject>();
            syncComponent.SyncEveryFrame = true;

            //Add a light to the entity
            switch (RemoteLightType)
            {
                case ObjectType.DirectionalLightComponent:
                    var remoteDirectional = RemoteRenderingCoordinator.CurrentSession.Actions.CreateComponent(ObjectType.DirectionalLightComponent, lightEntity) as DirectionalLightComponent;
                    //No additional properties
                    remoteLightComponent = remoteDirectional;
                    break;

                case ObjectType.PointLightComponent:
                    var remotePoint = RemoteRenderingCoordinator.CurrentSession.Actions.CreateComponent(ObjectType.PointLightComponent, lightEntity) as PointLightComponent;
                    remotePoint.Radius = 0;
                    remotePoint.Length = localLight.range;
                    //remotePoint.AttenuationCutoff = //No direct analog in Unity legacy lights
                    //remotePoint.ProjectedCubeMap = //No direct analog in Unity legacy lights

                    remoteLightComponent = remotePoint;
                    break;
                default:
                    LightReady = false;
                    return;
            }

            // Set the common values for all light types
            UpdateRemoteLightSettings();

            LightReady = true;
        }

        public override void UpdateRemoteLightSettings()
        {
            remoteLightComponent.Color = localLight.color.toRemote();
            remoteLightComponent.Intensity = localLight.intensity;
        }

        public override void DestroyLight()
        {
            if (remoteLightComponent == null)
                return; //Nothing to do!

            remoteLightComponent.Destroy();
            remoteLightComponent = null;
            LightReady = false;
        }

        [ContextMenu("Sync Remote Light Configuration")]
        public override void RecreateLight()
        {
            DestroyLight();
            CreateLight();
        }

        public override void SetIntensity(float intensity)
        {
            localLight.intensity = Mathf.Clamp(intensity, 0, 1);
            UpdateRemoteLightSettings();
        }

        public override void SetColor(Color color)
        {
            localLight.color = color;
            UpdateRemoteLightSettings();
        }
    }
    ```

    Bu betik, betiğin eklendiği yerel Unity ışığının türüne bağlı olarak farklı türlerde uzak ışıklar oluşturur. Uzak ışık, yerel ışığın bulunduğu konum, döndürme, renk ve yoğunluk içinde yinelenmesine sahip olur. Mümkün olduğunda, uzak ışık ek yapılandırma de ayarlayacaktır. Bu, Unity ışıkları PBR ışıkları olmadığından kusursuz bir eşleşme değildir.

1. Sahnede **DirectionalLight** oyunobject 'i bulun. Sahünüzden varsayılan **DirectionalLight** öğesini kaldırdıysanız: üstteki menü çubuğundan, sahünüzde yeni bir ışık oluşturmak Için *gameobject-> Light-> DirectionalLight* ' ı seçin.

1. **DirectionalLight** gameobject ' i seçin ve **Bileşen Ekle** düğmesini kullanarak **remotelight** betiğini ekleyin.

1. Bu betik Taban sınıfını uyguladığından `BaseRemoteLight` , uzak ışık ile etkileşim kurmak için verilen **appmenu** görünüm denetleyicisini kullanabilirsiniz. **Appmenu > oturum araçları-> yönlü ışık**' a gidin.

    > [!NOTE]
    > **Appmenu** 'deki Kullanıcı arabirimi basitlik için tek yönlü bir ışık ile sınırlandırılmıştır. Ancak, nokta ışıkları eklemek ve bu dosyalara **Remotelight** betiği eklemek yine de mümkündür ve önerilir. Bu ek ışıklar, düzenleyicide Unity ışığının özellikleri düzenlenerek değiştirilebilir. Denetçide **Remotelight** bağlam menüsünü kullanarak yerel değişiklikleri uzak hafif el ile eşitlemeniz gerekir:
    >
    > ![Uzak hafif el ile eşitleme](./media/sync-remote-light.png)

1. Unity düzenleyicisinde oynat ' a basın ve bir bağlantı verin.

1. Çalışma alanınızı uzak bir oturuma bağladıktan sonra, görüntü içinde yönlü ışık görünümü denetleyicisine sahip olmak için kameranızı konumlandırın ve hedefleyin (bağlantı ve sağ tıklama + fare Taşı). 
1. Işığın özelliklerini değiştirmek için uzak hafif görünüm denetleyicisini kullanın. Sahne 'ın aydınlatma üzerinde etkisini görmek için MRTK 'ın birlikte simülasyonu (döndürmek için CTRL tuşunu basılı tutun) ile Yön ışığı kullanın.

    ![Yönlü ışık](./media/directional-light-test.png)

## <a name="editing-materials"></a>Malzemeleri Düzenle

Uzaktan işlenmiş [malzemeler](../../../concepts/materials.md) ek görsel etkiler sağlamak, işlenen modellerin görsellerinin ince ayarlamalar veya kullanıcılara ek geri bildirim sağlamak için değiştirilebilir. Bir malzemeyi değiştirmek için birçok yol ve birçok neden vardır. Burada, bir malzemenin Albedo renginin nasıl değiştirileceğini ve bir PBR malzemelerinin kabalılığını ve metalness nasıl değiştirileceğini göstereceğiz.

> [!NOTE]
> Birçok durumda, bir özellik veya etki **HierarchicalStateOverrideComponent**kullanılarak uygulanmazsa, bu, malzemeyi değiştirmek yerine kullanmak için idealdir.

Hedef varlık kabul eden bir betik oluşturacağız ve `OverrideMaterialProperty` hedef varlığın malzemelerinin özelliklerini değiştirmek için birkaç nesne yapılandırır. Hedef varlığın [**Meshcomponent**](../../../concepts/meshes.md#meshcomponent)'u alarak, ağ üzerinde kullanılan malzemelerin bir listesini içeren bir başlangıç yaptık. Kolaylık olması için yalnızca bulunan ilk malzemeyi kullanacağız. İçeriğin yazılma şekline bağlı olarak bu Naïve stratejisi çok kolay bir şekilde başarısız olabilir. bu nedenle, uygun malzemeleri seçmek için büyük olasılıkla daha karmaşık bir yaklaşım almak isteyeceksiniz.

Malzemeden Albedo gibi ortak değerlere erişebiliyoruz. İlk olarak malzemeler, `PbrMaterial` `ColorMaterial` **Getmaterialcolor** yönteminde görüldüğü gibi, uygun türlerine veya değerlerini almak için ayarlanmalıdır. İstenen malzemeye başvurduktan sonra, değerleri ayarlamanız yeterlidir ve ARR, yerel malzeme özellikleri ile uzak malzemeler arasındaki eşitlemeyi idare eder.

1. **Entitymaterialcontroller** adlı bir komut dosyası oluşturun ve içeriğini şu kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using System.Linq;
    using UnityEngine;
    // to prevent namespace conflicts
    using ARRMaterial = Microsoft.Azure.RemoteRendering.Material;

    public class EntityMaterialController : BaseEntityMaterialController
    {
        public override bool RevertOnEntityChange { get; set; } = true;

        public override OverrideMaterialProperty<Color> ColorOverride { get; set; }
        public override OverrideMaterialProperty<float> RoughnessOverride { get; set; }
        public override OverrideMaterialProperty<float> MetalnessOverride { get; set; }

        private Entity targetEntity;
        public override Entity TargetEntity
        {
            get => targetEntity;
            set
            {
                if (targetEntity != value)
                {
                    if (targetEntity != null && RevertOnEntityChange)
                    {
                        Revert();
                    }

                    targetEntity = value;
                    ConfigureTargetEntity();
                    TargetEntityChanged?.Invoke(value);
                }
            }
        }

        private ARRMaterial targetMaterial;
        private ARRMeshComponent meshComponent;

        public override event Action<Entity> TargetEntityChanged;
        public UnityRemoteEntityEvent OnTargetEntityChanged;

        public void Start()
        {
            // Forward events to Unity events
            TargetEntityChanged += (entity) => OnTargetEntityChanged?.Invoke(entity);

            // If there happens to be a remote RayCaster on this object, assume we should listen for events from it
            if (GetComponent<BaseRemoteRayCastPointerHandler>() != null)
                GetComponent<BaseRemoteRayCastPointerHandler>().RemoteEntityClicked += (entity) => TargetEntity = entity;
        }

        protected override void ConfigureTargetEntity()
        {
            //Get the Unity object, to get the sync object, to get the mesh component, to get the material.
            var targetEntityGameObject = TargetEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

            var localSyncObject = targetEntityGameObject.GetComponent<RemoteEntitySyncObject>();
            meshComponent = targetEntityGameObject.GetComponent<ARRMeshComponent>();
            if (meshComponent == null)
            {
                var mesh = localSyncObject.Entity.FindComponentOfType<MeshComponent>();
                if (mesh != null)
                {
                    targetEntityGameObject.BindArrComponent<ARRMeshComponent>(mesh);
                    meshComponent = targetEntityGameObject.GetComponent<ARRMeshComponent>();
                }
            }

            meshComponent.enabled = true;

            targetMaterial = meshComponent.RemoteComponent.Mesh.Materials.FirstOrDefault();
            if (targetMaterial == default)
            {
                return;
            }

            ColorOverride = new OverrideMaterialProperty<Color>(
                GetMaterialColor(targetMaterial), //The original value
                targetMaterial, //The target material
                ApplyMaterialColor); //The action to take to apply the override

            //If the material is a PBR material, we can override some additional values
            if (targetMaterial.MaterialSubType == MaterialType.Pbr)
            {
                var firstPBRMaterial = (PbrMaterial)targetMaterial;

                RoughnessOverride = new OverrideMaterialProperty<float>(
                    firstPBRMaterial.Roughness, //The original value
                    targetMaterial, //The target material
                    ApplyRoughnessValue); //The action to take to apply the override

                MetalnessOverride = new OverrideMaterialProperty<float>(
                    firstPBRMaterial.Metalness, //The original value
                    targetMaterial, //The target material
                    ApplyMetalnessValue); //The action to take to apply the override
            }
            else //otherwise, ensure the overrides are cleared out from any previous entity
            {
                RoughnessOverride = null;
                MetalnessOverride = null;
            }
        }

        public override void Revert()
        {
            if (ColorOverride != null)
                ColorOverride.OverrideActive = false;

            if (RoughnessOverride != null)
                RoughnessOverride.OverrideActive = false;

            if (MetalnessOverride != null)
                MetalnessOverride.OverrideActive = false;
        }

        private Color GetMaterialColor(ARRMaterial material)
        {
            if (material == null)
                return default;

            if (material.MaterialSubType == MaterialType.Color)
                return ((ColorMaterial)material).AlbedoColor.toUnity();
            else
                return ((PbrMaterial)material).AlbedoColor.toUnity();
        }

        private void ApplyMaterialColor(ARRMaterial material, Color color)
        {
            if (material == null)
                return;

            if (material.MaterialSubType == MaterialType.Color)
                ((ColorMaterial)material).AlbedoColor = color.toRemoteColor4();
            else
                ((PbrMaterial)material).AlbedoColor = color.toRemoteColor4();
        }

        private void ApplyRoughnessValue(ARRMaterial material, float value)
        {
            if (material == null)
                return;

            if (material.MaterialSubType == MaterialType.Pbr) //Only PBR has Roughness
                ((PbrMaterial)material).Roughness = value;
        }

        private void ApplyMetalnessValue(ARRMaterial material, float value)
        {
            if (material == null)
                return;

            if (material.MaterialSubType == MaterialType.Pbr) //Only PBR has Metalness
                ((PbrMaterial)material).Metalness = value;
        }
    }
    ```

Bu `OverrideMaterialProperty` tür, istenirse diğer birkaç malzeme değerinin değiştirilmesine izin verecek kadar esnek olmalıdır. `OverrideMaterialProperty`Tür, bir geçersiz kılmanın durumunu izler, eski ve yeni değeri korur ve geçersiz kılmayı ayarlamak için bir temsilci kullanır. Örnek olarak, şuna göz atın `ColorOverride` :

```csharp
ColorOverride = new OverrideMaterialProperty<Color>(
    GetMaterialColor(targetMaterial), //The original value
    targetMaterial, //The target material
    ApplyMaterialColor); //The action to take to apply the override
```

Bu, `OverrideMaterialProperty` geçersiz kılmanın türü kaydıracağı yeni bir oluşturur `Color` . Geçersiz kılma oluşturulduğu sırada geçerli veya orijinal rengi sağlıyoruz. Ayrıca, üzerinde işlem yapmak için bir ARR malzemesi sunuyoruz. Son olarak, geçersiz kılmayı uygulayacak bir temsilci sağlanmış olur. Temsilci, bir ARR malzemesini kabul eden bir yöntemdir ve geçersiz kılma kaydırdığı tür. Bu yöntem, ARR 'nin malzeme değerlerini nasıl ayarladığına ilişkin en önemli parçasıdır.

, `ColorOverride` `ApplyMaterialColor` İşini yapmak için yöntemini kullanır:

```csharp
private void ApplyMaterialColor(ARRMaterial material, Color color)
{
    if (material.MaterialSubType == MaterialType.Color)
        ((ColorMaterial)material).AlbedoColor = color.toRemoteColor4();
    else
        ((PbrMaterial)material).AlbedoColor = color.toRemoteColor4();
}
```

Bu kod, bir malzemeyi ve rengi kabul eder. Bu, ne tür bir malzeme olduğunu kontrol eder ve bu durumda renk uygulamak için malzemenin bir saçılması olur.

Ve, işlerini `RoughnessOverride` `MetalnessOverride` `ApplyRoughnessValue` yapmak için ve yöntemlerini kullanarak benzer şekilde çalışır `ApplyMetalnessValue` .

Daha sonra, malzeme denetleyicisini test edelim.

1. **Entitymaterialcontroller** betiğini **testmodel** gameobject ' e ekleyin.
1. Sahneye başlamak ve ARR 'ye bağlanmak için Unity 'de oynat ' a basın.
1. Çalışma alanınızı uzak bir oturuma bağladıktan ve modeli yükledikten sonra, **Appmenu-> model araçları** ' na gidin ve malzemeyi düzenleyin >
1. **Testmodel**' e tıklayarak modelden bir varlık seçin.
1. Malzeme görünümü denetleyicisinin (**Appmenu->model araçları->düzenleme malzemesini**) hedeflenen varlığa güncelleştirildiğini doğrulayın.
1. Hedeflenen varlıktaki malzemeleri ayarlamak için malzeme görünümü denetleyicisini kullanın.

Yalnızca ağ ilk malzemesini değiştirdiğimiz için malzemenin değiştirilmesini göremeyebilirsiniz. Değiştirdiğiniz malzemenin ağ içinde olup olmadığını görmek için, **seethrough** hiyerarşik geçersiz kılmayı kullanın.

![Malzeme düzenleme örneği](./media/material-edit-example.png)

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Artık Azure uzaktan Işleme 'nin tüm temel işlevlerini uyguladık. Sonraki bölümde, Azure uzaktan Işleme ve BLOB depolama alanınızı güvenli hale getirmenin hakkında bilgi edineceksiniz. Bunlar, Azure uzaktan Işleme kullanan bir ticari uygulamayı serbest bırakan ilk adım olacaktır.

> [!div class="nextstepaction"]
> [Sonraki: Azure uzaktan Işleme ve model depolamanın güvenliğini sağlama](../security/security.md)