---
title: Sıfırdan BirBirlik projesi ayarlama
description: Azure Uzaktan İşleme ile kullanım için boş bir Unity projesinin nasıl yapılandırılabildiğini açıklar.
author: florianborn71
ms.author: flborn
ms.date: 01/30/2020
ms.topic: tutorial
ms.openlocfilehash: 33801316e4c0446865169560bb42f98052acba70
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679600"
---
# <a name="tutorial-setting-up-a-unity-project-from-scratch"></a>Öğretici: Sıfırdan bir Birlik projesi kurma

Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * ARR için bir çizik Birlik projesi yapılandırma.
> * Oluşturma ve durdurma işleme oturumları.
> * Varolan oturumları yeniden kullanma.
> * Oturumlara bağlanma ve bağlantı kesme.
> * Modelleri bir işleme oturumuna yükleme.
> * Bağlantı istatistiklerini görüntüleme.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için ihtiyacınız olan:

* Hesap bilgileriniz (hesap kimliği, hesap anahtarı, abonelik kimliği). Hesabınız yoksa, bir hesap [oluşturun.](../../how-tos/create-an-account.md)
* Windows SDK 10.0.18362.0 [(indir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Visual Studio 2019'un en son sürümü [(indir)](https://visualstudio.microsoft.com/vs/older-downloads/)
* GIT [(indir)](https://git-scm.com/downloads)
* Birlik 2019.3.1 [(indir)](https://unity3d.com/get-unity/download)
  * Bu modülleri Birlik olarak yükleyin:
    * **UWP** - Universal Windows Platform Build Desteği
    * **IL2CPP** - Windows Yapı Desteği (IL2CPP)

> [!TIP]
> [ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) tüm öğreticiler için hazırlanmış Birlik projeleri içerir. Bu projeleri başvuru olarak kullanabilirsiniz.

## <a name="create-a-new-unity-project"></a>Yeni bir Birlik projesi oluşturun

Unity Hub'dan yeni bir proje oluşturun.
Bu örnekte, projenin `RemoteRendering`.

![yeni proje penceresi](media/new-project.png)

## <a name="configure-the-projects-manifest"></a>Projenin bildirimini yapılandırma

Unity proje klasörünüzde bulunan dosyayı `Packages/manifest.json` değiştirmeniz gerekir. Dosyayı bir metin düzenleyicisinde açın ve aşağıda listelenen satırları ekleyin:

```json
{
  "scopedRegistries": [
    {
      "name": "Azure Mixed Reality Services",
      "url": "https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM/",
      "scopes": ["com.microsoft.azure"]
    }
   ],
  "dependencies": {
    "com.microsoft.azure.remote-rendering": "0.1.11",
    "com.unity.render-pipelines.universal": "7.2.1",
    ...existing dependencies...
  }
}
```

Universal render ardışık ardışık paket isteğe bağlıdır, ancak performans nedenleriyle önerilir.
Bildirimi değiştirip kaydettikten sonra Unity otomatik olarak yenilenir. Paketlerin *Proje* penceresine yüklendiğini onaylayın:

![paket ithalatı onaylamak](media/confirm-packages.png)

## <a name="ensure-you-have-the-latest-version-of-the-package"></a>Paketin en son sürümüne sahip olduğunuzdan emin olun

Aşağıdaki adımlar, projenizin uzaktan işleme paketinin en son sürümünü kullandığından emin olun.
1. Proje penceresindepaketi seçin ve paket simgesine ![tıklayın: Paket simgesini seçme](media/package-icons.png)
1. Denetçi'de "Paket Yöneticisinde Görüntüle"yi tıklatın: ![paket denetçisi](media/package-properties.png)
1. Uzaktan işleme paketi için paket yöneticisi sayfasında, güncelleştirme düğmesinin kullanılabilir olup olmadığını görün. Eğer öyleyse, o zaman tıklayarak en son kullanılabilir ![sürümü paketi günceller: paket yöneticisi ARR paketi](media/package-manager.png)
1. Bazen paketin güncelleştirilmesi konsolda hatalara neden olabilir. Bu durumda, projeyi kapatmayı ve yeniden açmayı deneyin.

## <a name="configure-the-camera"></a>Kamerayı yapılandırma

Ana **Kamera** düğümünü seçin.

1. *Dönüşümünü*Sıfırla:

    ![kamera dönüşümü sıfırlama](media/camera-reset-transform.png)

1. Bayrakları Düz *Renk* olarak **ayarla**

1. **Arka Planı** *Siyaha* Ayarlama

1. Kırpma **Düzlemlerini** *Near = 0.3* ve *Far = 20*olarak ayarlayın. Bu, işlemenin 30 cm'den daha yakın veya 20 metreden daha uzak geometriyi keseceği anlamına gelir.

    ![Unity kamera özellikleri](media/camera-properties.png)

## <a name="adjust-the-project-settings"></a>Proje ayarlarını ayarlama

1. *Proje Ayarlarını > Edit'i Aç...*
1. Soldaki listede Kalite'yi seçin.
1. Varsayılan **Kalite Düzeyini** *Düşük* olarak değiştirme

    ![proje kalite ayarlarını değiştirme](media/settings-quality.png)

1. Soldaki **Grafikler'i** seçin.
1. Komut **Dosyası Oluşturma Boru Hattı** ayarını *HybridRenderingPipeline olarak değiştirin.* Evrensel render ardışık adı kullanılmazsa bu adımı atlayın.

    ![Proje grafik](media/settings-graphics-lwrp.png) ayarlarını değiştirme Bazen UI, paketlerden kullanılabilir ardışık hatlar listesinin doldurulmasını sağlamaz, bu durumda *HybridRenderingPipeline* kıymetinin alana el ile sürülmesi gerekir: ![proje grafik ayarlarını değiştirme](media/hybrid-rendering-pipeline.png)
1. Soldaki **Player'ı** seçin.
1. Evrensel **Windows Platformu ayarları** sekmesini seçin
1. Windows Karışık Gerçeklik'i desteklemek için ![ **XR Ayarlarını** değiştirme: oyuncu ayarları](media/xr-player-settings.png)
1. Yukarıdaki ekran görüntüsündeki ayarları seçin:
    1. **Sanal Gerçekliği Desteklenebilmeyi Etkinleştirin**
    1. **Derinlik Biçimini** *16 Bit Derinliğe* Ayarlama
    1. **Derinlik Arabellek Paylaşımını** Etkinleştir
    1. **Stereo İşleme Modunu** *Tek GeçişLi Örnek olarak* ayarlayın

1. Aynı pencerede, *XR Ayarları'nın*üzerinde, **Yayımlama Ayarlarını** genişletin
1. **Özellikler'e** gidin ve seçin:
    * **InternetClient**
    * **InternetClientServer**
    * **Mekansal Algı**
    * Geliştirme için isteğe bağlı: **PrivateNetworkClientServer**

      Unity uzaktan hata ayıklama cihazını cihazınıza bağlamak istiyorsanız bu seçenek gereklidir.

1. **Desteklenen Aygıt Ailelerinde,** **Holografik** ve **Masaüstü**

1. Karışık Gerçeklik Araç Kiti'ni kullanmak istiyorsanız, önerilen ayarlar ve yetenekler hakkında daha fazla bilgi için [MRTK belgelerine](https://docs.microsoft.com/windows/mixed-reality/unity-development-overview)bakın.

## <a name="validate-project-setup"></a>Proje kurulumlarını doğrulama

Proje ayarlarının doğru olduğunu doğrulamak için aşağıdaki adımları gerçekleştirin.

1. Unity düzenleyici araç çubuğundaki RemoteRendering menüsünden Project'i doğrula girişini seçin.
1. Proje ayarlarını gerektiğinde denetlemek ve düzeltmek için Project'i doğrula penceresini kullanın.

    ![Unity editor proje doğrulama](media/arr-unity-validation.png)

## <a name="create-a-script-to-initialize-azure-remote-rendering"></a>Azure Uzaktan İşleme'yi başlatmayı sağlamak için bir komut dosyası oluşturma

Yeni bir [komut dosyası](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve ona **RemoteRendering**adını verin. Komut dosyası dosyasını açın ve tüm içeriğini aşağıdaki kodla değiştirin:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

#if UNITY_WSA
    using UnityEngine.XR.WSA;
#endif

// ask Unity to automatically append an ARRServiceUnity component when a RemoteRendering script is attached
[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRendering : MonoBehaviour
{
    // fill out the variables below with your account details

    // AccountDomain must be '<region>.mixedreality.azure.com' - if no '<region>' is specified, connections will fail
    // See the documentation for the list of available regions.
    public string AccountDomain = "westus2.mixedreality.azure.com";
    public string AccountId = "<enter your account id here>";
    public string AccountKey = "<enter your account key here>";

    public uint MaxLeaseTimeHours = 0;
    public uint MaxLeaseTimeMinutes = 10;
    public RenderingSessionVmSize VMSize = RenderingSessionVmSize.Standard;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        // initialize Azure Remote Rendering for use in Unity:
        // it needs to know which camera is used for rendering the scene
        RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
        RemoteManagerUnity.InitializeManager(clientInit);

        // lookup the ARRServiceUnity component and subscribe to session events
        arrService = GetComponent<ARRServiceUnity>();
        arrService.OnSessionErrorEncountered += ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged += ARRService_OnSessionStatusChanged;
    }

#if !UNITY_EDITOR
    private void Start()
    {
        StartCoroutine(AutoStartSessionAsync());
    }
#endif

    private void OnDestroy()
    {
        arrService.OnSessionErrorEncountered -= ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged -= ARRService_OnSessionStatusChanged;

        RemoteManagerStatic.ShutdownRemoteRendering();
    }

    private void CreateFrontend()
    {
        if (arrService.Frontend != null)
        {
            // early out if the front-end has been created before
            return;
        }

        // initialize the ARR service with our account details
        AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
        accountInfo.AccountKey = AccountKey;
        accountInfo.AccountId = AccountId;
        accountInfo.AccountDomain = AccountDomain;

        arrService.Initialize(accountInfo);
    }

    public void CreateSession()
    {
        CreateFrontend();

        // StartSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.StartSession(new RenderingSessionCreationParams(VMSize, MaxLeaseTimeHours, MaxLeaseTimeMinutes));
    }

    public void StopSession()
    {
        arrService.StopSession();
    }

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);
    }

    private void ARRService_OnSessionErrorEncountered(ARRServiceUnity caller, SessionGeneralContext context)
    {
        Debug.LogError($"Session error encountered. Context: {context.ErrorMessage}. Request Cv: {context.RequestCorrelationVector}. Response Cv: {context.ResponseCorrelationVector}");
    }

    private async void LogSessionStatus(AzureSession session)
    {
        if (session != null)
        {
            var sessionProperties = await session.GetPropertiesAsync().AsTask();
            LogSessionStatus(sessionProperties);
        }
        else
        {
            var sessionProperties = arrService.LastProperties;
            Debug.Log($"Session ended: Id={sessionProperties.Id}");
        }
    }

    private void LogSessionStatus(RenderingSessionProperties sessionProperties)
    {
        Debug.Log($"Session '{sessionProperties.Id}' is {sessionProperties.Status}. Size={sessionProperties.Size}" +
            (!string.IsNullOrEmpty(sessionProperties.Hostname) ? $", Hostname='{sessionProperties.Hostname}'" : "") +
            (!string.IsNullOrEmpty(sessionProperties.Message) ? $", Message='{sessionProperties.Message}'" : ""));
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
}
```

Bu komut dosyası Azure Uzaktan İşleme'yi başlatılmasını, hangi kamera nesnesinin işlenme için kullanılacağını söyler ve *Oynatma Modu* etkinleştirildiğinde **Oturum Oluştur** düğmesini viewport'a yerleştirir.

> [!CAUTION]
> Komut dosyasının değiştirilmesi ve oyun modu Birlik'te etkinken kaydedilmesi Unity dondurulmasıyla sonuçlanabilir ve görev yöneticisi aracılığıyla komut dosyasını kapatmak zorunda kalırsınız. Bu nedenle, RemoteRendering komut dosyasını düzenlemeden önce oynatma modunu her zaman *durdurun.*

## <a name="test-azure-remote-rendering-session-creation"></a>Azure Uzaktan İşleme oturumu oluşturmayı test edin

Sahnede yeni bir GameObject oluşturun ve *Uzaktan Rendering* bileşenini ekleyin. Uzaktan İşleme hesabınız için uygun *Hesap Etki Alanı,* *Hesap Kimliği*ve *Hesap Anahtarını* doldurun:

![Uzaktan görüntüleme bileşeni özellikleri](media/remote-rendering-component.png)

Uygulamayı editörden başlatın **(Oynat** veya CTRL+P'ye basın). **Oturum Oluştur** düğmesinin viewport'ta göründüğünü görmelisiniz. İlk ARR oturumunuzu başlatmak için tıklatın:

![İlk oturum oluşturma](media/test-create.png)

Bu başarısız olursa, hesap bilgilerinizi RemoteRendering bileşen özelliklerine doğru girdiğinizden emin olun. Aksi takdirde, konsol penceresinde size atanan oturum kimliğini gösteren ve oturumun şu anda *Başlangıç* durumunda olduğunu belirten bir ileti görüntülenir:

![Oturum başlangıç çıktısı](media/create-session-output.png)

Bu noktada Azure sizin için bir sunucu sağlar ve uzaktan işleme sanal makine başlatıyor. Bu genellikle **3 ila 5 dakika sürer.** VM hazır olduğunda, Unity komut `OnSessionStatusChanged` dosyasının geri araması yürütülür ve yeni oturum durumunu yazdırır:

![Oturumhazır çıktı](media/create-session-output-2.png)

İşte bu! Şimdilik başka bir şey olmayacak. Ücretleri önlemek için, artık gerekmediklerinde oturumları her zaman durdurmalısınız. Bu örnekte, oturumu **durdur** düğmesini tıklatarak veya Unity simülasyonunu durdurarak bunu yapabilirsiniz. Varsayılan olarak geçerli olan *ARRServiceUnity* bileşenindeki **Otomatik Durdurma Oturumu** özelliği nedeniyle, oturum sizin için otomatik olarak durdurulur. Çökmeler veya bağlantı sorunları nedeniyle her şey başarısız olursa, oturumunuz sunucu tarafından kapatılmadan önce *MaxLeaseTime'ınız* kadar uzun süre çalışabilir.

> [!NOTE]
> Oturumu durdurmak hemen etkili olur ve geri alınamaz. Durdurulduktan sonra, aynı başlangıç yükü yle yeni bir oturum oluşturmanız gerekir.

## <a name="reusing-sessions"></a>Oturumları yeniden kullanma

Yeni bir oturum oluşturmak, ne yazık ki, zaman alıcı bir işlemdir. Bu nedenle, nadiren oturumları oluşturmak ve mümkün olduğunca bunları yeniden denemek gerekir.

Aşağıdaki kodu *RemoteRendering* komut dosyasına ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

```csharp
    public string SessionId = null;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
        }
    }

    public async void QueryActiveSessions()
    {
        CreateFrontend();

        var allSessionsProperties = await arrService.Frontend.GetCurrentRenderingSessionsAsync().AsTask();

        Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");

        foreach (var sessionProperties in allSessionsProperties)
        {
            if (string.IsNullOrEmpty(SessionId))
            {
                Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");
                SessionId = sessionProperties.Id;
            }

            LogSessionStatus(sessionProperties);
        }
    }

    public void UseExistingSession()
    {
        CreateFrontend();

        // OpenSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.OpenSession(SessionId);
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
```

> [!CAUTION]
> Bu kodu çalıştırmadan önce, RemoteRendering bileşeninde **Otomatik Durdurma Oturumunu** devre dışı bıraktığından emin olun. Aksi takdirde, simülasyonu durduracağınız her oturum otomatik olarak durdurulur ve yeniden kullanmaya çalışırsanız başarısız olur.

*Play*tuşuna bastığınızda, artık viewport'ta üç düğme elde elabilirsiniz: **Oturum Oluştur**, Etkin **Oturumları Sorgula**ve **Varolan Oturumu Kullanın.** İlk düğme her zaman yeni bir oturum oluşturur. Etkin *oturumları* var olan ikinci düğme sorguları. Kullanmaya çalışmak üzere el ile oturum kimliği belirtmediyseniz, bu eylem ileride kullanmak üzere oturum kimliğini otomatik olarak seçer. Üçüncü düğme varolan bir oturuma bağlanmayı dener. *Oturum Kimliği* bileşeni özelliği nde el ile belirttiğiniz veya Sorgu *Etkin Oturumları*tarafından bulunan bir tane.

**AutoStartSessionAsync** işlevi, düzenleyicinin dışındaki düğmeye basan düğmeyi simüle etmek için kullanılır.

> [!TIP]
> Durdurulan, süresi dolmuş veya hata durumunda olan oturumları açmak mümkündür. Bunlar artık oluşturma için kullanılamasa da, etkin olmayan bir oturum açtıktan sonra ayrıntılarını sorgulayabilirsiniz. Yukarıdaki `ARRService_OnSessionStarted`kod, oturum kullanılamaz hale geldiğinde otomatik olarak durdurmak için bir oturumun durumunu denetler.

Bu işlevsellikle, geliştirme iş akışınızı önemli ölçüde artıracak oturumları oluşturabilir ve yeniden kullanabilirsiniz.

Genellikle, sunucuyu döndürmek için gereken süre nedeniyle istemci uygulamasıdışında bir oturum oluşturma tetiklenir.

## <a name="connect-to-an-active-session"></a>Etkin bir oturuma bağlanma

Şimdiye kadar oturumlar oluşturduk veya açtık. Bir sonraki adım bir oturuma *bağlanmaktır.* Bağlandıktan sonra, render sunucusu görüntü üretecek ve uygulamamıza bir video akışı gönderecektir.

Aşağıdaki kodu *RemoteRendering* komut dosyasına ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

```csharp
    private bool isConnected = false;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;

            // If our session properties are 'Ready' we are ready to start connecting to the runtime of the service.
            if (status == RenderingSessionStatus.Ready)
            {
                session.ConnectionStatusChanged += AzureSession_OnConnectionStatusChanged;
            }
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
            session.ConnectionStatusChanged -= AzureSession_OnConnectionStatusChanged;
        }
    }

    private void AzureSession_OnConnectionStatusChanged(ConnectionStatus status, Result result)
    {
        Debug.Log($"Connection status: '{status}', result: '{result}'");
        isConnected = (status == ConnectionStatus.Connected);
    }

    public void ConnectSession()
    {
        arrService.CurrentActiveSession?.ConnectToRuntime(new ConnectToRuntimeParams());
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }
    }

    private void LateUpdate()
    {
        // The session must have its runtime pump updated.
        // The update will push messages to the server, receive messages, and update the frame-buffer with the remotely rendered content.
        arrService.CurrentActiveSession?.Actions.Update();
    }

    private void OnDisable()
    {
        DisconnectSession();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
    }
