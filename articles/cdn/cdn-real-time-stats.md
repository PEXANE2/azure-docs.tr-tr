---
title: Azure CDN'de gerçek zamanlı istatistikler | Microsoft Dokümanlar
description: Gerçek zamanlı istatistikler, müşterilerinize içerik sunarken Azure CDN performansı hakkında gerçek zamanlı veriler sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d56007e5a196a0857f3b69ac51f5e3b5a88c4f6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593500"
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Microsoft Azure CDN'de gerçek zamanlı istatistikler
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belge, Microsoft Azure CDN'deki gerçek zamanlı istatistikleri açıklar.  Bu işlevsellik, istemcilerinize içerik teslim ederken bant genişliği, önbellek durumları ve CDN profilinize eşzamanlı bağlantılar gibi gerçek zamanlı veriler sağlar. Bu, canlı yayın etkinlikleri de dahil olmak üzere hizmetinizin durumunun her zaman sürekli olarak izlenmesini sağlar.

Aşağıdaki grafikler mevcuttur:

* [Bant genişliği](#bandwidth)
* [Durum Kodları](#status-codes)
* [Önbellek Durumları](#cache-statuses)
* [Bağlantılar](#connections)

## <a name="accessing-real-time-stats"></a>Gerçek zamanlı istatistiklere erişim
1. Azure [Portalı'nda](https://portal.azure.com)CDN profilinize göz atın.
   
    ![CDN profil bıçağı](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profil blade yönetme düğmesi](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
3. **Analytics** sekmesinin üzerine binin, ardından **Gerçek Zamanlı İstatistikler** uçuşunun üzerinde gezin.  HTTP **Büyük Nesne'ye**tıklayın.
   
    ![CDN yönetim portalı](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    Gerçek zamanlı istatistik grafikleri görüntülenir.

Grafiklerin her biri, sayfa yüklendiğinden itibaren seçili zaman aralığı için gerçek zamanlı istatistikler görüntüler.  Grafikler birkaç saniyede bir otomatik olarak güncellenir.  Varsa **Grafiği Yenile** düğmesi grafiği temizler ve ardından yalnızca seçili verileri görüntüler.

## <a name="bandwidth"></a>Bant genişliği
![Bant genişliği grafiği](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Bant genişliği** grafiği, seçili zaman aralığında geçerli platform için kullanılan bant genişliği miktarını görüntüler. Grafiğin gölgeli kısmı bant genişliği kullanımını gösterir. Şu anda kullanılmakta olan bant genişliğinin tam miktarı çizgi grafiğinin hemen altında görüntülenir.

## <a name="status-codes"></a>Durum Kodları
![Durum kodu grafiği](./media/cdn-real-time-stats/cdn-status-codes.png)

**Durum Kodları** grafiği, belirli HTTP yanıt kodlarının seçili zaman aralığında ne sıklıkta oluştuğunu gösterir.

> [!TIP]
> Her BIR HTTP durum kodu seçeneğinin açıklaması için [Azure CDN HTTP Durum Kodları'na](/previous-versions/azure/mt759238(v=azure.100))bakın.
> 
> 

HTTP durum kodlarının listesi grafiğin hemen üzerinde görüntülenir. Bu liste, satır grafiğine dahil edilebilen her durum kodunu ve bu durum kodu için saniyedeki geçerli oluşum sayısını gösterir. Varsayılan olarak, grafikteki bu durum kodlarının her biri için bir satır görüntülenir. Ancak, yalnızca CDN yapılandırmanız için özel öneme sahip durum kodlarını izlemeyi seçebilirsiniz. Bunu yapmak için, istenilen durum kodlarını kontrol edin ve diğer tüm seçenekleri temizleyin, ardından **Graph'ı Yenile'yi**tıklatın. 

Belirli bir durum kodu için günlüğe kaydedilmiş verileri geçici olarak gizleyebilirsiniz.  Grafiğin hemen altındaki göstergeden, gizlemek istediğiniz durum kodunu tıklatın. Durum kodu grafikten hemen gizlenir. Durum kodunu yeniden tıklatmak, bu seçeneğin yeniden görüntülenmesine neden olur.

## <a name="cache-statuses"></a>Önbellek Durumları
![Önbellek Durumları grafiği](./media/cdn-real-time-stats/cdn-cache-status.png)

**Önbellek Durumları** grafiği, belirli önbellek durumlarının belirli zaman aralığında ne sıklıkta oluştuğunu gösterir. 

> [!TIP]
> Her önbellek durum kodu seçeneğinin açıklaması için [Azure CDN Önbellek Durum Kodları'na](/previous-versions/azure/mt759237(v=azure.100))bakın.
> 
> 

Önbellek durum kodlarının listesi doğrudan grafiğin üzerinde görüntülenir. Bu liste, satır grafiğine dahil edilebilen her durum kodunu ve bu durum kodu için saniyedeki geçerli oluşum sayısını gösterir. Varsayılan olarak, grafikteki bu durum kodlarının her biri için bir satır görüntülenir. Ancak, yalnızca CDN yapılandırmanız için özel öneme sahip durum kodlarını izlemeyi seçebilirsiniz. Bunu yapmak için, istenilen durum kodlarını kontrol edin ve diğer tüm seçenekleri temizleyin, ardından **Graph'ı Yenile'yi**tıklatın. 

Belirli bir durum kodu için günlüğe kaydedilmiş verileri geçici olarak gizleyebilirsiniz.  Grafiğin hemen altındaki göstergeden, gizlemek istediğiniz durum kodunu tıklatın. Durum kodu grafikten hemen gizlenir. Durum kodunu yeniden tıklatmak, bu seçeneğin yeniden görüntülenmesine neden olur.

## <a name="connections"></a>Bağlantılar
![Bağlantılar grafiği](./media/cdn-real-time-stats/cdn-connections.png)

Bu grafik, kenar sunucularınıza kaç bağlantı kurulduğunu gösterir. CDN'mizden geçen bir varlık için yapılan her istek bir bağlantıyla sonuçlanır.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN'de gerçek zamanlı uyarılarla](cdn-real-time-alerts.md) bilgilendirilin
* [Gelişmiş HTTP raporlarıyla](cdn-advanced-http-reports.md) daha derine inin
* [Kullanım modellerini](cdn-analyze-usage-patterns.md) analiz edin

