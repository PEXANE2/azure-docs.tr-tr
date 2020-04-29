---
title: Grafik bağlama
description: Grafik bağlamaları ve kullanım durumlarının kurulumu
author: florianborn71
manager: jlyons
services: azure-remote-rendering
titleSuffix: Azure Remote Rendering
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.service: azure-remote-rendering
ms.openlocfilehash: 8b5db0532f3dcc8b6dfb024238d0cacff2e6d2a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681889"
---
# <a name="graphics-binding"></a>Grafik bağlama

Azure uzaktan oluşturma 'yı özel bir uygulamada kullanabilmeniz için uygulamanın işleme ardışık düzenine entegre olması gerekir. Bu tümleştirme, grafik bağlamanın sorumluluğundadır.

Ayarladıktan sonra grafik bağlama, işlenen görüntüyü etkileyen çeşitli işlevlere erişim sağlar. Bu işlevler iki kategoriye ayrılabilir: her zaman kullanılabilir olan genel işlevler ve yalnızca seçili `Microsoft.Azure.RemoteRendering.GraphicsApiType`olan belirli işlevler.

## <a name="graphics-binding-in-unity"></a>Unity 'de grafik bağlama

Unity 'de, tüm bağlama geçirilen `RemoteUnityClientInit` yapı tarafından işlenir. `RemoteManagerUnity.InitializeManager` Grafik modunu ayarlamak için `GraphicsApiType` alanın seçilen bağlamaya ayarlanması gerekir. Alan, bir XRDevice var olup olmadığına bağlı olarak otomatik olarak doldurulur. Davranışın aşağıdaki davranışlar ile el ile geçersiz kılınabilmesi:

