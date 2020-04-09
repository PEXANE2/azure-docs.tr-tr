---
title: Azure Application Insights ile web uygulaması kullanıcı tutma analizi
description: Uygulamanıza kaç kullanıcı geri döner?
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 05/03/2017
ms.reviewer: mbullwin
ms.openlocfilehash: e6d9be6bc6d7f90153e2fb58aa404e281568dbdd
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892419"
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Application Insights ile web uygulamaları için kullanıcı saklama analizi

Azure Uygulama [Öngörüleri'ndeki](../../azure-monitor/app/app-insights-overview.md) bekletme özelliği, kaç kullanıcının uygulamanıza geri döndüğünü ve belirli görevleri ne sıklıkta gerçekleştirdiklerini veya hedeflere ulaşmalarını analiz etmenize yardımcı olur. Örneğin, bir oyun sitesi çalıştırAcaksanız, bir oyunu kaybettikten sonra siteye dönen kullanıcı sayısını kazandıktan sonra geri dönen kullanıcı sayısıyla karşılaştırabilirsiniz. Bu bilgi, hem kullanıcı deneyiminizi hem de iş stratejinizi geliştirmenize yardımcı olabilir.

## <a name="get-started"></a>başlarken

Uygulama Öngörüleri portalındaki bekletme aracındaki verileri henüz göremiyorsanız, [kullanım araçlarıyla nasıl başlayın](usage-overview.md)gerektiğini öğrenin.

## <a name="the-retention-tool"></a>Bekletme aracı

![Elde tutma aracı](./media/usage-retention/retention.png)

1. Araç çubuğu, kullanıcıların yeni bekletme raporları oluşturmasına, varolan bekletme raporlarını açmasına, geçerli bekletme raporunu kaydetmesine veya kaydedilmiş olarak kaydetmesine, kaydedilen raporlarda yapılan değişiklikleri geri çevirmesine, rapordaki verileri yenilemesine, e-posta veya doğrudan bağlantı yoluyla raporu paylaşmasına ve dokümantasyon sayfasına erişmesine olanak tanır. 
2. Varsayılan olarak, bekletme, bir şey yapan tüm kullanıcıların geri geldiğini ve bir süre içinde başka bir şey yaptığını gösterir. Belirli kullanıcı etkinliklerine odaklanmayı daraltmak için farklı etkinlik bileşimi seçebilirsiniz.
3. Özelliklere bir veya daha fazla filtre ekleyin. Örneğin, belirli bir ülke veya bölgedeki kullanıcılara odaklanabilirsiniz. Filtreleri ayarladıktan sonra **Güncelleştir'i** tıklatın. 
4. Genel bekletme grafiği, seçilen zaman dilimi boyunca kullanıcı bekletme sinin bir özetini gösterir. 
5. Izgara, #2 sorgu oluşturucuya göre tutulan kullanıcı sayısını gösterir. Her satır, gösterilen zaman diliminde herhangi bir olayı gerçekleştiren bir kullanıcı kohortunun temsil ini temsil eder. Satırdaki her hücre, bu kohorttan kaç tanesinin daha sonraki bir dönemde en az bir kez geri döndüğünü gösterir. Bazı kullanıcılar birden fazla dönemde geri dönebilir. 
6. Öngörüler kartları, kullanıcılara bekletme raporlarını daha iyi anlamalarını sağlamak için ilk beş başlatan olayı ve iade edilen ilk beş olayı gösterir. 

![Bekletme faresi gezinme](./media/usage-retention/hover.png)

Kullanıcılar, hücrenin ne anlama geldiğini açıklayan analitik düğmesine ve araç ipuçlarına erişmek için bekletme aracındaki hücrelerin üzerinde gezinebilir. Analytics düğmesi, kullanıcıları hücreden kullanıcı oluşturmak için önceden doldurulmuş bir sorguyla Analytics aracına götürür. 

## <a name="use-business-events-to-track-retention"></a>Bekletme izleme için iş etkinliklerini kullanma

En yararlı bekletme çözümlemesi almak için, önemli iş etkinliklerini temsil eden olayları ölçün. 

Örneğin, birçok kullanıcı uygulamanızda görüntülenebilen oyunu oynamadan bir sayfa açabilir. Bu nedenle, sadece sayfa görünümlerini izlemek, daha önce oyunkeyfini çıkardıktan sonra kaç kişinin oyunu oynamak için geri döndüğünün yanlış bir tahminini sağlar. Geri dönen oyuncular hakkında net bir resim elde etmek için, uygulamanız bir kullanıcı gerçekten oynattığında özel bir etkinlik göndermelidir.  

Önemli iş eylemlerini temsil eden özel olayları kodlamak ve bunları bekletme çözümlemesiniz için kullanmak iyi bir uygulamadır. Oyun sonucunu yakalamak için, Application Insights'a özel bir etkinlik göndermek için bir kod satırı yazmanız gerekir. Web sayfası koduna veya Node.JS'ye yazarsanız, aşağıdaki gibi görünür:

```JavaScript
    appinsights.trackEvent("won game");
```

Veya ASP.NET sunucu kodu:

```csharp
   telemetry.TrackEvent("won game");
```

[Özel etkinlikler yazma hakkında daha fazla bilgi edinin.](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)


## <a name="next-steps"></a>Sonraki adımlar
- Kullanım deneyimlerini etkinleştirmek için [özel etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views)göndermeye başlayın.
- Zaten özel etkinlikler veya sayfa görünümleri gönderiyorsanız, kullanıcıların hizmetinizi nasıl kullandığını öğrenmek için Kullanım araçlarını keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Kullanıcı Akışları](usage-flows.md)
    - [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
    - [Kullanıcı bağlamı ekleme](usage-send-user-context.md)


