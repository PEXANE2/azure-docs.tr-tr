---
title: Arabirimler ve özel modeller
description: Azure uzaktan Işleme tarafından işlenecek görünüm denetleyicilerini ve alma özel modellerini ekleyin
author: florianborn71
ms.author: flborn
ms.date: 06/15/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: ebadaf51a7dfbb286dac0bbdb0c3c8437ae2356f
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022233"
---
# <a name="tutorial-interfaces-and-custom-models"></a>Öğretici: arabirimler ve özel modeller

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Projeye karma gerçeklik araç seti ekle
> * Model durumunu Yönet
> * Model alımı için Azure Blob depolamayı yapılandırma
> * Oluşturma için modelleri yükleme ve işleme

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici [öğretici: uzaktan işlenmiş bir modeli görüntüleme](../view-remote-models/view-remote-models.md).

## <a name="get-started-with-the-mixed-reality-toolkit-mrtk"></a>Karma Gerçeklik araç seti 'ni kullanmaya başlama (MRTK)

Karma Gerçeklik araç seti (MRTK), karma gerçeklik deneyimleri oluşturmaya yönelik platformlar arası bir araç setidir. Kendi etkileşim ve görselleştirme özellikleri için MRTK 2,3 kullanacağız.

MRTK eklemek için, [mrtk ile çalışmaya](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html)başlama bölümünde listelenen [gerekli adımları](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html#required) izleyin.

Bu adımlar şunlardır:
 - [En son MRTK Unity paketlerini al](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html#get-the-latest-mrtk-unity-packages)
     - "En son" diyor olsa da bu sürüm 2,3 ' dir.
     - Bu öğreticide yalnızca *Foundation* paketini kullanıyoruz. *Uzantılar*, *Araçlar*ve *örnekler* paketleri gerekli değildir.
 - [MRTK paketlerini Unity projenize aktarın](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html#import-mrtk-packages-into-your-unity-project)
 - [Unity projenizi hedef platforma değiştirme](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html#switch-your-unity-project-to-the-target-platform)
     - Bu adımı ilk bölümde yapmış olmanız gerekir, ancak şimdi de iki kez denetlenecek zaman iyi bir yoldur!
 - [Yeni bir sahneye veya yeni projeye MRTK ekleyin](https://microsoft.github.io/MixedRealityToolkit-Unity/version/releases/2.3.0/Documentation/GettingStartedWithTheMRTK.html#add-mrtk-to-a-new-scene-or-new-project)
     - MRTK 'yi yeni bir sahneye ekleyebilir ve düzenleyicinizi ve model nesnelerinizi/betikleri yeniden ekleyebilir ya da karma gerçeklik araç seti 'ni kullanarak var olan sahneye MRTK ekleyebilirsiniz *-> sahneye ekleyin ve* menü komutunu yapılandırın.

## <a name="import-assets-used-by-this-tutorial"></a>Bu öğretici tarafından kullanılan varlıkları içeri aktar

Bu bölümde başlayarak, kapsanan malzemenin büyük bir bölümü için basit bir [model-görünüm-denetleyici modeli](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) uygulayacağız. *Modelin model* parçası Azure uzaktan işleme özgü kodu ve Azure uzaktan işleme ile ilgili durum yönetimi ' dir. Düzenin *Görünüm* ve *Denetleyici* parçaları mrtk varlıkları ve bazı özel betikler kullanılarak uygulanır. Bu öğreticide, burada uygulanan *Görünüm denetleyicisi* olmadan *modeli* kullanmak mümkündür. Bu ayrım, bu öğreticide bulunan kodu, tasarım deseninin *Görünüm denetleyicisi* bölümünün üzerinde bulunacağı kendi uygulamanıza kolayca tümleştirmenize olanak sağlar.

MRTK 'nin tanıtılmasıyla birlikte, etkileşimleri ve görsel geri bildirimleri desteklemek için artık projeye eklenebilecek birçok komut dosyası, ön ek ve varlık vardır. **Öğretici varlıkları**olarak anılan bu varlıklar, ' \Unity\TutorialAssets\TutorialAssets.unitypackage ' Içindeki [Azure uzaktan işleme GitHub](https://github.com/Azure/azure-remote-rendering) ' da yer alan bir [Unity varlık paketine](https://docs.unity3d.com/Manual/AssetPackages.html)paketlenmiştir.

1. Yüklemeyi bilinen bir konuma ayıklayın, git deposu [Azure uzaktan oluşturma](https://github.com/Azure/azure-remote-rendering)'yı kopyalayın veya indirin.
1. Unity projenizde *varlıklar-> paketi Içeri aktar-> özel paket*' i seçin.
1. Dosya Gezgini 'nde, Azure uzaktan Işleme deposunu Klonladığınız veya sıkıştıraldığınız dizine gidin, ardından Unity 'de bulunan. unitypackage ' ı seçin **> TutorialAssets-> TutorialAssets. unitypackage**
1. Paketin içeriğini projenize aktarmak için **Içeri aktar** düğmesini seçin.
1. Unity düzenleyicisinde, *karışık gerçeklik araç seti-> yardımcı > programları ' nı seçin ve en üstteki menü çubuğundan hafif Işleme ardışık düzeni için komut ISTEMLERINI k standart gölgelendiriciyi yükseltin* ve gölgelendiriciyi yükseltmek için istemleri izleyin.

MRTK ve öğretici varlıkları projeye dahil edildikten sonra, MRTK profilini öğretici için daha uygun bir şekilde geçeceğiz.

1. Sahne hiyerarşisinde **Mixedrealitytoolkit** oyunobject ' i seçin.
1. Denetçisinde, **Mixedrealitytoolkit** bileşeni altında yapılandırma profilini *ARRMixedRealityToolkitConfigurationProfile*olarak değiştirin.
1. Değişikliklerinizi kaydetmek için *CTRL + S* tuşlarına basın.

Bu işlem, birincil olarak MRTK, varsayılan HoloLens 2 profilleriyle yapılandırılır. Belirtilen profiller aşağıdaki yollarla önceden yapılandırılmıştır:
 - Profil oluşturucuyu devre dışı bırakın (9 ' a basarak açın/kapatın veya cihazda "profil oluşturucuyu göster/gizle" deyin).
 - Göz kaze imlecini devre dışı bırakın.
 - Unity fare tıklamalarını etkinleştirin. bu nedenle, simülasyonu yerine fareyle MRTK Kullanıcı arabirimi öğeleri ' ne tıklayabilirsiniz.

## <a name="add-the-app-menu"></a>Uygulama menüsünü ekleyin

Bu öğreticideki görünüm denetleyicilerinin çoğu somut sınıflara göre değil soyut temel sınıflara karşı çalışır. Bu model daha fazla esneklik sağlar ve Azure uzaktan Işleme kodunu öğrenmemize yardımcı olmaya devam ederken, sizin için görünüm denetleyicileri sağlamamızı sağlar. Kolaylık olması için, **Remoterenderingcoordinator** sınıfı için bir soyut sınıf sağlanmadı ve görünüm denetleyicisi doğrudan somut sınıfa karşı çalışır.

Artık, geçerli oturum durumunu görsel geri bildirimde bulunmak için, sahneye prefab **Appmenu** ekleyebilirsiniz. Bu görünüm denetleyicisi, geliştirdiğimiz ve daha fazla ARR özelliği sahnede tümleştirtiğimiz için "daha fazla alt menü görünümü denetleyicisi" menüsünü kaldırır. Şimdilik, **Appmenu** , ARR durumuna ilişkin görsel bir belirtiye sahip olur ve kullanıcının, ARR 'ye bağlanmak üzere uygulamayı yetkilendirmek için kullandığı kalıcı paneli sunar.

1. *Varlıklar/Remoterenderingöğreticisi/Prefabs/appmenu* Içindeki **appmenu** prefab konumunu bulun
1. **Appmenu** prefab ' i sahneye sürükleyin.
1. Büyük olasılıkla bu, sahneye *metin ağı Pro* varlıkları dahil ettiğimiz Için **tmp İçeri Aktarıcı**için bir iletişim kutusu görürsünüz. **Tmp Essentials 'ı Içeri aktarmak**için istemleri izleyin. Ardından içeri aktarma iletişim kutusunu kapatın, örnekler ve ek özellikler gerekli değildir.
1. **Appmenu** otomatik olarak yedeklenecek ve bir oturuma bağlanmaya yönelik kalıcı olarak, daha önce yerleştirilmiş olan atlamayı kaldırabilmemiz için yapılandırılır. **Remoterenderingcoordinator** oyunobject üzerinde, daha önce uyguladığımız yetkilendirme Için, **Yetkilendirme isteğinde bulunan üzerinde** '-' düğmesine basarak atlayın.
 ![Atlamayı kaldırın ](./media/remove-bypass-event.png) .
1. Unity düzenleyicisinde **oynat** ' a basarak görünüm denetleyicisini test edin.
1. Düzenleyicide, şimdi MRTK 'nin yapılandırıldığından, görünümünüze konum değiştirmek ve sağ fare düğmesini basılı tutarak görünüm yönünü değiştirmek için fareyi hareket ettirmek için etıd anahtarlarını kullanabilirsiniz. Denetimler için bir fikir almak üzere sahne etrafında "itici" i deneyin.
1. Cihazda, avunuzu **uygulama menüsü**' ne kadar, Unity düzenleyicisinde ise kısayol tuşu ' nı kullanarak aktarabilirsiniz.
1. Menü hakkında görüş kaybı yaptıysanız, menüyü almak için 'M ' tuşuna basın. Menü kolay etkileşim için kameraya yerleştirilecektir.
1. Yetkilendirme artık **Appmenu**'nin sağ tarafında bulunan bir istek olarak gösterilecek. Bu işlem, uygulamayı uzaktan işleme oturumlarını yönetmek üzere yetkilendirmek için kullanacaksınız.
 ![UI yetkilendirme](./media/authorize-request-ui.png)
1. Öğreticiye devam etmek için Unity 'yi oynamadan durdurun.

## <a name="manage-model-state"></a>Model durumunu Yönet

Şimdi, durumu izlemek, olaylara yanıt vermek, olayları tetikleme ve yapılandırma için kullanılan yeni bir komut dosyası olan **Remoterenderedmodel** uygulayacağız. Temelde, **Remoterenderedmodel** , içindeki model verilerinin uzak yolunu depolar `modelPath` . Bu,, tanımladığı modeli otomatik olarak yüklemek veya kaldırmak gerekip gerekmediğini görmek için **Remoterenderingcoordinator** 'daki durum değişikliklerini dinler. Bu kendisine iliştirilmiş **Remoterenderedmodel** Içeren gameobject, uzak içerik için yerel üst olur.

**Remoterenderedmodel** betiğinin **öğretici varlıklarından**dahil olan **baseremoterenderedmodel**uyguladığından emin olun. Bu, uzak model görünüm denetleyicisinin betiğe bağlanmasına izin verir.

1. **Remoterenderedmodel** adlı yeni bir betiği, **Remoterenderingcoordinator**ile aynı klasörde oluşturun. Tüm içeriği şu kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Azure.RemoteRendering.Unity;
    using System;
    using UnityEngine;
    using UnityEngine.Events;

    public class RemoteRenderedModel : BaseRemoteRenderedModel
    {
        public bool AutomaticallyLoad = true;

        private ModelState currentModelState = ModelState.NotReady;

        [SerializeField]
        [Tooltip("The friendly name for this model")]
        private string modelDisplayName;
        public override string ModelDisplayName { get => modelDisplayName; set => modelDisplayName = value; }

        [SerializeField]
        [Tooltip("The URI for this model")]
        private string modelPath;
        public override string ModelPath
        {
            get => modelPath.Trim();
            set => modelPath = value;
        }

        public override ModelState CurrentModelState
        {
            get => currentModelState;
            protected set
            {
                if (currentModelState != value)
                {
                    currentModelState = value;
                    ModelStateChange?.Invoke(value);
                }
            }
        }

        public override event Action<ModelState> ModelStateChange;
        public override event Action<float> LoadProgress;
        public override Entity ModelEntity { get; protected set; }

        public UnityEvent OnModelNotReady = new UnityEvent();
        public UnityEvent OnModelReady = new UnityEvent();
        public UnityEvent OnStartLoading = new UnityEvent();
        public UnityEvent OnModelLoaded = new UnityEvent();
        public UnityEvent OnModelUnloading = new UnityEvent();

        public UnityFloatEvent OnLoadProgress = new UnityFloatEvent();

        public void Awake()
        {
            // Hook up the event to the Unity event
            LoadProgress += (progress) => OnLoadProgress?.Invoke(progress);

            ModelStateChange += HandleUnityStateEvents;
        }

        private void HandleUnityStateEvents(ModelState modelState)
        {
            switch (modelState)
            {
                case ModelState.NotReady:  OnModelNotReady?.Invoke();  break;
                case ModelState.Ready:     OnModelReady?.Invoke();     break;
                case ModelState.Loading:   OnStartLoading?.Invoke();   break;
                case ModelState.Loaded:    OnModelLoaded?.Invoke();    break;
                case ModelState.Unloading: OnModelUnloading?.Invoke(); break;
            }
        }

        private void Start()
        {
            //Attach to and initialize current state (in case we're attaching late)
            RemoteRenderingCoordinator.CoordinatorStateChange += Instance_CoordinatorStateChange;
            Instance_CoordinatorStateChange(RemoteRenderingCoordinator.instance.CurrentCoordinatorState);
        }

        /// <summary>
        /// Listen for state changes on the coordinator, clean up this model's remote objects if we're no longer connected.
        /// Automatically load if required
        /// </summary>
        private void Instance_CoordinatorStateChange(RemoteRenderingCoordinator.RemoteRenderingState state)
        {
            switch (state)
            {
                case RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected:
                    CurrentModelState = ModelState.Ready;
                    if (AutomaticallyLoad)
                        LoadModel();
                    break;
                default:
                    UnloadModel();
                    break;
            }
        }

        private void OnDestroy()
        {
            RemoteRenderingCoordinator.CoordinatorStateChange -= Instance_CoordinatorStateChange;
            UnloadModel();
        }

        /// <summary>
        /// Asks the coordinator to create a model entity and listens for coordinator state changes
        /// </summary>
        [ContextMenu("Load Model")]
        public override async void LoadModel()
        {
            if (CurrentModelState != ModelState.Ready)
                return; //We're already loaded, currently loading, or not ready to load

            CurrentModelState = ModelState.Loading;

            ModelEntity = await RemoteRenderingCoordinator.instance?.LoadModel(ModelPath, this.transform, SetLoadingProgress);

            if (ModelEntity != null)
                CurrentModelState = ModelState.Loaded;
            else
                CurrentModelState = ModelState.Error;
        }

        /// <summary>
        /// Clean up the local model instances
        /// </summary>
        [ContextMenu("Unload Model")]
        public override void UnloadModel()
        {
            CurrentModelState = ModelState.Unloading;

            if (ModelEntity != null)
            {
                var modelGameObject = ModelEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);
                Destroy(modelGameObject);
                ModelEntity.Destroy();
                ModelEntity = null;
            }

            if (RemoteRenderingCoordinator.instance.CurrentCoordinatorState == RemoteRenderingCoordinator.RemoteRenderingState.RuntimeConnected)
                CurrentModelState = ModelState.Ready;
            else
                CurrentModelState = ModelState.NotReady;
        }

        /// <summary>
        /// Update the Unity progress event
        /// </summary>
        /// <param name="progressValue"></param>
        public override void SetLoadingProgress(float progressValue)
        {
            LoadProgress?.Invoke(progressValue);
        }
    }
    ```

En temel koşullarda, **Remoterenderedmodel** , bir modeli yüklemek için gereken verileri (Bu durumda SAS veya *Builtin://* URI) barındırır ve uzak model durumunu izler. Yükleme zamanı olduğunda, `LoadModel` yöntemi **Remoterenderingcoordinator** 'da çağrılır ve modeli içeren varlık başvuru ve kaldırma için döndürülür.

## <a name="load-the-test-model"></a>Test modelini yükleme

Test modelini yeniden yükleyerek yeni betiği test edelim. Betiği içerecek bir oyun nesnesi oluşturacağız ve test modelinin üst öğesi olması gerekir.

1. Sahnede yeni bir boş oyun nesnesi oluşturun ve **Testmodel**olarak adlandırın.
1. *Remoterenderedmodel* betiğini **testmodel**' e ekleyin.
![RemoteRenderedModel bileşeni ekleme](./media/add-remote-rendered-model-script.png)
1. `Model Display Name`Ve öğesini `Model Path` sırasıyla "*testmodel*" ve "*Builtin://Engine*" ile doldurur.
![Model ayrıntılarını belirtin](./media/add-model-script.png)
1. **X = 0, y = 0, z = 3**konumundaki **testmodel** nesnesini kameranın önüne konumlandırın.
![Konum nesnesi](./media/test-model-position.png)
1. **Automaticallyload** 'un açık olduğundan emin olun.
1. Uygulamayı test etmek için Unity düzenleyicisinde **oynat** ' a basın.
1. Uygulamanın oturum oluşturmasına izin vermek için *Bağlan* düğmesine tıklayarak yetkilendirme izni verin ve bir oturuma bağlanıp modeli otomatik olarak yükler.

Uygulama durumlarında ilerlerken konsolu izleyin. Dikkat edin, bazı durumların tamamlanması biraz zaman alabilir ve ilerleme durumunu göstermez. Sonuç olarak, model yüklemeden günlükleri görürsünüz ve sonra test modeli sahnede işlenir.

**Testmodel** oyunnesnesi 'ni Inspector 'daki dönüştürme yoluyla veya sahne görünümünde taşımayı ve döndürmeyi deneyin. Modelin taşınmakta olduğunu ve oyun görünümünde döndürüleceğini görürsünüz.

![Unity günlüğü](./media/unity-loading-log.png)

## <a name="provision-blob-storage-in-azure-and-custom-model-ingestion"></a>Azure 'da BLOB depolama sağlama ve özel model alımı

Şimdi kendi modelinizi yüklemeyi deneyebilirsiniz. Bunu yapmak için, blob depolamayı yapılandırmanız ve Azure 'da, bir modeli karşıya yüklemeniz ve dönüştürmeniz gerekir, sonra da bu modeli **Remoterenderedmodel** betiği kullanarak yükleyeceğiz. Özel model yükleme adımları, şu anda yüklemek için kendi modeliniz yoksa güvenli bir şekilde atlanabilir.

[Hızlı başlangıç: bir modeli işleme Için dönüştürme](../../../quickstarts/convert-model.md)bölümünde belirtilen adımları izleyin. Bu öğreticinin amacı doğrultusunda **hızlı başlangıçta yeni model Ekle örnek uygulama** bölümüne atlayın. Tanımladığınız modelin *paylaşılan erişim imzası (SAS)* URI 'si olduktan sonra, aşağıdaki sonraki adıma geçin.

## <a name="load-and-rendering-a-custom-model"></a>Özel bir model yükleme ve işleme

1. Sahnede yeni bir boş Oyunobject oluşturun ve özel modelinize benzer şekilde adlandırın.
1. *Remoterenderedmodel* betiğini yeni oluşturulan oyunobject öğesine ekleyin.
 ![RemoteRenderedModel bileşeni ekleme](./media/add-remote-rendered-model-script.png)
1. `Model Display Name`Modelinize uygun bir adla girin.
1. `Model Path`Yukarıdaki giriş adımlarında oluşturduğunuz modelin *paylaşılan erişim IMZASı (SAS)* URI 'siyle birlikte girin.
1. **X = 0, y = 0, z = 3** konumundaki oyunun önünde kameranın önüne konumlandırın.
1. **Automaticallyload** 'un açık olduğundan emin olun.
1. Uygulamayı test etmek için Unity düzenleyicisinde **oynat** ' a basın.

    Konsolun geçerli durumla doldurulmaya başlayacağı ve sonuç olarak, model yükleme ilerleme durumu iletileri görüntülenir. Özel modeliniz daha sonra sahneye yüklenir.

1. Özel model nesnenizin sahneyi kaldırın. Bu öğreticide en iyi deneyim, test modelini kullanacaktır. ARR 'de çok sayıda model kesinlikle desteklenirken, bu öğretici tek seferde tek bir uzak modeli destekleyecek şekilde yazılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Artık kendi modellerinizi Azure uzaktan Işleme 'ye yükleyebilir ve bunları uygulamanızda görüntüleyebilirsiniz! Ardından, modellerinizi düzenleme konusunda size kılavuzluk ederiz.

> [!div class="nextstepaction"]
> [Sonraki: modelleri düzenleme](../manipulate-models/manipulate-models.md)
