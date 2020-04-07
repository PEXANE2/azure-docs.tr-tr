---
title: Birlik için Uzaktan İşleme'yi Ayarlama
description: Bir Unity projesinde Azure Uzaktan İşleme'yi başlatma
author: jakrams
ms.author: jakras
ms.date: 02/27/2020
ms.topic: how-to
ms.openlocfilehash: 0415c0e7ee1432521c3cc2026feff5fc2a41d77e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681148"
---
# <a name="set-up-remote-rendering-for-unity"></a>Birlik için Uzaktan İşleme'yi Ayarlama

Azure Uzaktan İşleme'yi (ARR) Unity olarak etkinleştirmek için, Birliğe özel bazı yönleri ele alan özel yöntemler sayılmak üzere özel yöntemler sayılabiliriz.

## <a name="startup-and-shutdown"></a>Başlatma ve kapatma

Uzaktan İşleme'yi başlatmayı sağlamak `RemoteManagerUnity`için . Bu sınıf genel `RemoteManager` çağrıyı çağırır, ancak sizin için Birliğe özel ayrıntıları zaten uygular. Örneğin, Unity belirli bir koordinat sistemi kullanır. Arama `RemoteManagerUnity.Initialize`yaparken, uygun kongre ayarlanır. Arama ayrıca uzaktan işlenen içeriği görüntülemek için kullanılması gereken Unity kamerayı sağlamanızı da gerektirir.

```cs
// initialize Azure Remote Rendering for use in Unity:
// it needs to know which camera is used for rendering the scene
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);
```

Uzaktan İşleme'yi kapatmak `RemoteManagerStatic.ShutdownRemoteRendering()`için .

Bir `AzureSession` oluşturma ve birincil işleme oturumu olarak seçildikten sonra, `RemoteManagerUnity`bu oturuma kaydedilmelidir:

```cs
RemoteManagerUnity.CurrentSession = ...
```

### <a name="full-example-code"></a>Tam örnek kodu

Aşağıdaki kod, Azure Uzaktan İşleme'yi Unity olarak başlatmanız için gereken tüm adımları göstermektedir:

```cs
// initialize Remote Rendering
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);

// create a frontend
AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
// ... fill out accountInfo ...
AzureFrontend frontend = new AzureFrontend(accountInfo);

// start a session
AzureSession session = await frontend.CreateNewRenderingSessionAsync(new RenderingSessionCreationParams(RenderingSessionVmSize.Standard, 0, 30)).AsTask();

// let RemoteManagerUnity know about the session we want to use
RemoteManagerUnity.CurrentSession = session;

session.ConnectToRuntime(new ConnectToRuntimeParams());

/// When connected, load and modify content

RemoteManagerStatic.ShutdownRemoteRendering();
```

## <a name="convenience-functions"></a>Kolaylık fonksiyonları

### <a name="session-state-events"></a>Oturum durumu olayları

`RemoteManagerUnity.OnSessionUpdate`oturum durumu değiştiğinde olayları yayır, ayrıntılar için kod belgelerine bakın.

### <a name="arrserviceunity"></a>ARRServiceUnity

`ARRServiceUnity`kurulum ve oturum yönetimini kolaylaştırmak için isteğe bağlı bir bileşendir. Uygulama çıkarken veya editörden oyun modu çıkarken oturumunu otomatik olarak durdurmak ve gerektiğinde oturum kirasını otomatik olarak yenileme seçenekleri içerir. Oturum özellikleri (değişkenine `LastProperties` bakın) gibi verileri önbelleğe almaz ve oturum durumu değişiklikleri ve oturum hataları için olayları ortaya çıkarır.

Aynı `ARRServiceUnity` anda birden fazla örnek olamaz. Bazı yaygın işlevleri uygulayarak daha hızlı başlamanızı sağlar. Daha büyük bir uygulama için olsa da, bu şeyleri kendiniz yapmak için tercih edilebilir.

Bkz. `ARRServiceUnity` [Öğretici: Bir Birlik projesini sıfırdan ayarlama](../../tutorials/unity/project-setup.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Unity için Uzaktan İşleme paketini yükleme](install-remote-rendering-unity-package.md)
* [Öğretici: Sıfırdan bir Birlik projesi kurma](../../tutorials/unity/project-setup.md)
