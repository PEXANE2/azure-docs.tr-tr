---
ms.openlocfilehash: 97c21ca300ee070b2cebaa01a585c1618899b1eb
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691890"
---

![Genel Bakış](../../../media/quickstarts/overview-grpc.png)

Bu diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](../../../media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [hareket algılama işlemcisi](../../../media-graph-concept.md#motion-detection-processor) düğümüne video çerçeveleri gönderir. Bu işlemci hareketi algılayacak ve algılama sonrasında, ekran çerçevelerini [GRPC uzantısı işlemci](../../../media-graph-concept.md#grpc-extension-processor) düğümüne gönderecektir.

GRPC uzantısı düğümü bir ara sunucu rolünü yürütür. Video çerçevelerini belirtilen görüntü türüne dönüştürür. Daha sonra, görüntüyü gRPC üzerinden bir gRPC uç noktasının arkasında [paylaşılan bir bellek](https://en.wikipedia.org/wiki/Shared_memory)üzerinde çalışan başka bir uç modüle geçirir. Bu örnekte, bu Edge modülü, birçok nesne türünü tespit eden [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) modeli kullanılarak oluşturulmuştur. GRPC uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [IoT Hub havuz](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/media-graph-concept#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](https://docs.microsoft.com/azure/iot-edge/iot-edge-glossary#iot-edge-hub)gönderir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.