#endif
```

Bu işlevselliği sınamak için:

1. Unity **Play** tuşuna basın.
1. Oturumu açın:
    1. Zaten bir oturumunuz varsa, **Etkin Oturumları Sorgula'ya** ve ardından **Varolan oturumu kullanın.**
    1. Aksi takdirde, **Oturum Oluştur'a**basın.
1. **Bağlan'a**basın.
1. Birkaç saniye sonra konsol çıktısı bağlı olduğunuzu yazdırmalıdır.
1. Şimdilik başka bir şey olmamalı.
1. Unity'nin oyun modunu **Durdur'a** basın veya durdurun.

>[!NOTE]
> Birden çok kullanıcı bilgilerini sorgulamak için bir oturum *açabilir,* ancak aynı anda bir oturuma yalnızca bir kullanıcı *bağlanabilir.* Başka bir kullanıcı zaten bağlıysa, bağlantı bir **el sıkışma hatasıyla**başarısız olur.

## <a name="load-a-model"></a>Bir model yükleme

Aşağıdaki kodu *RemoteRendering* komut dosyasına ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

```csharp

    public string ModelName = "builtin://Engine";

    private Entity modelEntity = null;
    private GameObject modelEntityGO = null;

#if UNITY_WSA
    private WorldAnchor modelWorldAnchor = null;
