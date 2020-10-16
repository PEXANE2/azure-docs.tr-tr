---
title: IoT Edge sürüm notlarında canlı video analizi-Azure
description: Bu konu, IoT Edge sürümler, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar üzerine canlı video analizinin sürüm notlarını sağlar.
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 18da50fe763ef7c5fc3c06ac00052679c488cae7
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92015643"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge sürüm notlarında canlı video analizi

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` RSS akışı okuyucusuna.

Bu makalede hakkında bilgi verilmektedir:

* En son yayınlar
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

<hr width=100%>

## <a name="september-22-2020"></a>22 Eylül 2020

Modülün Eylül 2020 yenilemesi için bu yayın etiketi:

```
mcr.microsoft.com/media/live-video-analytics:1.0.4
```

> [!NOTE]
> Hızlı başlangıç ve öğreticiler bölümünde, dağıtım bildirimleri 1 (canlı-video-analiz: 1) etiketini kullanır. Bu nedenle, bu tür bildirimleri yeniden dağıtmak Edge > cihazlarınızda modülü güncellemelidir.

### <a name="module-updates"></a>Modül güncelleştirmeleri

* Yeni bir Graf uzantısı düğümü olan [MediaGraphCognitiveServicesVisionExtension](spatial-analysis-tutorial.md) , bilişsel hizmetler 'Den [uzamsal analiz](/legal/cognitive-services/computer-vision/intro-to-spatial-analysis-public-preview)(Önizleme) modülüyle tümleştirilebilen şekilde kullanılabilir.
* Linux ARM64 cihazları için destek eklendi-bu tür cihazlara dağıtım için [el ile adımları](deploy-iot-edge-device.md) kullanın.

### <a name="documentation-updates"></a>Belge güncelleştirmeleri

* IoT Edge, Azure Stack Edge cihazlarındaki canlı video analizlerini kullanmaya yönelik [yönergeler](deploy-azure-stack-edge-how-to.md) sunulmaktadır.
* [Özel görüntü işleme hizmetini](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) kullanarak senaryoya özgü bilgisayar görme modelleri geliştirmeye ve gerçek zamanlı olarak [Canlı videoyu analiz](custom-vision-tutorial.md) etmeye yönelik yeni öğretici.

<hr width=100%>

## <a name="august-19-2020"></a>19 Ağustos 2020

Modülün Ağustos 2020 yenilemesi için bu yayın etiketi:

```
mcr.microsoft.com/media/live-video-analytics:1.0.3
```

> [!NOTE]
> Hızlı başlangıç ve öğreticiler bölümünde, dağıtım bildirimleri 1 (canlı-video-analiz: 1) etiketini kullanır. Bu nedenle, bu tür bildirimleri yeniden dağıtmak Edge > cihazlarınızda modülü güncellemelidir.

### <a name="module-updates"></a>Modül güncelleştirmeleri

* Artık, gRPC çerçevesini kullanarak IoT Edge ve özel uzantınız üzerinde canlı video analizi arasında yüksek veri içeriği aktarım performansı alabilirsiniz. Başlamak için [bunu](analyze-live-video-use-your-grpc-model-quickstart.md) inceleyin.
* Canlı video analizinin daha geniş bölgesel dağıtımı ve yalnızca bulut hizmeti güncelleştirilmiştir.  
* Canlı video analizi artık dünya genelinde 25 ek bölgede kullanılabilir. Tüm kullanılabilir bölgelerin [listesi](https://azure.microsoft.com/global-infrastructure/services/?products=media-services) aşağıda verilmiştir.  
* Hızlı [çalışmaya yönelik kurulum](https://aka.ms/lva-edge/setup-resources-for-samples) , yeni bölgelerin desteğiyle birlikte güncelleştirilmiştir.
    * Zaten kaynakları kuran herkes için eylem çağrısı yoktur

### <a name="bug-fixes"></a>Hata düzeltmeleri 

* Kurulum betikindeki kullanım dışı bırakılmış bir Azure uzantısının kullanımını kaldırma

<hr width=100%>

## <a name="july-13-2020"></a>13 Temmuz 2020

Modülün Temmuz 2020 yenilemesi için bu yayın etiketi:

```
mcr.microsoft.com/media/live-video-analytics:1.0.2
```

> [!NOTE]
> Hızlı başlangıç ve öğreticiler bölümünde, dağıtım bildirimleri 1 (canlı-video-analiz: 1) etiketini kullanır. Bu nedenle, bu tür bildirimleri yeniden dağıtmak Edge > cihazlarınızda modülü güncellemelidir.

### <a name="module-updates"></a>Modül güncelleştirmeleri

* Artık bir varlık havuzu düğümünün yanı sıra bir sinyal kapısı işlemcisi düğümünün bir dosya havuzu düğümü aşağı akış olan Graph topolojileri oluşturabilirsiniz. Bir [örnek için bkz](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-motion-assets-files) ..

### <a name="bug-fixes"></a>Hata düzeltmeleri

* İstenen özellikleri doğrulamaya yönelik iyileştirmeler

<hr width=100%>

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

[Genel Bakış](overview.md)