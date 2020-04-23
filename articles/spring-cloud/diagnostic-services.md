---
title: Azure Bulutu'ndaki günlükleri ve ölçümleri analiz edin | Microsoft Dokümanlar
description: Azure Bahar Bulutu'nda tanılama verilerini nasıl analiz edebilirsiniz öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: brendm
ms.openlocfilehash: adbcf28cfbbe2ea3b7cc9c7fd0d1c76246938344
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870401"
---
# <a name="analyze-logs-and-metrics-with-diagnostics-settings"></a>Günlükleri ve ölçümleri tanılama ayarlarıyla analiz edin

Azure Bahar Bulutu'nun tanılama işlevini kullanarak günlükleri ve ölçümleri aşağıdaki hizmetlerden herhangi biriyle analiz edebilirsiniz:

* Verilerin Azure Depolama'ya yazıldığı Azure Günlük Analizi'ni kullanın. Günlükleri Log Analytics'e dışa aktarırken bir gecikme vardır.
* Denetim veya el ile denetim için günlükleri bir depolama hesabına kaydedin. Bekletme süresini (gün içinde) belirtebilirsiniz.
* Üçüncü taraf bir hizmet veya özel analiz çözümü tarafından yutulmak için etkinlik merkezinize günlük akışı akışı.

İzlemek istediğiniz günlük kategorisini ve metrik kategorisini seçin.

