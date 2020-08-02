---
title: Azure Izleyici ile ölçümleri görüntüleme
description: Bu makalede Azure portal grafikleri ve Azure CLı ile ölçümlerin nasıl izleneceği gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.custom: devx-track-azurecli
ms.openlocfilehash: 154e5b5d9639203810e9d16dec4e2907fe5ee80a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504305"
---
# <a name="monitor-media-services-metrics"></a>Media Services ölçümlerini izleme

[Azure izleyici](../../azure-monitor/overview.md) , uygulamalarınızın nasıl çalıştığını anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak sağlar. Bu özelliğin ayrıntılı açıklaması ve Azure Media Services ölçümleri ve tanılama günlüklerini nasıl kullanmak istediğinizi görmek için bkz. [izleme Media Services ölçümleri ve tanılama günlükleri](media-services-metrics-diagnostic-logs.md).

Azure Izleyici, ölçümlerle etkileşimde bulunmak için, portalda grafik oluşturma, REST API aracılığıyla erişme veya Azure CLı kullanarak sorgulama gibi çeşitli yollar sağlar. Bu makalede Azure portal grafikleri ve Azure CLı ile ölçümlerin nasıl izleneceği gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Media Services hesabı oluşturma](./create-account-howto.md)
- [İzleme Media Services ölçümleri ve tanılama günlüklerini](media-services-metrics-diagnostic-logs.md) gözden geçirin

## <a name="view-metrics-in-azure-portal"></a>Azure portal ölçümleri görüntüleme

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Azure Media Services hesabınıza gidin ve **ölçümler**' i seçin.
1. **Kaynak** kutusuna tıklayın ve ölçümlerini izlemek istediğiniz kaynağı seçin.

    **Kaynak Seç** penceresi, sağda kullanabileceğiniz kaynakların listesiyle birlikte görünür. Bu durumda şunları görürsünüz:

    * &lt;Media Services hesap adı&gt;
    * &lt;Media Services hesap adı &gt; / &lt; akış uç noktası adı&gt;
    * &lt;depolama hesabı adı&gt;

    Kaynağı seçin ve **Uygula**' ya basın. Desteklenen kaynaklar ve ölçümler hakkında daha fazla bilgi için bkz. [izleme Media Services ölçümleri](media-services-metrics-diagnostic-logs.md).

    ![Ölçümler](media/media-services-metrics/metrics02.png)

    > [!NOTE]
    > Ölçümleri izlemek istediğiniz kaynaklar arasında geçiş yapmak için **kaynak** kutusuna yeniden tıklayın ve bu adımı tekrarlayın.
1. (İsteğe bağlı olarak) grafiğinize bir ad verin (üstteki kurşun kaleme basarak adı düzenleyin).
1. Görüntülemek istediğiniz ölçümleri ekleyin.

    ![Ölçümler](media/media-services-metrics/metrics03.png)
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

* [Azure Izleyici ölçümleri](../../azure-monitor/platform/data-platform.md)
* [Azure izleyici 'yi kullanarak ölçüm uyarıları oluşturun, görüntüleyin ve yönetin](../../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Sonraki adımlar

[Tanılama günlükleri](media-services-diagnostic-logs-howto.md)
