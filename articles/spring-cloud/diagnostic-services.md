---
title: Azure yay bulutu 'nda günlükleri ve ölçümleri çözümleme | Microsoft Docs
description: Azure Spring Cloud 'da tanılama verilerini çözümlemeyi öğrenin
services: spring-cloud
author: jpconnock
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: e6b90fb09c536f68bee7fd5d57507fe3920bcf1e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038892"
---
# <a name="analyze-logs-and-metrics-with-diagnostic-settings"></a>Tanılama ayarlarıyla günlükleri ve ölçümleri çözümleme

Azure Spring Cloud 'ın tanılama işlevselliği, aşağıdaki hizmetlerden birini kullanarak günlükleri ve ölçümleri analiz etmenizi sağlar:

* Verileri, verileri depolama alanına yazmaya gerek olmadan Azure Log Analytics anında yazıldığı Azure Log Analytics ile çözümleyin.
* Denetim veya el ile inceleme için bunları bir depolama hesabına kaydedin. Saklama süresini (gün cinsinden) belirtebilirsiniz.
* Bunları bir üçüncü taraf hizmeti veya özel analiz çözümünün alımı için Event Hubs akışa ayırın.

Başlamak için, bu hizmetlerden birini verileri almak üzere etkinleştirmeniz gerekir.  Log Analytics yapılandırma hakkında daha fazla bilgi edinmek için [Bu öğreticiyi](../azure-monitor/log-query/get-started-portal.md)inceleyin.  

## <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

1. Azure portal Azure Spring Cloud örneğinize gidin.
1. **Tanılama ayarları** menü seçeneğini belirleyin.
1. **Tanılama ayarı Ekle** düğmesini seçin.
1. Ayar için bir ad girin ve günlükleri nereye göndermek istediğinizi seçin. Aşağıdaki üç seçenekten herhangi bir birleşimini seçebilirsiniz:
    * Bir depolama hesabına Arşivle
    * Bir olay hub 'ına akış
    * Log Analytics’e gönderme

1. İzlemek istediğiniz günlük kategorisini ve ölçüm kategorisini seçin ve bekletme süresini (gün cinsinden) belirtin. Saklama süresi yalnızca depolama hesabı için geçerlidir.
1. Ayarı uygulamak için **Kaydet** ' i seçin.

> [!NOTE]
> Günlüklerin veya ölçümlerin ne zaman yayıldığını ve depolama hesabında/Olay Hub 'ında/Log Analytics görüntiklerinde 15 dakika kadar sürebilir.

## <a name="viewing-logs"></a>Günlükleri görüntüleme

### <a name="using-log-analytics"></a>Log Analytics kullanma

1. Azure portal sol taraftaki gezinti menüsünden Log Analytics ' i seçin.
1. Tanılama ayarlarını eklerken seçtiğiniz Log Analytics çalışma alanını seçin.
1. Günlük araması dikey penceresini açmak için `Logs` ' ı seçin.
1. Günlük arama kutusuna basit bir sorgu girin.  Örnek:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```

1. Arama sonucunu görmek için `Run` ' ı seçin.
1. Filtre koşulunu ayarlayarak belirli bir uygulamanın veya örneğin günlüklerinde arama yapabilirsiniz:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```

[Bu makaledeki](../azure-monitor/log-query/query-language.md) Log Analytics ' de kullanılan sorgu dili hakkında daha fazla bilgi edinin

### <a name="using-logs-and-metrics-in-storage-account"></a>Depolama hesabında günlüklerin ve ölçümlerin kullanılması

1. Azure portal, sol taraftaki gezinti menüsünden depolama hesapları ' nı seçin.
1. Tanılama ayarlarını eklerken seçtiğiniz depolama hesabını seçin.
1. Blob kapsayıcısı dikey penceresini açmak için `Blobs` girişi seçin.
1. Uygulama günlüklerini gözden geçirmek için `insights-logs-applicationconsole` adlı bir kapsayıcı bulun.
1. Uygulama ölçümlerini gözden geçirmek için `insights-metrics-pt1m` adlı bir kapsayıcı bulun.

[Tanılama bilgilerini bir depolama hesabına gönderme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-to-storage)

### <a name="using-event-hubs"></a>Event Hubs kullanma

1. Azure portal sol taraftaki gezinti menüsünden Event Hubs ' i seçin.
1. Tanılama ayarlarını eklerken seçtiğiniz Event Hubs bulun ve seçin.
1. Olay Hub 'ı liste dikey penceresini açmak için `Event Hubs` ' ı seçin.
1. Uygulama günlüklerini gözden geçirmek için `insights-logs-applicationconsole` adlı bir olay hub 'ı bulun.
1. Uygulama ölçümlerini gözden geçirmek için `insights-metrics-pt1m` adlı bir olay hub 'ı bulun.

[Tanılama bilgilerini bir olay hub 'ına gönderme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs)

## <a name="analyzing-logs"></a>Günlükler çözümleniyor

Azure Log Analytics, kusto sağlamak için günlüklerinizi analiz etmek üzere sorgulamanızı sağlar.  Kusto kullanarak günlükleri sorgulamaya yönelik hızlı bir giriş için [Log Analytics öğreticisini](../azure-monitor/log-query/get-started-portal.md) gözden geçirin.

Uygulama günlükleri, uygulamanızın sistem durumu, performansı ve daha fazlası hakkında önemli bilgiler sağlar.  Aşağıda, uygulamanızın geçerli ve geçmiş durumlarını anlamanıza yardımcı olacak bazı basit sorgular verilmiştir.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Azure Spring Cloud 'dan uygulama günlüklerini göster

Azure Spring Cloud 'dan uygulama günlüklerinin listesini gözden geçirmek için ilk olarak gösterilen en son günlüklere göre sıralanır:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Hata veya özel durum içeren günlük girişlerini göster

Bu sorgu bir hata veya özel durumdan bahsetme günlük girişlerini incelemenizi sağlar.  Sonuçlar sıralanmaz.

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Hataları bulmak için bu sorguyu kullanın veya belirli hata kodlarını veya özel durumları bulmak için sorgu terimlerini değiştirin.  

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Son bir saat içinde uygulamanız tarafından bildirilen hata ve özel durum sayısını göster

Bu sorgu, son bir saat içinde uygulamanız tarafından günlüğe kaydedilen hata sayısını ve özel durumları gösteren bir pasta grafiği oluşturur:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Uygulama günlüklerini sorgulama hakkında daha fazla bilgi edinin

Azure Izleyici, Log Analytics kullanarak uygulama günlüklerini sorgulamak için kapsamlı destek sağlar.  Bu hizmet hakkında daha fazla bilgi edinmek için Azure Izleyici 'yi kullanarak [günlük sorgularının](../azure-monitor/log-query/get-started-queries.md) öğreticisini gözden geçirin. [Azure izleyici 'de günlük sorgularına genel bakış](../azure-monitor/log-query/log-query-overview.md) , uygulama günlüklerinizi çözümlemek için sorgu oluşturma hakkında daha fazla bilgi sağlar.
