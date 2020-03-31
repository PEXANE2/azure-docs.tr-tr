---
title: Azure Monitor ile ölçümleri görüntüleme
description: Bu makalede, Azure portal grafikleri ve Azure CLI ile ölçümlernasıl izlenir gösterilmektedir.
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
ms.openlocfilehash: c230e1e950bb924631032940642a6202acf4ade8
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382945"
---
# <a name="monitor-media-services-metrics"></a>Media Services ölçümlerini izleme

[Azure Monitor,](../../azure-monitor/overview.md) uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak tanır. Bu özelliğin ayrıntılı açıklaması ve Azure Medya Hizmetleri ölçümlerini ve tanılama günlüklerini neden kullanmak istediğinizi görmek için [Bkz.](media-services-metrics-diagnostic-logs.md)

Azure Monitor, ölçümleri portalda grafiklendirmek, REST API'si aracılığıyla bunlara erişmek veya Azure CLI kullanarak sorgulamak gibi ölçümlerle etkileşim kurmanın çeşitli yollarını sağlar. Bu makalede, Azure portal grafikleri ve Azure CLI ile ölçümlernasıl izlenir gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Media Services hesabı oluşturma](create-account-cli-how-to.md)
- [Monitör Medya Hizmetleri ölçümlerini ve tanılama günlüklerini](media-services-metrics-diagnostic-logs.md) gözden geçirin

## <a name="view-metrics-in-azure-portal"></a>Azure portalında ölçümleri görüntüleme

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Azure Medya Hizmetleri hesabınıza gidin ve **Ölçümler'i**seçin.
1. **KAYNAK** kutusunu tıklatın ve ölçümleri izlemek istediğiniz kaynağı seçin.

    **Kaynak Seç** penceresi sağda, kullanabileceğiniz kaynakların listesiyle birlikte görünür. Bu durumda bkz:

    * &lt;Medya Hizmetleri hesap adı&gt;
    * &lt;Medya Hizmetleri&gt;/&lt;hesap adı akışı uç nokta adı&gt;
    * &lt;depolama hesabı adı&gt;

    Kaynağı seçin ve **Uygula'ya**basın. Desteklenen kaynaklar ve ölçümler hakkında ayrıntılı bilgi [için](media-services-metrics-diagnostic-logs.md)bkz.

    ![Ölçümler](media/media-services-metrics/metrics02.png)

    > [!NOTE]
    > Ölçümleri izlemek istediğiniz kaynaklar arasında geçiş yapmak için **KAYNAK** kutusunu yeniden tıklatın ve bu adımı yineleyin.
1. (İsteğe bağlı olarak) grafiğinize bir ad verin (üstteki kaleme basarak adı edin).
1. Görüntülemek istediğiniz ölçümleri ekleyin.

    ![Ölçümler](media/media-services-metrics/metrics03.png)
1. Grafiğinizi panonuza sabitleyebilirsiniz.

## <a name="view-metrics-with-azure-cli"></a>Azure CLI ile ölçümleri görüntüleme

Azure CLI ile "Çıkış" ölçümlerini almak için aşağıdaki `az monitor metrics` komutu çalıştırırdınız:

```azurecli-interactive
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Diğer ölçümleri almak için, ilgilendiğiniz metrik ad için "Çıkış" yerine geçin.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Monitör Ölçümleri](../../azure-monitor/platform/data-platform.md)
* [Azure Monitor'u kullanarak metrik uyarıları oluşturun, görüntüleyin ve yönetin.](../../azure-monitor/platform/alerts-metric.md)

## <a name="next-steps"></a>Sonraki adımlar

[Tanılama günlükleri](media-services-diagnostic-logs-howto.md)
