---
title: Modelleri düzenleme
description: Taşıma, döndürme ve daha fazlasını gerçekleştirerek uzaktan işlenmiş modelleri işleme
author: michael-house
ms.author: v-mihous
ms.date: 06/15/2020
ms.topic: tutorial
ms.openlocfilehash: d3ccdc54e50c3b5a722f490e04ce50d4aaf6e2fd
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85569800"
---
# <a name="tutorial-manipulating-models"></a>Öğretici: modelleri düzenleme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Uzaktan işlenmiş modellerin çevresine görsel ve düzenleme sınırları ekleme
> * Taşıma, döndürme ve ölçekleme
> * Uzamsal sorgularla raycast
> * Uzaktan işlenen nesneler için basit animasyonlar ekleme

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici [öğretici: arabirimler ve özel modeller](../custom-models/custom-models.md)üzerinde oluşturulur.

## <a name="query-remote-object-bounds-and-apply-to-local-bounds"></a>Uzak nesne sınırlarını sorgula ve yerel sınırlara Uygula

Uzak nesnelerle etkileşim kurmak için, ilk olarak etkileşimde bulunmak üzere yerel bir gösterimle ihtiyacımız var. [Nesne sınırları](../../../concepts/object-bounds.md) , uzak bir nesnenin hızlı bir şekilde işlenmesi için faydalıdır. Uzak sınırlar, yerel varlık bir başvuru olarak kullanılarak ARR 'den sorgulanabilir. Sınır, model uzak oturuma yüklendikten sonra sorgulanır.

