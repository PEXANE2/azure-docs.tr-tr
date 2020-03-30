---
title: Azure Uygulama Öngörülerinde Akıllı Algılama | Microsoft Dokümanlar
description: Application Insights, uygulama telemetrinizin otomatik derin analizini yapar ve sizi olası sorunlar konusunda uyarır.
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: e232295f9da2a2ae8f3c6fafdd1dc33a42e92e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671809"
---
# <a name="smart-detection-in-application-insights"></a>Application Insights'da Akıllı Algılama
 Akıllı Algılama, web uygulamanızdaki olası performans sorunları ve arıza anormallikleri konusunda sizi otomatik olarak uyarır. Uygulamanızın [Uygulama Öngörüleri'ne](../../azure-monitor/app/app-insights-overview.md)gönderdiği telemetrinin proaktif analizini gerçekleştirir. Hata oranlarında ani bir artış veya istemci veya sunucu performansında anormal desenler varsa, bir uyarı alırsınız. Bu özellik yapılandırma gerektirmez. Uygulamanız yeterli telemetri gönderirse çalışır.

Smart Detection tarafından verilen algılamalara hem aldığınız e-postalardan hem de Smart Detection blade'den erişebilirsiniz.

## <a name="review-your-smart-detections"></a>Akıllı Algılamalarınızı Gözden Geçirin
Algılamaları iki şekilde keşfedebilirsiniz:

* Application Insights'tan **bir e-posta alırsınız.** Aşağıda tipik bir örnek verilmiştir:
  
    ![E-posta uyarısı](./media/proactive-diagnostics/03.png)
  
    Portalda daha fazla ayrıntı açmak için büyük düğmeye tıklayın.
* Uygulama Öngörülerinde **Akıllı Algılama bıçağı.** Son algılamaların listesini görmek için **Araştır** menüsü altında **Akıllı Algılama'yı** seçin.

![En son algılamaları görüntüleme](./media/proactive-diagnostics/04.png)

Ayrıntılarını görmek için bir algılama seçin.

## <a name="what-problems-are-detected"></a>Hangi sorunlar algılanır?
Akıllı Algılama, şu gibi çeşitli sorunları algılar ve bu konuda ilerler:

* [Akıllı algılama - Arıza Anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md). Uygulamanız için beklenen başarısız istek oranını ayarlamak için makine öğrenimini kullanırız, yük ve diğer faktörlerle ilişkilidir. Arıza oranı beklenen zarfın dışına çıkarsa, bir uyarı göndeririz.
* [Akıllı algılama - Performans Anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md). Bir işlemin yanıt süresi veya bağımlılık süresi geçmiş taban çizgisine göre yavaşlıyorsa veya yanıt süresi nde veya sayfa yükleme süresinde anormal bir desen tanımlıyorsak bildirimler alırsınız.   
* Genel bozulmalar ve sorunlar, [Trace degredation](https://docs.microsoft.com/azure/azure-monitor/app/proactive-trace-severity)gibi , [Bellek sızıntısı](https://docs.microsoft.com/azure/azure-monitor/app/proactive-potential-memory-leak), Özel Durum hacmi ve [Güvenlik anti-desenlera](https://docs.microsoft.com/azure/azure-monitor/app/proactive-application-security-detection-pack) [anormal artış](https://docs.microsoft.com/azure/azure-monitor/app/proactive-exception-volume) .

(Her bildirimdeki yardım bağlantıları sizi ilgili makalelere götürür.)

## <a name="smart-detection-email-notifications"></a>Akıllı Algılama e-posta bildirimleri

_Önizleme_olarak işaretlenmiş kurallar dışında tüm Akıllı Algılama kuralları, algılamalar bulunduğunda e-posta bildirimleri göndermek için varsayılan olarak yapılandırılır.

Belirli bir Akıllı Algılama kuralı için e-posta bildirimlerinin yapılandırılması, Akıllı Algılama **Ayarları** bıçağını açarak ve **Düzenleme kuralını** açacak kuralı seçerek yapılabilir.

Alternatif olarak, Azure Kaynak Yöneticisi şablonlarını kullanarak yapılandırmayı değiştirebilirsiniz. Daha fazla ayrıntı için [Azure Kaynak Yöneticisi şablonlarını kullanarak Uygulama Öngörülerini Yönet akıllı algılama kurallarına bakın.](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Bu tanı araçları, uygulamanızdaki telemetriyi incelemenize yardımcı olur:

* [Metrik kaşif](../../azure-monitor/app/metrics-explorer.md)
* [Arama gezgini](../../azure-monitor/app/diagnostic-search.md)
* [Analitik - güçlü sorgu dili](../../azure-monitor/log-query/get-started-portal.md)

Akıllı Algılama tamamen otomatiktir. Ama belki biraz daha uyarı kurmak istersin?

* [El ile yapılandırılan metrik uyarılar](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md) 

