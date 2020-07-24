---
title: Azure CDN kullanım biçimlerini çözümleme
description: Bu makalede Azure CDN ürünleri için kullanılabilen farklı analiz raporu türleri açıklanmaktadır.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/22/2020
ms.author: allensu
ms.openlocfilehash: 169889dbb87d00fdde44ff72c0d2004c331604ab
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073042"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

Uygulamanız için CDN 'yi etkinleştirdikten sonra CDN kullanımını izleyebilir, teslimin durumunu denetleyebilir ve olası sorunları giderebilirsiniz. Azure CDN aşağıdaki yollarla bu özellikleri sağlar: 

## <a name="raw-logs-for-azure-cdn-from-microsoft"></a>Microsoft 'tan Azure CDN için ham Günlükler
Standart bir Microsoft profiliyle, ham günlükleri etkinleştirebilir ve günlüklerin akışını şu şekilde seçebilirsiniz:

* Azure Storage
* Event Hubs
* Azure Log Analytics

Azure Log Analytics izleme ölçümlerini görüntüleyebilir ve uyarıları ayarlayabilirsiniz. 

Daha fazla bilgi için bkz. [http RAW günlüklerini Azure CDN](enable-raw-logs.md).


## <a name="core-analytics-via-azure-diagnostic-logs"></a>Azure tanılama günlükleri aracılığıyla temel analiz

Çekirdek analizi tüm fiyatlandırma katmanları için CDN uç noktalarında kullanılabilir. Azure tanılama günlükleri, çekirdek analizlerinin Azure depolama, Olay Hub 'ları veya Azure Izleyici günlüklerine aktarılmasını sağlar. Azure Izleyici günlükleri, Kullanıcı tarafından yapılandırılabilen ve özelleştirilebilen grafiklerle bir çözüm sunar. Azure tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

**Verizon profillerden Verizon veya Azure CDN Premium** **'dan Azure CDN Standart** çekirdek raporlar sağlar. Temel raporları Verizon ek portalında görüntüleyebilirsiniz. Verizon Core raporlarına Azure portal **Yönet** seçeneği aracılığıyla erişilebilir ve farklı türlerde grafikler ve görünümler sunar. Daha fazla bilgi için bkz. [Verizon from Core Reports](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

**Verizon profillerden Verizon veya Azure CDN Premium** **'dan Azure CDN Standart** özel raporlar sağlar. Verizon ek portalındaki özel raporları görüntüleyebilirsiniz. Verizon özel raporlara Azure portal **Yönet** seçeneği aracılığıyla erişilebilir. 

Özel raporlar her bir kenar CNAME için aktarılan isabetlerin veya verilerin sayısını görüntüler. Veriler HTTP yanıt kodu veya süre içinde önbellek durumu tarafından gruplandırılır. Daha fazla bilgi için [Verizon adresinden özel raporlar](cdn-verizon-custom-reports.md)bölümüne bakın.

## <a name="azure-cdn-premium-from-verizon-reports"></a>Verizon raporlarında Premium Azure CDN

**Verizon ' den Azure CDN Premium**ile aşağıdaki raporlara de erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Azure CDN Edge düğümü performansı](cdn-edge-performance.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure CDN analiz raporları için farklı seçenekler hakkında bilgi edindiniz.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz.:

* [Azure CDN nedir?](cdn-overview.md)
* [HTTP ham günlüklerini Azure CDN](enable-raw-logs.md)


