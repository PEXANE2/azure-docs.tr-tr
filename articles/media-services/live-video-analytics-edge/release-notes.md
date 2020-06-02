---
title: IoT Edge sürüm notlarında canlı video analizi-Azure
description: Bu konu, IoT Edge sürümler, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar üzerine canlı video analizinin sürüm notlarını sağlar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: fd607ee027b46cf386180338440e6530da911302
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261040"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge sürüm notlarında canlı video analizi

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` RSS akışı okuyucusuna.

Bu makalede hakkında bilgi verilmektedir:

* En son yayınlar
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="june-1-2020"></a>1 Haziran 2020

Bu sürüm, IoT Edge üzerindeki canlı video analizinin ilk genel önizleme sürümüdür. Yayın etiketi

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>Desteklenen işlevler
* Seçtiğiniz AI modüllerini kullanarak canlı video akışlarını çözümleyin ve isteğe bağlı olarak, uç cihazında veya bulutta video kaydedin
* IoT Edge tarafından [desteklenen](https://docs.microsoft.com/azure/iot-edge/support) Linux AMD64 işletim sistemlerinde kullanın
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

[Genel Bakış](overview.md)
