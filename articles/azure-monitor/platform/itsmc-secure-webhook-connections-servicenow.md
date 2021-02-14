---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma-ServiceNow ile yapılandırma
description: Bu makalede, Azure Izleyici 'de güvenli dışarı aktarma hakkında ServiceNow ile ıTSM ürünlerinizi/hizmetlerinizin nasıl bağlanacağı gösterilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: 7801145ba1bcaa2ffd543becfe190f05d232e8c8
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100104969"
---
# <a name="connect-servicenow-to-azure-monitor"></a>ServiceNow 'ı Azure Izleyici 'ye bağlama

Aşağıdaki bölümlerde, Azure 'da ServiceNow ürününüzü bağlama ve güvenli dışarı aktarma işlemlerinin nasıl yapılacağı hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları karşılatığınızdan emin olun:

* Azure AD kaydedilir.
* ServiceNow olay yönetimi-ıTOM 'un desteklenen sürümüne sahipsiniz (New York veya üzeri sürümü).

## <a name="configure-the-servicenow-connection"></a>ServiceNow bağlantısını yapılandırma

1. Https://(örnek adı) bağlantısını kullanın. Service-Now. com/API/sn_em_connector/em/inbound_event? Source = azuremonitor, güvenli dışarı aktarma tanımı için URI.

2. Sürüme göre yönergeleri izleyin:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Tarihleri arasında Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [New Yok](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
