---
title: Azure Izleyici günlük sorgularıyla denetim sorguları
description: Azure Izleyici 'de çalıştırılan günlük sorguları hakkında telemetri sağlayan günlük sorgusu denetim günlüklerinin ayrıntıları.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/03/2020
ms.openlocfilehash: bfaa9d8908d9401441d8811c3edcd087781b1d89
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89458646"
---
# <a name="audit-queries-in-azure-monitor-logs-preview"></a>Azure Izleyici günlüklerinde denetim sorguları (Önizleme)
Günlük sorgusu denetim günlükleri Azure Izleyici 'de çalıştırılan günlük sorguları hakkında telemetri sağlar. Bu, bir sorgunun ne zaman çalıştırıldığı, onu kimin çalıştırdığını, hangi aracın kullanıldığını, sorgu metnini ve sorgu yürütmesini açıklayan performans istatistiklerini içerir.


## <a name="configure-query-auditing"></a>Sorgu denetimini yapılandırma
Sorgu denetimi, Log Analytics çalışma alanında bir [Tanılama ayarı](../platform/diagnostic-settings.md) ile etkinleştirilir. Bu, denetim verilerini geçerli çalışma alanına ya da aboneliğinizdeki başka bir çalışma alanına, Azure Event Hubs Azure 'un dışına veya arşivleme için Azure Storage 'a göndermenizi sağlar. 

### <a name="azure-portal"></a>Azure portal
Aşağıdaki konumlardan birindeki Azure portal bir Log Analytics çalışma alanı için tanılama ayarına erişin:

