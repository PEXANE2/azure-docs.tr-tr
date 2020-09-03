---
title: Kendi gRPC modelinizi kullanarak canlı videoyu çözümleyin-Azure
description: Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için bilgisayar vizyonu uygularsınız.
ms.topic: quickstart
ms.date: 08/14/2020
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: 907d402a4b0ef5d5e437322d02431a0ccd2d8da3
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421552"
---
# <a name="quickstart-analyze-live-video-by-using-your-own-grpc-model"></a>Hızlı başlangıç: kendi gRPC modelinizi kullanarak canlı videoyu çözümleyin

Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Nesneleri algılamak için bir bilgisayar Vision modelinin nasıl uygulanacağını göreceksiniz. Canlı video akışındaki çerçevelerin bir alt kümesi bir çıkarım hizmetine gönderilir. Sonuçlar IoT Edge hub 'ına gönderilir.

::: zone pivot="programming-language-csharp"
[!INCLUDE [header](includes/analyze-live-video-your-grpc-model-quickstart/csharp/header.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [header](includes/analyze-live-video-your-grpc-model-quickstart/python/header.md)]
::: zone-end

## <a name="prerequisites"></a>Önkoşullar

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-grpc-model-quickstart/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/analyze-live-video-your-grpc-model-quickstart/python/prerequisites.md)]
::: zone-end

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin

::: zone pivot="programming-language-csharp"
[!INCLUDE [review sample video](includes/analyze-live-video-your-grpc-model-quickstart/csharp/review-sample-video.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [review sample video](includes/analyze-live-video-your-grpc-model-quickstart/python/review-sample-video.md)]
::: zone-end

## <a name="overview"></a>Genel bakış

::: zone pivot="programming-language-csharp"
[!INCLUDE [overview](includes/analyze-live-video-your-grpc-model-quickstart/csharp/overview.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [overview](includes/analyze-live-video-your-grpc-model-quickstart/python/overview.md)]
::: zone-end

## <a name="create-and-deploy-the-media-graph"></a>Medya grafiğini oluşturma ve dağıtma

::: zone pivot="programming-language-csharp"
[!INCLUDE [create and deploy the media graph](includes/analyze-live-video-your-grpc-model-quickstart/csharp/create-deploy-media-graph.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [create and deploy the media graph](includes/analyze-live-video-your-grpc-model-quickstart/python/create-deploy-media-graph.md)]
::: zone-end

## <a name="interpret-results"></a>Sonuçları yorumlama

::: zone pivot="programming-language-csharp"
[!INCLUDE [interpret results](includes/analyze-live-video-your-grpc-model-quickstart/csharp/interpret-results.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [interpret results](includes/analyze-live-video-your-grpc-model-quickstart/python/interpret-results.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıç yapmayı düşünüyorsanız oluşturduğunuz kaynakları saklayın. Aksi takdirde, Azure portal gidin, kaynak gruplarınıza gidin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* GRPC protokolünü kullanarak farklı medya grafiği topolojileri çalıştırmayı deneyin.
* **Örnek canlı video analizi (LVA) uzantıları oluşturma ve çalıştırma**
<br/>[Onnx](http://onnx.ai/) tabanlı Yolo modellerini canlı video analizi (LVA) uzantısı olarak oluşturmanıza ve çalıştırmanıza olanak tanıyan Jupyıter örnek Not defterlerimizi deneyin.
    * [Örnek YOLOv3 modeli](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/readme.md)
    * [Örnek YOLOv4 modeli](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov4/yolov4-grpc-icpu-onnx/readme.md)

