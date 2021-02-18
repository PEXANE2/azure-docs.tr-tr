---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma-ServiceNow ile yapılandırma
description: Bu makalede, Azure Izleyici 'de güvenli dışarı aktarma hakkında ServiceNow ile ıTSM ürünlerinizi/hizmetlerinizin nasıl bağlanacağı gösterilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: 8ca77c1fa1232f7aed88b0d6942d8a0085caf0d5
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100625747"
---
# <a name="connect-servicenow-to-azure-monitor"></a>ServiceNow 'ı Azure Izleyici 'ye bağlama

Aşağıdaki bölümlerde, Azure 'da ServiceNow ürününüzü bağlama ve güvenli dışarı aktarma işlemlerinin nasıl yapılacağı hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları karşılatığınızdan emin olun:

* Azure AD kaydedilir.
* ServiceNow olay yönetimi-ıTOM 'un desteklenen sürümüne sahipsiniz (New York veya üzeri sürümü).
* ServiceNow örneğinde yüklü [uygulama](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ac4c9c57dbb1d090561b186c1396191a/1.2.0?referer=%2Fstore%2Fsearch%3Flistingtype%3Dallintegrations%25253Bancillary_app%25253Bcertified_apps%25253Bcontent%25253Bindustry_solution%25253Boem%25253Butility%26q%3Devent%2520management%2520connectors&sl=sh) .

## <a name="configure-the-servicenow-connection"></a>ServiceNow bağlantısını yapılandırma

1. Https://(örnek adı) bağlantısını kullanın. Service-Now. com/API/sn_em_connector/em/inbound_event? Source = azuremonitor, güvenli dışarı aktarma tanımı için URI.

2. Sürüme göre yönergeleri izleyin:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Tarihleri arasında Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [New Yok](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