#endif

    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();

        // get the game object representation of this entity
        modelEntityGO = modelEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the entity will sync translations with the server
        var sync = modelEntityGO.GetComponent<RemoteEntitySyncObject>();
        sync.SyncEveryFrame = true;

        // set position to an arbitrary distance from the parent
        modelEntityGO.transform.position = Camera.main.transform.position + Camera.main.transform.forward * 2;
        modelEntityGO.transform.localScale = Vector3.one;

#if UNITY_WSA
        // anchor the model in the world
        modelWorldAnchor = modelEntityGO.AddComponent<WorldAnchor>();
#endif

        // load a model that will be parented to the entity
        // We are using the 'from SAS' flavor because that variant can load the built-in model
        var loadModelParams = new LoadModelFromSASParams(ModelName, modelEntity);
        var async = arrService.CurrentActiveSession.Actions.LoadModelFromSASAsync(loadModelParams);
        async.ProgressUpdated += (float progress) =>
        {
            Debug.Log($"Loading: {progress * 100.0f}%");
        };

        await async.AsTask();
    }

    public void DestroyModel()
    {
        if (modelEntity == null)
        {
            return;
        }

#if UNITY_WSA
        DestroyImmediate(modelWorldAnchor);
#endif

        modelEntity.Destroy();
        modelEntity = null;

        DestroyImmediate(modelEntityGO);
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }

        DestroyModel();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }

                    if (modelEntity == null)
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Load Model"))
                        {
                            LoadModel();
                        }
                    }
                    else
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Destroy Model"))
                        {
                            DestroyModel();
                        }
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
        while (!isConnected)
        {
            yield return null;
        }
        LoadModel();
    }
