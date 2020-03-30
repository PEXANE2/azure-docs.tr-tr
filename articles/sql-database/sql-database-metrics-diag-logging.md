---
title: Ölçümlerin ve kaynak günlüklerinin akış dışa aktarımını yapılandırma
description: Azure SQL Veritabanı'ndan kaynak kullanımı ve sorgu yürütme istatistikleri hakkında bilgi depolamak için seçtiğiniz hedefe akıllı tanılama çözümlemesi de dahil olmak üzere, ölçümlerin ve kaynak günlüklerinin akış dışa aktarılmasını nasıl yapılandıracağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 3784b94a8571ab57d191d0bdb1e38aaa16d3cabb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255983"
---
# <a name="configure-streaming-export-of-azure-sql-database-diagnostic-telemetry"></a>Azure SQL Veritabanı tanısal telemetrinin akış dışa aktarma yapılandırma

Bu makalede, Azure SQL Veritabanı için çözümleme için birkaç hedeften birine dışa aktarabileceğiniz performans ölçümleri ve kaynak günlükleri hakkında bilgi edineceksiniz. Bu tanısal telemetrinin akış dışa aktarımını Azure portalı, PowerShell, Azure CLI, REST API ve Azure Kaynak Yöneticisi şablonları aracılığıyla nasıl yapılandıracağınızı öğreneceksiniz.

Ayrıca, bu tanısal telemetriyi aktarabileceğiniz yerler ve bu seçenekler arasında nasıl seçim yapabileceğiniz hakkında da bilgi edineceksiniz. Hedef seçenekleriniz şunlardır:

