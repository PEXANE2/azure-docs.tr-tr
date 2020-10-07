---
title: Unity’de Holographic Remoting’i ve Remote Rendering’i kullanma
description: Holographic Remoting Preview 'ın Azure uzaktan Işleme ile birlikte nasıl kullanılabileceği
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: 180af30f57a8123b6e90cc8b11848b92b3c86db1
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91802183"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Unity’de Holographic Remoting’i ve Remote Rendering’i kullanma

[Holographic Remoting](https://docs.microsoft.com/windows/mixed-reality/holographic-remoting-player) ve Azure uzaktan işleme, bir uygulama içinde birbirini dışlıyor. Bu nedenle, [Unity oynatma modu](https://docs.microsoft.com/windows/mixed-reality/unity-play-mode) da kullanılamaz.

Unity düzenleyicisinin her çalışması için, bunlardan yalnızca biri kullanılabilir. Diğerini kullanmak için önce Unity 'yi yeniden başlatın.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>HoloLens 2 ' de önizleme yapmak için Unity Play modunu kullanma

 Unity oynatma modu, örneğin uygulamanın kullanıcı arabirimini test etmek için hala kullanılabilir. Ancak, ARR 'nin hiç başlatılmaması çok önemlidir. Aksi takdirde kilitlenme olur.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Masaüstünde önizleme yapmak için bir WMR VR kulaklık kullanın

Windows Mixed Reality VR kulaklığı varsa, Unity içinde önizleme yapmak için kullanılabilir. Bu durumda, ARR 'yi başlatmak iyi bir durumdur, ancak WMR kulaklık kullanıldığı sırada bir oturuma bağlanabilmek mümkün olmayacaktır.