#endif
```

Şimdi oynat düğmesine bastığında, bir oturumu açtığınızda ve ona **bağlandığınızda, Yükle Modeli** düğmesi görüntülenir. Tıklattıktan sonra konsol çıkışı yükleme ilerlemesini gösterir ve %100'e ulaştığında bir motorun modelinin göründüğünü görmeniz gerekir:

![Editöre yüklenen model](media/model-loaded-replace-me.png)

[WorldAnchor](https://docs.unity3d.com/ScriptReference/XR.WSA.WorldAnchor.html) [hologram stabilitesi](https://docs.microsoft.com/windows/mixed-reality/hologram-stability)için kullanılan önemli bir bileşendir. Ancak, yalnızca Karma Gerçeklik aygıtına dağıtıldığında etkili olacaktır.

> [!TIP]
> [Quickstart'ı izlediyseniz: Oluşturma için bir modeli dönüştürün,](../../quickstarts/convert-model.md)kendi modellerinizi nasıl dönüştürersiniz zaten biliyorsunuz. Bunu işlemek için şimdi yapmanız gereken tek şey, *URI model adı* özelliği ne dönüştürülmüş bir model ekidir.

## <a name="display-frame-statistics"></a>Görüntü çerçevesi istatistikleri

Azure Uzaktan İşleme, bağlantının kalitesi yle ilgili çeşitli bilgileri izler. Bu bilgileri görüntülemek için hızlı bir yol için aşağıdakileri yapın:

Yeni bir [komut dosyası](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve ona **RemoteFrameStats**adını verin. Komut dosyası dosyasını açın ve tüm içeriğini aşağıdaki kodla değiştirin:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class RemoteFrameStats : MonoBehaviour
{
    public Text FrameStats = null;

    ARRServiceStats arrServiceStats = null;

#if UNITY_EDITOR
    private void OnEnable()
    {
        arrServiceStats = new ARRServiceStats();
    }

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            FrameStats.text = string.Empty;
            return;
        }

        arrServiceStats.Update(RemoteManagerUnity.CurrentSession);

        if (FrameStats != null)
        {
            FrameStats.text = arrServiceStats.GetStatsString();
        }
    }
#endif
}
```

