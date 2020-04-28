---
title: Azure CDN 'de gerçek zamanlı istatistikler | Microsoft Docs
description: Gerçek zamanlı istatistikler, istemcilerinize içerik teslim edilirken Azure CDN performansına ilişkin gerçek zamanlı veriler sağlar.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67593500"
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Microsoft Azure CDN 'de gerçek zamanlı istatistikler
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belgede Microsoft Azure CDN 'deki gerçek zamanlı istatistikler açıklanmaktadır.  Bu işlevsellik, istemcilerinize içerik teslim edilirken bant genişliği, önbellek durumları ve CDN profiliniz için eşzamanlı bağlantı gibi gerçek zamanlı veriler sağlar. Bu, canlı etkinlikler dahil olmak üzere, hizmetinizin sistem durumunun sürekli izlenmesini mümkün bir zamanda sunar.

Aşağıdaki grafikler mevcuttur:

* [Bant genişliği](#bandwidth)
* [Durum kodları](#status-codes)
* [Önbellek durumları](#cache-statuses)
* [Bağlantılar](#connections)

## <a name="accessing-real-time-stats"></a>Gerçek zamanlı istatistikler 'e erişme
1. [Azure portalında](https://portal.azure.com), CDN profilinize gidin.
   
    ![CDN profili dikey penceresi](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. CDN profili dikey penceresinde **Yönet** düğmesine tıklayın.
   
    ![CDN profili dikey penceresi Yönet düğmesi](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
3. **Analiz** sekmesinin üzerine gelin ve ardından **gerçek zamanlı istatistikler** açılır öğesi üzerine gelin.  **Http büyük nesne**' ye tıklayın.
   
    ![CDN yönetim portalı](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    Gerçek zamanlı istatistikler grafikleri görüntülenir.

Grafiklerin her biri, sayfa yüklendiğinde başlayarak seçili zaman aralığı için gerçek zamanlı istatistikleri görüntüler.  Grafikler her birkaç saniyede bir otomatik olarak güncelleştirilecek.  Varsa **grafiği Yenile** düğmesi grafiği temizler, sonrasında yalnızca seçili veriler görüntülenir.

## <a name="bandwidth"></a>Bant genişliği
![Bant genişliği grafiği](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Bant genişliği** grafiğinde, seçili zaman aralığında geçerli platform için kullanılan bant genişliği miktarı görüntülenir. Grafiğin gölgeli kısmı bant genişliği kullanımını gösterir. Şu anda kullanılmakta olan bant genişliğinin tam miktarı doğrudan çizgi grafiğinin altında görüntülenir.

## <a name="status-codes"></a>Durum Kodları
![Durum kodu grafiği](./media/cdn-real-time-stats/cdn-status-codes.png)

**Durum kodları** grafiği, belirli http yanıt kodlarının seçili zaman aralığında ne sıklıkla oluştuğunu gösterir.

> [!TIP]
> Her HTTP durum kodu seçeneğinin açıklaması için bkz. [Azure CDN http durum kodları](/previous-versions/azure/mt759238(v=azure.100)).
> 
> 

Grafik üzerinde doğrudan bir HTTP durum kodları listesi görüntülenir. Bu liste, bu durum kodu için çizgi grafiğine dahil edilebilir her durum kodunu ve saniye başına geçerli oluşum sayısını belirtir. Varsayılan olarak, grafikteki bu durum kodlarının her biri için bir satır görüntülenir. Bununla birlikte, yalnızca CDN yapılandırmanız için özel anlam içeren durum kodlarını izlemeyi tercih edebilirsiniz. Bunu yapmak için, istenen durum kodlarını denetleyip diğer tüm seçenekleri temizleyin ve ardından **grafiği Yenile**' ye tıklayın. 

Günlüğe kaydedilen verileri, belirli bir durum kodu için geçici olarak gizleyebilirsiniz.  Göstergenin doğrudan grafiğin altında, gizlemek istediğiniz durum koduna tıklayın. Durum kodu hemen grafikten gizlenir. Bu durum koduna yeniden tıklamak, bu seçeneğin yeniden görüntülenmesine neden olur.

## <a name="cache-statuses"></a>Önbellek durumları
![Önbellek durumları grafiği](./media/cdn-real-time-stats/cdn-cache-status.png)

**Önbellek durumları** grafiği, seçilen zaman dilimi boyunca belirli önbellek durumları türlerinin ne sıklıkla oluştuğunu gösterir. 

> [!TIP]
> Her önbellek durum kodu seçeneğinin açıklaması için bkz. [Azure CDN önbelleği durum kodları](/previous-versions/azure/mt759237(v=azure.100)).
> 
> 

Grafiğin üzerinde doğrudan bir önbellek durum kodları listesi görüntülenir. Bu liste, bu durum kodu için çizgi grafiğine dahil edilebilir her durum kodunu ve saniye başına geçerli oluşum sayısını belirtir. Varsayılan olarak, grafikteki bu durum kodlarının her biri için bir satır görüntülenir. Bununla birlikte, yalnızca CDN yapılandırmanız için özel anlam içeren durum kodlarını izlemeyi tercih edebilirsiniz. Bunu yapmak için, istenen durum kodlarını denetleyip diğer tüm seçenekleri temizleyin ve ardından **grafiği Yenile**' ye tıklayın. 

Günlüğe kaydedilen verileri, belirli bir durum kodu için geçici olarak gizleyebilirsiniz.  Göstergenin doğrudan grafiğin altında, gizlemek istediğiniz durum koduna tıklayın. Durum kodu hemen grafikten gizlenir. Bu durum koduna yeniden tıklamak, bu seçeneğin yeniden görüntülenmesine neden olur.

## <a name="connections"></a>Bağlantılar
![Bağlantılar grafiği](./media/cdn-real-time-stats/cdn-connections.png)

Bu grafik Edge sunucularınızda kaç bağlantı yapıldığını gösterir. Bir varlık için CDN 'ümüzden geçen her istek bir bağlantıyla sonuçlanır.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN gerçek zamanlı uyarılarla](cdn-real-time-alerts.md) bildirim alın
* [GELIŞMIŞ http raporları](cdn-advanced-http-reports.md) ile daha derin bilgi
* [Kullanım düzenlerini](cdn-analyze-usage-patterns.md) çözümleme

