---
title: Azure Uygulama Öngörüleri'nde kullanıcı, oturum ve olay analizi
description: Web uygulamanızın kullanıcılarının demografik analizi.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 56059304026b060e2215ce73e0e94e3200573a14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670993"
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Uygulama Öngörüleri'nde kullanıcılar, oturumlar ve olay analizi

Kullanıcıların web uygulamanızı ne zaman kullandığını, en çok hangi sayfaları ilgilendiklerini, kullanıcılarınızın nerede bulunduğunu ve hangi tarayıcıları ve işletim sistemlerini kullandıklarını öğrenin. [Azure Uygulama Öngörüleri'ni](../../azure-monitor/app/app-insights-overview.md)kullanarak iş ve kullanım telemetrisini analiz edin.

![Uygulama Öngörüleri Kullanıcılarının Ekran Görüntüsü](./media/usage-segmentation/0001-users.png)

## <a name="get-started"></a>Kullanmaya başlayın

Application Insights portalındaki kullanıcılar, oturumlar veya olay bıçaklarında henüz veri görmüyorsanız, [kullanım araçlarıyla nasıl başlayın.](usage-overview.md)

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Kullanıcılar, Oturumlar ve Etkinlikler segmentasyon aracı

Kullanım bıçaklarından üçü, web uygulamanızdan üç perspektiften telemetriyi dilimlemek ve zarlamak için aynı aracı kullanır. Verileri filtreleyerek ve bölerek, farklı sayfaların ve özelliklerin göreli kullanımıyla ilgili öngörüleri ortaya çıkarabilirsiniz.

* **Kullanıcı aracı**: Uygulamanızı ve özelliklerini kaç kişinin kullandığı.  Kullanıcılar tarayıcı tanımlama bilgilerinde depolanan anonim kimlikler kullanılarak sayılır. Farklı tarayıcılar veya makineler kullanan tek bir kişi birden fazla kullanıcı olarak sayılır.
* **Oturumlar aracı**: Uygulamanızın belirli sayfalarını ve özelliklerini kaç oturumda kullanıcı etkinliği dahil etti. Bir oturum, yarım saatlik kullanıcı etkinligeçtikten veya 24 saatlik sürekli kullanımdan sonra sayılır.
* **Etkinlikler aracı**: Uygulamanızın belirli sayfalarının ve özelliklerinin ne sıklıkta kullanıldığıdır. Bir tarayıcı uygulamanızdan bir sayfa yüklerse, sayfa görünümü [sayılır.](../../azure-monitor/app/javascript.md) 

    Özel bir olay, uygulamanızda meydana gelen bir şeyin, genellikle bir düğme tıklaması veya bazı görevin tamamlanması gibi bir kullanıcı etkileşiminin meydana gelişini temsil eder. [Özel olaylar oluşturmak](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)için uygulamanıza kod eklersiniz.

## <a name="querying-for-certain-users"></a>Belirli kullanıcılar için sorgulama

Kullanıcılar aracının üst kısmındaki sorgu seçeneklerini ayarlayarak farklı kullanıcı gruplarını keşfedin:

* Göster: Çözümlemek için bir kullanıcı kohortu seçin.
* Kimler kullanılır: Özel olayları ve sayfa görünümlerini seçin.
* Sırasında: Bir zaman aralığı seçin.
* By: Belirli bir süre veya tarayıcı veya şehir gibi başka bir özellik tarafından veri kova nasıl seçin.
* Bölme: Verileri bölmek veya bölmek için bir özellik seçin. 
* Filtreler Ekle: Sorguyu tarayıcı veya şehir gibi özelliklerine göre belirli kullanıcılarla, oturumlarla veya olaylarla sınırlandırın. 
 
## <a name="saving-and-sharing-reports"></a>Raporları kaydetme ve paylaşma 
Raporlarım bölümünde yalnızca size özel olarak veya Paylaşılan Raporlar bölümünde bu Uygulama Öngörüleri kaynağına erişimi olan herkesle paylaşılan Kullanıcı raporlarını kaydedebilirsiniz.

Bir Kullanıcı, Oturum veya Etkinlik raporuna bağlantı paylaşmak için; araç çubuğunda **Paylaş'ı** tıklatın ve ardından bağlantıyı kopyalayın.

Verilerin bir kopyasını Kullanıcılar, Oturumlar veya Etkinlikler raporunda paylaşmak için; araç çubuğunda **Paylaş'ı** tıklatın ve ardından verilerle birlikte bir Word belgesi oluşturmak için **Word simgesini** tıklatın. Veya, ana grafiğin üzerindeki **Word simgesini** tıklatın.

## <a name="meet-your-users"></a>Kullanıcılarınızla tanışın

**Kullanıcılarınızın Karşıla** bölümü, geçerli sorguyla eşleşen beş örnek kullanıcı hakkındaki bilgileri gösterir. Bireylerin davranışlarını göz önünde bulundurmak ve araştırmak, agregalara ek olarak, insanların uygulamanızı gerçekte nasıl kullandıkları hakkında öngörüler sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için [özel etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views)göndermeye başlayın.
- Zaten özel etkinlikler veya sayfa görünümleri gönderiyorsanız, kullanıcıların hizmetinizi nasıl kullandığını öğrenmek için Kullanım araçlarını keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](usage-retention.md)
    - [Kullanıcı Akışları](usage-flows.md)
    - [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
    - [Kullanıcı bağlamı ekleme](usage-send-user-context.md)