Bir GameObject oluşturun ve *framestats*adını. *Ana Kamera* nesnesine alt düğüm olarak takın ve konumunu x **= 0, y = 0, z = 0,325**olarak ayarlayın. **RemoteFrameStats** bileşenini nesneye ekleyin.

*FrameStats* nesnesine bir **Arama > Kanvas** alt nesnesi ekleyin ve özelliklerini şu şekilde ayarlayın:

![tuval özellikleri](media/framestats-canvas.png)

Tuvalin alt bölümü olarak bir **Kullanıcı > Metin nesnesi** ekleyin ve özelliklerini şu şekilde ayarlayın:

![metin özellikleri](media/framestats-text.png)

*FrameStats* nesnesini seçin ve daire simgesine tıklayarak ve **Metin** nesnesini seçerek **FrameStats alanını** doldurun:

![metin özelliğini ayarlama](media/framestats-set-text.png)

Şimdi, uzak oturuma bağlandığında, metin akış istatistiklerini göstermelidir:

![çerçeve istatistikleri çıktı](media/framestats-output.png)

Kod, baş kilitli bir metin kutusu dikkat dağıtıcı olacağından, istatistiklerin düzenleyicinin dışında güncelleştirmeyi devre dışı kılabilir. [Quickstart](../../quickstarts/render-model.md) projesinde daha karmaşık bir uygulama bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, boş bir Unity projesi almak ve Azure Uzaktan İşleme ile çalışmasını sağlamak için gereken tüm adımları öğrendiniz. Bir sonraki öğreticide, uzak varlıklarla nasıl çalışabileceğimize daha yakından bakacağız.

> [!div class="nextstepaction"]
> [Öğretici: Unity içinde uzak varlıklarla çalışma](working-with-remote-entities.md)
