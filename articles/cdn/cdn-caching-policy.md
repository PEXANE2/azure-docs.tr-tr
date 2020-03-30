---
title: Azure Medya Hizmetleri'nde Azure CDN önbelleğe alma ilkesini yönetme | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri'nde Azure CDN önbelleğe alma ilkesinin nasıl yönetilen olduğu açıklanmaktadır.
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: ''
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: dc0482fbcbb1c9d1618ec18e1f48b03f686a6573
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74892592"
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Azure Medya Hizmetleri'nde Azure CDN önbelleğe alma ilkesini yönetme
Azure Media Services, HTTP tabanlı Uyarlanabilir Akış ve aşamalı indirme sağlar. HTTP tabanlı akış, proxy ve CDN katmanlarında önbelleğe almanın yanı sıra istemci tarafı önbelleğe alma nın avantajlarıyla yüksek oranda ölçeklenebilir. Akış uç noktaları, http önbellek üstbilgisi için genel akış özellikleri ve yapılandırma sağlar. Akış uç noktaları http Önbellek Denetimi ayarlar: maksimum yaş ve Sona Erer üstleri. [W3.org](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)http önbellek üstbilgileri için daha fazla bilgi alabilirsiniz.

## <a name="default-caching-headers"></a>Varsayılan Önbelleğe Alma üstbilgi
Varsayılan olarak akış uç noktaları isteğe bağlı akış verileri (gerçek ortam parçaları/parçaları) ve bildirim (çalma listesi) için 3 günlük önbellek üstbilgileri uygular. Canlı akış için akış uç noktaları, veri için 3 günlük önbellek üstbilgileri (gerçek ortam parçaları/parçaları) ve bildirim (çalma listesi) istekleri için 2 saniye önbellek üstbilgisi uygular. Canlı program isteğe bağlı (canlı arşiv) döndüğünde, isteğe bağlı akış önbellek üstbilgilerini uygular.

## <a name="azure-cdn-integration"></a>Azure CDN tümleştirmesi
Azure Media Services, akış uç noktaları için [tümleşik CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) sağlar. Önbellek denetimi üstgeçitleri, CDN etkin leştirilmiş akış uç noktalarına akış uç noktaları gibi uygulanır. Azure CDN, dahili önbelleğe alınmış nesnelerin kullanım süresini tanımlamak için akış uç noktası yapılandırılmış önbellek değerlerini kullanır ve ayrıca teslim önbelleği üstbilgilerini ayarlamak için bu değeri kullanır. CDN etkin akış uç noktaları kullanırken küçük önbellek değerleri ayarlamak için önerilmez. Küçük değerler ayarlamak performansı düşürür ve CDN'nin yararını azaltır. CDN etkin akış uç noktaları için 600 saniyeden küçük önbellek üstbilgilerine ayarlanmasına izin verilmez.

> [!IMPORTANT]
>Azure Medya Hizmetleri, Azure CDN ile tam tümleştirmeye sahiptir. Tek bir tıklamayla, standart ve premium ürünler de dahil olmak üzere tüm kullanılabilir Azure CDN sağlayıcılarını akış bitiş noktanıza entegre edebilirsiniz. Daha fazla bilgi için bu [duyuruya](https://azure.microsoft.com/blog/standardstreamingendpoint/)bakın.
> 
> Son nokta akışından CDN'ye veri yükleri yalnızca CDN son nokta API'leri akışı veya Azure portalının akış uç noktası bölümünü kullanarak etkinleştirildiğinde devre dışı bırakılır. CDN API'lerini veya portal bölümünü kullanarak el ile tümleştirme veya doğrudan CDN bitiş noktası oluşturma, veri ücretlerini devre dışı kılmaz.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Önbellek üstbilgilerini Azure Medya Hizmetleri ile yapılandırma
Önbellek üstbilgi değerlerini yapılandırmak için Azure portalı veya Azure Medya Hizmetleri API'lerini kullanabilirsiniz.

1. Azure portalını kullanarak önbellek üstbilgilerini yapılandırmak için, [Akış Bitiş Noktasını](../media-services/previous/media-services-portal-manage-streaming-endpoints.md) Yapılandırma Bölümü'ne bakın.
2. Azure Medya Hizmetleri REST API, [StreamingEndpoint](/rest/api/media/operations/streamingendpoint#StreamingEndpointCacheControl).
3. Azure Medya Hizmetleri .NET SDK, [StreamingEndpointCacheControl Özellikleri](https://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Önbellek yapılandırma öncelik sırası
1. Azure Medya Hizmetleri yapılandırılan önbellek değeri varsayılan değeri geçersiz kılar.
2. El ile yapılandırma yoksa, varsayılan değerler uygulanır.
3. Varsayılan olarak 2 saniye önbellek üstbilgileri, Azure Media veya Azure Depolama yapılandırmasına bakılmaksızın canlı akış bildirimi (çalma listesi) için geçerlidir ve bu değerin geçersiz kılınması kullanılamaz.

