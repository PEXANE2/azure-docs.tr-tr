---
ms.openlocfilehash: 0e800668bea2f744f8862292b21be814858b0c16
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97486874"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs5.svg" alt-text="Sinyal akışı":::

Bu diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , Real-Time Akış Protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetir. Bir [RTSP kaynak](../../../media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [http uzantısı işlemci](../../../media-graph-concept.md#http-extension-processor) düğümüne video çerçeveleri gönderir. 

HTTP uzantısı düğümü bir ara sunucu rolünü yürütür. Bu, alanı kullanarak ayarlanan gelen video çerçevelerini örnekler `samplingOptions` ve ayrıca video çerçevelerini belirtilen görüntü türüne dönüştürür. Ardından, görüntüyü REST üzerinden bir HTTP uç noktasının arkasında bulunan bir AI modeli çalıştıran başka bir Edge modülüne geçirir. Bu örnekte, bu Edge modülü, birçok nesne türünü tespit eden [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) modeli kullanılarak oluşturulmuştur. HTTP uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [IoT Hub havuz](../../../media-graph-concept.md#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](../../../../../iot-edge/iot-edge-glossary.md#iot-edge-hub)gönderir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.
