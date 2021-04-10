---
title: Azure Izleyici ile ölçümleri görüntüleme
description: Bu makalede Azure portal grafikleri ve Azure CLı ile ölçümlerin nasıl izleneceği gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/17/2021
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: cedadf6b6026c74e25e9c81feb7f8291058b6f69
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105961668"
---
# <a name="monitor-media-services-metrics"></a>Media Services ölçümlerini izleme

[!INCLUDE [media services api v3 logo](../includes/v3-hr.md)]

[Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview.md) , uygulamalarınızın nasıl çalıştığını anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak sağlar. Bu özelliğin ayrıntılı bir açıklaması için ve Azure Media Services ölçümleri ve tanılama günlüklerini nasıl kullanmanız gerektiğini anlamak için bkz. [izleme Media Services ölçümleri ve tanılama günlükleri](monitor-media-services-data-reference.md).

Azure Izleyici, ölçümlerle etkileşimde bulunmak için, portalda grafik oluşturma, REST API aracılığıyla erişme veya Azure CLı kullanarak sorgulama gibi çeşitli yollar sağlar. Bu makalede Azure portal grafikleri ve Azure CLı ile ölçümlerin nasıl izleneceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- [Media Services hesabı oluşturma](../account-create-how-to.md)
- [İzleme Media Services ölçümleri ve tanılama günlüklerini](monitor-media-services-data-reference.md) gözden geçirin

## <a name="view-metrics-in-azure-portal"></a>Azure portal ölçümleri görüntüleme

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Azure Media Services hesabınıza gidin ve **ölçümler**' i seçin.
1. **Kapsam** kutusuna tıklayın ve izlemek istediğiniz kaynağı seçin.

    **Kapsam seçin** penceresi, sağda kullanabileceğiniz kaynakların listesiyle birlikte görünür. Bu durumda şunları görürsünüz:

    * &lt;Media Services hesap adı&gt;
    * &lt;Media Services hesap adı &gt; / &lt; akış uç noktası adı&gt;
    * &lt;depolama hesabı adı&gt;

    Ardından, kaynağı seçin ve **Uygula**' ya basın. Desteklenen kaynaklar ve ölçümler hakkında daha fazla bilgi için bkz. [izleme Media Services ölçümleri](monitor-media-services-data-reference.md).

    > [!NOTE]
    > İzlemek istediğiniz kaynaklar arasında geçiş yapmak için, **kaynak** kutusuna yeniden tıklayın ve bu adımı tekrarlayın.

1. İsteğe bağlı: grafiğinize bir ad verin (üstteki kurşun kaleme basarak adı düzenleyin).
1. Görüntülemek istediğiniz ölçümleri ekleyin.
1. Grafiğinizi panonuza sabitleyebilir.

## <a name="view-metrics-with-azure-cli"></a>Azure CLı ile ölçümleri görüntüleme

Azure CLı ile "çıkış" ölçümleri almak için aşağıdaki `az monitor metrics` komutu çalıştırın:

```azurecli-interactive
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Diğer ölçümleri almak için ilgilendiğiniz ölçüm adına "çıkış" koyun.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure İzleyici Ölçümleri](https://docs.microsoft.com/azure/azure-monitor/data-platform.md)
- [Azure izleyici 'yi kullanarak ölçüm uyarıları oluşturun, görüntüleyin ve yönetin](https://docs.microsoft.com/azure/azure-monitor/alerts/alerts-metric.md).

## <a name="next-steps"></a>Sonraki adımlar

[Tanılama günlükleri](../media-services-diagnostic-logs-howto.md)
