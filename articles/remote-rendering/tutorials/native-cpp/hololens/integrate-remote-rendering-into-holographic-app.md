---
title: Uzaktan Işlemeyi C++/DirectX11 holographic uygulamasına tümleştirme
description: Uzaktan Işlemenin Visual Studio proje sihirbazıyla oluşturulmuş bir düz C++/DirectX11 holographic uygulamasına nasıl tümleştirileceğini açıklar.
author: florianborn71
ms.author: flborn
ms.date: 05/04/2020
ms.topic: tutorial
ms.openlocfilehash: e68cbe664facbd63153171b7d9be2cbf4850b56f
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84434402"
---
# <a name="tutorial-integrate-remote-rendering-into-a-hololens-holographic-app"></a>Öğretici: uzaktan Işlemeyi bir HoloLens holographic uygulamasına tümleştirme

Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * HoloLens 'e dağıtılabilecek bir holographic uygulaması oluşturmak için Visual Studio 'Yu kullanma
> * Yerel işlemeyi uzaktan işlenmiş içerikle birleştirmek için gerekli kod parçacıklarını ve proje ayarlarını ekleyin

Bu öğretici, `Holographic App` Yerel Işlemeyi Azure uzaktan işlemeyle birleştirmek için gerekli bitleri yerel bir örneğe ekleme konusunda odaklanır. Bu uygulamadaki durum geri bildirimin tek türü, Visual Studio 'nun içindeki hata ayıklama çıkışı panelinden yapılır, bu nedenle örneği Visual Studio 'Nun içinden başlatmanız önerilir. Doğru uygulama içi geri bildirimin eklenmesi bu örneğin kapsamının ötesinde, sıfırdan dinamik metin paneli oluşturmak çok sayıda kodlama içerir. İyi bir başlangıç noktası `StatusDisplay` , [GitHub 'Da uzaktan iletişim oynatıcı örnek projesinin](https://github.com/microsoft/MixedReality-HolographicRemoting-Samples/tree/master/player/common/Content)bir parçası olan sınıftır. Aslında, Bu öğreticinin önceden tamamlanmış sürümü bu sınıfın yerel bir kopyasını kullanır.

> [!TIP]
> [ARR örnekleri deposu](https://github.com/Azure/azure-remote-rendering) , Bu öğreticinin, kullanıma yönelik bir Visual Studio projesi olarak sonucunu içerir. Ayrıca, Kullanıcı arabirimi sınıfı aracılığıyla doğru hata ve durum raporlama ile zenginleştirilmiş olur `StatusDisplay` . Öğreticide, tüm ARR eklemeleri tarafından kapsamlandırılır `#ifdef USE_REMOTE_RENDERING`  /  `#endif` , bu nedenle uzaktan işleme eklemelerinin belirlenmesi kolaydır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide şunlar gerekir:

* Hesap bilgileriniz (Hesap KIMLIĞI, hesap anahtarı, abonelik KIMLIĞI). Hesabınız yoksa [hesap oluşturun](../../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk).
* Visual Studio 2019 'nin en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/).
* [Karma gerçeklik Için Visual Studio Araçları](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). Özellikle, aşağıdaki *Iş yükü* yüklemeleri zorunludur:
  * **C++ ile masaüstü geliştirme**
  * **Evrensel Windows Platformu (UWP) geliştirme**
* Visual Studio için Windows Mixed Reality uygulama şablonları [(indirme)](https://marketplace.visualstudio.com/items?itemName=WindowsMixedRealityteam.WindowsMixedRealityAppTemplatesVSIX).

## <a name="create-a-new-holographic-app-sample"></a>Yeni bir holographic App örneği oluşturma

İlk adım olarak, uzaktan Işleme tümleştirmesinin temelini oluşturan bir hisse senedi örneği oluşturacağız. Visual Studio 'Yu açın ve "yeni proje oluştur" öğesini seçin ve "holographic DirectX 11 uygulaması (Evrensel Windows) (C++/Wınrt)" araması yapın

![Yeni proje oluşturma](media/new-project-wizard.png)

Seçtiğiniz bir proje adını yazın, bir yol seçin ve "Oluştur" düğmesini seçin.
Yeni projede, yapılandırmayı **"Debug/ARM64"** olarak değiştirin. Artık bunu, bağlı bir HoloLens 2 cihazına derleyip dağıtabilmelisiniz. HoloLens üzerinde çalıştırırsanız, sizin için bir döndürme küpü görmeniz gerekir.

## <a name="add-remote-rendering-dependencies-through-nuget"></a>NuGet aracılığıyla uzaktan Işleme bağımlılıklarını ekleme

Uzaktan Işleme özellikleri eklemenin ilk adımı, istemci tarafı bağımlılıklarını eklemektir. İlgili bağımlılıklar, bir NuGet paketi olarak kullanılabilir.
Çözüm Gezgini, projeye sağ tıklayın ve bağlam menüsünden **"NuGet Paketlerini Yönet..."** öğesini seçin.

İstendiğinde iletişim kutusunda **"Microsoft. Azure. RemoteRendering. cpp"** adlı NuGet paketine gözatamazsınız:

![NuGet paketine gözatamıyorum](media/add-nuget.png)

ve paketi seçip "Install" düğmesine basarak projeye ekleyin.

NuGet paketi, uzaktan Işleme bağımlılıklarını projeye ekler. Daha ayrıntılı şekilde belirtmek gerekirse:
* İstemci kitaplığına yönelik bağlantı (RemoteRenderingClient. lib).
* . Dll bağımlılıklarını ayarlayın.
* İçerme dizinine doğru yolu ayarlayın.

## <a name="project-preparation"></a>Proje hazırlığı

Var olan projede küçük değişiklikler yapmanız gerekiyor. Bu değişiklikler hafif, ancak bunlar uzaktan oluşturma çalışmayacaktır.

### <a name="enable-multithread-protection-on-directx-device"></a>DirectX cihazında çoklu iş parçacıklı korumayı etkinleştir
`DirectX11`Cihazda çoklu iş parçacığı koruması etkinleştirilmiş olmalıdır. Bunu değiştirmek için, "Common" klasöründeki DeviceResources. cpp dosyasını açın ve işlevin sonuna aşağıdaki kodu ekleyin `DeviceResources::CreateDeviceResources()` :

```cpp
// Enable multi thread protection as now multiple threads use the immediate context.
Microsoft::WRL::ComPtr<ID3D11Multithread> contextMultithread;
if (context.As(&contextMultithread) == S_OK)
{
    contextMultithread->SetMultithreadProtected(true);
}
```

### <a name="enable-network-capabilities-in-the-app-manifest"></a>Uygulama bildiriminde ağ özelliklerini etkinleştirme
Dağıtılan uygulama için ağ özellikleri açıkça etkinleştirilmelidir. Bu yapılandırılmadan bağlantı sorguları, zaman aşımına neden olur. Etkinleştirmek için `package.appxmanifest` Çözüm Gezgini 'nde öğeye çift tıklayın. Sonraki Kullanıcı arabiriminde, **yetenekler** sekmesine gidin ve şunları seçin:
* Internet (Istemci & sunucusu)
* İnternet (İstemci)

![Ağ özellikleri](media/appx-manifest-caps.png)


## <a name="integrate-remote-rendering"></a>Uzaktan Işlemeyi tümleştirme

Artık proje hazırlandığına göre, kodla başlayabiliriz. `HolographicAppMain`Başlatma, serbest bırakma ve işleme için gerekli tüm kancalar içerdiğinden, uygulama için iyi bir giriş noktası (dosya HolographicAppMain. h/cpp) sınıfıdır.

### <a name="includes"></a>Dahil Eder

Gerekli eklemeleri ekleyerek başladık. HolographicAppMain. h dosyasına aşağıdakileri ekleyin:

```cpp
#include <AzureRemoteRendering.h>
```

... ve bu ek `include` yönergeyi HolographicAppMain. cpp dosyasına ekleyin:

```cpp
#include <AzureRemoteRendering.inl>
#include <RemoteRenderingExtensions.h>
```

Kod basitliği için, yönergesinden sonra HolographicAppMain. h dosyasının en üstünde aşağıdaki ad alanı kısayolunu tanımladık `include` :

```cpp
namespace RR = Microsoft::Azure::RemoteRendering;
```

Bu kısayol yararlı olduğundan, tam ad alanını her yerde yazmak zorunda kalmaz, ancak yine de ARR 'e özgü veri yapılarını tanıyabiliriz. Kuşkusuz, yönergesini de kullanabiliriz `using namespace...` .

### <a name="remote-rendering-initialization"></a>Uzaktan Işleme başlatma
 
Uygulamanın kullanım ömrü boyunca, oturum için birkaç nesne tutmanız gerekir. Yaşam süresi saatle çakışan, uygulama nesnesinin kullanım ömrü boyunca `HolographicAppMain` nesnelerimizi sınıfa üye olarak ekliyoruz `HolographicAppMain` . Sonraki adım, HolographicAppMain. h dosyasına şu sınıf üyelerini ekliyor:

```cpp
class HolographicAppMain
{
    ...
    // members:
    std::string m_sessionOverride;                // if we have a valid session ID, we specify it here. Otherwise a new one is created
    RR::ApiHandle<RR::AzureFrontend> m_frontEnd;  // the front end instance
    RR::ApiHandle<RR::AzureSession> m_session;    // the current remote rendering session
    RR::ApiHandle<RR::RemoteManager> m_api;       // the API instance, that is used to perform all the actions. This is just a shortcut to m_session->Actions()
    RR::ApiHandle<RR::GraphicsBindingWmrD3d11> m_graphicsBinding; // the graphics binding instance
}
```

Gerçek uygulamanın, sınıfının Oluşturucusu olması iyi bir yerdir `HolographicAppMain` . Burada üç tür başlatma yapmanız gerekir:
1. Uzaktan Işleme sisteminin bir kerelik başlatması
1. Ön uç oluşturma
1. Oturum oluşturma

Tüm bunları oluşturucuda sırayla yaptık. Ancak gerçek kullanım durumlarında, bu adımları ayrı olarak yapmak uygun olabilir.

Aşağıdaki kodu, HolographicAppMain. cpp dosyasındaki Oluşturucu gövdesinin başlangıcına ekleyin:

```cpp
HolographicAppMain::HolographicAppMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources)
{
    // 1. One time initialization
    {
        RR::RemoteRenderingInitialization clientInit;
        memset(&clientInit, 0, sizeof(RR::RemoteRenderingInitialization));
        clientInit.connectionType = RR::ConnectionType::General;
        clientInit.graphicsApi = RR::GraphicsApiType::WmrD3D11;
        clientInit.toolId = "<sample name goes here>"; // <put your sample name here>
        clientInit.unitsPerMeter = 1.0f;
        clientInit.forward = RR::Axis::Z_Neg;
        clientInit.right = RR::Axis::X;
        clientInit.up = RR::Axis::Y;
        RR::StartupRemoteRendering(clientInit);
    }


    // 2. Create front end
    {
        // Users need to fill out the following with their account data and model
        RR::AzureFrontendAccountInfo init;
        init.AccountId = "00000000-0000-0000-0000-000000000000";
        init.AccountKey = "<account key>";
        init.AccountDomain = "westus2.mixedreality.azure.com"; // <change to your region>
        m_modelURI = "builtin://Engine";
        m_sessionOverride = ""; // If there is a valid session ID to re-use, put it here. Otherwise a new one is created

        m_frontEnd = RR::ApiHandle(RR::AzureFrontend(init));
    }

    // 3. Open/create rendering session
    {
        bool createNewSession = true;

        // If we had an old (valid) session that we can recycle, we call synchronous function m_frontEnd->OpenRenderingSession
        if (!m_sessionOverride.empty())
        {
            auto openSessionRes = m_frontEnd->OpenRenderingSession(m_sessionOverride);
            if (openSessionRes->valid())
            {
                SetNewSession(*openSessionRes);
                createNewSession = false;
            }
        }

        if (createNewSession)
        {
            // create a new session
            RR::RenderingSessionCreationParams init;
            memset(&init, 0, sizeof(RR::RenderingSessionCreationParams));
            init.MaxLease.minute = 10; // session is leased for 10 minutes
            init.Size = RR::RenderingSessionVmSize::Standard;
            auto createSessionAsync = *m_frontEnd->CreateNewRenderingSessionAsync(init);
            createSessionAsync->Completed([&](auto handler)
                {
                    if (handler->Result())
                    {
                        SetNewSession(*handler->Result());
                    }
                    else
                    {
                        SetNewState(AppConnectionStatus::ConnectionFailed, "failed");
                    }
                });
            SetNewState(AppConnectionStatus::CreatingSession, nullptr);
        }
    }

    // Rest of constructor code:
    ...
}
```

Kod, üye işlevlerini çağırır `SetNewSession` ve `SetNewState` Bu durum makine kodunun geri kalanı ile birlikte bir sonraki paragrafta uygulayacağız.

Kimlik bilgilerinin örnekte sabit kodlanmış olduğunu ve bu anahtarın yerinde ([hesap kimliği, hesap anahtarı](../../../how-tos/create-an-account.md#retrieve-the-account-information)ve [etki alanı](../../../reference/regions.md)) doldurulması gerektiğini unutmayın.

Başlatma işlemini, yıkıcı gövdesinin sonuna kadar simetrik olarak ve ters sırada yaptık:

```cpp
HolographicAppMain::~HolographicAppMain()
{
    // Existing destructor code:
    ...
    
    // Destroy session:
    if (m_session != nullptr)
    {
        m_session->DisconnectFromRuntime();
        m_session = nullptr;
    }

    // Destroy front end:
    m_frontEnd = nullptr;

    // One-time de-initialization:
    RR::ShutdownRemoteRendering();
}
```

## <a name="state-machine"></a>Durum makinesi

Uzaktan Işlemede, bir oturum oluşturmak ve bir modeli yüklemek için anahtar işlevler zaman uyumsuz işlevlerdir. Bunu hesaba eklemek için, aslında aşağıdaki durumlardan otomatik olarak geçiş yapan basit bir durum makinesine ihtiyacımız vardır:

*Başlatma-> oturum oluşturma-> oturum başlatma > model yükleme (devam ediyor)*

Buna uygun olarak, bir sonraki adım olarak, sınıfına bir durum makine işleme biti ekledik. `AppConnectionStatus`Uygulamamız içinde olabilecek çeşitli durumlar için kendi numaralandırmamızı bildiririz. Buna benzerdir `RR::ConnectionStatus` , ancak başarısız bağlantı için ek bir durum içerir.

Sınıf bildirimine aşağıdaki üyeleri ve işlevleri ekleyin:

```cpp
namespace HolographicApp
{
    // Our application's possible states:
    enum class AppConnectionStatus
    {
        Disconnected,

        CreatingSession,
        StartingSession,
        Connecting,
        Connected,

        // error state:
        ConnectionFailed,
    };

    class HolographicAppMain
    {
        ...
        // Member functions for state transition handling
        void OnConnectionStatusChanged(RR::ConnectionStatus status, RR::Result error);
        void SetNewState(AppConnectionStatus state, const char* statusMsg);
        void SetNewSession(RR::ApiHandle<RR::AzureSession> newSession);
        void StartModelLoading();

        // Members for state handling:

        // Model loading:
        std::string m_modelURI;
        RR::ApiHandle<RR::LoadModelAsync> m_loadModelAsync;

        // Connection state machine:
        AppConnectionStatus m_currentStatus = AppConnectionStatus::Disconnected;
        std::string m_statusMsg;
        RR::Result m_connectionResult = RR::Result::Success;
        RR::Result m_modelLoadResult = RR::Result::Success;
        bool m_isConnected = false;
        bool m_sessionStarted = false;
        RR::ApiHandle<RR::SessionPropertiesAsync> m_sessionPropertiesAsync;
        bool m_modelLoadTriggered = false;
        float m_modelLoadingProgress = 0.f;
        bool m_modelLoadFinished = false;

    }
```

. Cpp dosyasındaki uygulama tarafında şu işlev gövdelerini ekleyin:

```cpp
void HolographicAppMain::StartModelLoading()
{
    m_modelLoadingProgress = 0.f;

    RR::LoadModelFromSASParams params;
    params.ModelUrl = m_modelURI.c_str();
    params.Parent = nullptr;

    // Start the async model loading
    if (auto loadModel = m_api->LoadModelFromSASAsync(params))
    {
        m_loadModelAsync = *loadModel;
        m_loadModelAsync->Completed([this](const RR::ApiHandle<RR::LoadModelAsync>& async)
        {
            m_modelLoadResult = *async->Status();
            m_modelLoadFinished = true; // successful if m_modelLoadResult==RR::Result::Success
            m_loadModelAsync = nullptr;
            char buffer[1024];
            sprintf_s(buffer, "Remote Rendering: Model loading completed. Result: %s\n", RR::ResultToString(m_modelLoadResult));
            OutputDebugStringA(buffer);
            });
        m_loadModelAsync->ProgressUpdated([this](float progress)
        {
            // Progress callback
            m_modelLoadingProgress = progress;

            // Output progress percentage to VS output
            char buffer[1024];
            sprintf_s(buffer, "Remote Rendering: Model loading progress: %.1f\n", m_modelLoadingProgress * 100.f);
            OutputDebugStringA(buffer);
        });
    }
}



void HolographicAppMain::SetNewState(AppConnectionStatus state, const char* statusMsg)
{
    m_currentStatus = state;
    m_statusMsg = statusMsg ? statusMsg : "";

    // Some log for the VS output panel:
    const char* appStatus = nullptr;

    switch (state)
    {
        case AppConnectionStatus::Disconnected: appStatus = "Disconnected"; break;
        case AppConnectionStatus::CreatingSession: appStatus = "CreatingSession"; break;
        case AppConnectionStatus::StartingSession: appStatus = "StartingSession"; break;
        case AppConnectionStatus::Connecting: appStatus = "Connecting"; break;
        case AppConnectionStatus::Connected: appStatus = "Connected"; break;
        case AppConnectionStatus::ConnectionFailed: appStatus = "ConnectionFailed"; break;
    }

    char buffer[1024];
    sprintf_s(buffer, "Remote Rendering: New status: %s, result: %s\n", appStatus, m_statusMsg.c_str());
    OutputDebugStringA(buffer);
}

void HolographicAppMain::SetNewSession(RR::ApiHandle<RR::AzureSession> newSession)
{
    SetNewState(AppConnectionStatus::StartingSession, nullptr);

    m_session = newSession;
    m_api = m_session->Actions();
    m_graphicsBinding = m_session->GetGraphicsBinding().as<RR::GraphicsBindingWmrD3d11>();
    m_session->ConnectionStatusChanged([this](auto status, auto error)
        {
            OnConnectionStatusChanged(status, error);
        });

};

void HolographicAppMain::OnConnectionStatusChanged(RR::ConnectionStatus status, RR::Result error)
{
    const char* asString = RR::ResultToString(error);
    m_connectionResult = error;

    switch (status)
    {
    case RR::ConnectionStatus::Connecting:
        SetNewState(AppConnectionStatus::Connecting, asString);
        break;
    case RR::ConnectionStatus::Connected:
        if (error == RR::Result::Success)
        {
            SetNewState(AppConnectionStatus::Connected, asString);
        }
        else
        {
            SetNewState(AppConnectionStatus::ConnectionFailed, asString);
        }
        m_modelLoadTriggered = m_modelLoadFinished = false;
        m_isConnected = error == RR::Result::Success;
        break;
    case RR::ConnectionStatus::Disconnected:
        if (error == RR::Result::Success)
        {
            SetNewState(AppConnectionStatus::Disconnected, asString);
        }
        else
        {
            SetNewState(AppConnectionStatus::ConnectionFailed, asString);
        }
        m_modelLoadTriggered = m_modelLoadFinished = false;
        m_isConnected = false;
        break;
    default:
        break;
    }
    
}
```

### <a name="per-frame-update"></a>Çerçeve başına güncelleştirme

Benzetimi her simülasyon için bir kez daha fazla çentik olmalıdır. Sınıfı `HolographicApp1Main` , çerçeve başına güncelleştirmeler için iyi bir kanca sağlar. Ayrıca, oturumun durumunu yoklamız ve duruma geçiş olup olmadığını görmeniz gerekir `Ready` . Başarılı bir şekilde bağlandık, son olarak, aracılığıyla model yüklemeyi kapattık `StartModelLoading` .

Aşağıdaki kodu işlevinin gövdesine ekleyin `HolographicApp1Main::Update` :

```cpp
// Updates the application state once per frame.
HolographicFrame HolographicAppMain::Update()
{
    if (m_session != nullptr)
    {
        // Tick the client to receive messages
        m_api->Update();

        // query session status periodically until we reach 'session started'
        if (!m_sessionStarted && m_sessionPropertiesAsync == nullptr)
        {
            if (auto propAsync = m_session->GetPropertiesAsync())
            {
                m_sessionPropertiesAsync = *propAsync;
                m_sessionPropertiesAsync->Completed([this](const RR::ApiHandle<RR::SessionPropertiesAsync>& async)
                    {
                        if (auto res = async->Result())
                        {
                            switch (res->Status)
                            {
                            case RR::RenderingSessionStatus::Ready:
                            {
                                // The following is async, but we'll get notifications via OnConnectionStatusChanged
                                m_sessionStarted = true;
                                SetNewState(AppConnectionStatus::Connecting, nullptr);
                                RR::ConnectToRuntimeParams init;
                                init.ignoreCertificateValidation = false;
                                init.mode = RR::ServiceRenderMode::Default;
                                m_session->ConnectToRuntime(init);
                            }
                            break;
                            case RR::RenderingSessionStatus::Error:
                                SetNewState(AppConnectionStatus::ConnectionFailed, "Session error");
                                break;
                            case RR::RenderingSessionStatus::Stopped:
                                SetNewState(AppConnectionStatus::ConnectionFailed, "Session stopped");
                                break;
                            case RR::RenderingSessionStatus::Expired:
                                SetNewState(AppConnectionStatus::ConnectionFailed, "Session expired");
                                break;
                            }

                        }
                        else
                        {
                            SetNewState(AppConnectionStatus::ConnectionFailed, "Failed to retrieve session status");
                        }
                        m_sessionPropertiesAsync = nullptr; // next try
                        m_needsStatusUpdate = true;
                    });
            }
        }

        if (m_isConnected && !m_modelLoadTriggered)
        {
            m_modelLoadTriggered = true;
            StartModelLoading();
        }
    }

    // Rest of the body:
    ...
}
```

### <a name="rendering"></a>İşleme

Yapmanız gereken son şey, uzak içeriğin işlenmesini çağırıyor. Bu çağrıyı, işleme işlem hattının içindeki doğru konumda, işleme hedefi temizledikten sonra gerçekleştirmemiz gerekir. Aşağıdaki kod parçacığını `UseHolographicCameraResources` kilit içinde kilitle işlevine ekleyin `HolographicAppMain::Render` :

```cpp
        ...
        // Existing clear function:
        context->ClearDepthStencilView(depthStencilView, D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);

        // Inject remote rendering: as soon as we are connected, start blitting the remote frame.
        // We do the blitting after the Clear, and before cube rendering.
        if (m_isConnected)
        {
            m_graphicsBinding->BlitRemoteFrame();
        }

        ...
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnek, şimdi derleme ve çalıştırma durumunda olmalıdır.

Örnek düzgün şekilde çalıştığında, bu, en önde gelen döndürme küpünü gösterir ve bazı oturum oluşturma ve model yükleme işleminden sonra geçerli kafa konumunda bulunan motor modelini işler. Oturum oluşturma ve model yükleme birkaç dakika sürebilir. Geçerli durum yalnızca Visual Studio 'nun çıkış paneline yazılır. Bu nedenle, örneği Visual Studio 'Nun içinden başlatmanız önerilir.

> [!CAUTION]
> Tick işlevi birkaç saniye çağırçağrılmayan istemci, sunucu bağlantısını keser. Bu nedenle, kesme noktaları tetiklemenin uygulamanın bağlantısının kesilmesine çok daha kolay yol açabilir.

Metin paneliyle doğru durum görüntüsü için, GitHub 'da Bu öğreticinin önceden uyumlu sürümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Stock **holographic App** C++/DirectX11 örneğine uzaktan işleme eklemek için gereken tüm adımları öğrendiniz.
Kendi modelinizi dönüştürmek için aşağıdaki hızlı başlangıca başvurun:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir modeli işleme için dönüştürme](../../../quickstarts/convert-model.md)
