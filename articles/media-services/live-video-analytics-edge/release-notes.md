---
title: IoT Edge sürüm notlarında canlı video analizi-Azure
description: Bu konu, IoT Edge sürümler, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar üzerine canlı video analizinin sürüm notlarını sağlar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 28260728532d9db52b8d36488c2e456bd11803ea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091788"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge sürüm notlarında canlı video analizi

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` RSS akışı okuyucusuna.

Bu makalede hakkında bilgi verilmektedir:

* En son yayınlar
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="july-13-2020"></a>13 Temmuz 2020

Modülün Temmuz 2020 yenilemesi için bu yayın etiketi:

```
     mcr.microsoft.com/media/live-video-analytics:1.0.2
```

> [!NOTE]
> Hızlı başlangıç ve öğreticiler bölümünde, dağıtım bildirimleri 1 (canlı-video-analiz: 1) etiketini kullanır. Bu nedenle, bu tür bildirimleri yeniden dağıtmak Edge > cihazlarınızda modülü güncellemelidir.

### <a name="new-features"></a>Yeni özellikler
* Artık bir varlık havuzu düğümünün yanı sıra bir sinyal kapısı işlemcisi düğümünün bir dosya havuzu düğümü aşağı akış olan Graph topolojileri oluşturabilirsiniz. Bir [örnek için bkz](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-motion-assets-files) ..

### <a name="bug-fixes"></a>Hata düzeltmeleri
* İstenen özellikleri doğrulamaya yönelik iyileştirmeler

## <a name="june-1-2020"></a>1 Haziran 2020

Bu sürüm, IoT Edge üzerindeki canlı video analizinin ilk genel önizleme sürümüdür. Yayın etiketi

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>Desteklenen işlevler
* Seçtiğiniz AI modüllerini kullanarak canlı video akışlarını çözümleyin ve isteğe bağlı olarak, uç cihazında veya bulutta video kaydedin
* IoT Edge tarafından [desteklenen](../../iot-edge/support.md) Linux AMD64 işletim sistemlerinde kullanın
* IoT Hub Azure portal veya Visual Studio Code kullanarak modül dağıtın ve yapılandırın
* [Grafik topolojilerini ve grafik örneklerini](media-graph-concept.md#media-graph-topologies-and-instances) , aşağıdaki doğrudan Yöntem çağrıları aracılığıyla uzaktan veya yerel olarak yönetin

    *   Graphtopologyıget
    *   Graphtopologyıset
    *   Graphtopologyıdelete
    *   Graphtopologyılist
    *   Graphınstanceget
    *   Graphınstanceset
    *   Graphınstancedelete
    *   Graphınstancelist


## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](overview.md)
