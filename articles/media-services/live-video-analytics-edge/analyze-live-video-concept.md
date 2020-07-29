---
title: Canlı videoyu herhangi bir kayıt olmadan çözümleme-Azure
description: Bir medya grafiği, bir canlı video akışından gelen analizi, bir kenara veya buluta kaydetmeye gerek kalmadan ayıklamak için kullanılabilir. Bu makalede bu kavram ele alınmaktadır.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 29e00b9c04a652771ca150e2a45e980d20f8bc1f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84260942"
---
# <a name="analyzing-live-video-without-any-recording"></a>Canlı videoyu herhangi bir kayıt olmadan çözümleme

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma 

* [Medya grafiği kavramı](media-graph-concept.md)
* [Olay tabanlı video kaydı](event-based-video-recording-concept.md)

## <a name="overview"></a>Genel Bakış  

Videonun herhangi bir bölümünü bir dosya veya varlığa kaydetmeden canlı videoyu çözümlemek için bir medya grafiği kullanabilirsiniz. Aşağıda gösterilen medya grafikleri, [olay tabanlı video kaydındaki](event-based-video-recording-concept.md)makalesindeki, ancak bir varlık havuzu düğümü veya dosya havuzu düğümü olmadan makaledeki olanlarla benzerdir.

### <a name="motion-detection"></a>Hareket algılama

Aşağıda gösterilen medya grafiği bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümünden, bir [hareket algılama işlemcisi](media-graph-concept.md#motion-detection-processor) düğümünden ve bir [IoT Hub ileti havuzu](media-graph-concept.md#iot-hub-message-sink) düğümünden oluşur. Bu tür bir medya grafiğinin grafik topolojisinin JSON temsili [burada](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/motion-detection/topology.json)bulunabilir. Bu grafik, gelen canlı video akışındaki hareketi algılamanıza ve hareket olaylarını IoT Hub ileti havuzu düğümü aracılığıyla diğer uygulama ve hizmetlere geçirebilmenizi sağlar. Dış uygulamalar veya hizmetler bir uyarı tetikleyip uygun personele bildirim gönderebilir.

![Hareket algılamayı temel alan canlı video analizi](./media/analyze-live-video/motion-detection.png)

### <a name="analyzing-video-using-a-custom-vision-model"></a>Özel bir Vision modeli kullanarak video çözümleme 

Aşağıda gösterilen medya grafiği, ayrı bir modülde paketlenmiş özel bir Vision modeli kullanarak canlı bir video akışını analiz etmenizi sağlar. Bu tür bir medya grafiğinin grafik topolojisinin JSON temsili [burada](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json)bulunabilir. Bir çıkarım hizmeti olarak çalışan IoT Edge modüllerine model sarmalama [üzerinde bazı örnekler bulabilirsiniz.](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis)

![Dış bir ınlaç modülü temelinde canlı video analizi](./media/analyze-live-video/external-inferencing-module.png)

Bu medya grafiğinde, çerçeve hızı filtre işlemcisi düğümü, gelen canlı video akışının kare hızını, bekleyen bir dış çıkarım hizmetine görüntü çerçeveleri (JPEG, BMP veya PNG biçiminde) gönderen bir [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümüne göndermeden önce azaltır. Dış çıkarım hizmetinin sonuçları HTTP uzantısı düğümü tarafından alınır ve IoT Hub ileti havuzu düğümü aracılığıyla IoT Edge hub 'ına işlenir. Bu tür medya grafiği, bir kesişme noktasındaki araçlar 'ın zaman serisi dağıtımını anlama, bir perakende mağazasındaki tüketici trafiği modelini anlama vb. gibi çeşitli senaryolar için çözümler oluşturmak üzere kullanılabilir.

Bu örnek için bir geliştirme, çerçeve hızı filtresi işlemci düğümündeki bir hareket algılayıcısı işlemcisini kullanmaktır. Bu, yalnızca videoda bir hareket etkinliği olduğunda kullanıldığından, çıkarım hizmetindeki yükü azaltır.

![Dış ınlaç modülü aracılığıyla hareket algılanan çerçeveleri temel alan canlı video analizi](./media/analyze-live-video/motion-detected-frames.png)

## <a name="next-steps"></a>Sonraki adımlar

[Sürekli video kaydı](continuous-video-recording-concept.md)