- [Log Analytics ve SQL Analytics](#stream-into-sql-analytics)
- [Etkinlik Hub'ları](#stream-into-event-hubs)
- [Azure Depolama](#stream-into-azure-storage)

## <a name="diagnostic-telemetry-for-export-for-azure-sql-database"></a>Azure SQL Veritabanı için dışa aktarma için tanılama telemetrisi

Dışa aktarabileceğiniz tanısal telemetriler arasında en önemlisi Intelligent Insights (SQLInsights) günlüğüdür. [Intelligent Insights,](sql-database-intelligent-insights.md) yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve düşük performansa neden olan yıkıcı olayları tespit etmek için yerleşik zekayı kullanır. Bir kez algılandıktan sonra, sorunun akıllı bir değerlendirmesi ile Intelligent Insights günlüğü oluşturan ayrıntılı bir analiz gerçekleştirilir. Bu değerlendirme, veritabanı performans sorununun temel neden çözümlemesi ve mümkünse performans iyileştirmeleri için önerilerden oluşur. İçeriğini görüntülemek için bu günlüğün akış dışa aktarımını yapılandırmanız gerekir.

Intelligent Insights günlüğünün dışa aktaran akışına ek olarak, çeşitli performans ölçümleri ve ek SQL Veritabanı günlükleri de dışa aktarabilirsiniz. Aşağıdaki tabloda, aktarım akışı için yapılandırabileceğiniz performans ölçümleri ve kaynak günlükleri açıklanmaktadır. Bu tanısal telemetri tek veritabanları, elastik havuzlar ve havuzlu veritabanları ve yönetilen örnekleri ve örnek veritabanları için yapılandırılabilir.

| Veritabanları için tanısal telemetri | Tek veritabanı ve havuzlu veritabanı desteği | Yönetilen örnek veritabanı desteği |
| :------------------- | ----- | ----- |
| [Temel ölçümler](#basic-metrics): DTU/CPU yüzdesi, DTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, Başarılı/Başarısız/Güvenlik Duvarı bağlantıları tarafından engellenen, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi ve XTP depolama yüzdesi içerir. | Evet | Hayır |
| [Örnek ve Uygulama Gelişmiş](#advanced-metrics): Tempdb sistem veritabanı verileri ve günlük dosya boyutu ve tempdb yüzde günlük dosyası kullanılır içerir. | Evet | Hayır |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics): CPU kullanımı ve sorgu süresi istatistikleri gibi sorgu çalışma zamanı istatistikleri hakkında bilgi içerir. | Evet | Evet |
| [QueryStoreWaitStatistics](#query-store-wait-statistics): CPU, LOG ve LOCKING gibi sorgu bekleme istatistikleri (sorgularınızın ne beklediği) hakkında bilgi içerir. | Evet | Evet |
| [Hatalar](#errors-dataset): Veritabanındaki SQL hataları hakkında bilgi içerir. | Evet | Evet |
| [DatabaseWaitStatistics](#database-wait-statistics-dataset): Bir veritabanının farklı bekleme türlerini beklerken ne kadar zaman harcadığı hakkında bilgi içerir. | Evet | Hayır |
| [Zaman Ekmeleri](#time-outs-dataset): Veritabanındaki zaman ekmeleri hakkında bilgi içerir. | Evet | Hayır |
| [Bloklar](#blockings-dataset): Veritabanındaki olayları engelleme hakkında bilgi içerir. | Evet | Hayır |
| [Kilitlenmeler](#deadlocks-dataset): Veritabanındaki kilitlenme olayları hakkında bilgi içerir. | Evet | Hayır |
| [AutomaticTuning](#automatic-tuning-dataset): Veritabanı için otomatik ayar önerileri hakkında bilgi içerir. | Evet | Hayır |
| [SQLInsights](#intelligent-insights-dataset): Bir veritabanı için performans içine Intelligent Insights içerir. Daha fazla bilgi edinmek için [Intelligent Insights](sql-database-intelligent-insights.md)'a bakın. | Evet | Evet |

> [!NOTE]
> Tanılama ayarları **sistem veritabanları**için yapılandırılamaz , master, msdb, model, kaynak ve tempdb veritabanları gibi.

## <a name="streaming-export-destinations"></a>Akış lı dışa aktarma hedefleri

Bu tanısal telemetri, analiz için aşağıdaki Azure kaynaklarından birine aktarılabilir.

- **[Log Analytics çalışma alanı](#stream-into-sql-analytics)**:

  [Log Analytics çalışma alanına](../azure-monitor/platform/resource-logs-collect-workspace.md) aktarılan veriler SQL [Analytics](../azure-monitor/insights/azure-sql.md)tarafından tüketilebilir. SQL Analytics, performans raporları, uyarılar ve azaltma önerileri içeren veritabanlarınızın akıllı izlemesini sağlayan bir bulut yalnızca izleme çözümüdür. Log Analytics çalışma alanına aktarılan veriler, toplanan diğer izleme verileriyle analiz edilebilir ve uyarılar ve görselleştirmeler gibi diğer Azure Monitör özelliklerinden yararlanmanızı sağlar
- **[Azure Etkinlik Hub'ları](#stream-into-event-hubs)**:

  [Azure Etkinlik Hub'ına](../azure-monitor/platform/resource-logs-stream-event-hubs.md)aktarılan veriler aşağıdaki işlevleri sağlar:

  - **Üçüncü taraf günlük ve telemetri sistemlerine akış günlükleri**: Tüm ölçümlerinizi ve kaynak günlüklerinizi tek bir olay hub'ına aktarın ve günlük verilerini üçüncü taraf bir SIEM veya günlük analiz aracına aktarın.
  - **Özel bir telemetri ve günlük platformu oluşturun**: Etkinlik hub'larının yüksek ölçeklenebilir yayımlama-abone yapısı, ölçümleri ve kaynak günlüklerini esnek bir şekilde özel bir telemetri platformuna almanızı sağlar. Ayrıntılar [için Azure Etkinlik Hub'larında Küresel Ölçekli Bir Telemetri Platformu Tasarlama ve Boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) bölümüne bakın.
  - **Power BI'ye veri aktararak hizmet durumunu görüntüleyin**: Tanılama verilerinizi Azure hizmetlerinizle ilgili neredeyse gerçek zamanlı öngörülere dönüştürmek için Olay Hub'larını, Akış Analizlerini ve Power BI'yi kullanın. Bkz. Akış Analizi ve Power BI: Bu çözümle ilgili ayrıntılar [için veri akışı için gerçek zamanlı analiz panosu.](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard)
- **[Azure Depolama](#stream-into-azure-storage)**:

  [Azure Depolama'ya](../azure-monitor/platform/resource-logs-collect-storage.md) aktarılan veriler, önceki iki akış seçeneğinin maliyetinin çok küçük bir kısmı için büyük miktarda tanısal telemetri arşivlemenize olanak tanır.

Bu hedeflerden birine aktarılan bu tanısal telemetri, daha kolay performans izleme için kaynak kullanımını ve sorgu yürütme istatistiklerini ölçmek için kullanılabilir.

![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-and-configure-the-streaming-export-of-diagnostic-telemetry"></a>Tanısal telemetrinin akış export'u etkinleştirme ve yapılandırma

Aşağıdaki yöntemlerden birini kullanarak ölçümleri ve tanısal telemetri günlüğe kaydetmeyi etkinleştirebilir ve yönetebilirsiniz:

- Azure portalında
- PowerShell
- Azure CLI
- Azure İzleyici REST API'si
- Azure Resource Manager şablonu

> [!NOTE]
> Güvenlik telemetrisinin denetim günlüğü akışını etkinleştirmek [için](sql-database-auditing.md#subheading-2) bkz. [auditing logs in Azure Monitor logs and Azure Event Hubs](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242)

## <a name="configure-the-streaming-export-of-diagnostic-telemetry"></a>Tanısal telemetrinin akış export'u yapılandırma

Tanılama telemetrisinin akışını etkinleştirmek ve yapılandırmak için Azure portalındaki **Tanılama ayarları** menüsünü kullanabilirsiniz. Ayrıca, tanısal telemetri akışını yapılandırmak için PowerShell, Azure CLI, [REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)ve [Kaynak Yöneticisi şablonlarını](../azure-monitor/platform/diagnostic-settings-template.md) kullanabilirsiniz. Tanılama telemetrisini aktaracak şekilde aşağıdaki hedefleri ayarlayabilirsiniz: Azure Depolama, Azure Etkinlik Hub'ları ve Azure Monitor günlükleri.

> [!IMPORTANT]
> Tanısal telemetrinin akış dışa aktarımı varsayılan olarak etkinleştirilir.

Azure portalında tanısal telemetrinin akış dışa aktarAn dışa aktarAn yapılandırmave PowerShell ve Azure CLI ile aynı şeyi gerçekleştirmek için komut dosyaları için adım adım kılavuziçin aşağıdaki sekmelerden birini seçin.

# <a name="azure-portal"></a>[Azure portalında](#tab/azure-portal)

### <a name="elastic-pools"></a>Esnek havuzlar

Aşağıdaki tanısal telemetriyi toplamak için elastik bir havuz kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetriizleme |
| :------------------- | ------------------- |
| **Elastik havuz** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) eDTU/CPU yüzdesi, eDTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, depolama sınırı ve XTP depolama yüzdesi içerir. |

Elastik havuzlar ve havuzlu veritabanları için tanısal telemetri akışını yapılandırmak için her birini ayrı ayrı yapılandırmanız gerekir:

- Elastik bir havuz için tanısal telemetrinin akışını etkinleştirin
- Elastik havuzda her veritabanı için tanısal telemetri akışını etkinleştirin

Elastik havuz konteyneri, havuzlu her veritabanının telemetrisinden ayrı olarak kendi telemetrisine sahiptir.

Esnek bir havuz kaynağı için tanısal telemetri akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalındaki **esnek havuz** kaynağına gidin.
2. **Tanılama ayarlarını**seçin.
3. Önceki ayarlar yoksa **tanılamayı aç'ı** seçin veya önceki bir ayarı ayarlamak için **Ayarla'yı** seçin.

   ![Elastik havuzlar için tanılamayı etkinleştirin](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleyin,** **bir olay hub'ına akış**yapın veya Günlük **Analizi'ne gönderin.**
6. Günlük analitiği için **Yapıl'ı** seçin ve **+Yeni Çalışma Alanı Oluştur'u**seçerek yeni bir çalışma alanı oluşturun veya varolan bir çalışma alanı seçin.
7. Elastik havuz tanıt telemetrisi için onay kutusunu seçin: **Temel** ölçümler.
   ![Elastik havuzlar için tanılamayı yapılandırın](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

8. **Kaydet'i**seçin.
9. Buna ek olarak, izlemek istediğiniz elastik havuzdaki her veritabanı için tanısal telemetri akışını sonraki bölümde açıklanan adımları izleyerek yapılandırın.

> [!IMPORTANT]
> Elastik bir havuz için tanısal telemetri yapılandırmaya ek olarak, elastik havuzda her veritabanı için tanıt telemetri yapılandırmak gerekir.

### <a name="single-or-pooled-database"></a>Tek veya havuzlu veritabanı

Aşağıdaki tanısal telemetriyi toplamak için tek veya havuza toplanmış bir veritabanı kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetriizleme |
| :------------------- | ------------------- |
| **Tek veya havuzlu veritabanı** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) DTU yüzdesi, DTU kullanılan, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, Başarılı/Başarısız/Güvenlik Duvarı bağlantıları, oturumyüzdesi, işçi yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeleri içerir. |

Tek bir veya havuzlu veritabanı için tanılama telemetrisinin akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure **SQL veritabanı** kaynağına gidin.
2. **Tanılama ayarlarını**seçin.
3. Önceki ayarlar yoksa **tanılamayı aç'ı** seçin veya önceki bir ayarı ayarlamak için **Ayarla'yı** seçin. Tanısal telemetri akışı için en fazla üç paralel bağlantı oluşturabilirsiniz.
4. Tanılama verilerinin birden çok kaynağa paralel akışını yapılandırmak için **tanılama ayarını ekle'yi** seçin.

   ![Tek ve birleştirilmiş veritabanları için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

5. Kendi başvurunuz için bir ayar adı girin.
6. Akış tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleyin,** **bir olay hub'ına akış**yapın veya Günlük **Analizi'ne gönderin.**
7. Standart, olay tabanlı izleme deneyimi için, veritabanı tanılama günlük telemetri için aşağıdaki onay kutularını seçin: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics**, **Hatalar**, **DatabaseWaitStatistics**, **Timeouts**, **Bloklar**, ve **Kilitler**.
8. Gelişmiş, bir dakikalık tabanlı izleme deneyimi için **Temel** ölçümler için onay kutusunu seçin.

   ![Tek, havuzlu veya örnek veritabanları için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)
9. **Kaydet'i**seçin.
10. İzlemek istediğiniz her veritabanı için bu adımları yineleyin.

> [!TIP]
> İzlemek istediğiniz her tek ve havuzlu veritabanı için bu adımları yineleyin.

### <a name="managed-instance"></a>Yönetilen örnek

Aşağıdaki tanısal telemetriyi toplamak için yönetilen bir örnek kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetriizleme |
| :------------------- | ------------------- |
| **Yönetilen örnek** | [ResourceUsageStats](#resource-usage-stats-for-managed-instances) vCores sayısı, ortalama CPU yüzdesi, IO istekleri, bayt okuma /yazılı, ayrılmış depolama alanı ve kullanılan depolama alanı içerir. |

Yönetilen örnek ve örnek veritabanları için tanısal telemetri akışını yapılandırmak için, her birini ayrı ayrı yapılandırmanız gerekir:

- Yönetilen örneğin tanısal telemetri akışını etkinleştirme
- Her örnek veritabanı için tanısal telemetri akışını etkinleştirme

Yönetilen örnek kapsayıcının her örnek veritabanının telemetrisinden ayrı kendi telemetrisi vardır.

Yönetilen bir örnek kaynağı için tanısal telemetri akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında **yönetilen örnek** kaynağına gidin.
2. **Tanılama ayarlarını**seçin.
3. Önceki ayarlar yoksa **tanılamayı aç'ı** seçin veya önceki bir ayarı ayarlamak için **Ayarla'yı** seçin.

   ![Yönetilen örneğin tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleyin,** **bir olay hub'ına akış**yapın veya Günlük **Analizi'ne gönderin.**
6. Günlük analitiği için **Yapıl'ı** seçin ve **+Yeni Çalışma Alanı Oluştur'u**seçerek yeni bir çalışma alanı oluşturun veya varolan bir çalışma alanını kullanın.
7. Örneğin tanısal telemetri için onay kutusunu seçin: **ResourceUsageStats**.

   ![Yönetilen örnek için tanılama yı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)

8. **Kaydet'i**seçin.
9. Buna ek olarak, sonraki bölümde açıklanan adımları izleyerek izlemek istediğiniz yönetilen örnek içinde her örnek veritabanı için tanıt telemetri akışı yapılandırmak.

> [!IMPORTANT]
> Yönetilen bir örnek için tanısal telemetri yapılandırmaya ek olarak, her örnek veritabanı için tanılama telemetriyapılandırmanız gerekir.

### <a name="instance-database"></a>Örnek veritabanı

Aşağıdaki tanısal telemetriyi toplamak için bir örnek veritabanı kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetriizleme |
| :------------------- | ------------------- |
| **Örnek veritabanı** | [ResourceUsageStats](#resource-usage-stats-for-managed-instances) vCores sayısı, ortalama CPU yüzdesi, IO istekleri, bayt okuma /yazılı, ayrılmış depolama alanı ve kullanılan depolama alanı içerir. |

Örnek veritabanı için tanısal telemetri akışını etkinleştirmek için aşağıdaki adımları izleyin:

1. Yönetilen örnek içinde **örnek veritabanı** kaynağına gidin.
2. **Tanılama ayarlarını**seçin.
3. Önceki ayarlar yoksa **tanılamayı aç'ı** seçin veya önceki bir ayarı ayarlamak için **Ayarla'yı** seçin.
   - Tanısal telemetri akışı için en fazla üç (3) paralel bağlantı oluşturabilirsiniz.
   - Tanılama verilerinin birden çok kaynağa paralel akışını yapılandırmak için **+Tanılama ayarı ekle'yi** seçin.

   ![Örneğin veritabanları için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleyin,** **bir olay hub'ına akış**yapın veya Günlük **Analizi'ne gönderin.**
6. Veritabanı tanılama telemetrisi için onay kutularını seçin: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics**ve **Hatalar**.
   ![Örneğin veritabanları için tanılamayapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)
7. **Kaydet'i**seçin.
8. İzlemek istediğiniz her örnek veritabanı için bu adımları yineleyin.

> [!TIP]
> İzlemek istediğiniz her örnek veritabanı için bu adımları yineleyin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

PowerShell'i kullanarak ölçümleri ve tanılama günlüğe kaydetmeyi etkinleştirebilirsiniz.

- Ölçümlerin ve kaynak günlüklerinin bir depolama hesabında depolanmasını etkinleştirmek için şu komutu kullanın:

  ```powershell
  Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
  ```

  Depolama hesabı kimliği, hedef depolama hesabının kaynak kimliğidir.

- Ölçümlerin ve kaynak günlüklerinin bir olay hub'ına akışını etkinleştirmek için şu komutu kullanın:

  ```powershell
  Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
  ```

  Azure Service Bus kural kimliği, şu biçime sahip bir dizedir:

  ```powershell
  {service bus resource ID}/authorizationrules/{key name}
  ```

- Günlük Analizi çalışma alanına metrik ve kaynak günlükleri göndermeyi etkinleştirmek için şu komutu kullanın:

  ```powershell
  Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
  ```

- Log Analytics çalışma alanının kaynak kimliğini almak için aşağıdaki komutu kullanabilirsiniz:

  ```powershell
  (Get-AzOperationalInsightsWorkspace).ResourceId
  ```

Bu parametreleri bir arada kullanarak birden fazla çıkış seçeneği oluşturabilirsiniz.

**Birden çok Azure kaynağı yapılandırmak için**

Birden çok aboneliği desteklemek için PowerShell komut dosyasını [PowerShell'i kullanarak Azure kaynak ölçümlerini etkinleştir'i kullanın.](https://blogs.technet.microsoft.com/msoms/20../../enable-azure-resource-metrics-logging-using-powershell/)

Birden çok kaynaktan \<\> çalışma alanına tanılama verilerini `Enable-AzureRMDiagnostics.ps1` göndermek için komut dosyasını çalıştırırken çalışma alanı kaynak kimliğini $WSID bir parametre olarak sağlayın.

- Tanılama verilerinizin \<hedefin çalışma alanı kimliğini $WSID\> almak için aşağıdaki komut dosyasını kullanın:

  ```powershell
  $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
  .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
  ```

  Alt \<\> ID'yi abonelik \<kimliğiyle, kaynak grup adıyla RG_NAME\> ve \<çalışma alanı adı ile WS_NAME\> değiştirin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'yi kullanarak ölçümleri ve tanılama günlüğe kaydetmeyi etkinleştirebilirsiniz.

> [!IMPORTANT]
> Azure CLI v1.0 için tanılama günlüğe kaydetmeyi etkinleştiren komut dosyaları desteklenir. Azure CLI v2.0 şu anda desteklenmiş.

- Ölçümlerin ve kaynak günlüklerinin bir depolama hesabında depolanmasını etkinleştirmek için şu komutu kullanın:

  ```azurecli-interactive
  azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
  ```

  Depolama hesabı kimliği, hedef depolama hesabının kaynak kimliğidir.

- Ölçümlerin ve kaynak günlüklerinin bir olay hub'ına akışını etkinleştirmek için şu komutu kullanın:

  ```azurecli-interactive
  azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
  ```

  Hizmet Veri Servisi kural kimliği bu biçime sahip bir dizedir:

  ```azurecli-interactive
  {service bus resource ID}/authorizationrules/{key name}
  ```

- Günlük Analizi çalışma alanına ölçümlerin ve kaynak günlüklerinin gönderilmesini etkinleştirmek için şu komutu kullanın:

  ```azurecli-interactive
  azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
  ```

Bu parametreleri bir arada kullanarak birden fazla çıkış seçeneği oluşturabilirsiniz.

---

## <a name="stream-into-sql-analytics"></a>SQL Analytics'e Akış

Log Analytics çalışma alanına aktarılan SQL Veritabanı ölçümleri ve kaynak günlükleri Azure SQL Analytics tarafından kullanılabilir. Azure SQL Analytics, tek veritabanlarının, esnek havuzların ve havuzlu veritabanlarının performansını izleyen ve ölçekte ve birden çok abonelik te yönetilen örnek ve örnek veritabanlarını izleyen bir bulut çözümüdür. Azure SQL Veritabanı performans ölçümlerini toplamanıza ve görselleştirmenize yardımcı olabilir ve performans sorun giderme için yerleşik zekaya sahiptir.

![Azure SQL Analytics'e Genel Bakış](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

### <a name="installation-overview"></a>Yüklemeye genel bakış

Aşağıdaki adımları gerçekleştirerek Azure SQL Analytics ile Azure SQL veritabanları koleksiyonunu izleyebilirsiniz:

1. Azure Marketi'nden bir Azure SQL Analytics çözümü oluşturun.
2. Çözümde bir Log Analytics çalışma alanı oluşturun.
3. Tanısal telemetriyi çalışma alanına aktaracak şekilde veritabanlarını yapılandırın.

Azure portalındaki tanı ayarları sekmesinde yerleşik **Log Analytics'e Gönder** seçeneğini kullanarak bu tanılama telemetrisinin akış dışa aktarımını yapılandırabilirsiniz. PowerShell [cmdlets](sql-database-metrics-diag-logging.md?tabs=azure-powershell#configure-the-streaming-export-of-diagnostic-telemetry), [Azure CLI](sql-database-metrics-diag-logging.md?tabs=azure-cli#configure-the-streaming-export-of-diagnostic-telemetry), [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)veya Kaynak Yöneticisi [şablonları](../azure-monitor/platform/diagnostic-settings-template.md)aracılığıyla tanılama ayarlarını kullanarak Bir Günlük Analizi çalışma alanına akışı da etkinleştirebilirsiniz.

### <a name="create-an-azure-sql-analytics-resource"></a>Azure SQL Analytics kaynağı oluşturma

1. Azure Marketi'nde Azure SQL Analytics'i arayın ve seçin.

   ![Portalda Azure SQL Analytics ara](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)

2. Çözümün genel bakış ekranında **Oluştur'u** seçin.

3. Azure SQL Analytics formunu gerekli olan ek bilgilerle doldurun: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Azure SQL Analytics'i portalda yapılandırma](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Onaylamak için **Tamam'ı** seçin ve ardından **Oluştur'u**seçin.

### <a name="configure-the-resource-to-record-metrics-and-resource-logs"></a>Kaynağı ölçümleri ve kaynak günlüklerini kaydetmek için yapılandırma

Tek ve havuzlu veritabanları, elastik havuzlar, yönetilen örnekler ve örnek veritabanları için tanılama telemetri akışını ayrı ayrı yapılandırmanız gerekir. Bir kaynağın ölçümleri kaydettiği yeri yapılandırmanın en kolay yolu Azure portalını kullanmaktır. Ayrıntılı adımlar için [bkz.](sql-database-metrics-diag-logging.md?tabs=azure-portal#configure-the-streaming-export-of-diagnostic-telemetry)

### <a name="use-azure-sql-analytics-for-monitoring-and-alerting"></a>İzleme ve uyarı için Azure SQL Analytics'i kullanın

SQL veritabanı kaynaklarınızı görüntülemek için SQL Analytics'i hiyerarşik bir pano olarak kullanabilirsiniz.

- Azure SQL Analytics'i nasıl kullanacağınızı öğrenmek için [SQL Analytics'i kullanarak SQL Veritabanını İzle](../log-analytics/log-analytics-azure-sql.md)bölümüne bakın.
- SQL Analytics'te nasıl uyarı lar ayarlayabilirsiniz öğrenmek [için veritabanı, elastik havuzlar ve yönetilen örnekler için uyarı oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts)bölümüne bakın.

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

Azure portalındaki **bir olay hub'ına** yerleşik Akış'ı kullanarak SQL Veritabanı ölçümlerini ve kaynak günlüklerini Olay Hub'larına aktarabilirsiniz. Ayrıca PowerShell cmdlets, Azure CLI veya Azure Monitor REST API üzerinden tanılama ayarlarını kullanarak Hizmet Veri Servisi kural kimliğini etkinleştirebilirsiniz.

### <a name="what-to-do-with-metrics-and-resource-logs-in-event-hubs"></a>Olay Hub'larında ölçümler ve kaynak günlükleriyle ne yapmalı?

Seçili veriler Olay Hub'larına aktarıldıktan sonra gelişmiş izleme senaryolarını etkinleştirmeye bir adım daha yaklaşmış oluyorsunuz. Olay Hub'ları bir olay boru hattı için ön kapı görevi görür. Veriler bir etkinlik hub'ına toplandıktan sonra, gerçek zamanlı bir analiz sağlayıcısı veya depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Olay Hub'ları, bir olay akışı üretimini bu olayların tüketiminden ayırıyor. Bu şekilde, etkinlik tüketicileri olaylara kendi zamanlamaları yla erişebilirler. Etkinlik Hub'ları hakkında daha fazla bilgi için bkz:

- [Azure Etkinlik Hub'ları nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Olay Hub'larında akışlı ölçümleri şu şekilde kullanabilirsiniz:

- **Power BI'ye sıcak yol verileri aktararak hizmet durumunu görüntüleme**

  Etkinlik Hub'larını, Akış Analizini ve Power BI'yi kullanarak, metrik ve tanılama verilerinizi Azure hizmetlerinizle ilgili neredeyse gerçek zamanlı öngörülere kolayca dönüştürebilirsiniz. Bir etkinlik hub'ının nasıl kurulabildiğini, verileri Akış Analizi ile nasıl işleyip, Power BI'yi çıktı olarak nasıl kullanacağınız hakkında genel bilgi için [Bkz. Stream Analytics ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

- **Üçüncü taraf günlük ve telemetri akışlarına akış günlükleri**

  Olay Hub'ları akışını kullanarak, ölçümlerinizi ve kaynak günlüklerinizi çeşitli üçüncü taraf izleme ve günlük analizi çözümlerine aktarabilirsiniz.

- **Özel bir telemetri ve günlük platformu oluşturun**

  Zaten özel olarak inşa edilmiş bir telemetri platformu nuz var mı veya bir tane oluşturmayı düşünüyor musunuz? Olay Hub'larının yüksek ölçeklenebilir yayımlama-abone lik yapısı, ölçümleri ve kaynak günlüklerini esnek bir şekilde yutmanızı sağlar. [Dan Rosanova'nın olay hub'larını küresel ölçekte bir telemetri platformunda kullanma kılavuzuna](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)bakın.

## <a name="stream-into-azure-storage"></a>Azure Depolama alanına akış

Yerleşik Arşiv'i Azure portalındaki **bir depolama hesabı seçeneğine** kullanarak ölçümleri ve kaynak günlüklerini Azure Depolama'da depolayabilirsiniz. Ayrıca PowerShell cmdlets, Azure CLI veya Azure Monitor REST API üzerinden tanılama ayarlarını kullanarak Depolama'yı etkinleştirebilirsiniz.

### <a name="schema-of-metrics-and-resource-logs-in-the-storage-account"></a>Depolama hesabındaki ölçümlerin ve kaynak günlüklerinin şeması

Ölçümler ivedilik ve kaynak günlükleri koleksiyonunu ayarladıktan sonra, ilk veri satırları kullanılabilir olduğunda seçtiğiniz depolama hesabında bir depolama kapsayıcısı oluşturulur. Lekelerin yapısı:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Ya da daha basit:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, Temel ölçümler için bir blob adı olabilir:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Elastik bir havuzdan veri depolamak için bir blob adı gibi görünür:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

## <a name="data-retention-policy-and-pricing"></a>Veri saklama ilkesi ve fiyatlandırma

Olay Hub'ları veya Bir Depolama hesabı seçerseniz, bir bekletme ilkesi belirtebilirsiniz. Bu ilke, seçili bir zaman diliminden daha eski verileri siler. Log Analytics belirtirseniz, bekletme ilkesi seçili fiyatlandırma katmanına bağlıdır. Bu durumda, sağlanan ücretsiz veri alma birimleri her ay birden fazla veritabanının ücretsiz izlenmesini sağlayabilir. Serbest üniteleri aşan tanısal telemetri herhangi bir tüketimi maliyetlere neden olabilir.

> [!IMPORTANT]
> Daha ağır iş yüklerine sahip etkin veritabanları, boşta kalan veritabanlarından daha fazla veri yutarak. Daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/details/monitor/)

Azure SQL Analytics kullanıyorsanız, Azure SQL Analytics'in navigasyon menüsünde **OMS Çalışma Alanı'nı** seçip **ardından Kullanım** ve **Tahmini Maliyetler'i**seçerek veri alım tüketiminizi izleyebilirsiniz.

## <a name="metrics-and-logs-available"></a>Ölçümler ve günlükler kullanılabilir

Tek veritabanları, havuzlu veritabanları, elastik havuzlar, yönetilen örnek ve örnek veritabanları için kullanılabilir izleme telemetri makalenin bu bölümünde belgelenmiştir. SQL Analytics içinde toplanan izleme telemetrisi, [Azure Monitor günlük sorguları](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) dilini kullanarak kendi özel çözümlemeniz ve uygulama geliştirmeniz için kullanılabilir.

### <a name="basic-metrics"></a>Temel ölçümler

Kaynağa göre Temel ölçümler hakkında ayrıntılar için aşağıdaki tablolara bakın.

> [!NOTE]
> Temel ölçümler seçeneği eskiden Tüm ölçümler olarak biliniyordu. Yapılan değişiklik yalnızca adlandırma da yapıldı ve izlenen ölçümlerde herhangi bir değişiklik olmadı. Bu değişiklik, gelecekte ek metrik kategorilerin getirilmesine izin vermek için başlatıldı.

#### <a name="basic-metrics-for-elastic-pools"></a>Elastik havuzlar için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Elastik havuz|eDTU yüzdesi, eDTU kullanılan, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, işçi yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |

#### <a name="basic-metrics-for-single-and-pooled-databases"></a>Tek ve havuzlu veritabanları için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Tek ve havuzlu veritabanı|DTU yüzdesi, DTU kullanılan, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, Başarılı/Başarısız/Güvenlik Duvarı bağlantıları tarafından engellendi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeler |

### <a name="advanced-metrics"></a>Gelişmiş ölçümler

Gelişmiş ölçümler le ilgili ayrıntılar için aşağıdaki tabloya bakın.

|**Ölçüm**|**Metrik Görüntü Adı**|**Açıklama**|
|---|---|---|
|tempdb_data_size| Tempdb Veri Dosya Boyutu Kilobayt |Tempdb Veri Dosya Boyutu Kilobayt. Veri ambarları için geçerli değildir. Bu metrik, 2 vCore ve daha yüksek vCore satÝnýr ýsýnýs veya DTU tabanlı satýn alma modelleri için 200 DTU ve daha yüksek olan vCore satın alma modelini kullanan veritabanları için kullanılabilecektir. Bu metrik şu anda Hiper ölçekli veritabanları için kullanılamıyor.|
|tempdb_log_size| Tempdb Log Dosya Boyutu Kilobayt |Tempdb Log Dosya Boyutu Kilobayt. Veri ambarları için geçerli değildir. Bu metrik, 2 vCore ve daha yüksek vCore satÝnýr ýsýnýs veya DTU tabanlı satýn alma modelleri için 200 DTU ve daha yüksek olan vCore satın alma modelini kullanan veritabanları için kullanılabilecektir. Bu metrik şu anda Hiper ölçekli veritabanları için kullanılamıyor.|
|tempdb_log_used_percent| Tempdb Yüzde Günlük Kullanılan |Tempdb Yüzde Günlüğü Kullanılır. Veri ambarları için geçerli değildir. Bu metrik, 2 vCore ve daha yüksek vCore satÝnýr ýsýnýs veya DTU tabanlı satýn alma modelleri için 200 DTU ve daha yüksek olan vCore satın alma modelini kullanan veritabanları için kullanılabilecektir. Bu metrik şu anda Hiper ölçekli veritabanları için kullanılamıyor.|

### <a name="basic-logs"></a>Temel günlükler

Tüm günlükler için kullanılabilir telemetri ayrıntıları aşağıdaki tablolarda belgelenmiştir. Belirli bir veritabanı lezzeti için hangi günlüklerin destekleniyi anlamak için [desteklenen tanılama telemetrisine](#diagnostic-telemetry-for-export-for-azure-sql-database) bakın - Azure SQL tek, havuzlu veya örnek veritabanı.

#### <a name="resource-usage-stats-for-managed-instances"></a>Yönetilen örnekler için kaynak kullanım istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure|
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: KaynakKullanımİstatistiks |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: MANAGEDINSTANCES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Yönetilen örneğin adı |
|ResourceId|Kaynak URI |
|SKU_s|Yönetilen örnek ürün SKU |
|virtual_core_count_s|Kullanılabilir vCore sayısı |
|avg_cpu_percent_s|Ortalama CPU yüzdesi |
|reserved_storage_mb_s|Yönetilen örnekte ayrılmış depolama kapasitesi |
|storage_space_used_mb_s|Yönetilen örnekte kullanılmış depolama |
|io_requests_s|IOPS sayısı |
|io_bytes_read_s|IOPS bayt okuyun |
|io_bytes_written_s|IOPS bayt yazılı |

#### <a name="query-store-runtime-statistics"></a>Sorgu La Store çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: QueryStoreRuntimeStatistics |
|ThrottledRequests|Operasyonun adı. Her zaman: QueryStoreRuntimeStatisticsEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|query_hash_s|Sorgu karma |
|query_plan_hash_s|Sorgu planı karma |
|statement_sql_handle_s|Ekstre sql kolu |
|interval_start_time_d|1900-1-1 arasındaki kene sayısındaki aralığın başlangıç datetimeoffset'i |
|interval_end_time_d|1900-1-1 arasındaki kene sayısındaki aralığın bitiş datetimeoffset'i |
|logical_io_writes_d|Toplam mantıksal IO yazma sayısı |
|max_logical_io_writes_d|Yürütme başına en fazla mantıksal IO yazma sayısı |
|physical_io_reads_d|Toplam fiziksel IO okuma sayısı |
|max_physical_io_reads_d|Yürütme başına en fazla mantıksal IO okuma sayısı |
|logical_io_reads_d|Toplam mantıksal IO okuma sayısı |
|max_logical_io_reads_d|Yürütme başına en fazla mantıksal IO okuma sayısı |
|execution_type_d|Yürütme türü |
|count_executions_d|Sorgunun yürütme sayısı |
|cpu_time_d|Sorgu tarafından mikrosaniye cinsinden tüketilen toplam CPU süresi |
|max_cpu_time_d|Mikrosaniyelerde tek bir yürütme ile Maksimum CPU süresi tüketici |
|dop_d|Paralellik derecelerinin toplamı |
|max_dop_d|Tek yürütme için kullanılan en yüksek paralellik derecesi |
|rowcount_d|Döndürülen toplam satır sayısı |
|max_rowcount_d|Tek yürütmede döndürülen en yüksek satır sayısı |
|query_max_used_memory_d|KB'de kullanılan toplam bellek miktarı |
|max_query_max_used_memory_d|KB'de tek bir yürütme tarafından kullanılan maksimum bellek miktarı |
|duration_d|Mikrosaniye cinsinden toplam yürütme süresi |
|max_duration_d|Tek bir yürütmenin maksimum yürütme süresi |
|num_physical_io_reads_d|Toplam fiziksel okuma sayısı |
|max_num_physical_io_reads_d|Yürütme başına en fazla fiziksel okuma sayısı |
|log_bytes_used_d|Kullanılan günlük baytların toplam miktarı |
|max_log_bytes_used_d|Yürütme başına kullanılan günlük baytların maksimum miktarı |
|query_id_d|Sorgu Deposu'ndaki sorgunun kimliği |
|plan_id_d|Sorgu Deposu'ndaki planın kimliği |

[Sorgu Deposu çalışma zamanı istatistik verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql)hakkında daha fazla bilgi edinin.

#### <a name="query-store-wait-statistics"></a>Sorgu Mağaza bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: QueryStoreWaitStatistics |
|ThrottledRequests|Operasyonun adı. Her zaman: QueryStoreWaitStatisticsOlay |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|wait_category_s|Bekleme kategorisi |
|is_parameterizable_s|Sorgu parametreli mi |
|statement_type_s|İfadenin türü |
|statement_key_hash_s|İfade anahtarı karma |
|exec_type_d|Yürütme türü |
|total_query_wait_time_ms_d|Belirli bekleme kategorisindeki sorgunun toplam bekleme süresi |
|max_query_wait_time_ms_d|Belirli bekleme kategorisinde tek tek yürütme sorgunun maksimum bekleme süresi |
|query_param_type_d|0 |
|query_hash_s|Sorgu Deposu'nda sorgu karma |
|query_plan_hash_s|Sorgu Deposu'nda plan karma sorgu |
|statement_sql_handle_s|Sorgu Deposu'nda deyim tutamacı |
|interval_start_time_d|1900-1-1 arasındaki kene sayısındaki aralığın başlangıç datetimeoffset'i |
|interval_end_time_d|1900-1-1 arasındaki kene sayısındaki aralığın bitiş datetimeoffset'i |
|count_executions_d|Sorgunun yürütme sayısı |
|query_id_d|Sorgu Deposu'ndaki sorgunun kimliği |
|plan_id_d|Sorgu Deposu'ndaki planın kimliği |

[Sorgu Lektüs istatistikleri verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql)hakkında daha fazla bilgi edinin.

#### <a name="errors-dataset"></a>Hatalar veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: Hatalar |
|ThrottledRequests|Operasyonun adı. Her zaman: ErrorEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|İleti|Düz metinde hata iletisi |
|user_defined_b|Hata kullanıcı tanımlı bit mi |
|error_number_d|Hata kodu |
|Severity|Hatanın önem derecesi |
|state_d|Hatadurumu |
|query_hash_s|Varsa, başarısız sorgunun sorgu karma |
|query_plan_hash_s|Varsa, başarısız sorgunun sorgu planı karma |

[SQL Server hata iletileri](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors?view=sql-server-ver15)hakkında daha fazla bilgi edinin.

#### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: VeritabanıWaitStatistics |
|ThrottledRequests|Operasyonun adı. Her zaman: DatabaseWaitStatisticsOlay |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|wait_type_s|Bekleme türünün adı |
|start_utc_date_t [UTC]|Ölçülen dönem başlangıç zamanı |
|end_utc_date_t [UTC]|Ölçülen dönem bitiş saati |
|delta_max_wait_time_ms_d|Max yürütme başına zaman bekledi |
|delta_signal_wait_time_ms_d|Toplam sinyaller bekleme süresi |
|delta_wait_time_ms_d|Dönemdeki toplam bekleme süresi |
|delta_waiting_tasks_count_d|Bekleyen görev sayısı |

[Veritabanı bekleme istatistikleri](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)hakkında daha fazla bilgi edinin.

#### <a name="time-outs-dataset"></a>Zaman-outs veri seti

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: Zaman dilimleri |
|ThrottledRequests|Operasyonun adı. Her zaman: TimeoutEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|error_state_d|Hata durumu kodu |
|query_hash_s|Varsa karma sorgula |
|query_plan_hash_s|Varsa sorgu planı karma |

#### <a name="blockings-dataset"></a>Veri kümesini engelleme

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: Bloklar |
|ThrottledRequests|Operasyonun adı. Her zaman: BlockEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|lock_mode_s|Sorgu tarafından kullanılan kilit modu |
|resource_owner_type_s|Kilidin sahibi |
|blocked_process_filtered_s|Engellenen işlem raporu XML |
|duration_d|Mikrosaniyelerde kilidin süresi |

#### <a name="deadlocks-dataset"></a>Kilitler veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC] |Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: Kilitlenmeler |
|ThrottledRequests|Operasyonun adı. Her zaman: DeadlockOlay |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|deadlock_xml_s|Kilitlenme raporu XML |

#### <a name="automatic-tuning-dataset"></a>Otomatik ayarlama veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı kimliğiniz |
|SourceSystem|Her zaman: Azure |
|Zaman Oluşturuldu [UTC]|Günlük kaydedildiğinde zaman damgası |
|Tür|Her zaman: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcısının adı. Her zaman: MICROSOFT. Sql |
|Kategori|Kategorinin adı. Her zaman: Otomatik Tuning |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Her zaman: SUNUCULAR / DATABASES |
|SubscriptionId|Veritabanı için Abonelik GUID |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucunun adı |
|LogicalDatabaseName_s|Veritabanının adı |
|ElasticPoolName_s|Varsa, veritabanı için elastik havuzun adı |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI |
|RecommendationHash_s|Otomatik ayar önerisinin benzersiz karma |
|OptionName_s|Otomatik ayar işlemi |
|Schema_s|Veritabanı şeması |
|Table_s|Etkilenen tablo |
|IndexName_s|Dizin adı |
|IndexColumns_s|Sütun adı |
|IncludedColumns_s|Sütunlar dahil |
|EstimatedImpact_s|Otomatik ayar önerisi JSON tahmini etkisi |
|Event_s|Otomatik ayar olayı türü |
|Timestamp_t|Son güncellenen zaman damgası |

#### <a name="intelligent-insights-dataset"></a>Akıllı Öngörüler veri seti

[Intelligent Insights günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Günlüğe kaydetmeyi nasıl etkinleştireceklerini öğrenmek ve çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini anlamak için bkz.

- [Microsoft Azure'daki ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure platform günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)

Etkinlik Hub'ları hakkında bilgi edinmek için şunları okuyun:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Günlük analitiğinden telemetriye dayalı uyarılarınasıl ayarlayatılabildiğini öğrenmek için bkz:

- [SQL Veritabanı ve yönetilen örnek için uyarılar oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts)