* **HoloLens 2**: [Windows Mixed Reality](#windows-mixed-reality) grafik bağlaması her zaman kullanılır.
* **Düz UWP masaüstü uygulaması**: [Benzetim](#simulation) her zaman kullanılır. Bu modu kullanmak için öğreticideki adımları izlediğinizden emin olun [: bir Unity projesini sıfırdan ayarlama](../tutorials/unity/project-setup.md).
* **Unity Düzenleyicisi**: BIR WMR VR kulaklık bağlantısı yoksa [simülasyon](#simulation) her zaman kullanılır ve bu durumda, uygulamanın ARR ile ilgili olmayan bölümlerinin hata ayıklamasına izin vermek için ARR 'nin devre dışı bırakılması gerekir. Ayrıca bkz. [holographic Remoting](../how-tos/unity/holographic-remoting.md).

Unity 'nin yalnızca diğer ilgili bölümü [temel bağlamaya](#access)erişiyor, aşağıdaki diğer tüm bölümler atlanabilir.

## <a name="graphics-binding-setup-in-custom-applications"></a>Özel uygulamalarda grafik bağlama kurulumu

Grafik bağlamayı seçmek için aşağıdaki iki adımı uygulayın: Ilk olarak, program başlatıldığında grafik bağlamasının statik olarak başlatılması gerekir:

``` cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

Yukarıdaki çağrı, Holographic API 'Lerinde Azure uzaktan Işlemeyi başlatmak için gereklidir. Bu işlev, herhangi bir holographic API çağrılmadan önce ve diğer uzaktan Işleme API 'Lerine erişilmadan önce çağrılmalıdır. Benzer şekilde, artık holographic API 'Lerini çağırıldıktan sonra karşılık gelen de init işlevi `RemoteManagerStatic.ShutdownRemoteRendering();` çağrılmalıdır.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Grafik bağlamaya erişme

İstemci kurulduktan sonra, `AzureSession.GraphicsBinding` alıcı ile temel grafik bağlamaya erişilebilir. Örnek olarak, son çerçeve istatistikleri şu şekilde alınabilir:

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

## <a name="graphic-apis"></a>Grafik API 'Leri

Şu anda seçilebilir `WmrD3D11` Iki grafik API 'si vardır `SimD3D11`. Üçüncü bir tane `Headless` var ancak istemci tarafında henüz desteklenmiyor.

### <a name="windows-mixed-reality"></a>Windows Mixed Reality

`GraphicsApiType.WmrD3D11`, HoloLens 2 üzerinde çalıştırılacak varsayılan bağlamadır. `GraphicsBindingWmrD3d11` Bağlama oluşturacaktır. Bu modda Azure uzaktan Işleme kancaları doğrudan holographic API 'Lerine takılır.

Türetilmiş grafik bağlamalarına erişmek için, taban `GraphicsBinding` , atama olmalıdır.
WMR bağlamasını kullanmak için yapılması gereken iki şey vardır:

#### <a name="inform-remote-rendering-of-the-used-coordinate-system"></a>Kullanılan koordinat sisteminin uzaktan Işlenmesini bilgilendirme

``` cs
AzureSession currentSesson = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (binding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

Yukarıdaki `ptr` , API 'deki koordinatların ifade edildiği dünya alanı `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem` koordinat sistemini tanımlayan yerel bir nesne işaretçisi olmalıdır.

#### <a name="render-remote-image"></a>Uzak görüntüyü işle

Her bir karenin başlangıcında, uzak çerçevenin arka arabellekte oluşturulması gerekir. Bu, hem renk hem `BlitRemoteFrame`de derinlik bilgilerini o anda bağlı işleme hedefine dolduracak şekilde çağırarak yapılır. Bu nedenle, geri arabelleği bir işleme hedefi olarak bağladıktan sonra bunun yapılması önemlidir.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
binding.BlitRemoteFrame();
```

### <a name="simulation"></a>Benzetim

`GraphicsApiType.SimD3D11`simülasyon bağlamadır ve seçili ise `GraphicsBindingSimD3d11` grafik bağlamayı oluşturur. Bu arabirim, örneğin bir masaüstü uygulamasındaki baş taşımanın benzetimini yapmak için kullanılır ve tek bir scopc görüntüsü oluşturur.
Kurulum biraz daha karmaşıktır ve aşağıdaki gibi çalışmaktadır:

#### <a name="create-proxy-render-target"></a>Proxy oluşturma hedefi oluştur

Uzak ve yerel içeriğin, `GraphicsBindingSimD3d11.Update` işlev tarafından verilen proxy kamera verilerini kullanarak ' Proxy ' adlı bir ekran genişliği renk/derinlik işleme hedefine işlenmesi gerekir. Proxy, geri arabelleğin çözümlenmesinden uyuşmalıdır. Bir oturum başlamaya başladıktan sonra, `GraphicsBindingSimD3d11.InitSimulation` kendisine bağlanmadan önce çağrılması gerekir:

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

İnit işlevinin, yerel D3D-cihazının işaretçileriyle birlikte, proxy oluşturma hedefinin renk ve derinlik dokusunu sağlaması gerekir. Başlatıldıktan `AzureSession.ConnectToRuntime` ve `DisconnectFromRuntime` birden çok kez çağrılabilir, ancak farklı bir oturuma geçiş yapıldığında, `GraphicsBindingSimD3d11.DeinitSimulation` başka bir oturumda çağrılabilmesi için önce `GraphicsBindingSimD3d11.InitSimulation` eski oturumda çağrılmalıdır.

#### <a name="render-loop-update"></a>Döngü güncelleştirmesini işle

İşleme döngüsü güncelleştirmesi birden çok adımdan oluşur:

1. Her bir çerçeve, herhangi bir işleme gerçekleşmeden önce `GraphicsBindingSimD3d11.Update` , işlenecek sunucuya gönderilen geçerli kamera dönüşümüyle çağrılır. Aynı zamanda, proxy oluşturma hedefine işlemek için, döndürülen proxy dönüştürmesinin proxy kameraya uygulanması gerekir.
Döndürülen proxy güncelleştirmesi `SimulationUpdate.frameId` null ise, henüz uzak veri yok. Bu durumda, proxy oluşturma hedefine işlemek yerine, tüm yerel içerik, geçerli kamera verileri kullanılarak doğrudan arabellekte işlenmelidir ve sonraki iki adım atlanır.
1. Uygulama artık proxy oluşturma hedefini ve çağrısını `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy`bağlamalıdır. Bu, uzak renk ve derinlik bilgilerini ara sunucu işleme hedefine doldurur. Tüm yerel içerikler artık proxy kamera dönüşümü kullanılarak proxy üzerinde oluşturulabilir.
1. Ardından, arka arabelleğin bir işleme hedefi olarak bağlanması ve `GraphicsBindingSimD3d11.ReprojectProxy` arka arabelleğin sunulabileceği noktada çağrılması gerekir.

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

* [Öğretici: sıfırdan Unity projesi ayarlama](../tutorials/unity/project-setup.md)