- **Azure izleyici** menüsünde, **Tanılama ayarları**' nı seçin ve sonra çalışma alanını bulun ve seçin.

    [![Tanılama ayarları Azure Izleyici ](media/log-query-audit/diagnostic-setting-monitor.png)](media/log-query-audit/diagnostic-setting-monitor.png#lightbox) 

- **Log Analytics çalışma alanları** menüsünde, çalışma alanını seçin ve ardından **Tanılama ayarları**' nı seçin.

    [![Tanılama ayarları Log Analytics çalışma alanı ](media/log-query-audit/diagnostic-setting-workspace.png)](media/log-query-audit/diagnostic-setting-workspace.png#lightbox) 

### <a name="resource-manager-template"></a>Resource Manager şablonu
[Log Analytics çalışma alanı Için tanılama ayarından](../samples/resource-manager-diagnostic-settings.md#diagnostic-setting-for-log-analytics-workspace)örnek kaynak yöneticisi şablonu alabilirsiniz.

## <a name="audit-data"></a>Denetim verileri
Bir sorgu her çalıştırıldığında bir denetim kaydı oluşturulur. Verileri bir Log Analytics çalışma alanına gönderiyorsanız, bu, *Laquerylogs*adlı bir tabloda depolanır. Aşağıdaki tabloda denetim verilerinin her kaydındaki özellikler açıklanmaktadır.

| Alan | Açıklama |
|:---|:---|
| TimeGenerated         | Sorgunun gönderildiği UTC saati. |
| CorrelationId         | Sorguyu tanımlamak için benzersiz KIMLIK. , Yardım için Microsoft 'a bağlanırken sorun giderme senaryolarında kullanılabilir. |
| Aadobjectıd           | Sorguyu başlatan kullanıcı hesabının KIMLIĞI Azure Active Directory.  |
| Aadtenantıd           | Sorguyu başlatan kullanıcı hesabının kiracının KIMLIĞI.  |
| AADEmail              | Sorguyu başlatan kullanıcı hesabının kiracının e-postası.  |
| Aadclientıd           | Sorguyu başlatmak için kullanılan uygulamanın KIMLIĞI ve çözümlenmiş adı. |
| RequestClientApp      | Sorguyu başlatmak için kullanılan uygulamanın çözümlenmiş adı. |
| QueryTimeRangeStart   | Sorgu için seçilen zaman aralığının başlangıcı. Bu, sorgunun Log Analytics başladığı ve zaman aralığının saat seçici yerine sorgu içinde belirtildiği gibi belirli senaryolarda doldurulmayabilir. |
| QueryTimeRangeEnd     | Sorgu için seçilen zaman aralığının sonu. Bu, sorgunun Log Analytics başladığı ve zaman aralığının saat seçici yerine sorgu içinde belirtildiği gibi belirli senaryolarda doldurulmayabilir.  |
| QueryText             | Çalıştırılan sorgunun metni. |
| RequestTarget         | Sorguyu göndermek için API URL 'SI kullanıldı.  |
| Öğesinden        | Sorgunun çalıştırmak istediği kaynakların listesi. En fazla üç dize dizisi içerir: çalışma alanları, uygulamalar ve kaynaklar. Abonelik veya kaynak grubu ile hedeflenen sorgular, *kaynak*olarak gösterilir. RequestTarget tarafından kapsanan hedefi içerir.<br>Her kaynağın kaynak KIMLIĞI, çözümlenebiliyorsa dahil edilir. Kaynağa erişilirken bir hata döndürülürse bu durum çözülebilir. Bu durumda, sorgudaki belirli metin kullanılacaktır.<br>Sorgu, birden fazla aboneliğe sahip olan bir çalışma alanı adı gibi belirsiz bir ad kullanıyorsa, bu belirsiz ad kullanılacaktır. |
| RequestContextFilters | Sorgu çağrısının bir parçası olarak belirtilen filtre kümesi. En çok üç olası dize dizisi içerir:<br>-ResourceTypes-sorgunun kapsamını sınırlayan kaynak türü<br>-Workspaces-sorguyu sınırlandıran çalışma alanlarının listesi<br>-WorkspaceRegions-sorguyu sınırlamak için çalışma alanı bölgelerinin listesi |
| Yanıt kodu          | Sorgu gönderildiğinde döndürülen HTTP yanıt kodu. |
| ResponseDurationMs    | Yanıtın döndürüldüğü zaman.  |
| ResponseRowCount     | Sorgu tarafından döndürülen toplam satır sayısı. |
| StatsCPUTimeMs       | Bilgi işlem, ayrıştırma ve veri getirme için kullanılan toplam işlem süresi. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |
| StatsDataProcessedKB | Sorguyu işlemek için erişilen veri miktarı. Hedef tablonun boyutuyla, kullanılan zaman aralığı, uygulanan filtrelerin ve başvurulan sütun sayısının etkilenerek etkilenir. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |
| StatsDataProcessedStart | Sorguyu işlemek için erişilen en eski veri süresi. Sorgu açık zaman dilimi ve filtreleri uygulandıktan sonra etkilendi. Bu, veri bölümleme nedeniyle açık zamandan daha büyük olabilir. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |
| StatsDataProcessedEnd  |Sorguyu işlemek için erişilen en yeni veri süresi. Sorgu açık zaman dilimi ve filtreleri uygulandıktan sonra etkilendi. Bu, veri bölümleme nedeniyle açık zamandan daha büyük olabilir. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |
| Statsworkspace sayısı | Sorgu tarafından erişilen çalışma alanı sayısı. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |
| StatsRegionCount | Sorgu tarafından erişilen bölge sayısı. Yalnızca sorgu 200 durum koduyla döndürülürse doldurulur. |

## <a name="considerations"></a>Dikkat edilmesi gerekenler

- Azure Veri Gezgini proxy 'den gelen sorgularda performans istatistikleri kullanılamaz. Bu sorgulara ilişkin diğer tüm veriler yine de doldurulacak.
- [Dize sabit değerlerini gizleme](/azure/data-explorer/kusto/query/scalar-data-types/string#obfuscated-string-literals) dizelerindeki *h* ipucu sorgu denetim günlükleri üzerinde bir etkiye sahip olmayacaktır. Sorgular, bir dize belirsizlendirilemeden tamamen gönderildiği şekilde yakalanacaktır. Log Analytics çalışma alanlarında bulunan çeşitli RBAC modlarını kullanarak bu verileri görmek için yalnızca Uyumluluk haklarına sahip olan kullanıcıların bunu yapabilmesini sağlayabilirsiniz.
- Birden çok çalışma alanındaki verileri içeren sorgular için, sorgu yalnızca kullanıcının erişimi olan çalışma alanlarında yakalanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Tanılama ayarları](../platform/diagnostic-settings.md)hakkında daha fazla bilgi edinin.
- [Günlük sorgularını iyileştirme](query-optimization.md)hakkında daha fazla bilgi edinin.