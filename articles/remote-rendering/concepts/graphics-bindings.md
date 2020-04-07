---
title: Grafik bağlama
description: Grafik bağlamalarının kurulumu ve kullanım örnekleri
author: florianborn71
manager: jlyons
services: azure-remote-rendering
titleSuffix: Azure Remote Rendering
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.service: azure-remote-rendering
ms.openlocfilehash: 8b5db0532f3dcc8b6dfb024238d0cacff2e6d2a1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681889"
---
# <a name="graphics-binding"></a>Grafik bağlama

Azure Uzaktan İşleme'yi özel bir uygulamada kullanabilmek için uygulamanın işleme ardışık hattına entegre edilmesi gerekir. Bu tümleştirme grafik bağlama sorumluluğundadır.

Kurulduktan sonra, grafik bağlama işlenen görüntüyü etkileyen çeşitli işlevlere erişim sağlar. Bu işlevler iki kategoriye ayrılabilir: her zaman kullanılabilir olan genel işlevler ve yalnızca seçili `Microsoft.Azure.RemoteRendering.GraphicsApiType`işlevler için geçerli olan belirli işlevler.

## <a name="graphics-binding-in-unity"></a>Unity'de grafik bağlama

Birlik'te, tüm bağlama, `RemoteUnityClientInit` ''ye `RemoteManagerUnity.InitializeManager`geçirilen yapı tarafından işlenir. Grafik modunu ayarlamak `GraphicsApiType` için alanın seçilen bağlamaya ayarlanmalıdır. Alan, XRDevice'ın bulunup bulunmadığına bağlı olarak otomatik olarak doldurulur. Davranış, aşağıdaki davranışlarla el ile geçersiz kılınabilir:

