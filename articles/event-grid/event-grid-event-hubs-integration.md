---
title: 'Öğretici: veri ambarına Event Hubs veri gönderme-Event Grid'
description: Azure Işlevleri ve Event Grid Tetikleyicileri aracılığıyla Azure SYNAPSE Analytics 'te yakalanan verileri nasıl Event Hubs depolayabileceğinizi açıklar.
ms.topic: tutorial
ms.date: 12/07/2020
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 7b0e471e32650490e1896bb6ea171c8223b21378
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96854725"
---
# <a name="tutorial-stream-big-data-into-a-data-warehouse"></a>Öğretici: veri ambarına büyük veri akışı
Azure [Event Grid](overview.md) , uygulama ve hizmetlerden gelen bildirimlere veya olaylara tepki sağlamanıza olanak tanıyan akıllı bir olay yönlendirme hizmetidir. Örneğin, bir Azure Işlevini bir blob depolamaya veya Data Lake Storage yakalanan Event Hubs verileri işleyecek şekilde tetikleyebilirler. Bu [örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) , yakalanan Event Hubs verilerini blob depolamadan Azure SYNAPSE Analytics 'e (özel olarak ayrılmış bir SQL havuzuna) geçirmek için Event Grid ve Azure işlevlerinin nasıl kullanılacağını gösterir.

[!INCLUDE [event-grid-event-hubs-functions-synapse-analytics.md](../../includes/event-grid-event-hubs-functions-synapse-analytics.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Azure mesajlaşma servislerindeki farklar hakkında bilgi sahibi olmak için, bkz. [iletiler teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Hubs Capture’a giriş için bkz. [Azure portalını kullanarak Event Hubs Capture’ı etkinleştirme](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Örneği ayarlama ve çalıştırma hakkında daha fazla bilgi için bkz. [Event Hubs Capture ve Event Grid örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).
