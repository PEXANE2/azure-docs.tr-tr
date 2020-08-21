---
title: IoT Edge sürüm notlarında canlı video analizi-Azure
description: Bu konu, IoT Edge sürümler, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar üzerine canlı video analizinin sürüm notlarını sağlar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: c18977807d8cdca264a27d7fcff37aec7a61eb55
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690660"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge sürüm notlarında canlı video analizi

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` RSS akışı okuyucusuna.

Bu makalede hakkında bilgi verilmektedir:

* En son yayınlar
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="august-19-2020"></a>19 Ağustos 2020

Modülün Ağustos 2020 yenilemesi için bu yayın etiketi:

```
mcr.microsoft.com/media/live-video-analytics:1.0.3
```

> [!NOTE]
> Hızlı başlangıç ve öğreticiler bölümünde, dağıtım bildirimleri 1 (canlı-video-analiz: 1) etiketini kullanır. Bu nedenle, bu tür bildirimleri yeniden dağıtmak Edge > cihazlarınızda modülü güncellemelidir.

## <a name="new-features"></a>Yeni özellikler 

* Artık, gRPC çerçevesini kullanarak IoT Edge ve özel uzantınız üzerinde canlı video analizi arasında yüksek veri içeriği aktarım performansı alabilirsiniz. Başlamak için [bunu](analyze-live-video-use-your-grpc-model-quickstart.md) inceleyin.
* Canlı video analizinin daha geniş bölgesel dağıtımı ve yalnızca bulut hizmeti güncelleştirilmiştir.  
* Canlı video analizi artık dünya genelinde 25 ek bölgede kullanılabilir. Tüm kullanılabilir bölgelerin [listesi](https://azure.microsoft.com/global-infrastructure/services/?products=media-services) aşağıda verilmiştir.  
* Hızlı çalışmaya yönelik [ayarla ayarı](https://aka.ms/lva-edge/setup-resources-for-samples) da güncelleştirildi. 

## <a name="bug-fixes"></a>Hata düzeltmeleri 

Hiçbiri 

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
