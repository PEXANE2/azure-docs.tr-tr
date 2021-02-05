---
title: Uzaktan Işlemeyi C++/DirectX11 holographic uygulamasına tümleştirme
description: Uzaktan Işlemenin Visual Studio proje sihirbazıyla oluşturulmuş bir düz C++/DirectX11 holographic uygulamasına nasıl tümleştirileceğini açıklar.
author: florianborn71
ms.author: flborn
ms.date: 05/04/2020
ms.topic: tutorial
ms.openlocfilehash: a07a8a9c50e0f71daa48f65ebf8c2e7a7f166cc5
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594308"
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

* Hesap bilgileriniz (Hesap KIMLIĞI, hesap anahtarı, hesap etki alanı, abonelik KIMLIĞI). Hesabınız yoksa [hesap oluşturun](../../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk).
* Visual Studio 2019 'nin en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/).
* [Karma gerçeklik Için Visual Studio Araçları](/windows/mixed-reality/install-the-tools). Özellikle, aşağıdaki *Iş yükü* yüklemeleri zorunludur:
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

NuGet paketi, uzaktan Işleme bağımlılıklarını projeye ekler. Özellikle:
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

... ve bu ek `include` yönergeleri HolographicAppMain. cpp dosyasına ekleyin:

```cpp
#include <AzureRemoteRendering.inl>
#include <RemoteRenderingExtensions.h>
#include <windows.perception.spatial.h>
```

Kod basitliği için, yönergelerden sonra HolographicAppMain. h dosyasının en üstünde aşağıdaki ad alanı kısayolunu tanımladık `include` :

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
    RR::ApiHandle<RR::RemoteRenderingClient> m_client;  // the client instance
    RR::ApiHandle<RR::RenderingSession> m_session;    // the current remote rendering session
    RR::ApiHandle<RR::RenderingConnection> m_api;       // the API instance, that is used to perform all the actions. This is just a shortcut to m_session->Connection()
    RR::ApiHandle<RR::GraphicsBindingWmrD3d11> m_graphicsBinding; // the graphics binding instance
}
```

Gerçek uygulamanın, sınıfının Oluşturucusu olması iyi bir yerdir `HolographicAppMain` . Burada üç tür başlatma yapmanız gerekir:
1. Uzaktan Işleme sisteminin bir kerelik başlatması
1. İstemci oluşturma (kimlik doğrulaması)
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
        clientInit.ConnectionType = RR::ConnectionType::General;
        clientInit.GraphicsApi = RR::GraphicsApiType::WmrD3D11;
        clientInit.ToolId = "<sample name goes here>"; // <put your sample name here>
        clientInit.UnitsPerMeter = 1.0f;
        clientInit.Forward = RR::Axis::NegativeZ;
        clientInit.Right = RR::Axis::X;
        clientInit.Up = RR::Axis::Y;
        if (RR::StartupRemoteRendering(clientInit) != RR::Result::Success)
        {
            // something fundamental went wrong with the initialization
            throw std::exception("Failed to start remote rendering. Invalid client init data.");
        }
    }


    // 2. Create Client
    {
        // Users need to fill out the following with their account data and model
        RR::SessionConfiguration init;
        init.AccountId = "00000000-0000-0000-0000-000000000000";
        init.AccountKey = "<account key>";
        init.RemoteRenderingDomain = "westus2.mixedreality.azure.com"; // <change to the region that the rendering session should be created in>
        init.AccountDomain = "westus2.mixedreality.azure.com"; // <change to the region the account was created in>
        m_modelURI = "builtin://Engine";
        m_sessionOverride = ""; // If there is a valid session ID to re-use, put it here. Otherwise a new one is created
        m_client = RR::ApiHandle(RR::RemoteRenderingClient(init));
    }

    // 3. Open/create rendering session
    {
        auto SessionHandler = [&](RR::Status status, RR::ApiHandle<RR::CreateRenderingSessionResult> result)
        {
            if (status == RR::Status::OK)
            {
                auto ctx = result->GetContext();
                if (ctx.Result == RR::Result::Success)
                {
                    SetNewSession(result->GetSession());
                }
                else
                {
                    SetNewState(AppConnectionStatus::ConnectionFailed, ctx.ErrorMessage.c_str());
                }
            }
            else
            {
                SetNewState(AppConnectionStatus::ConnectionFailed, "failed");
            }
        };

        // If we had an old (valid) session that we can recycle, we call async function m_client->OpenRenderingSessionAsync
        if (!m_sessionOverride.empty())
        {
            m_client->OpenRenderingSessionAsync(m_sessionOverride, SessionHandler);
            SetNewState(AppConnectionStatus::CreatingSession, nullptr);
        }
        else
        {
            // create a new session
            RR::RenderingSessionCreationOptions init;
            init.MaxLeaseInMinutes = 10; // session is leased for 10 minutes
            init.Size = RR::RenderingSessionVmSize::Standard;
            m_client->CreateNewRenderingSessionAsync(init, SessionHandler);
            SetNewState(AppConnectionStatus::CreatingSession, nullptr);
        }
    }

    // Rest of constructor code:
    ...
}
```