> [!TIP]
> Sadece günlüklerinizi akış istiyorum? Bu [Azure CLI komutuna](https://docs.microsoft.com/cli/azure/ext/spring-cloud/spring-cloud/app?view=azure-cli-latest#ext-spring-cloud-az-spring-cloud-app-logs)göz atın!

## <a name="logs"></a>Günlükler

|Günlük | Açıklama |
|----|----|
| **Uygulama Konsolu** | Tüm müşteri uygulamalarının konsol günlüğü. | 
| **SystemLogs** | Şu anda, bu kategoride yalnızca [Spring Cloud Config Server](https://cloud.spring.io/spring-cloud-config/reference/html/#_spring_cloud_config_server) günlükleri. |

## <a name="metrics"></a>Ölçümler

Ölçümlerin tam listesi için [Bkz. Bahar Bulut Uyrcusu Ölçümleri.](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-concept-metrics#user-metrics-options)

Başlamak için, bu hizmetlerden birinin verileri almasını etkinleştirin. Günlük Analitiği'ni yapılandırma hakkında bilgi edinmek için Azure [Monitor'da Log Analytics ile başlayın](../azure-monitor/log-query/get-started-portal.md)bölümüne bakın. 

## <a name="configure-diagnostics-settings"></a>Tanılama ayarlarını yapılandırma

1. Azure portalında Azure İlkbahar Bulutu örneğinize gidin.
1. **Tanılama ayarları** seçeneğini seçin ve ardından **Tanılama Ayarını Ekle'yi**seçin.
1. Ayar için bir ad girin ve ardından günlükleri göndermek istediğiniz yeri seçin. Aşağıdaki üç seçeneğin herhangi bir birleşimini seçebilirsiniz:
    * **Depolama hesabına arşivleme**
    * **Olay merkezine akış**
    * **Log Analytics’e gönderme**

1. İzlemek istediğiniz günlük kategorisini ve metrik kategorisini seçin ve ardından bekletme süresini (gün içinde) belirtin. Bekletme süresi yalnızca depolama hesabı için geçerlidir.
1. **Kaydet**’i seçin.

> [!NOTE]
> Günlüklerin veya ölçümlerin yayıldığında ve depolama hesabınızda, etkinlik merkezinizde veya Log Analytics'te göründükleri zaman arasında 15 dakikaya kadar bir boşluk olabilir.

## <a name="view-the-logs-and-metrics"></a>Günlükleri ve ölçümleri görüntüleme
Aşağıdaki başlıklar altında açıklandığı gibi günlükleri ve ölçümleri görüntülemek için çeşitli yöntemler vardır.

### <a name="use-logs-blade"></a>Günlükleri blade kullanın

1. Azure portalında Azure İlkbahar Bulutu örneğinize gidin.
1. **Günlük Arama** bölmesini açmak için **Günlükler'i**seçin.
1. **Günlük** arama kutusunda
   * Günlükleri görüntülemek için, şu gibi basit bir sorgu girin:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```
   * Ölçümleri görüntülemek için, şu gibi basit bir sorgu girin:

    ```sql
    AzureMetrics
    | limit 50
    ```
1. Arama sonucunu görüntülemek için **Çalıştır'ı**seçin.

### <a name="use-log-analytics"></a>Log Analytics kullanma

1. Azure portalında, sol bölmede **Günlük Analizi'ni**seçin.
1. Tanılama ayarlarınızı eklediğinizde seçtiğiniz Log Analytics çalışma alanını seçin.
1. **Günlük Arama** bölmesini açmak için **Günlükler'i**seçin.
1. **Günlük** arama kutusunda,
   * günlükleri görüntülemek için, gibi basit bir sorgu girin:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```
    * ölçümleri görüntülemek için, gibi basit bir sorgu girin:

    ```sql
    AzureMetrics
    | limit 50
    ```

1. Arama sonucunu görüntülemek için **Çalıştır'ı**seçin.
1. Bir filtre koşulu ayarlayarak belirli bir uygulama nın veya örneğin günlüklerinde arama yapabilirsiniz:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```
> [!NOTE]  
> `==`büyük/küçük harf `=~` duyarlıdır, ancak değildir.

Log Analytics'te kullanılan sorgu dili hakkında daha fazla bilgi edinmek için [Azure Monitor günlük sorgularına](../azure-monitor/log-query/query-language.md)bakın.

### <a name="use-your-storage-account"></a>Depolama hesabınızı kullanma 

1. Azure portalında, sol bölmede **Depolama hesapları'nı**seçin.

1. Tanılama ayarlarınızı eklediğinizde seçtiğiniz depolama hesabını seçin.
1. **Blob Container** bölmesini açmak için **Blobs'u**seçin.
1. Uygulama günlüklerini incelemek **için, insights-logs-applicationconsole**adı verilen bir kapsayıcı arayın.
1. Uygulama ölçümlerini gözden geçirmek **için, öngörüler-ölçümler-pt1m**adı verilen bir kapsayıcı arayın.

Tanılama bilgilerini bir depolama hesabına gönderme hakkında daha fazla bilgi edinmek için Azure [Depolama'da depolama ve tanılama verilerini görüntüleyin.](../storage/common/storage-introduction.md)

### <a name="use-your-event-hub"></a>Etkinlik merkezinizi kullanma

1. Azure portalında, sol bölmede **Olay Hub'ları'nı**seçin.

1. Tanılama ayarlarınızı eklediğinizde seçtiğiniz etkinlik merkezini arayın ve seçin.
1. **Olay Hub Listesi** bölmesini açmak için Olay **Hub'ları'nı**seçin.
1. Uygulama günlüklerini incelemek **için, insights-logs-applicationconsole**adlı bir olay hub'ı arayın.
1. Uygulama ölçümlerini gözden geçirmek **için, öngörüler-ölçümler-pt1m**adı verilen bir olay hub'ı arayın.

Bir olay hub'ına tanılama bilgileri gönderme hakkında daha fazla bilgi edinmek [için, Olay Hub'larını kullanarak Azure Tanılama verilerini sıcak yolda akışı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs)konusuna bakın.

## <a name="analyze-the-logs"></a>Günlükleri analiz edin

Azure Log Analytics, analiz için günlüklerinizi sorgulayabilmeniz için bir Kusto motoruyla çalışıyor. Kusto'yu kullanarak günlükleri sorgulamaya hızlı bir giriş için [Log Analytics öğreticisini](../azure-monitor/log-query/get-started-portal.md)inceleyin.

Uygulama günlükleri, uygulamanızın durumu, performansı ve daha fazlası hakkında kritik bilgiler ve ayrıntılı günlükler sağlar. Sonraki bölümlerde, uygulamanızın geçerli ve geçmiş durumlarını anlamanıza yardımcı olacak bazı basit sorgular verilmiştir.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Azure Bahar Bulutu'ndan uygulama günlüklerini göster

Azure Bahar Bulutu'ndan gelen ve önce gösterilen en son günlüklerle zamana göre sıralanmış uygulama günlüklerinin listesini gözden geçirmek için aşağıdaki sorguyu çalıştırın:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Hata veya özel durum içeren günlükgirişlerini göster

Bir hata veya özel durum söz sıralanmamış günlük girişleri gözden geçirmek için aşağıdaki sorguyu çalıştırın:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Hataları bulmak veya belirli hata kodlarını veya özel durumları bulmak için sorgu terimlerini değiştirmek için bu sorguyu kullanın. 

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Son saat içinde uygulamanız tarafından bildirilen hata ve özel durum sayısını göster

Son bir saat içinde uygulamanız tarafından günlüğe kaydedilen hata ve özel durum sayısını görüntüleyen bir pasta grafiği oluşturmak için aşağıdaki sorguyu çalıştırın:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Uygulama günlüklerini sorgulama hakkında daha fazla bilgi edinin

Azure Monitor, Log Analytics'i kullanarak uygulama günlüklerini sorgulamak için kapsamlı destek sağlar. Bu hizmet hakkında daha fazla bilgi edinmek için Azure [Monitor'daki günlük sorgularına başlayın' a](../azure-monitor/log-query/get-started-queries.md)bakın. Uygulama günlüklerinizi analiz etmek için sorgu oluşturma hakkında daha fazla bilgi için Azure [Monitor'daki günlük sorgularına genel bakış](../azure-monitor/log-query/log-query-overview.md)bölümüne bakın.
