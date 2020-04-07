---
title: Oluşturma modları
description: Farklı sunucu tarafı oluşturma modlarını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 7f2b1031659864ae338bb0aa320c048ea23c21f3
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681707"
---
# <a name="rendering-modes"></a>Oluşturma modları

Uzaktan İşleme iki ana çalışma modu, **TileBasedComposition** modu ve **DepthBasedComposition** modu sunar. Bu modlar, iş yükünün sunucudaki birden çok GPU'da nasıl dağıtıldığını belirler. Mod bağlantı zamanında belirtilmelidir ve çalışma sırasında değiştirilemez.

Her iki mod da avantajları ile birlikte gelir ama aynı zamanda doğal özellik sınırlamaları ile, bu nedenle en uygun modu seçerek kullanım durumda özel.

## <a name="modes"></a>Modu

İki mod artık daha ayrıntılı olarak tartışılmıştır.

### <a name="tilebasedcomposition-mode"></a>'TileBasedComposition' modu

**TileBasedComposition** modunda, ilgili her GPU ekranda belirli alt dikdörtgenler (fayans) işler. Ana GPU, istemciye video çerçevesi olarak gönderilmeden önce kutucuklardan son görüntüyü oluşturur. Buna göre, tüm GPU'ların işlemek için aynı kaynaklar kümesine sahip olması gerekir, bu nedenle yüklenen varlıkların tek bir GPU'nun bellene sığması gerekir.

MSAA her GPU için tam geometri kümesi üzerinde çalışabildiği için, bu moddaki görüntüleme kalitesi **DepthBasedComposition** modundan biraz daha iyidir. Aşağıdaki ekran görüntüsü, antialiasing aynı şekilde her iki kenar için düzgün çalıştığını gösterir:

![TileBasedComposition'da MSAA](./media/service-render-mode-quality.png)

Ayrıca, bu modda her parça saydam bir malzemeye geçilebilir veya [HiyerarşikStateOverrideComponent](../overview/features/override-hierarchical-state.md) üzerinden **görme** moduna geçilebilir

### <a name="depthbasedcomposition-mode"></a>'DepthBasedComposition' modu

**DepthBasedComposition** modunda, ilgili her GPU tam ekran çözünürlüğünde, ancak yalnızca bir alt meshes'te işlenir. Ana GPU'daki son görüntü kompozisyonu, parçaların derinlik bilgilerine göre düzgün bir şekilde birleştirilmesine dikkat eder. Doğal olarak, bellek yükü GPU'lar arasında dağıtılır ve böylece tek bir GPU'nun belleğine sığmayan modelleri işlemeye olanak sağlar.

Her bir GPU, yerel içeriği antialias için MSAA kullanır. Ancak, farklı GPU'lardan kenarlar arasında doğal bir diğer ad olabilir. Bu efekt son görüntünün işlenmesiyle azaltılır, ancak MSAA kalitesi yine de **TileBasedComposition** modundakinden daha kötüdür.

MSAA yapıları aşağıdaki resimde gösterilmiştir: ![MsAA In DepthBasedComposition](./media/service-render-mode-balanced.png)

Her iki bölüm de aynı GPU üzerinde işlenir, çünkü antialiasing, heykel ve perde arasında düzgün çalışır. Öte yandan, perde ve duvar arasındaki kenar, bu iki bölüm ayrı GPU'lardan oluştuğu için bazı takma adlar gösterir.

Bu modun en büyük sınırlama, geometri parçaları dinamik olarak saydam malzemelere geçilemez ne de [hiyerarşikStateOverrideComponent](../overview/features/override-hierarchical-state.md)için **see-through** modu çalışır. Diğer durum geçersiz kılma özellikleri (anahat, renk tonu, ...) olsa da, iş yok. Ayrıca dönüştürme zamanında saydam olarak işaretlenmiş malzemeler de bu modda düzgün çalışır.

### <a name="performance"></a>Performans

Her iki modun performans özellikleri kullanım durumuna bağlı olarak değişir ve genel öneriler vermek veya mantıklı hale getirmek zordur. Yukarıda belirtilen sınırlamalarla (bellek veya saydamlık/gezinme) kısıtlanmazsanız, her iki modu da denemeniz ve performansı çeşitli kamera konumlarını kullanarak izlemeniz önerilir.

## <a name="setting-the-render-mode"></a>İşlem modunu ayarlama

Uzaktan İşleme VM'de `AzureSession.ConnectToRuntime` `ConnectToRuntimeParams`kullanılan render modu.

```cs
async void ExampleConnect(AzureSession session)
{
    ConnectToRuntimeParams parameters = new ConnectToRuntimeParams();

    // Connect with one rendering mode
    parameters.mode = ServiceRenderMode.TileBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();

    session.DisconnectFromRuntime();

    // Wait until session.IsConnected == false

    // Reconnect with a different rendering mode
    parameters.mode = ServiceRenderMode.DepthBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Oturumlar](../concepts/sessions.md)
* [Hiyerarşik durum geçersiz kılma bileşeni](../overview/features/override-hierarchical-state.md)