* **HoloLens 2**: [Windows Mixed Reality](#windows-mixed-reality) grafik bağlama her zaman kullanılır.
* **Düz UWP masaüstü uygulaması**: [Simülasyon](#simulation) her zaman kullanılır. Bu modu kullanmak [için, Tutorial: Setting'ten bir Birlik projesi ayarlama'daki](../tutorials/unity/project-setup.md)adımları izlediğinizden emin olun.
* **Unity editor**: Bir WMR VR kulaklık bağlı olmadığı sürece [simülasyon](#simulation) her zaman kullanılır ve bu durumda ARR, uygulamanın ARR ile ilgili olmayan kısımlarını hata ayıklamak için devre dışı bırakılır. Ayrıca bakınız [holografik remoting](../how-tos/unity/holographic-remoting.md).

Unity için diğer ilgili bölüm [temel bağlama](#access)erişim, aşağıdaki tüm diğer bölümleri atlanabilir.

## <a name="graphics-binding-setup-in-custom-applications"></a>Özel uygulamalarda grafik bağlama kurulumu

Grafik bağlama seçmek için aşağıdaki iki adımı izleyin: İlk olarak, program başharfe geçtiğinde grafik bağlamanın statik olarak başlatılması gerekir:

``` cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

Azure Uzaktan İşleme'yi holografik API'lere dönüştürmek için yukarıdaki çağrı gereklidir. Bu işlev, holografik API çağrılmadan ve diğer Uzaktan İşlem API'leri erişilmeden önce çağrılmalıdır. Benzer şekilde, holografik API'ler artık çağrılmadan karşılık gelen de-init işlevi `RemoteManagerStatic.ShutdownRemoteRendering();` çağrılmalıdır.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Grafik bağlamaya erişim

İstemci kurulduktan sonra, temel grafik bağlama `AzureSession.GraphicsBinding` yırtılayıcıyla erişilebilir. Örnek olarak, son kare istatistikleri aşağıdaki gibi alınabilir:

``` cs
AzureSession currentSesson = ...;
if (currentSesson.GraphicsBinding)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        ...
    }
}
```

## <a name="graphic-apis"></a>Grafik API'ler

Şu anda seçilebilen iki grafik `WmrD3D11` `SimD3D11`API'si vardır ve . Üçüncü bir `Headless` tane var, ancak istemci tarafında henüz desteklenmedi.

### <a name="windows-mixed-reality"></a>Windows Mixed Reality

`GraphicsApiType.WmrD3D11`HoloLens 2'de çalışmak için varsayılan bağlayıcıdır. `GraphicsBindingWmrD3d11` Bu bağlama yaratacaktır. Bu modda Azure Uzaktan İşleme doğrudan holografik API'lere bağlanır.

Türetilen grafik bağlamalarına erişmek `GraphicsBinding` için tabanın dökümü gerekir.
WMR bağlamasını kullanmak için yapılması gereken iki şey vardır:

#### <a name="inform-remote-rendering-of-the-used-coordinate-system"></a>Kullanılan koordinat sisteminin Uzaktan Görüntüle'yi Bilgilendirme

``` cs
AzureSession currentSesson = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (binding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

Yukarıdaki, `ptr` API'deki koordinatların `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem` ifade edildiği dünya uzay koordinat sistemini tanımlayan yerel bir nesneye işaretçi olması gerekir.

#### <a name="render-remote-image"></a>Uzak görüntüyü oluşturma

Her çerçevenin başında uzak çerçevenin arka arabelleğe işlenmesi gerekir. Bu, hem `BlitRemoteFrame`renk hem de derinlik bilgilerini şu anda bağlı olan render hedefine dolduracak olan arayarak yapılır. Bu nedenle, arka arabelleği bir render hedefi olarak bağladıktan sonra bunun yapılması önemlidir.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
binding.BlitRemoteFrame();
```

### <a name="simulation"></a>Benzetim

`GraphicsApiType.SimD3D11`simülasyon bağlayıcıve seçilirse `GraphicsBindingSimD3d11` grafik bağlama oluşturur. Bu arabirim, örneğin bir masaüstü uygulamasında kafa hareketini simüle etmek için kullanılır ve monoskobik bir görüntü oluşturur.
Kurulum biraz daha ilgili ve aşağıdaki gibi çalışır:

#### <a name="create-proxy-render-target"></a>Proxy render hedefi oluşturma

Uzaktan ve yerel içerik `GraphicsBindingSimD3d11.Update` işlevi tarafından sağlanan proxy kamera verileri kullanılarak 'proxy' olarak adlandırılan bir ekran dışı renk / derinlik render hedef işlenmesi gerekir. Proxy arka arabellek çözünürlüğü eşleşmelidir. Bir oturum hazır `GraphicsBindingSimD3d11.InitSimulation` olduğunda, oturuma bağlanmadan önce çağrılması gerekir:

``` cs
AzureSession currentSesson = ...;
IntPtr d3dDevice = ...; // native pointer to ID3D11Device
IntPtr color = ...; // native pointer to ID3D11Texture2D
IntPtr depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
simBinding.InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

Init işlevi yerel d3d-aygıt işaretçileri yanı sıra proxy render hedef renk ve derinlik dokusu ile sağlanmalıdır. Bir kez `AzureSession.ConnectToRuntime` başharfve `DisconnectFromRuntime` birden çok kez çağrılabilir, `GraphicsBindingSimD3d11.DeinitSimulation` ancak farklı bir oturuma `GraphicsBindingSimD3d11.InitSimulation` geçerken, başka bir oturumda çağrılabilmesi için önce eski oturumda ilk çağrılması gerekir.

#### <a name="render-loop-update"></a>Render döngü güncelleştirmesi

Render döngüsü güncelleştirmesi birden çok adımdan oluşur:

1. Her kare, herhangi bir işleme `GraphicsBindingSimD3d11.Update` gerçekleşmeden önce, işlenecek sunucuya gönderilen geçerli kamera dönüşümü ile çağrılır. Aynı zamanda döndürülen proxy dönüşümü proxy render hedefi ne işlemek için proxy kamera uygulanmalıdır.
Döndürülen proxy `SimulationUpdate.frameId` güncelleştirmesi null ise, henüz uzak veri yok. Bu durumda, proxy render hedefine işlemek yerine, herhangi bir yerel içerik doğrudan geçerli kamera verileri kullanılarak arka arabelleğe işlenmeli ve sonraki iki adım atlanmalıdır.
1. Uygulama artık proxy render hedef ve `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy`arama bağlamak gerekir. Bu proxy render hedef içine uzak renk ve derinlik bilgileri dolduracaktır. Herhangi bir yerel içerik artık proxy kamera dönüşümü kullanarak proxy üzerine işlenebilir.
1. Daha sonra, arka arabellek bir render hedef `GraphicsBindingSimD3d11.ReprojectProxy` olarak bağlı olması gerekir ve hangi noktada geri arabellek sunulabilir çağrılabilir.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
SimulationUpdate update = new SimulationUpdate();
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate = new SimulationUpdate();
simBinding.Update(update, out proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding.BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding.ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Sıfırdan bir Birlik projesi kurma](../tutorials/unity/project-setup.md)
