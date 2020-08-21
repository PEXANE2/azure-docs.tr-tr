---
ms.openlocfilehash: 768e79c6a2471715b336f90748ad97ecfcc4bbc2
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682273"
---

![genel bakış](../../../media/quickstarts/overview-qs4.png)

Yukarıdaki diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Sınır modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı bir akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](../../../media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [hareket algılama işlemcisi](../../../media-graph-concept.md#motion-detection-processor) düğümüne video çerçeveleri gönderir. RTSP kaynağı bir olay tarafından tetiklenene kadar kapalı kalan bir [sinyal kapısı işlemci](../../../media-graph-concept.md#signal-gate-processor) düğümüne aynı video çerçevelerini gönderir.

Hareket algılama işlemcisi videoda hareket algıladığında, sinyal kapısı işlemci düğümüne bir olay gönderir ve bunu tetikler. Ağ Geçidi, yapılandırılan süre boyunca açılır ve video çerçevelerini [Dosya havuzu](../../../media-graph-concept.md#file-sink) düğümüne gönderir. Bu havuz düğümü, videoyu uç cihazınızın yerel dosya sisteminde bir MP4 dosyası olarak kaydeder. Dosya yapılandırılan konuma kaydedilir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.
