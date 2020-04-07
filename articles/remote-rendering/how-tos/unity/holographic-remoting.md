---
title: Holografik Remoting ve Uzaktan İşlemi Birlikte Kullanma
description: Holografik Remoting önizlemesi Azure Uzaktan İşleme ile birlikte nasıl kullanılabilir?
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: ac47a2922e92233f0acabf75817a712671306bc1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681213"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Holografik Remoting ve Uzaktan İşlemi Birlikte Kullanma

[Holografik Remoting](https://docs.microsoft.com/windows/mixed-reality/holographic-remoting-player) ve Azure Uzaktan İşlem, tek bir uygulamada birbirini dışlar. Bu nedenle, [Unity oyun modu](https://docs.microsoft.com/windows/mixed-reality/unity-play-mode) da kullanılamaz.

Unity düzenleyicisinin her çalışması için ikisinden yalnızca biri kullanılabilir. Diğerini kullanmak için önce Birlik'i yeniden başlatın.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>Hololens 2'de önizleme yapmak için Unity oyun modunu kullanma

 Örneğin uygulamanın kullanıcı arasını test etmek için unity oyun modu hala kullanılabilir. Ancak, ARR'ın hiçbir zaman baş harfe başlatılanmamış olması hayati önem taşımaktadır. Yoksa çökecek.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Masaüstünde önizleme yapmak için WMR VR kulaklık kullanma

Windows Mixed Reality VR kulaklık varsa, Unity içinde önizleme yapmak için kullanılabilir. Bu durumda, ARR'ı başlatmada sorun yoktur, ancak WMR kulaklık kullanılırken bir oturuma bağlanmak mümkün olmayacaktır.
