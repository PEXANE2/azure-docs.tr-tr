---
title: Unity’de Holographic Remoting’i ve Remote Rendering’i kullanma
description: Holographic Remoting Preview 'ın Azure uzaktan Işleme ile birlikte nasıl kullanılabileceği
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: ac47a2922e92233f0acabf75817a712671306bc1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80681213"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Unity’de Holographic Remoting’i ve Remote Rendering’i kullanma

[Holographic Remoting](https://docs.microsoft.com/windows/mixed-reality/holographic-remoting-player) ve Azure uzaktan işleme, bir uygulama içinde birbirini dışlıyor. Bu nedenle, [Unity oynatma modu](https://docs.microsoft.com/windows/mixed-reality/unity-play-mode) da kullanılamaz.

Unity düzenleyicisinin her çalışması için, bunlardan yalnızca biri kullanılabilir. Diğerini kullanmak için önce Unity 'yi yeniden başlatın.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>HoloLens 2 ' de önizleme yapmak için Unity Play modunu kullanma

 Unity oynatma modu, örneğin uygulamanın kullanıcı arabirimini test etmek için hala kullanılabilir. Ancak, ARR 'nin hiç başlatılmaması çok önemlidir. Aksi takdirde kilitlenme olur.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Masaüstünde önizleme yapmak için bir WMR VR kulaklık kullanın

Windows Mixed Reality VR kulaklığı varsa, Unity içinde önizleme yapmak için kullanılabilir. Bu durumda, ARR 'yi başlatmak iyi bir durumdur, ancak WMR kulaklık kullanıldığı sırada bir oturuma bağlanabilmek mümkün olmayacaktır.