Kod, üye işlevlerini çağırır `SetNewSession` ve `SetNewState` Bu durum makine kodunun geri kalanı ile birlikte bir sonraki paragrafta uygulayacağız.

Kimlik bilgilerinin örnekte sabit kodlanmış olduğunu ve bu anahtarın yerinde ([hesap kimliği, hesap anahtarı, hesap etki alanı](../../../how-tos/create-an-account.md#retrieve-the-account-information)ve [Uzaktan işleme etki alanı](../../../reference/regions.md)) doldurulması gerektiğini unutmayın.

Başlatma işlemini, yıkıcı gövdesinin sonuna kadar simetrik olarak ve ters sırada yaptık:

```cpp
HolographicAppMain::~HolographicAppMain()
{
    // Existing destructor code:
    ...
    
    // Destroy session:
    if (m_session != nullptr)
    {
        m_session->Disconnect();
        m_session = nullptr;
    }

    // Destroy front end:
    m_client = nullptr;

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
        void SetNewSession(RR::ApiHandle<RR::RenderingSession> newSession);
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
        double m_timeAtLastRESTCall = 0;
        bool m_needsCoordinateSystemUpdate = true;
    }
```

. Cpp dosyasındaki uygulama tarafında şu işlev gövdelerini ekleyin:

```cpp
void HolographicAppMain::StartModelLoading()
{
    m_modelLoadingProgress = 0.f;

    RR::LoadModelFromSasOptions options;
    options.ModelUri = m_modelURI.c_str();
    options.Parent = nullptr;

    // start the async model loading
    m_api->LoadModelFromSasAsync(options,
        // completed callback
        [this](RR::Status status, RR::ApiHandle<RR::LoadModelResult> result)
        {
            m_modelLoadResult = RR::StatusToResult(status);
            m_modelLoadFinished = true; // successful if m_modelLoadResult==RR::Result::Success
            char buffer[1024];
            sprintf_s(buffer, "Remote Rendering: Model loading completed. Result: %s\n", RR::ResultToString(m_modelLoadResult));
            OutputDebugStringA(buffer);
        },
        // progress update callback
            [this](float progress)
        {
            // progress callback
            m_modelLoadingProgress = progress;
            m_needsStatusUpdate = true;
        });
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

void HolographicAppMain::SetNewSession(RR::ApiHandle<RR::RenderingSession> newSession)
{
    SetNewState(AppConnectionStatus::StartingSession, nullptr);

    m_sessionStartingTime = m_timeAtLastRESTCall = m_timer.GetTotalSeconds();
    m_session = newSession;
    m_api = m_session->Connection();
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

Her simülasyon için istemciyi bir kez güncelleştirmemiz ve bazı ek durum güncelleştirmeleri yapmanız gerekir. İşlev `HolographicAppMain::Update` , çerçeve başına güncelleştirmeler için iyi bir kanca sağlar.

#### <a name="state-machine-update"></a>Durum makinesi güncelleştirmesi

Oturumun durumunu yoklamız ve duruma geçiş olup olmadığını görmeniz gerekir `Ready` . Başarılı bir şekilde bağlandık, son olarak, aracılığıyla model yüklemeyi kapattık `StartModelLoading` .

Aşağıdaki kodu işlevinin gövdesine ekleyin `HolographicAppMain::Update` :

```cpp
// Updates the application state once per frame.
HolographicFrame HolographicAppMain::Update()
{
    if (m_session != nullptr)
    {
        // Tick the client to receive messages
        m_api->Update();

        if (!m_sessionStarted)
        {
            // Important: To avoid server-side throttling of the requests, we should call GetPropertiesAsync very infrequently:
            const double delayBetweenRESTCalls = 10.0;

            // query session status periodically until we reach 'session started'
            if (m_sessionPropertiesAsync == nullptr && m_timer.GetTotalSeconds() - m_timeAtLastRESTCall > delayBetweenRESTCalls)
            {
                m_timeAtLastRESTCall = m_timer.GetTotalSeconds();
                m_session->GetPropertiesAsync([this](RR::Status status, RR::ApiHandle<RR::RenderingSessionPropertiesResult> propertiesResult)
                    {
                        if (status == RR::Status::OK)
                        {
                            auto ctx = propertiesResult->GetContext();
                            if (ctx.Result == RR::Result::Success)
                            {
                                auto res = propertiesResult->GetSessionProperties();
                                switch (res.Status)
                                {
                                case RR::RenderingSessionStatus::Ready:
                                {
                                    // The following ConnectAsync is async, but we'll get notifications via OnConnectionStatusChanged
                                    m_sessionStarted = true;
                                    SetNewState(AppConnectionStatus::Connecting, nullptr);
                                    RR::RendererInitOptions init;
                                    init.IgnoreCertificateValidation = false;
                                    init.RenderMode = RR::ServiceRenderMode::Default;
                                    m_session->ConnectAsync(init, [](RR::Status, RR::ConnectionStatus) {});
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
                                SetNewState(AppConnectionStatus::ConnectionFailed, ctx.ErrorMessage.c_str());
                            }
                        }
                        else
                        {
                            SetNewState(AppConnectionStatus::ConnectionFailed, "Failed to retrieve session status");
                        }
                        m_sessionPropertiesQueryInProgress = false; // next try
                    });                }
            }
        }
        if (m_isConnected && !m_modelLoadTriggered)
        {
            m_modelLoadTriggered = true;
            StartModelLoading();
        }
    }

    if (m_needsCoordinateSystemUpdate && m_stationaryReferenceFrame && m_graphicsBinding)
    {
        // Set the coordinate system once. This must be called again whenever the coordinate system changes.
        winrt::com_ptr<ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem> ptr{ m_stationaryReferenceFrame.CoordinateSystem().as<ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem>() };
        m_graphicsBinding->UpdateUserCoordinateSystem(ptr.get());
        m_needsCoordinateSystemUpdate = false;
    }

    // Rest of the body:
    ...
}
```

#### <a name="coordinate-system-update"></a>Koordinat sistemi güncelleştirmesi

Kullanılacak koordinat sisteminde işleme hizmetini kabul etmemiz gerekiyor. Kullanmak istediğimiz koordinat sistemine erişmek için, `m_stationaryReferenceFrame` işlevinin sonunda oluşturulmuş olması gerekir `HolographicAppMain::OnHolographicDisplayIsAvailableChanged` .

Bu koordinat sistemi genellikle değişmez, bu nedenle bir kerelik başlatma. Uygulamanız koordinat sistemini değiştirirse, tekrar çağrılmalıdır.

Yukarıdaki kod, `Update` her ikisi de bir başvuru koordinat sistemi ve bağlı bir oturum olduğunda, işlev içinde koordinat sistemini bir kez ayarlar.

#### <a name="camera-update"></a>Kamera güncelleştirmesi

Kamera klip düzlemleri, sunucu kameranın yerel kamerayla eşitlenmiş şekilde tutulması için güncelleştirmemiz gerekir. Bunu, işlevinin en sonunda yapabiliriz `Update` :

```cpp
    ...
    if (m_isConnected)
    {
        // Any near/far plane values of your choosing.
        constexpr float fNear = 0.1f;
        constexpr float fFar = 10.0f;
        for (HolographicCameraPose const& cameraPose : prediction.CameraPoses())
        {
            // Set near and far to the holographic camera as normal
            cameraPose.HolographicCamera().SetNearPlaneDistance(fNear);
            cameraPose.HolographicCamera().SetFarPlaneDistance(fFar);
        }

        // The API to inform the server always requires near < far. Depth buffer data will be converted locally to match what is set on the HolographicCamera.
        auto settings = m_api->GetCameraSettings();
        settings->SetNearAndFarPlane(std::min(fNear, fFar), std::max(fNear, fFar));
        settings->SetEnableDepth(true);
    }

    // The holographic frame will be used to get up-to-date view and projection matrices and
    // to present the swap chain.
    return holographicFrame;
}

```

### <a name="rendering"></a>İşleme

Yapmanız gereken son şey, uzak içeriğin işlenmesini çağırıyor. Bu çağrıyı, işleme işlem hattının tam doğru konumunda, oluşturma hedefinin işaretini temizledikten sonra ve görünüm penceresinin ayarından sonra gerçekleştirmemiz gerekir. Aşağıdaki kod parçacığını `UseHolographicCameraResources` kilit içinde kilitle işlevine ekleyin `HolographicAppMain::Render` :

```cpp
        ...
        // Existing clear function:
        context->ClearDepthStencilView(depthStencilView, D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
        
        // ...

        // Existing check to test for valid camera:
        bool cameraActive = pCameraResources->AttachViewProjectionBuffer(m_deviceResources);


        // Inject remote rendering: as soon as we are connected, start blitting the remote frame.
        // We do the blitting after the Clear, and before cube rendering.
        if (m_isConnected && cameraActive)
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
> [Hızlı başlangıç: Modeli işlenmek üzere dönüştürme](../../../quickstarts/convert-model.md)