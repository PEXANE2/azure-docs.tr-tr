---
title: Azure CDN kullanım modellerini analiz edin | Microsoft Dokümanlar
description: Bu makalede, Azure CDN ürünleri için kullanılabilen farklı analiz raporları türleri açıklanmaktadır.
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
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253620"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım biçimlerini çözümleme

Uygulamanız için CDN'yi etkinleştirdikten sonra CDN kullanımını izleyebilir, doğumunuzun durumunu kontrol edebilir ve olası sorunları giderebilirsiniz. Azure CDN bu özellikleri aşağıdaki şekillerde sağlar: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Azure tanı lama günlükleri aracılığıyla temel analitik

Tüm fiyatlandırma katmanları için CDN uç noktaları için çekirdek analitiği kullanılabilir. Azure tanılama günlükleri, temel analizlerin Azure depolamasına, etkinlik merkezlerine veya Azure Monitor günlüklerine dışa aktarılmasına olanak tanır. Azure Monitor günlükleri, kullanıcı tarafından yapılandırılabilir ve özelleştirilebilir grafiklere sahip bir çözüm sunar. Azure tanı günlükleri hakkında daha fazla bilgi için [Azure tanı günlüklerine](cdn-azure-diagnostic-logs.md)bakın.

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

**Verizon profilinden Bir Azure CDN Standardı** veya **Azure CDN Premium** ile bir Azure CDN kullanıcısı olarak, Verizon ek portalında Verizon çekirdek raporlarını görüntüleyebilirsiniz. Verizon çekirdek raporlarına Azure portalından **Yönet** seçeneği üzerinden erişilebilir ve çeşitli grafikler ve görünümler sunar. Daha fazla bilgi için, [Verizon Çekirdek Raporlar](cdn-analyze-usage-patterns.md)bakın.

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

**Verizon profilinden Bir Azure CDN Standardı** veya **Azure CDN Premium** ile bir Azure CDN kullanıcısı olarak, Verizon ek portalında Verizon özel raporlarını görüntüleyebilirsiniz. Verizon özel raporlarına Azure portalından **Yönet** seçeneği yle erişilebilir. Verizon özel raporlar sayfası, Azure CDN profiline ait her kenar CName için aktarılan isabet veya veri sayısını gösterir. Veriler, herhangi bir süre içinde HTTP yanıt koduna veya önbellek durumuna göre gruplandırılabilir. Daha fazla bilgi için, [Verizon Özel Raporlar](cdn-verizon-custom-reports.md)bakın.

## <a name="azure-cdn-premium-from-verizon-reports"></a>Verizon raporlarından Azure CDN Premium

**Verizon'dan Azure CDN Premium**ile aşağıdaki raporlara da erişebilirsiniz:
   * [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
   * [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)
   * [Uç düğümü performansı](cdn-edge-performance.md)

