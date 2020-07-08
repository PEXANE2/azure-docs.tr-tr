---
title: Azure CDN kullanım düzenlerini çözümleme | Microsoft Docs
description: Bu makalede Azure CDN ürünleri için kullanılabilen farklı analiz raporu türleri açıklanmaktadır.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: allensu
ms.openlocfilehash: c78dff9f5258023d90100c70bd244e8e0d016d6f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81253620"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

Uygulamanız için CDN 'yi etkinleştirdikten sonra CDN kullanımını izleyebilir, teslimin durumunu denetleyebilir ve olası sorunları giderebilirsiniz. Azure CDN aşağıdaki yollarla bu özellikleri sağlar: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Azure tanılama günlükleri aracılığıyla temel analiz

Çekirdek analizi tüm fiyatlandırma katmanları için CDN uç noktalarında kullanılabilir. Azure tanılama günlükleri, çekirdek analizlerinin Azure depolama, Olay Hub 'ları veya Azure Izleyici günlüklerine aktarılmasını sağlar. Azure Izleyici günlükleri, Kullanıcı tarafından yapılandırılabilen ve özelleştirilebilen grafiklerle bir çözüm sunar. Azure tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

Verizon profilinden Verizon veya **Azure CDN Premium** **'Dan bir Azure CDN standardı** olan Azure CDN bir kullanıcı olarak, Verizon ek portalındaki Verizon çekirdek raporlarını görüntüleyebilirsiniz. Verizon Core raporlarına Azure portal **Yönet** seçeneği aracılığıyla erişilebilir ve çeşitli grafikler ve görünümler sunar. Daha fazla bilgi için bkz. [Verizon from Core Reports](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

Verizon profilinden Verizon veya **Azure CDN Premium** **'Dan bir Azure CDN standardı** olan Azure CDN bir kullanıcı olarak, Verizon ek portalındaki Verizon özel raporlarını görüntüleyebilirsiniz. Verizon özel raporlara Azure portal **Yönet** seçeneği aracılığıyla erişilebilir. Verizon özel raporlar sayfası, bir Azure CDN profiline ait olan her bir Edge için aktarılan isabetlerin veya verilerin sayısını gösterir. Veriler, herhangi bir süre içinde HTTP yanıt kodu veya önbellek durumuyla gruplandırılabilir. Daha fazla bilgi için [Verizon adresinden özel raporlar](cdn-verizon-custom-reports.md)bölümüne bakın.

## <a name="azure-cdn-premium-from-verizon-reports"></a>Verizon raporlarında Premium Azure CDN

**Verizon ' den Azure CDN Premium**ile aşağıdaki raporlara de erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Uç düğümü performansı](cdn-edge-performance.md)

