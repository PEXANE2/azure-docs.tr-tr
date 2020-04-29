---
title: Unity için Remote Rendering’i ayarlama
description: Unity projesinde Azure uzaktan Işlemeyi başlatma
author: jakrams
ms.author: jakras
ms.date: 02/27/2020
ms.topic: how-to
ms.openlocfilehash: 0415c0e7ee1432521c3cc2026feff5fc2a41d77e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681148"
---
# <a name="set-up-remote-rendering-for-unity"></a>Unity için Remote Rendering’i ayarlama

Unity 'de Azure uzaktan oluşturma 'yı (ARR) etkinleştirmek için, Unity 'ye özgü bazı yönlerden oluşan adanmış yöntemler sunuyoruz.

## <a name="startup-and-shutdown"></a>Başlatma ve kapatmadan

Uzaktan Işlemeyi başlatmak için kullanın `RemoteManagerUnity`. Bu sınıf genel `RemoteManager` ' i çağırır, ancak sizin için Unity 'ye özgü ayrıntıları zaten uygular. Örneğin, Unity belirli bir koordinat sistemi kullanır. Çağrılırken `RemoteManagerUnity.Initialize`, uygun kural ayarlanır. Çağrı Ayrıca, uzaktan işlenmiş içeriği görüntülemek için kullanılması gereken Unity kamerayı sağlamanızı gerektirir.

```cs
// initialize Azure Remote Rendering for use in Unity:
// it needs to know which camera is used for rendering the scene
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);
```

Uzaktan Işlemeyi kapatmak için çağrısı `RemoteManagerStatic.ShutdownRemoteRendering()`yapın.

Bir `AzureSession` oluşturup, birincil işleme oturumu olarak seçildikten sonra, ile `RemoteManagerUnity`kaydolmalıdır:

```cs
RemoteManagerUnity.CurrentSession = ...
```

### <a name="full-example-code"></a>Tam örnek kod

Aşağıdaki kod, Unity 'de Azure uzaktan Işlemesini başlatmak için gereken tüm adımları göstermektedir:

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

## <a name="convenience-functions"></a>Kullanışlı işlevler

### <a name="session-state-events"></a>Oturum durumu olayları

`RemoteManagerUnity.OnSessionUpdate`oturum durumu değiştiğinde olayları yayar, Ayrıntılar için kod belgelerine bakın.

### <a name="arrserviceunity"></a>ARRServiceUnity

`ARRServiceUnity`, kurulum ve oturum yönetimini kolaylaştırmak için isteğe bağlı bir bileşendir. Uygulamanın çıkış sırasında veya yürütme modunun düzenleyicide çıkış yapıldığında oturumunu otomatik olarak durdurma ve gerektiğinde oturum kiralamasını otomatik olarak yenileme seçeneklerini içerir. Oturum özellikleri gibi verileri önbelleğe alır (kendi `LastProperties` değişkenine bakın) ve oturum durumu değişiklikleri ve oturum hataları için olayları ortaya koyar.

Tek seferde birden fazla örneği `ARRServiceUnity` olamaz. Bazı yaygın işlevleri uygulayarak daha hızlı bir şekilde başlamanıza yöneliktir. Daha büyük bir uygulama için, bu şeyleri sizin yapmanız tercih edilebilir, ancak.

Bir örnek için `ARRServiceUnity` bkz. [öğretici: bir Unity projesini sıfırdan ayarlama](../../tutorials/unity/project-setup.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Unity için Remote Rendering paketini yükleme](install-remote-rendering-unity-package.md)
* [Öğretici: sıfırdan Unity projesi ayarlama](../../tutorials/unity/project-setup.md)
