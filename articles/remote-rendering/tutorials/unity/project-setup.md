---
title: Sıfırdan Unity projesi ayarlama
description: Azure uzaktan Işleme ile kullanılmak üzere boş bir Unity projesinin nasıl yapılandırılacağını açıklar.
author: florianborn71
ms.author: flborn
ms.date: 01/30/2020
ms.topic: tutorial
ms.openlocfilehash: c05daa998829c4ac0687f75ae5678695127a50b0
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84659907"
---
# <a name="tutorial-setting-up-a-unity-project-from-scratch"></a>Öğretici: sıfırdan Unity projesi ayarlama

Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * ARR için bir karalama Unity projesi yapılandırma.
> * İşleme oturumları oluşturuluyor ve durduruluyor.
> * Mevcut oturumlar yeniden kullanılıyor.
> * Oturumlarla bağlantı kuruluyor ve bağlantıları kesiliyor.
> * Modeller bir işleme oturumuna yükleniyor.
> * Bağlantı istatistikleri gösteriliyor.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide şunlar gerekir:

* Hesap bilgileriniz (Hesap KIMLIĞI, hesap anahtarı, abonelik KIMLIĞI). Hesabınız yoksa [hesap oluşturun](../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Visual Studio 2019 'nin en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/). 
* [Karma gerçeklik Için Visual Studio Araçları](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). Özellikle, aşağıdaki *Iş yükü* yüklemeleri zorunludur:
  * **C++ ile masaüstü geliştirme**
  * **Evrensel Windows Platformu (UWP) geliştirme**
* GIT [(İndir)](https://git-scm.com/downloads)
* Unity 2019.3.1 [(indirme)](https://unity3d.com/get-unity/download)
  * Bu modülleri Unity 'ye yükler:
    * **UWP** -Evrensel Windows platformu derleme desteği
    * **IL2CPP** -Windows derleme DESTEĞI (IL2CPP)

> [!TIP]
> [ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) tüm öğreticiler Için hazırlanan Unity projelerini içerir. Bu projeleri başvuru olarak kullanabilirsiniz.

## <a name="create-a-new-unity-project"></a>Yeni bir Unity projesi oluştur

Unity hub 'ında yeni bir proje oluşturun.
Bu örnekte, projenin adlı bir klasörde oluşturulduğunu varsayacağız `RemoteRendering` .

![Yeni Proje penceresi](media/new-project.png)

## <a name="configure-the-projects-manifest"></a>Projenin bildirimini yapılandırma

`Packages/manifest.json`Unity proje klasörünüzde bulunan dosyasını değiştirmeniz gerekir. Dosyayı bir metin düzenleyicisinde açın ve aşağıda listelenen satırları ekleyin:

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

Evrensel işleme işlem hattı paketi isteğe bağlıdır, ancak performans nedenleriyle önerilir.
Bildirim değiştirildikten ve kaydedildikten sonra Unity otomatik olarak yenilenir. *Proje* penceresinde paketlerin yüklendiğini onaylayın:

![paket içeri aktarmaları Onayla](media/confirm-packages.png)

## <a name="ensure-you-have-the-latest-version-of-the-package"></a>Paketin en son sürümüne sahip olduğunuzdan emin olun

Aşağıdaki adımlarda, projenizin uzaktan işleme paketinin en son sürümünü kullandığından emin olabilirsiniz.
1. Proje penceresinde paketi seçin ve :::no-loc text="package"::: simgeye tıklayın: ![ paket simgesini seçme](media/package-icons.png)
1. Denetçisinde, "Paket Yöneticisi 'nde görünüm" ' e tıklayın: ![ paket denetçisi](media/package-properties.png)
1. Uzaktan işleme paketine ait Paket Yöneticisi sayfasında, Güncelleştir düğmesinin kullanılabilir olup olmadığını görün. Bu durumda, bunu tıklatmak, paketi paketin en son sürümüne güncelleştirir: ![ paket yöneticisinde ARR paketi](media/package-manager.png)
1. Bazen paketin güncelleştirilmesi konsolunda hatalara neden olabilir. Bu durumda projeyi kapatıp yeniden açmayı deneyin.

## <a name="configure-the-camera"></a>Kamerayı yapılandırma

**Ana kamera** düğümünü seçin.

1. *Dönüşümünü*sıfırlayın:

    ![Kamera dönüşümünü Sıfırla](media/camera-reset-transform.png)

1. Ayarla **:::no-loc text="Clear flags":::***:::no-loc text="Solid Color":::*

1. Ayarla **:::no-loc text="Background":::***:::no-loc text="Black":::*

1. Öğesini **:::no-loc text="Clipping Planes":::** *Near = 0,3* ve *Far = 20*olarak ayarlayın. Bu, işlemenin 30 cm 'den daha yakın veya 20 ölçümden daha büyük olan bir geometriyi kırpmak anlamına gelir.

    ![Unity kamera özellikleri](media/camera-properties.png)

## <a name="adjust-the-project-settings"></a>Proje ayarlarını ayarla

1. *> proje ayarlarını Düzenle 'yi aç...*
1. Sol taraftaki listede kalite ' yi seçin.
1. **Etkin kalite düzeyini** , seçerek *düşük* olarak değiştirin
1. **Varsayılan kalite düzeyini** *düşük* olarak değiştirme

    ![Proje kalitesi ayarlarını değiştir](media/settings-quality.png)

1. Sol taraftaki **grafikler** ' i seçin.
1. **Komut dosyası oluşturma Işlem hattı** ayarını *Hybridrenderingpipeline*olarak değiştirin. Evrensel işleme işlem hattı kullanılmazsa bu adımı atlayın.

    ![Proje grafik ayarlarını değiştirme ](media/settings-graphics-lwrp.png) bazen kullanıcı arabirimi, kullanılabilir işlem hattı türlerinin listesini paketlerden doldurmaz, bu durumda *Hybridrenderingpipeline* varlığı alanın el ile sürüklenmesi gerekir: ![ Proje grafik ayarlarını değiştirme](media/hybrid-rendering-pipeline.png)
1. Sol taraftaki **oynatıcı** ' yı seçin.
1. **Evrensel Windows platformu ayarları** sekmesini seçin
1. **XR ayarlarını** Windows Mixed Reality 'yi destekleyecek şekilde değiştirin: ![ oynatıcı ayarları](media/xr-player-settings.png)
1. Yukarıdaki ekran görüntüsünde olduğu gibi ayarları seçin:
    1. **Sanal Reality** 'Yi etkinleştirme destekleniyor
    1. **Derinlik biçimini** *16 bit derinliğe* ayarla
    1. **Derinlik arabelleği paylaşımını** etkinleştir
    1. **Stereo Işleme modunu** tek bir *geçiş* için ayarla

1. Aynı pencerede, *XR ayarları*' nda, **Yayımlama ayarları** ' nı genişletin.
1. **Özellikleri** aşağı kaydırın ve şunları seçin:
    * **InternetClient**
    * **Internetclientserver**
    * **SpatialPerception**
    * Geliştirme için isteğe bağlı: **PrivateNetworkClientServer**

      Unity uzaktan hata ayıklayıcısını cihazınıza bağlamak istiyorsanız bu seçenek gereklidir.

1. **Desteklenen cihaz ailelerinde**, **holographic** ve **Desktop** 'ı etkinleştirin

1. Karma Gerçeklik araç takımını kullanmak istiyorsanız, önerilen ayarlar ve yetenekler hakkında daha fazla bilgi için [mrtk belgelerine](https://docs.microsoft.com/windows/mixed-reality/unity-development-overview)bakın.

## <a name="validate-project-setup"></a>Proje kurulumunu doğrula

Proje ayarlarının doğru olduğunu doğrulamak için aşağıdaki adımları gerçekleştirin.

1. Unity Düzenleyicisi araç çubuğundaki RemoteRendering menüsünden ValidateProject girişini seçin.
1. Gerektiğinde proje ayarlarını denetlemek ve onarmak için ValidateProject penceresini kullanın.

    ![Unity Düzenleyicisi proje doğrulaması](media/arr-unity-validation.png)

## <a name="create-a-script-to-initialize-azure-remote-rendering"></a>Azure uzaktan Işleme başlatmak için betik oluşturma

Yeni bir [betik](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve **RemoteRendering**adını verin. Komut dosyasını açın ve tüm içeriğini aşağıdaki kodla değiştirin:

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

Bu betik, Azure uzaktan Işlemeyi başlatır, işleme için hangi kamera nesnesinin kullanılacağını, sonra da *oynatma modunun* etkinleştirildiği Görünüm penceresi 'Ne bir **oturum oluştur** düğmesi koyar.

> [!CAUTION]
> Kod değiştirme ve oynatma modu Unity 'de etkin durumdayken kaydetme, Unity 'nin donmasına neden olabilir ve Görev Yöneticisi aracılığıyla kapanmaya zorlanmıştır. Bu nedenle, *RemoteRendering* betiğini düzenlemeden önce her zaman oynat modunu durdurun.

## <a name="test-azure-remote-rendering-session-creation"></a>Azure uzaktan Işleme oturumunun oluşturulmasını test etme

Sahnede yeni bir GameObject oluşturun ve *RemoteRendering* bileşenini buna ekleyin. Uzaktan Işleme hesabınız için uygun *hesap etki alanını*, *hesap kimliğini*ve *hesap anahtarını* girin:

![Uzaktan işleme bileşen özellikleri](media/remote-rendering-component.png)

Uygulamayı düzenleyicide başlatın (**oynat** veya CTRL + P). Görünüm penceresinin içinde **oturum oluştur** düğmesinin göründüğünü görmeniz gerekir. İlk ARR oturumunuzu başlatmak için tıklayın:

![İlk oturum oluşturma](media/test-create.png)

Bu başarısız olursa, RemoteRendering bileşeni özelliklerine hesap ayrıntılarınızı doğru girdiğinizden emin olun. Aksi halde, size atanan oturum KIMLIĞINI gösteren konsol penceresinde bir ileti görünür ve oturumun Şu anda *Başlangıç* durumunda olduğunu ifade eder:

![Oturum başlatma çıkışı](media/create-session-output.png)

Bu noktada Azure, sizin için bir sunucu sağlıyor ve Uzaktan işleme sanal makinesini başlatıyor. Bu genellikle **3 ila 5 dakika sürer**. VM 'ye hazırsanız, Unity betiğinin `OnSessionStatusChanged` geri çağırması yürütülür ve yeni oturum durumunu yazdıracaktır:

![Oturum için hazırlanma çıkışı](media/create-session-output-2.png)

Bu,! Daha fazla şey olmayacak. Ücretleri engellemek için, artık gerek duyulmadığında oturumları her zaman durdurmanız gerekir. Bu örnekte, **Oturumu Durdur** düğmesine tıklayarak ya da Unity simülasyonu durdurarak bunu yapabilirsiniz. Varsayılan olarak açık olan *ARRServiceUnity* bileşenindeki **otomatik durdurma oturumu** özelliği nedeniyle, oturum sizin için otomatik olarak durdurulur. Çökmeler veya bağlantı sorunları nedeniyle her şey başarısız olursa oturumunuz, sunucu tarafından kapatılmadan önce *Maxleasetime* olarak çalışabilir.

> [!NOTE]
> Bir oturumu durdurmak hemen etkili olur ve geri alınamaz. Durdurulduktan sonra, aynı başlangıç yüküyle yeni bir oturum oluşturmanız gerekir.

## <a name="reusing-sessions"></a>Oturumları yeniden kullanma

Yeni bir oturum oluşturma, saat tüketen bir işlemdir. Bu nedenle, oturum açmayı nadiren oluşturmayı denemeli ve mümkün olan her durumda yeniden kullanmalıdır.

Aşağıdaki kodu *RemoteRendering* betiğe ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

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
> Bu kodu çalıştırmadan önce, RemoteRendering bileşenindeki **otomatik durdurma oturumunu** devre dışı bıraktığınızdan emin olun. Aksi takdirde, benzetimi durdurduğunuzda oluşturduğunuz her oturum otomatik olarak durdurulur ve yeniden kullanılmaya çalışıldığında başarısız olur.

*Oynat*'a bastığınızda artık görünüm penceresi: **oturum oluşturma**, **etkin oturumları sorgulama**ve **mevcut oturumu kullanma**gibi üç düğme alırsınız. İlk düğme her zaman yeni bir oturum oluşturur. İkinci düğme, hangi *etkin* oturumların mevcut olduğunu sorgular. Kullanmayı denemek için el ile bir oturum KIMLIĞI belirtmediyseniz, bu eylem ileride kullanılmak üzere bu oturum KIMLIĞINI otomatik olarak seçer. Üçüncü düğme mevcut bir oturuma bağlanmaya çalışır. *Oturum kimliği* bileşen özelliği aracılığıyla el ile belirlediğiniz veya *sorgu etkin oturumlar*tarafından bulunan bir tane.

**Automatic Startsessionasync** işlevi, düzenleyicinin dışında düğme basma işleminin benzetimini yapmak için kullanılır.

> [!TIP]
> Durdurulmuş, zaman aşımına uğramış veya bir hata durumunda olan oturumları açmak mümkündür. Artık işleme için kullanılamazken, etkin olmayan bir oturumu açtıktan sonra ayrıntılarını sorgulayabilirsiniz. Yukarıdaki kod, `ARRService_OnSessionStarted` oturum kullanılamaz duruma geldiğinde otomatik olarak durdurmak için bir oturumun durumunu denetler.

Bu işlevle artık, geliştirme iş akışınızı önemli ölçüde iyileştirebilmeniz için oturumları oluşturabilir ve yeniden kullanabilirsiniz.

Genellikle, bir oturum oluşturma, sunucuyu çalıştırmak için gereken süre nedeniyle istemci uygulaması dışında tetiklenir.

## <a name="connect-to-an-active-session"></a>Etkin bir oturuma bağlanma

Şimdiye kadar oturum oluşturmuş veya açtık. Bir sonraki adım bir oturuma *bağlandır* . Bağlandıktan sonra, işleme sunucusu görüntü oluşturur ve uygulamamıza bir video akışı gönderir.

Aşağıdaki kodu *RemoteRendering* betiğe ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

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

Bu işlevi test etmek için:

1. Unity 'de **oynat** ' a basın.
1. Bir oturum açın:
    1. Zaten bir oturumunuz varsa, **sorgu etkin oturumlar** ' a basın ve ardından **mevcut oturumu kullanın**.
    1. Aksi halde, **oturum oluştur**' a basın.
1. **Bağlan**' a basın.
1. Birkaç saniye sonra, bağlı olduğunuz konsol çıkışının yazdırılması gerekir.
1. Şimdilik başka hiçbir şey gerçekleşmemelidir.
1. **Bağlantıyı kes** veya Unity 'nin yürütme modunu Durdur ' a basın.

>[!NOTE]
> Birden çok Kullanıcı, bilgilerini sorgulamak için bir oturum *açabilir* , ancak aynı anda yalnızca bir Kullanıcı bir oturuma *bağlanabilir* . Başka bir kullanıcı zaten bağlıysa bağlantı, bir **el sıkışma hatasıyla**başarısız olur.

## <a name="load-a-model"></a>Model yükleme

Aşağıdaki kodu *RemoteRendering* betiğe ekleyin ve yinelenen işlevlerin eski sürümlerini kaldırın:

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

Şimdi oynat 'a bastığınızda bir oturum açıp bu sunucuya bağlandığınızda, **Yük modeli** düğmesi görünür. Tıkladıktan sonra, konsol çıktısı yükleme ilerlemesini gösterir ve %100 ' a ulaştığında bir altyapının modelinin görüntülendiğini görmeniz gerekir:

![Düzenleyicide yüklü olan model](media/model-loaded-replace-me.png)

[Worldanchor](https://docs.unity3d.com/ScriptReference/XR.WSA.WorldAnchor.html) , [hologram kararlılığı](https://docs.microsoft.com/windows/mixed-reality/hologram-stability)için kullanılan önemli bir bileşendir. Ancak, yalnızca bir karma gerçeklik cihazına dağıtıldığında bu işlem yürürlüğe girer.

> [!TIP]
> [Hızlı başlangıç: bir modeli işlemek Için dönüştürürseniz](../../quickstarts/convert-model.md), kendi modellerinizi nasıl dönüştürebileceğinizi zaten bilirsiniz. Şimdi bunu işlemek için yapmanız gereken, URI 'yi, dönüştürülen modele *model adı* özelliğine yerleştirmesidir.

## <a name="display-frame-statistics"></a>Çerçeve istatistiklerini görüntüle

Azure uzaktan Işleme bağlantının kalitesi hakkında çeşitli bilgileri izler. Bu bilgileri görüntülemenin hızlı bir yolu için aşağıdakileri yapın:

Yeni bir [komut dosyası](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) oluşturun ve bu dosyayı **Remoteframestats**adına verin. Komut dosyasını açın ve tüm içeriğini aşağıdaki kodla değiştirin:

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

Bir Oyunobject oluşturun ve *Framestats*olarak adlandırın. *Ana kamera* nesnesine bir alt düğüm olarak ekleyin ve konumunu **x = 0, y = 0, z = 0,325**olarak ayarlayın. **Remoteframestats** bileşenini nesnesine ekleyin.

*Framestats* nesnesine bir **UI > tuval** alt nesnesi ekleyin ve özelliklerini şu şekilde ayarlayın:

![tuval özellikleri](media/framestats-canvas.png)

Tuvalin alt öğesi olarak bir **uı > metin** nesnesi ekleyin ve özelliklerini aşağıdaki gibi ayarlayın:

![metin özellikleri](media/framestats-text.png)

*Framestats* nesnesini seçin ve daire simgesine tıklayıp **metin** nesnesini seçerek **framestats alanını** doldurun:

![metin özelliği ayarlanıyor](media/framestats-set-text.png)

Artık, uzak oturuma bağlanıldığında metin akış istatistiklerini göstermelidir:

![çerçeve istatistikleri çıkışı](media/framestats-output.png)

Kod, bir baş kilitli metin kutusu olarak düzenleyici 'nin dışındaki istatistik güncelleştirmesini devre dışı bırakır. [Hızlı başlangıç](../../quickstarts/render-model.md) projesinde daha karmaşık bir uygulama bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, boş bir Unity projesi almak için gereken tüm adımları öğrendiniz ve Azure uzaktan Işleme ile çalışmaya başlayın. Sonraki öğreticide, uzak varlıklarla nasıl çalıştığına daha yakından bakacağız.

> [!div class="nextstepaction"]
> [Öğretici: Unity 'de uzak varlıklarla çalışma](working-with-remote-entities.md)
