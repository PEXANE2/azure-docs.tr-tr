---
title: Azure Uygulama Öngörüleri ile Azure Tanılama tümleştirmesini kullanarak Azure Bulut Hizmetleri'ndeki sorunlar hakkında uyarı | Microsoft Dokümanlar
description: Azure Uygulama Öngörüleri ile Azure Bulut Hizmetleri'ndeki başlangıç hataları, kilitlenmeler ve rol geri dönüşüm döngüleri gibi sorunları izleme
ms.topic: conceptual
ms.date: 06/07/2018
ms.reviewer: harelbr
ms.openlocfilehash: 997c5e063c4181a597520e60e2a7669401b9677d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669752"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Azure Uygulama Öngörüleri ile Azure tanılama tümleştirmesini kullanarak Azure Bulut Hizmetleri'ndeki sorunlar hakkında uyarı

Bu makalede, Azure Bulut Hizmetleri'ndeki (web ve çalışan rolleri) başlangıç hataları, kilitlenmeler ve rol geri dönüşüm döngüleri gibi sorunları izleyen uyarı kurallarının nasıl ayarlanması gerektiği açıklanmıştır.

Bu makalede açıklanan yöntem, [Uygulama Öngörüleri ile Azure Tanılama tümleştirmesini](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/)ve uygulama istatistikleri için en son yayımlanan [Günlük Uyarıları özelliğine](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) dayanmaktadır.

## <a name="define-a-base-query"></a>Temel sorgu tanımlama

Başlamak için, Windows Azure kanalından Windows Olay Günlüğü olaylarını alan ve Uygulama Öngörüleri'ne izleme kayıtları olarak yakalanan bir temel sorgu tanımlayacağız.
Bu kayıtlar, başlangıç hataları, çalışma zamanı hataları ve geri dönüşüm döngüleri gibi Azure Bulut Hizmetleri'ndeki çeşitli sorunları algılamak için kullanılabilir.

> [!NOTE]
> Aşağıdaki temel sorgu, 30 dakikalık bir zaman penceresindeki sorunları denetler ve telemetri kayıtlarını sindirirken 10 dakikalık bir gecikme varsayar. Bu varsayılanlar uygun gördüğünüz şekilde yapılandırılabilir.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Belirli olay lı ların ayrıntılarını denetleme

Windows Olay Günlüğü olaylarını aldıktan sonra, ilgili olay kimliği ve ileti özelliklerini denetleyerek belirli sorunlar algılanabilir (aşağıdaki örneklere bakın).
Yukarıdaki temel sorguyu aşağıdaki sorgulardan biriyle birleştirmenız yeterlidir ve günlük uyarısı kuralını tanımlarken bu birleştirilmiş sorguyu kullanabilirsiniz.

> [!NOTE]
> Aşağıdaki örneklerde, çözümlenen zaman penceresinde üçten fazla olay bulunursa bir sorun algılanır. Bu varsayılan uyarı kuralının duyarlılığını değiştirmek için yapılandırılabilir.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Uyarı oluşturma

Application Insights kaynağınızdaki gezinti menüsünde **Uyarılar'a**gidin ve ardından **Yeni Uyarı Kuralı'nı**seçin.

![Oluştur kuralının ekran görüntüsü](./media/proactive-cloud-services/001.png)

Oluştur **kuralı** penceresinde, **Uyarı koşulunu tanımla** bölümünde, **ölçüt ekle'yi**tıklatın ve ardından **Özel günlük aramasını**seçin.

![Uyarı için tanım koşul ölçütlerinin ekran görüntüsü](./media/proactive-cloud-services/002.png)

Arama **sorgu** kutusuna, önceki adımda hazırladığınız birleştirilmiş sorguyu yapıştırın.

Ardından, **Eşik** kutusuna devam edin ve değerini 0 olarak ayarlayın. İsteğe bağlı olarak **Dönem** ve Frekans **alanlarını**değiştirebilirsiniz.
**Bitti**’ye tıklayın.

![Yapılandırma sinyali mantığı sorgusunun ekran görüntüsü](./media/proactive-cloud-services/003.png)

Uyarı **ayrıntılarını tanımla** bölümünde, uyarı kuralına bir **Ad** ve **Açıklama** sağlayın ve **Önem Derecesini**ayarlayın.
Ayrıca, **oluşturma üzerine Etkinleştir kuralının** **Evet**olarak ayarlandıkından emin olun.

![Ekran görüntüsü uyarı ayrıntıları](./media/proactive-cloud-services/004.png)

Eylem **grubu tanımla** bölümünde, varolan bir Eylem grubu seçebilir veya yeni bir eylem **oluşturabilirsiniz.**
Eylem grubunun çeşitli türlerde birden çok eylem içermesi için seçebilirsiniz.

![Ekran görüntüsü eylem grubu](./media/proactive-cloud-services/005.png)

Eylem grubunu tanımladıktan sonra, değişikliklerinizi onaylayın ve **uyarı kuralını oluştur'u**tıklatın.

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

[Arıza anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md)
[Bellek Sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
[Performans anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md)