Bir modelin [**sınırları, tüm**](https://docs.unity3d.com/Manual/class-BoxCollider.html)modelin (örneğin, x, y, z eksenleri için bir merkezi ve boyutu tanımlanmış olan) tüm modeli içeren kutuya göre tanımlanır. Aslında, uzak modelin sınırlarını göstermek için Unity 'nin **BoxCollider** kullanacağız.

1. **Remoterenderedmodel** ile aynı dizinde yeni bir komut dosyası oluşturun ve bu dosyayı **remotelimitle**adlandırın.
1. Komut dosyasının içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using UnityEngine;

    [RequireComponent(typeof(BaseRemoteRenderedModel))]
    public class RemoteBounds : BaseRemoteBounds
    {
        //Remote bounds works with a specific remotely rendered model
        private BaseRemoteRenderedModel targetModel = null;

        private BoundsQueryAsync remoteBoundsQuery = null;

        private RemoteBoundsState currentBoundsState = RemoteBoundsState.NotReady;

        public override RemoteBoundsState CurrentBoundsState
        {
            get => currentBoundsState;
            protected set
            {
                if (currentBoundsState != value)
                {
                    currentBoundsState = value;
                    BoundsStateChange?.Invoke(value);
                }
            }
        }

        public override event Action<RemoteBoundsState> BoundsStateChange;

        public void Awake()
        {
            BoundsStateChange += HandleUnityEvents;
            targetModel = GetComponent<BaseRemoteRenderedModel>();

            targetModel.ModelStateChange += TargetModel_OnModelStateChange;
            TargetModel_OnModelStateChange(targetModel.CurrentModelState);
        }

        private void TargetModel_OnModelStateChange(ModelState state)
        {
            switch (state)
            {
                case ModelState.Loaded:
                    QueryBounds();
                    break;
                default:
                    BoundsBoxCollider.enabled = false;
                    CurrentBoundsState = RemoteBoundsState.NotReady;
                    break;
            }
        }

        // Create a query using the model entity
        private void QueryBounds()
        {
            //Implement me
        }

        // Check the result and apply it to the local Unity bounding box if it was successful
        private void ProcessQueryResult(BoundsQueryAsync remoteBounds)
        {
            //Implement me
        }
    }
    ```

    > [!NOTE]
    > Visual Studio 'da bir hata görürseniz *, ' X ' üzerinde talep ediliyor özelliği C# 6 ' da kullanılamaz. Lütfen dil sürümü 7,0 veya üzerini kullanın*, bu hata güvenle yoksayılabilir. Bu, Unity 'nin çözümü ve proje üretimi ile ilgilidir.

    Bu betik, **Baseremoterenderedmodel**uygulayan komut dosyasıyla aynı gameobject öğesine eklenmelidir. Bu durumda, **Remoterenderedmodel**anlamına gelir. Önceki betiklerle benzer şekilde, bu ilk kod tüm durum değişikliklerini, olayları ve uzak sınırlara ilişkin verileri işleyecektir.

    Uygulamak için iki yöntem vardır: **Querylimitlar** ve **processqueryresult**. **Querylimitler** , sınırları getirir ve **processqueryresult** sorgunun sonucunu alır ve bunu yerel **BoxCollider**uygular.

    **Querysýnýrlılar** yöntemi basittir: Uzaktan işleme oturumuna bir sorgu gönderin ve `Completed` olayı dinleyin.

1. **Querylimitle** yöntemini aşağıdaki tamamlanmış yöntemle değiştirin:

    ```csharp
    // Create a query using the model entity
    private void QueryBounds()
    {
        remoteBoundsQuery = targetModel.ModelEntity.QueryLocalBoundsAsync();
        CurrentBoundsState = RemoteBoundsState.Updating;
        remoteBoundsQuery.Completed += ProcessQueryResult;
    }
    ```

    **Processqueryresult** de basittir. Başarılı olup olmadığını görmek için sonucu denetliyoruz. Yanıtınız evet ise, döndürülen sınırları **BoxCollider** kabul edebileceği bir biçimde dönüştürün ve uygulayın.    

1. **Processqueryresult** yöntemini aşağıdaki tamamlanmış yöntemle değiştirin:

    ```csharp
    // Check the result and apply it to the local Unity bounding box if it was successful
    private void ProcessQueryResult(BoundsQueryAsync remoteBounds)
    {
        if (remoteBounds.IsRanToCompletion)
        {
            var newBounds = remoteBounds.Result.toUnity();
            BoundsBoxCollider.center = newBounds.center;
            BoundsBoxCollider.size = newBounds.size;
            BoundsBoxCollider.enabled = true;
            CurrentBoundsState = RemoteBoundsState.Ready;
        }
        else
        {
            CurrentBoundsState = RemoteBoundsState.Error;
        }
    }
    ```

Artık, **Remotesınır** betiği, **Remoterenderedmodel**ile aynı oyun nesnesine eklendiğinde, gerekirse bir **BoxCollider** eklenir ve model `Loaded` durumuna ulaştığında, sınırlar otomatik olarak sorgulanacaktır ve **BoxCollider**öğesine uygulanır.

1. Daha önce oluşturulan **Testmodel** oyunu nesnesini kullanarak **remotelimiti** bileşenini ekleyin.
1. Betiğin eklendiğini onaylayın.

     ![Remotesınır bileşeni ekleme](./media/remote-bounds-script.png)

1. Uygulamayı yeniden çalıştırın. Model yüklendikten kısa bir süre sonra uzak nesnenin sınırlarını görürsünüz. Aşağıdaki değerlere benzer bir şey göreceksiniz:

     ![Sınır güncelleştirildi](./media/updated-bounds.png)

Artık Unity nesnesinde doğru sınırlarla yapılandırılmış yerel bir **BoxCollider** vardır. Sınır, yerel olarak işlenmiş bir nesne için kullandığımız stratejileri kullanarak görselleştirme ve etkileşime izin verir. Örneğin, dönüşüm, fizik ve daha fazlasını değiştirecek betikler.

## <a name="move-rotate-and-scale"></a>Taşıma, döndürme ve ölçekleme  

Uzaktan işlenen nesneleri taşıma, döndürme ve ölçekleme diğer Unity nesneleriyle aynı şekilde çalışacaktır. Bu yöntemde bulunan **Remoterenderingcoordinator**, `LateUpdate` `Update` Şu anda etkin olan oturumu çağırıyor. `Update`Eşitlenecek yerel model varlığı, uzak karşılıklarıyla birlikte dönüşümler olan bir parçasıdır. Uzaktan işlenmiş bir modeli taşımak, döndürmek veya ölçeklendirmek için, yalnızca uzak modeli temsil eden GameObject dönüşümünü taşımanız, döndürmeniz veya ölçeklendirmeniz gerekir. Burada, bir ana oyun nesnesinin, bu kendisine ekli olan **Remoterenderedmodel** betiğinin dönüşümünü değiştirebiliyoruz.

Bu öğretici, nesne etkileşimi için MRTK kullanıyor. Bir nesnenin taşınması, döndürülmesi ve ölçeklendirilmesi için MRTK 'a özgü uygulamanın çoğu Bu öğreticinin kapsamı dışındadır. **Model araçları** menüsünde **appmenu**içinde önceden yapılandırılmış bir model görünüm denetleyicisi vardır.

1. Daha önce oluşturulan **Testmodel** gameobject 'in sahnede bulunduğundan emin olun.
1. **Appmenu** prefab 'nin sahnede bulunduğundan emin olun.
1. Sahneyi oynatmak ve **Appmenu**Içindeki **model araçları** menüsünü açmak için Unity 'nin Play düğmesine basın.
![Denetleyiciyi görüntüle](./media/model-with-view-controller.png)

**Appmenu** , modeliyle bağlama için bir görünüm denetleyicisi uygulayan bir alt menü **modeli araçlarına** sahiptir. GameObject bir **Remotesınır** bileşeni içerdiğinde, görünüm denetleyicisi bir **BoxCollider**ile nesne etrafında bir sınırlayıcı kutu oluşturan bir mrtk bileşeni olan bir [**BoundingBox**](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_BoundingBox.html) bileşeni ekler. Bir Nesneel etkileşiminden sorumlu olan [**Objectlıör**](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/api/Microsoft.MixedReality.Toolkit.Experimental.UI.ObjectManipulator.html?q=ObjectManipulator). Bu komut dosyaları, uzaktan işlenmiş modeli taşımanızı, döndürmemizi ve ölçeklendirmemize olanak sağlayacak.

1. Farenizi oyun paneline taşıyın ve odaklanmak için buraya tıklayın.
1. [Mrtk 'nin el benzetimini](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/InputSimulation/InputSimulationService.html#hand-simulation)kullanarak sol SHIFT tuşuna basın ve basılı tutun.
1. El ışın test modelini işaret ettiğinden, sanal bir yandan

    ![Sivri uçlu ışın](./media/handray-engine.png)

1. Sol tıklama tuşunu basılı tutarak modeli taşımak için sürükleyin.

Uzaktan işlenmiş içeriğin sınırlayıcı kutusuyla birlikte taşınacağını görmeniz gerekir. Uzak içeriğin sınırlayıcı kutusuyla bir gecikme veya gecikme fark edebilirsiniz. Bu gecikme, internet gecikme süresine ve bant genişliğine bağlıdır.

## <a name="ray-cast-and-spatial-queries-of-remote-models"></a>Uzak modellerin ışın dönüştürme ve uzamsal sorguları

Modellerin etrafındaki bir Box Collider, modelin tamamı ile etkileşime geçmek için uygundur, ancak modelin tek tek bölümleriyle etkileşimde bulunmak için yeterince ayrıntılı değildir. Bunu gidermek için, [uzak Ray atama](../../../overview/features/spatial-queries.md#ray-casts)kullanabiliriz. Uzak Ray atama, Azure uzaktan Işleme tarafından, ışınları uzak sahneye dönüştürmek ve isabet sonuçlarını yerel olarak döndürmek için sunulan bir API 'dir. Bu teknik, büyük bir modelin alt varlıklarını seçmek veya konum, yüzey normal ve mesafe gibi isabet sonucu bilgileri almak için kullanılabilir.

Test modeli, sorgulanabilen ve seçilen sayıda alt varlık içerir. Şimdilik, seçim seçili varlığın adını Unity konsoluna çıktı. Seçili varlığı vurgulamak için [malzemeler, aydınlatma ve efektler](../materials-lighting-effects/materials-lighting-effects.md#highlighting-and-outlining) bölümünü kontrol edin.

İlk olarak, uzak Ray cast sorguları etrafında statik bir sarmalayıcı oluşturalım. Bu betik, Unity alanında bir konum ve yön kabul eder, bunu uzak Ray cast tarafından kabul edilen veri türlerine dönüştürür ve sonuçları döndürür. Betik, `RayCastQueryAsync` API 'yi kullanır.

1. **Remoteraycaster** adlı yeni bir betik oluşturun ve içeriğini şu kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System.Linq;
    using System.Threading.Tasks;
    using UnityEngine;

    /// <summary>
    /// Wraps the Azure Remote Rendering RayCast queries to easily send requests using Unity data types
    /// </summary>
    public class RemoteRayCaster
    {
        public static double maxDistance = 30.0;

        public static async Task<RayCastHit[]> RemoteRayCast(Vector3 origin, Vector3 dir, HitCollectionPolicy hitPolicy = HitCollectionPolicy.ClosestHit)
        {
            if(RemoteRenderingCoordinator.instance.CurrentCoordinatorState == RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected)
            {
                var rayCast = new RayCast(origin.toRemotePos(), dir.toRemoteDir(), maxDistance, hitPolicy);
                return await RemoteRenderingCoordinator.CurrentSession.Actions.RayCastQueryAsync(rayCast).AsTask();
            }
            else
            {
                return new RayCastHit[0];
            }
        }

        public static async Task<Entity[]> RemoteRayCastEntities(Vector3 origin, Vector3 dir, HitCollectionPolicy hitPolicy = HitCollectionPolicy.ClosestHit)
        {
            var hits = await RemoteRayCast(origin, dir, hitPolicy);
            return hits.Select(hit => hit.HitEntity).Where(entity => entity != null).ToArray();
        }
    }
    ```

    > [!NOTE]
    > Unity, [**raycasthit**](https://docs.unity3d.com/ScriptReference/RaycastHit.html)adlı bir sınıfa sahiptir ve Azure uzaktan Işleme, [**Raycasthit**](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.raycasthit)adlı bir sınıfa sahiptir. Büyük harf **C** derleme hatalarını önlemek için önemli bir farktır.

    **Remoteraycaster** , uzak ışınları geçerli oturuma atama için ortak bir erişim noktası sağlar. Daha belirgin olması için bir MRTK işaretçi işleyicisi daha sonra uygulayacağız. Betik, `IMixedRealityPointerHandler` Bu komut dosyasının [karma gerçeklik işaretçisi](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Input/Pointers.html) olaylarını DINLEYEBILMEMIZ için mrtk öğesine söylediğimiz arabirimi uygular.

1. **Remoterayyon Pointerhandler** adlı yeni bir betik oluşturun ve kodu şu kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.MixedReality.Toolkit.Input;
    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using UnityEngine;

    public class RemoteRayCastPointerHandler : BaseRemoteRayCastPointerHandler, IMixedRealityPointerHandler
    {
        public UnityRemoteEntityEvent OnRemoteEntityClicked = new UnityRemoteEntityEvent();

        public override event Action<Entity> RemoteEntityClicked;

        public void Awake()
        {
            // Forward events to Unity events
            RemoteEntityClicked += (entity) => OnRemoteEntityClicked?.Invoke(entity);
        }

        public async void OnPointerClicked(MixedRealityPointerEventData eventData)
        {
            if (RemoteEntityClicked != null) //Ensure someone is listening before we do the work
            {
                var firstHit = await PointerDataToRemoteRayCast(eventData.Pointer);
                if (firstHit.success)
                    RemoteEntityClicked.Invoke(firstHit.hit.HitEntity);
            }
        }

        public void OnPointerDown(MixedRealityPointerEventData eventData) { }

        public void OnPointerDragged(MixedRealityPointerEventData eventData) { }

        public void OnPointerUp(MixedRealityPointerEventData eventData) { }

        private async Task<(bool success, RayCastHit hit)> PointerDataToRemoteRayCast(IMixedRealityPointer pointer, HitCollectionPolicy hitPolicy = HitCollectionPolicy.ClosestHit)
        {
            RayCastHit hit;
            var result = pointer.Result;
            if (result != null)
            {
                var endPoint = result.Details.Point;
                var direction = pointer.Rays[pointer.Result.RayStepIndex].Direction;
                Debug.DrawRay(endPoint, direction, Color.green, 0);
                hit = (await RemoteRayCaster.RemoteRayCast(endPoint, direction, hitPolicy)).FirstOrDefault();
            }
            else
            {
                hit = new RayCastHit();
            }
            return (hit.HitEntity != null, hit);
        }
    }
    ```

**RemoteRayCastPointerHandler** `OnPointerClicked` "Tıklamız" gibi bir işaretçi bir Collider üzerinde ' tıkladığı zaman, remoterk,, mrtk tarafından çağırılır. Bundan sonra, `PointerDataToRemoteRayCast` işaretçinin sonucunu bir nokta ve yöne dönüştürmek için çağırılır. Bu nokta ve yön, uzak oturumunda uzak bir ışın dönüştürmek için kullanılır.

![Sınır güncelleştirildi](./media/raycast-local-remote.png)

Tıklama üzerinde ışın dönüşümü için gönderme istekleri, uzak nesneleri sorgulamak için etkili bir stratejidir. Ancak, imleç modelin kendisini değil, Collider Box ile çakışdığından ideal bir kullanıcı deneyimi değildir.

Ayrıca, Uzak oturumdaki ışınını daha sık oluşturan yeni bir MRTK işaretçisi de oluşturabilirsiniz. Bu daha karmaşık bir yaklaşım olsa da, Kullanıcı deneyimi daha iyi olacaktır. Bu strateji, Bu öğreticinin kapsamı dışındadır, ancak bu yaklaşımın bir örneği, [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering/tree/master/Unity/AzureRemoteRenderingShowcase)bulunan gösterimi uygulamasında görülebilir.

Bir Ray cast, **remoterbir Pointerhandler**öğesinde başarıyla tamamlandığında, bu isabet `Entity` `OnRemoteEntityClicked` Unity olayından yayılır. Bu olaya yanıt vermek için, öğesini kabul eden `Entity` ve üzerinde bir eylem gerçekleştiren bir yardımcı komut dosyası oluşturacağız. Hata ayıklama günlüğüne adını yazdırmak için betiği almaya başlayalım `Entity` .

1. **Remoteentityhelper** adlı yeni bir betik oluşturun ve içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.
    
    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using UnityEngine;
    
    public class RemoteEntityHelper : MonoBehaviour
    {
        public void EntityToDebugLog(Entity entity)
        {
            Debug.Log(entity.Name);
        }
    }
    ```

2. Daha önce oluşturulan **Testmodel** oyunobject ' te, hem **Remoterayyon pointerhandler** bileşeni hem de **remoteentityhelper** bileşenini ekleyin.
1. `EntityToDebugLog`Olayına yöntemine atayın `OnRemoteEntityClicked` . Olayın çıkış türü ve yönteminin giriş türü eşleşiyorsa, Unity 'nin dinamik olay kancaı ' nı kullanarak olay değeri otomatik olarak yönteme geçer.
    1. Yeni bir geri arama alanı oluştur \
    ![Geri arama Ekle](./media/add-callback-remote-entity-clicked.png)
    1. **Uzak varlık Yardımcısı** bileşenini, üst oyun nesnesine başvuracak şekilde nesne alanına sürükleyin.
    ![Nesne ata](./media/assign-object.png)
    1. `EntityToDebugLog`Geri arama olarak ata \
    ![Geri arama ata](./media/remote-entity-event.png)
1. Unity düzenleyicisinde Yürüt ' e basarak sahneyi başlatın, uzak bir oturuma bağlanın ve test modelini yükleyin.
1. MRTK 'nin el simülasyonu kullanarak sol SHIFT tuşuna basın ve basılı tutun.
1. El ışın test modelini işaret ettiğinden, sanal bir yandan
1. Bir AIR tap benzetimini yapmak için uzun tıklama `OnPointerClicked` olayı yürütülüyor.
1. Seçili alt varlık adına sahip bir günlük iletisi için Unity konsolunu gözlemleyin. Örneğin: \
![Alt varlık örneği](./media/child-entity-example.png)

## <a name="synchronizing-the-remote-object-graph-into-the-unity-hierarchy"></a>Uzak nesne grafiğini Unity hiyerarşisine eşitleme

Bu noktaya kadar, yalnızca modelin tamamını temsil eden tek bir yerel oyun nesnesi gördük. Bu, tüm modelin işlenmesi ve düzenlemesi için iyi bir sonuç verir. Ancak, etkileri uygulamak veya belirli alt varlıkları işlemek istiyorsam, bu varlıkları temsil etmek için yerel oyun nesneleri oluşturmanız gerekir. İlk olarak, test modelinde el ile keşfedebiliriz.

1. Sahneyi başlatın ve test modelini yükleyin.
1. Unity 'nin hiyerarşisinde **Testmodel** gameobject alt öğelerini genişletin ve **TestModel_Entity** gameobject ' i seçin.
1. Denetçisinde *alt öğeleri göster* düğmesine tıklayın. \
![Alt öğeleri göster](./media/show-remote-children.png)
1. Hiyerarşide alt öğeleri genişletmeye ve alt öğelerin büyük bir listesi gösterilene kadar *alt öğeleri göster* ' e tıklayarak devam edin. \
![Tüm alt öğeler](./media/test-model-children.png)

Onlarca varlıkların listesi artık hiyerarşiyi dolduracaktır. Bunlardan birinin seçilmesi, `Transform` `RemoteEntitySyncObject` Inspector 'daki ve bileşenlerini gösterir. Varsayılan olarak, her bir varlık her karede otomatik olarak eşitlenmez, bu nedenle üzerinde yerel değişiklikler `Transform` sunucu ile eşitlenmez. *Her çerçevenin eşitlemesini* denetleyebilir ve sonra sahneyi sahne görünümünde taşıyabilir, ölçeklendirebilir veya döndürebilir, sahne görünümünde işlenen modeli görmezsiniz, modelin konum ve döndürme görsel güncelini görmek için oyun görünümünü izleyin.

Aynı işlem programlı bir şekilde yapılabilir ve belirli uzak varlıkları değiştirmenin ilk adımıdır.

1. **Remoteentityhelper** betiğini aşağıdaki yöntemi de içerecek şekilde değiştirin:

    ```csharp
    public void MakeSyncedGameObject(Entity entity)
    {
        var entityGameObject = entity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
        var sync = entityGameObject.GetComponent<RemoteEntitySyncObject>();
        sync.SyncEveryFrame = true;
    }
    ```

1. **Remoterayroyon Pointerhandler** olayına ek bir geri çağırma ekleyin `OnRemoteEntityClicked` ve bunu olarak ayarlayarak `MakeSyncedGameObject` . \
![Ek geri arama](./media/additional-callback.png)
1. MRTK 'nin el simülasyonu kullanarak sol SHIFT tuşuna basın ve basılı tutun.
1. El ışın test modelini işaret ettiğinden, sanal bir yandan
1. Bir AIR tap benzetimini yapmak için uzun tıklama `OnPointerClicked` olayı yürütülüyor.
1. Tıklamış varlığı temsil eden yeni bir alt nesne görmek için hiyerarşiyi denetleyip genişletin. \
![Oyunobject temsili](./media/gameobject-representing-entity.png)\
1. Test ettikten sonra, `MakeSyncedGameObject` Bu, daha sonra diğer efektlerin bir parçası olarak birleştireceğiz için geri çağırma işlemini kaldırın.

> [!NOTE]
> Her çerçevenin eşitlenmesi yalnızca dönüşüm verilerini eşitlemeniz gerektiğinde gereklidir. Dönüşümleri eşitlemek için bazı ek yük vardır. bu nedenle, çok dikkatli kullanılmalıdır.

Yerel bir örnek oluşturmak ve bunu otomatik olarak eşitlemeye getirmek, alt varlıkların işlenmesine yönelik ilk adımdır. Modeli bir bütün olarak işlemek için kullandığımız tekniklerin aynısına de alt varlıklarda de erişilebilir. Örneğin, bir varlığın eşitlenmiş yerel bir örneğini oluşturduktan sonra, sınırlarını sorgulayabilir ve kullanıcı tarafında hareket altına alınabilmesini sağlamak için işleme işleyicileri ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık uzaktan işlenmiş modellerinizi işleyebilir ve bunlarla etkileşim kurabilirsiniz! Sonraki öğreticide, malzemeleri değiştirmeyi, aydınlatmayı değiştirmeyi ve uzaktan işlenmiş modellere etkileri uygulamayı ele alacağız.

> [!div class="nextstepaction"]
> [Sonraki: malzemeleri, aydınlatmayı ve etkileri Iyileştirme](../materials-lighting-effects/materials-lighting-effects.md)
