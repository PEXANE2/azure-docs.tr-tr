---
title: Compare Azure Content Delivery Network (CDN) ürün özelliklerini karşılaştırın | Microsoft Docs
description: Her Azure Content Delivery Network (CDN) ürününün desteklediği özellikler hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: mdgattuso
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 0e57ae691bf4b07b8161bc343929510d6be041a8
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260522"
---
# <a name="compare-azure-cdn-product-features"></a>Azure CDN ürün özelliklerini karşılaştırın

Azure Content Delivery Network (CDN), dört ürün içerir: **Microsoft’tan Azure CDN Standart**, **Akamai’den Azure CDN Standart**, **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium**. **Verizon'dan Azure CDN Standart** profilinden **Verizon’dan Azure CDN Premium** profiline geçme hakkında bilgi için bkz. [Verizon'dan Azure CDN Standart profilinden Verizon’dan Azure CDN Premium profiline geçiş yapma](cdn-migrate.md). Standart Verizon Premium Verizon bir yükseltme yolu olsa da, şu anda diğer ürünler arasında hiçbir dönüşüm mekanizması olduğunu unutmayın.

Aşağıdaki tabloda her ürünle birlikte sunulan özellikler karşılaştırılmaktadır.

| **Performans özellikleri ve optimizasyonlar** | **Standart Microsoft** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** |
| --- | --- | --- | --- | --- |
| [Dinamik alan ivmesi](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  | Azure [Ön Kapı Hizmeti](https://docs.microsoft.com/azure/frontdoor/front-door-overview) ile sunulur | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site ivmesi - uyarlanabilir görüntü sıkıştırma](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dinamik site ivmesi - nesne prefetch](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| [Genel web teslimat optimizasyonu](https://docs.microsoft.com/azure/cdn/cdn-optimization-overview#general-web-delivery)  | **&#x2713;** | **&#x2713;**, Ortalama dosya boyutunuz 10 MB'dan küçükse bu optimizasyon türünü seçin  | **&#x2713;** |  **&#x2713;** |
| [Video akışı optimizasyonu](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | Genel Web Teslimatı ile | **&#x2713;**  | Genel Web Teslimatı ile |  Genel Web Teslimatı ile |
| [Büyük dosya optimizasyonu](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | Genel Web Teslimatı ile | **&#x2713;**, Ortalama dosya boyutunuz 10 MB'dan büyükse bu optimizasyon türünü seçin   | Genel Web Teslimatı ile |  Genel Web Teslimatı ile |
| Optimizasyon türünü değiştirme | |**&#x2713;** | | |
| Origin Bağlantı Noktası |Tüm TCP bağlantı noktaları |[İzin verilen kaynak bağlantı noktaları](https://docs.microsoft.com/previous-versions/azure/mt757337(v%3Dazure.100)#allowed-origin-ports) |Tüm TCP bağlantı noktaları |Tüm TCP bağlantı noktaları |
| [Genel sunucu yük dengeleme (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Hızlı temizleme](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;**, Tüm ve Wildcard temizleme şu anda Akamai Gelen Azure CDN tarafından desteklenmez |**&#x2713;** |**&#x2713;** |
| [Varlık önceden yükleme](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| Önbellek/üst bilgi ayarları ([önbelleğe alma](cdn-caching-rules.md) kullanılarak)  |Standart [kurallar motorundan](cdn-standard-rules-engine.md) **&#x2713;**  |**&#x2713;** |**&#x2713;** | |
| Özelleştirilebilir, kurallara dayalı içerik dağıtım motoru |Standart [kurallar motorundan](cdn-standard-rules-engine.md) **&#x2713;**  | | |kurallar [motorundan](cdn-rules-engine.md) **&#x2713;** |
| Önbellek/üstbilgi ayarları  |Standart [kurallar motorundan](cdn-standard-rules-engine.md) **&#x2713;** | | |Premium [kurallar motorini](cdn-rules-engine.md) kullanarak **&#x2713;** |
| URL yönlendirme/yeniden yazma |Standart [kurallar motorundan](cdn-standard-rules-engine.md) **&#x2713;**  | | |Premium [kurallar motorini](cdn-rules-engine.md) kullanarak **&#x2713;** |
| Mobil cihaz kuralları  |Standart [kurallar motorundan](cdn-standard-rules-engine.md) **&#x2713;** | | |Premium [kurallar motorini](cdn-rules-engine.md) kullanarak **&#x2713;** |
| [Sorgu dizesini önbelleğe alma](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| IPv4/IPv6 ikili yığını | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [HTTP/2 desteği](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Güvenlik** | **Standart Microsoft** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| CDN uç noktasıyla HTTPS desteği | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı HTTPS](cdn-custom-ssl.md)  | **&#x2713;** | **&#x2713;**, Etkinleştirmek için doğrudan CNAME gerektirir |**&#x2713;** |**&#x2713;** |
| [Özel etki alanı adı desteği](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Coğrafi filtreleme](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Belirteç kimlik doğrulaması](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [DDOS koruması](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Kendi sertifikanızı getirme](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) |**&#x2713;** |  | **&#x2713;** | **&#x2713;** |
| Desteklenen TLS Sürümleri | TLS 1.2, TLS 1.0/1.1 - [Yapılandırılabilir](https://docs.microsoft.com/rest/api/cdn/customdomains/enablecustomhttps#usermanagedhttpsparameters) | TLS 1.2 | TLS 1.2 | TLS 1.2 |
||||
| **Analiz ve raporlama** | **Standart Microsoft** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Verizon’dan alınan çekirdek raporlar](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Verizon özel raporları](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [Gerçek zamanlı istatistikler](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [Uç düğümü performansı](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [Gerçek zamanlı uyarılar](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **Kullanım kolaylığı** | **Standart Microsoft** | **Standart Akamai** | **Standart Verizon** | **Premium Verizon** | 
| [Depolama](cdn-create-a-storage-account-with-cdn.md), [Web Apps](cdn-add-to-web-app.md) ve [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) gibi Azure hizmetleriyle kolay tümleştirme  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| REST [API](/rest/api/cdn/), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md)veya [PowerShell](cdn-manage-powershell.md) ile Yönetim  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Sıkıştırma MIME tipleri](https://docs.microsoft.com/azure/cdn/cdn-improve-performance)  |Yalnızca varsayılan |Yapılandırılabilir |Yapılandırılabilir  |Yapılandırılabilir  |
| Sıkıştırma kodlamaları  |gzip, brotli |Gzip |gzip, deflate, bzip2, brotili  |gzip, deflate, bzip2, brotili  |






