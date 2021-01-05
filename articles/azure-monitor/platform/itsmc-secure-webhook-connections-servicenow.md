---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma-ServiceNow ile yapılandırma
description: Bu makalede, Azure Izleyici 'de güvenli dışarı aktarma hakkında ServiceNow ile ıTSM ürünlerinizi/hizmetlerinizin nasıl bağlanacağı gösterilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: fc384ffbc246f5ce9fa84b8161cbc4a5226fa5c8
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97843622"
---
# <a name="connect-servicenow-to-azure-monitor"></a>ServiceNow 'ı Azure Izleyici 'ye bağlama

Aşağıdaki bölümlerde, Azure 'da ServiceNow ürününüzü bağlama ve güvenli dışarı aktarma işlemlerinin nasıl yapılacağı hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları karşılatığınızdan emin olun:

* Azure AD kaydedilir.
* ServiceNow olay yönetimi-ıTOM 'un desteklenen sürümüne sahipsiniz (sürüm Orlando veya üzeri).

## <a name="configure-the-servicenow-connection"></a>ServiceNow bağlantısını yapılandırma

1. Https://(örnek adı) bağlantısını kullanın. Service-Now. com/API/sn_em_connector/em/inbound_event? Source = azuremonitor, güvenli dışarı aktarma tanımı için URI.

2. Sürüme göre yönergeleri izleyin:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Tarihleri arasında Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [New Yok](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/task/azure-events-authentication.html)