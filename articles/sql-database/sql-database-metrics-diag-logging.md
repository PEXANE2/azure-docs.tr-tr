---
title: Azure SQL veritabanı ölçümleri ve tanılama günlüğü | Microsoft Docs
description: Kaynak kullanımı ve sorgu yürütme istatistikleri hakkında bilgi depolamak için Azure SQL veritabanı 'nda tanılamayı nasıl etkinleştireceğinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 05/21/2019
ms.openlocfilehash: d9f1afdff53ada2df7722fcfdd7014fb6c417e39
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70135183"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL veritabanı ölçümleri ve tanılama günlüğü

Bu konu başlığında, Azure SQL veritabanı için Azure portal, PowerShell, Azure CLı, Azure Izleyici REST API ve Azure Resource Manager şablonu aracılığıyla tanılama telemetrinin günlüğe kaydetmenin nasıl yapılandırılacağını öğreneceksiniz. Bu Tanılamalar, kaynak kullanımını ve sorgu yürütme istatistiklerini ölçmek için kullanılabilir.

Tek veritabanları, elastik havuzlardaki havuza alınmış veritabanları ve yönetilen bir örnekteki örnek veritabanları, daha kolay performans izleme için ölçümleri ve tanılama günlüklerini akışa alabilir. Kaynak kullanımını, çalışanları ve oturumları ve aşağıdaki Azure kaynaklarından birine bağlantıyı iletmek için bir veritabanı yapılandırabilirsiniz:

- **Azure SQL Analytics**: performans raporları, uyarılar ve risk azaltma önerilerini IÇEREN Azure SQL veritabanlarınızın akıllı bir şekilde izlenmesini sağlamak için.
- **Azure Event Hubs**: SQL veritabanı telemetrisini özel izleme çözümlerinizle veya etkin işlem hatlarınız ile tümleştirin.
- **Azure depolama**: fiyatın bir bölümü için çok miktarda Telemetriyi arşivlemek için.

    ![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

Çeşitli Azure hizmetleri tarafından desteklenen ölçümler ve günlük kategorileri hakkında daha fazla bilgi için bkz.:

- [Microsoft Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md)

Bu makalede, Azure SQL veritabanları, elastik havuzlar ve yönetilen örnekler için tanılama telemetrisini etkinleştirmenize yardımcı olacak rehberlik sunulmaktadır. Ayrıca, veritabanı tanılama telemetrisini görüntülemek için Azure SQL Analytics bir izleme aracı olarak nasıl yapılandırılacağını anlamanıza yardımcı olabilir.

## <a name="enable-logging-of-diagnostics-telemetry"></a>Tanılama telemetrinin günlüğe kaydedilmesini etkinleştir

Aşağıdaki yöntemlerden birini kullanarak ölçümler ve tanılama telemetri günlüğünü etkinleştirebilir ve yönetebilirsiniz:

- Azure portal
- PowerShell
- Azure CLI
- Azure Izleyici REST API
- Azure Resource Manager şablonu

Ölçümleri ve tanılama günlüğünü etkinleştirdiğinizde, tanılama telemetrisini toplamak için Azure Kaynak hedefini belirtmeniz gerekir. Kullanılabilir seçenekler şunlardır:

- Azure SQL Analytics
- Azure Event Hubs
- Azure Storage

Yeni bir Azure kaynağı temin edebilir veya var olan bir kaynağı seçebilirsiniz. **Tanılama ayarları** seçeneğini kullanarak bir kaynak seçtikten sonra toplanacak verileri belirtin.

## <a name="supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases"></a>Azure SQL veritabanları ve örnek veritabanları için desteklenen tanılama günlüğü

SQL veritabanlarında ölçümleri ve tanılama günlüğünü etkinleştirme-varsayılan olarak etkinleştirilmemiştir.

Azure SQL veritabanlarını ve örnek veritabanlarını aşağıdaki tanılama telemetrisini toplayacak şekilde ayarlayabilirsiniz:

| Veritabanları için telemetri izleme | Tek veritabanı ve havuza alınmış veritabanı desteği | Örnek veritabanı desteği |
| :------------------- | ----- | ----- |
| [Temel ölçümler](#basic-metrics): DTU/CPU yüzdesi, DTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturum yüzdesi, çalışan yüzdesi, depolama, depolama alanı yüzdesi ve XTP depolama yüzdesi içerir. | Evet | Hayır |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics): CPU kullanımı ve sorgu süresi istatistikleri gibi sorgu çalışma zamanı istatistikleri hakkındaki bilgileri içerir. | Evet | Evet |
| [Querystorewaitstatistics](#query-store-wait-statistics): CPU, günlük ve KILITLEME gibi sorgu bekleme istatistikleri (sorguların ne kadar bekledikleriniz) hakkındaki bilgileri içerir. | Evet | Evet |
| [Hatalar](#errors-dataset): Bir veritabanındaki SQL hatalarıyla ilgili bilgileri içerir. | Evet | Evet |
| [Databasewaitstatistics](#database-wait-statistics-dataset): Bir veritabanının farklı bekleme türlerini beklerken ne kadar zaman harcadığını gösteren bilgiler içerir. | Evet | Hayır |
| [Zaman aşımları](#time-outs-dataset): Bir veritabanındaki zaman aşımları hakkında bilgi içerir. | Evet | Hayır |
| [Bloklar](#blockings-dataset): Bir veritabanındaki olayları engelleme hakkında bilgi içerir. | Evet | Hayır |
| [Kilitlenmeler](#deadlocks-dataset): Bir veritabanındaki kilitlenme olayları hakkında bilgi içerir. | Evet | Hayır |
| Otomatik olarak [ayarlama](#automatic-tuning-dataset): Bir veritabanı için otomatik ayarlama önerileri hakkındaki bilgileri içerir. | Evet | Hayır |
| [Sqlinsıghts](#intelligent-insights-dataset): Bir veritabanının performansına Akıllı İçgörüler içerir. Daha fazla bilgi için bkz. [akıllı içgörüler](sql-database-intelligent-insights.md). | Evet | Evet |

> [!IMPORTANT]
> Elastik havuzlar ve yönetilen örneklerin içerdikleri veritabanlarından kendi ayrı tanılama telemetrisi vardır. Tanılama telemetrisi, aşağıda belirtildiği gibi, bu kaynakların her biri için ayrı olarak yapılandırıldığından, dikkat edilmesi önemlidir.

> [!NOTE]
> Güvenlik denetimi ve SQLSecurityAuditEvents günlükleri, veritabanı tanılama ayarlarından (ekranda gösterilmekle birlikte) etkinleştirilemez. Denetim günlüğü akışını etkinleştirmek için bkz. [Veritabanınız için denetim ayarlama](sql-database-auditing.md#subheading-2)ve [Azure Izleyici günlüklerinde ve Azure Event Hubs günlükleri denetleme](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).

## <a name="azure-portal"></a>Azure portal

Tanılama telemetrinin akışını yapılandırmak için Azure portal içindeki her bir tek, havuza alınmış veya örnek veritabanı için **Tanılama ayarları** menüsünü kullanabilirsiniz. Ayrıca, tanılama telemetrisi veritabanı kapsayıcıları için ayrı olarak da yapılandırılabilir: elastik havuzlar ve yönetilen örnekler. Tanılama telemetrisini akışa almak için aşağıdaki hedefleri ayarlayabilirsiniz: Azure depolama, Azure Event Hubs ve Azure Izleyici günlükleri.

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Elastik havuzlar için tanılama telemetrinin akışını yapılandırma

   ![Elastik havuz simgesi](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

Aşağıdaki tanılama telemetrisini toplamak için bir elastik havuz kaynağı ayarlayabilirsiniz:

| Resource | Telemetri izleme |
| :------------------- | ------------------- |
| **Elastik havuz** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) EDTU/CPU yüzdesi, EDTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, depolama sınırı ve XTP depolama yüzdesi içerir. |

Esnek havuzlardaki elastik havuzlar ve veritabanları için tanılama telemetrinin akışını yapılandırmak için, aşağıdakilerin **her ikisini de** ayrı ayrı yapılandırmanız gerekir:

- Elastik havuz için tanılama telemetrinin akışını etkinleştirin **ve**
- Elastik havuzdaki her veritabanı için tanılama telemetrinin akışını etkinleştirme

Bunun nedeni, elastik havuzun kendi telemetrisine sahip bir veritabanı kapsayıcısı olan tek bir veritabanı telemetrisinden ayrı bir veritabanıdır.

Esnek havuz kaynağı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure portal ' deki **elastik havuz** kaynağına gidin.
1. **Tanılama ayarları**' nı seçin.
1. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.

   ![Elastik havuzlar için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

1. Kendi başvurunuz için bir ayar adı girin.
1. Akış Tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleme**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
1. Log Analytics için, **Yapılandır** ' ı seçin ve yeni çalışma alanı oluştur' u seçerek yeni bir çalışma alanı oluşturun veya mevcut bir çalışma alanı seçin.
1. Elastik havuz tanılama telemetrisi onay kutusunu seçin: **Temel** ölçümler.
   ![Elastik havuzlar için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)
1. **Kaydet**’i seçin.
1. Ayrıca, sonraki bölümde açıklanan adımları izleyerek izlemek istediğiniz elastik havuzda bulunan her bir veritabanı için tanılama telemetrinin akışını yapılandırın.

> [!IMPORTANT]
> Esnek havuz için tanılama telemetrisini yapılandırmanın yanı sıra, aşağıda belirtildiği gibi, elastik havuzdaki her veritabanı için de tanılama telemetrisini yapılandırmanız gerekir.

### <a name="configure-streaming-of-diagnostics-telemetry-for-single-database-or-database-in-elastic-pool"></a>Tek veritabanı veya elastik havuzda veritabanı için tanılama telemetrinin akışını yapılandırın

   ![SQL veritabanı simgesi](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

Tek veya havuza alınmış veritabanları için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure **SQL veritabanı** kaynağına gidin.
1. **Tanılama ayarları**' nı seçin.
1. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.
   - Akış tanılama telemetrisine en fazla üç paralel bağlantı oluşturabilirsiniz.
   - Tanılama verilerinin paralel akışını birden çok kaynağa yapılandırmak için **+ Tanılama ayarı Ekle** ' yi seçin.

   ![Tek, havuza alınmış veya örnek veritabanları için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)
1. Kendi başvurunuz için bir ayar adı girin.
1. Akış Tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleme**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
1. Standart, olay tabanlı izleme deneyimi için, veritabanı tanılama günlüğü telemetrisi için aşağıdaki onay kutularını seçin: **Sqlinsıghts**, **automatictuning**, **QueryStoreRuntimeStatistics**, **querystorewaitstatistics**, **hatalar**, **databasewaitstatistics**, **zaman aşımları**, **bloklar**ve **kilitlenmeler**.
1. Gelişmiş, tek dakikalık tabanlı izleme deneyimi için **temel** ölçümler onay kutusunu seçin.
   ![Tek, havuza alınmış veya örnek veritabanları için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)
1. **Kaydet**’i seçin.
1. İzlemek istediğiniz her veritabanı için bu adımları tekrarlayın.

> [!NOTE]
> Güvenlik denetimi ve SQLSecurityAuditEvents günlükleri veritabanı tanılama ayarlarından (ekranda gösterilmese de) etkinleştirilemez. Denetim günlüğü akışını etkinleştirmek için bkz. [Veritabanınız için denetim ayarlama](sql-database-auditing.md#subheading-2)ve [Azure Izleyici günlüklerinde ve Azure Event Hubs günlükleri denetleme](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).
> [!TIP]
> İzlemek istediğiniz her Azure SQL veritabanı için bu adımları tekrarlayın.

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instances"></a>Yönetilen örnekler için tanılama telemetrinin akışını yapılandırın

   ![Yönetilen örnek simgesi](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

Aşağıdaki tanılama telemetrisini toplamak için bir yönetilen örnek kaynağı ayarlayabilirsiniz:

| Resource | Telemetri izleme |
| :------------------- | ------------------- |
| **Yönetilen örnek** | [Resourceusagestats](#resource-usage-stats-for-managed-instance) sanal çekirdekler sayısını, ortalama CPU YÜZDESINI, GÇ isteklerini, okunan/yazılan bayt, ayrılmış depolama alanını ve kullanılan depolama alanını içerir. |

Yönetilen örnek ve örnek veritabanları için tanılama telemetrinin akışını yapılandırmak için, aşağıdakilerin **her ikisini de** ayrı ayrı yapılandırmanız gerekir:

- Yönetilen örnek için tanılama telemetrinin akışını etkinleştirin **ve**
- Her örnek veritabanı için tanılama telemetrinin akışını etkinleştir

Bunun nedeni, yönetilen örneğin kendi telemetrisine sahip bir veritabanı kapsayıcısı, tek bir örnek veritabanı telemetrisinden ayrı bir veritabanıdır.

Yönetilen örnek kaynağı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure portal **yönetilen örnek** kaynağına gidin.
1. **Tanılama ayarları**' nı seçin.
1. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.

   ![Yönetilen örnek için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

1. Kendi başvurunuz için bir ayar adı girin.
1. Akış Tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleme**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
1. Log Analytics için, **Yapılandır** ' ı seçin ve yeni çalışma alanı oluştur' u seçerek yeni bir çalışma alanı oluşturun veya mevcut bir çalışma alanını kullanın.
1. Örnek tanılama telemetrisi onay kutusunu seçin: **Resourceusagestats**.
   ![Yönetilen örnek için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)
1. **Kaydet**’i seçin.
1. Ayrıca, sonraki bölümde açıklanan adımları izleyerek, izlemek istediğiniz yönetilen örnek içindeki her örnek veritabanı için tanılama telemetrinin akışını yapılandırın.

> [!IMPORTANT]
> Yönetilen bir örnek için tanılama telemetrisini yapılandırmaya ek olarak, aşağıda belirtildiği gibi her örnek veritabanı için tanılama telemetrisini de yapılandırmanız gerekir.

### <a name="configure-streaming-of-diagnostics-telemetry-for-instance-databases"></a>Örnek veritabanları için tanılama telemetrinin akışını yapılandırma

   ![Yönetilen örnek 'de örnek veritabanı simgesi](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

Örnek veritabanları için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Yönetilen örnek içinde **örnek veritabanı** kaynağına gidin.
1. **Tanılama ayarları**' nı seçin.
1. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.
   - Akış tanılama telemetrisine kadar en fazla üç (3) paralel bağlantı oluşturabilirsiniz.
   - Tanılama verilerinin paralel akışını birden çok kaynağa yapılandırmak için **+ Tanılama ayarı Ekle** ' yi seçin.

   ![Örnek veritabanları için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

1. Kendi başvurunuz için bir ayar adı girin.
1. Akış Tanılama verileri için bir hedef kaynak seçin: **Depolama hesabına arşivleme**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
1. Veritabanı tanılama telemetrisi onay kutularını seçin: **Sqlinsıghts**, **QueryStoreRuntimeStatistics**, **Querystorewaitstatistics** ve **hatalar**.
   ![Örnek veritabanları için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)
1. **Kaydet**’i seçin.
1. İzlemek istediğiniz her örnek veritabanı için bu adımları tekrarlayın.

> [!TIP]
> İzlemek istediğiniz her örnek veritabanı için bu adımları tekrarlayın.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

PowerShell kullanarak ölçümleri ve tanılama günlük kaydını etkinleştirebilirsiniz.

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı KIMLIĞI, hedef depolama hesabının kaynak KIMLIĞIDIR.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanınızın kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="to-configure-multiple-azure-resources"></a>Birden çok Azure kaynağını yapılandırmak için

Birden çok aboneliği desteklemek için PowerShell komut dosyasını kullanarak PowerShell 'i [kullanarak Azure Kaynak ölçümleri günlüğünü etkinleştirin](https://blogs.technet.microsoft.com/msoms/20../../enable-azure-resource-metrics-logging-using-powershell/).

Birden çok kaynaktan tanılama verilerini \<çalışma\> alanına göndermek için betiği `Enable-AzureRMDiagnostics.ps1` yürütürken bir parametre olarak $WSID çalışma alanı kaynak kimliği sağlayın.

- Tanılama verilerinize ilişkin hedefin çalışma \<alanı\> kimliğini $WSID almak için aşağıdaki betiği kullanın:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

   Alt \<kimliği \<\> \<abonelik kimliğiyle, kaynak grubu adı ile RG_NAME ve WS_NAME\> çalışma alanı adıyla değiştirin.\>

### <a name="azure-cli"></a>Azure CLI

Azure CLı kullanarak ölçümleri ve tanılama günlük kaydını etkinleştirebilirsiniz.

> [!NOTE]
> Tanılama günlüğünü etkinleştirme betikleri Azure CLı v 1.0 için desteklenir. CLı v 2.0 'ın Şu anda desteklendiğine lütfen emin olun.

- Bir depolama hesabında tanılama günlüklerinin depolanmasını etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Depolama hesabı KIMLIĞI, hedef depolama hesabının kaynak KIMLIĞIDIR.

- Tanılama günlüklerinin bir olay hub 'ına akışını etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Service Bus kural KIMLIĞI Şu biçimdeki bir dizedir:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Tanılama günlüklerinin bir Log Analytics çalışma alanına gönderilmesini etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

[Azure izleyici REST API kullanarak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)hakkında bilgi edinin.

### <a name="resource-manager-template"></a>Resource Manager şablonu

[Kaynak Yöneticisi şablonu kullanarak kaynak oluşturmada tanılama ayarlarının nasıl etkinleştirileceği](../azure-monitor/platform/diagnostic-logs-stream-template.md)hakkında bilgi edinin.

## <a name="stream-into-azure-sql-analytics"></a>Azure SQL Analytics içine akış

Azure SQL Analytics, Azure SQL veritabanlarının, elastik havuzların ve yönetilen örneklerin ölçeğini ölçekteki ve birden çok aboneliğin performansını izleyen bir bulut çözümüdür. Azure SQL veritabanı performans ölçümlerini toplamanıza ve görselleştirmenize yardımcı olabilir ve performans sorunlarını gidermek için yerleşik zekaya sahiptir.

![Azure SQL Analytics genel bakış](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

SQL veritabanı ölçümleri ve tanılama günlükleri, portalda Tanılama Ayarları sekmesinde yerleşik **Log Analytics gönder** seçeneği kullanılarak Azure SQL Analytics akışı yapılabilir. Log Analytics 'i PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla bir tanılama ayarı kullanarak da etkinleştirebilirsiniz.

### <a name="installation-overview"></a>Yüklemeye genel bakış

Azure SQL Analytics bir SQL veritabanı filosu izleyebilirsiniz. Aşağıdaki adımları uygulayın:

1. Azure Marketi 'nden bir Azure SQL Analytics çözümü oluşturun.
2. Çözümde bir izleme çalışma alanı oluşturun.
3. Tanılama telemetrisini çalışma alanına akışa almak için veritabanlarını yapılandırın.

Elastik havuzlar veya yönetilen örnekler kullanıyorsanız, bu kaynaklardan tanılama telemetri akışını da yapılandırmanız gerekir.

### <a name="create-azure-sql-analytics-resource"></a>Azure SQL Analytics kaynağı oluşturma

1. Azure Market 'te Azure SQL Analytics arayın ve seçin.

   ![Portalda Azure SQL Analytics arama](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)

2. Çözümün genel bakış ekranında **Oluştur** ' u seçin.

3. Azure SQL Analytics formunu, gereken ek bilgilerle girin: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Portalda Azure SQL Analytics yapılandırma](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Onaylamak için **Tamam** ' ı seçin ve ardından **Oluştur**' u seçin.

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Ölçümleri ve tanılama günlüklerini kaydetmek için veritabanlarını yapılandırma

Veritabanlarının, ölçüm kayıtlarını nerede Azure portal kullanarak yapılandırmanın en kolay yolu. Daha önce açıklandığı gibi, Azure portal SQL veritabanı kaynağına gidin ve **Tanılama ayarları**' nı seçin.

Elastik havuzlar veya yönetilen örnekler kullanıyorsanız, tanılama telemetrinin çalışma alanına akışını sağlamak için bu kaynaklarda tanılama ayarlarını da yapılandırmanız gerekir.

### <a name="use-the-sql-analytics-solution-for-monitoring-and-alerting"></a>İzleme ve uyarma için SQL Analytics çözümünü kullanma

SQL veritabanı kaynaklarınızı görüntülemek için SQL Analytics 'i hiyerarşik bir pano olarak kullanabilirsiniz.

- SQL Analytics çözümünü nasıl kullanacağınızı öğrenmek için bkz. SQL [Analytics çözümünü kullanarak SQL veritabanını izleme](../log-analytics/log-analytics-azure-sql.md).
- SQL veritabanı ve yönetilen örnek için SQL Analytics 'e göre uyarıları ayarlamayı öğrenmek için bkz. [SQL veritabanı ve yönetilen örnek için uyarı oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts).

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

SQL veritabanı ölçümleri ve tanılama günlüklerini, Azure portal **bir olay hub 'ı** seçeneğinde yerleşik akışı kullanarak Event Hubs ' a akışını sağlayabilirsiniz. Ayrıca, PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla bir tanılama ayarı kullanarak Service Bus kuralı KIMLIĞINI etkinleştirebilirsiniz.

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Event Hubs ölçümler ve tanılama günlükleri ile ne yapmalı

Seçili veriler Event Hubs akışa alındıktan sonra, Gelişmiş izleme senaryolarına olanak tanımak için bir adım daha yakınına sahip olursunuz. Event Hubs, bir olay işlem hattının ön kapısı olarak davranır. Veriler bir olay hub 'ına toplandıktan sonra, gerçek zamanlı bir analiz sağlayıcısı veya bir depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs, bir olay akışının üretimini bu olayların kullanımından ayırır. Bu şekilde olay tüketicileri, olaylara kendi zamanlamalarıyla erişebilirler. Event Hubs hakkında daha fazla bilgi için bkz.

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Event Hubs için akan ölçümleri kullanabilirsiniz:

- **Power BI için etkin yol verilerini akışa alarak hizmet durumunu görüntüleyin**. Event Hubs, Stream Analytics ve Power BI kullanarak ölçümler ve tanılama verilerinizi Azure hizmetlerinizden neredeyse gerçek zamanlı içgörüler halinde kolayca dönüştürebilirsiniz. Bir olay hub 'ı ayarlama, Stream Analytics ile verileri işleme ve çıkış olarak Power BI kullanma hakkında genel bilgi için bkz. [Stream Analytics ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

- **Üçüncü taraf günlüğe kaydetme ve telemetri akışlarına yönelik günlükleri akışa**alma. Event Hubs akışı kullanarak, ölçümlerinizi ve tanılama günlüklerinizi çeşitli üçüncü taraf izleme ve Log Analytics çözümlerinde bulabilirsiniz.

- **Özel telemetri ve günlüğe kaydetme platformu oluşturun**. Zaten özel olarak oluşturulmuş bir telemetri platformudur veya bir tane oluşturmayı düşünülüyor musunuz? Yüksek düzeyde ölçeklenebilir yayımla-abone ol Event Hubs, tanılama günlüklerini esnek bir şekilde almanızı sağlar. [Küresel ölçekli bir telemetri platformunda Event Hubs kullanmak için bkz. Dan Rosanova Kılavuzu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Depolama alanına akış

Azure depolama 'da SQL veritabanı ölçümleri ve tanılama günlüklerini, Azure portal **bir depolama hesabı** seçeneğinde yerleşik Arşiv ' i kullanarak da saklayabilirsiniz. Ayrıca, PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla bir tanılama ayarı kullanarak depolamayı etkinleştirebilirsiniz.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Depolama hesabındaki ölçüm ve tanılama günlüklerinin şeması

Ölçümler ve tanılama günlükleri koleksiyonu ayarladıktan sonra, ilk veri satırları kullanılabilir olduğunda seçtiğiniz depolama hesabında bir depolama kapsayıcısı oluşturulur. Blobların yapısı şu şekilde olur:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Ya da daha basit:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, temel ölçümler için bir blob adı şu olabilir:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Elastik bir havuzdan veri depolamak için bir blob adı şöyle görünür:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

## <a name="data-retention-policy-and-pricing"></a>Veri bekletme ilkesi ve fiyatlandırma

Event Hubs veya depolama hesabı ' nı seçerseniz bir bekletme ilkesi belirleyebilirsiniz. Bu ilke seçilen bir zaman aralığından daha eski olan verileri siler. Log Analytics belirtirseniz, bekletme ilkesi seçili fiyatlandırma katmanına bağlıdır. Bu durumda, sunulan ücretsiz veri alma birimleri her ay çeşitli veritabanlarının ücretsiz olarak izlenmesini etkinleştirebilir. Her türlü ücretsiz birimi aşan tanılama telemetrisine ilişkin tüm tüketimler maliyette bulunabilir. Yoğun iş yüklerine sahip etkin veritabanlarının, boştaki veritabanlarından daha fazla veri aldığını unutmayın. Daha fazla bilgi için bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).

Azure SQL Analytics kullanıyorsanız, Azure SQL Analytics gezinti menüsünde **OMS çalışma alanı** ' nı seçerek ve ardından **kullanım** ve **tahmini maliyetler**' i seçerek, çözüm içindeki veri alma kullanımınızı izleyebilirsiniz.

## <a name="metrics-and-logs-available"></a>Ölçümler ve Günlükler kullanılabilir

Azure SQL veritabanı, elastik havuzlar ve yönetilen örnek için izleme telemetrisini aşağıda bulabilirsiniz. SQL Analytics içinde toplanan izleme telemetrisi, [Azure izleyici günlük sorguları](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) dilini kullanarak kendi özel analiz ve uygulama geliştirme için kullanılabilir.

## <a name="basic-metrics"></a>Temel ölçümler

Kaynağa göre temel ölçümler hakkında daha fazla bilgi için aşağıdaki tablolara bakın.

> [!NOTE]
> Temel ölçümler seçeneği önceden tüm ölçümler olarak bilinirdi. Yapılan değişiklik yalnızca adlandırma amaçlıdır ve izlenen ölçümlerde hiçbir değişiklik yapılmadı. Bu değişiklik, gelecekte ek ölçüm kategorilerinin tanıtılmasıyla izin verecek şekilde başlatıldı.

### <a name="basic-metrics-for-elastic-pools"></a>Elastik havuzlar için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Elastik havuz|eDTU yüzdesi, eDTU kullanımı, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |

### <a name="basic-metrics-for-azure-sql-databases"></a>Azure SQL veritabanları için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Azure SQL veritabanı|DTU yüzdesi, DTU kullanımı, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve Çık |

## <a name="basic-logs"></a>Temel Günlükler

Tüm günlüklerde kullanılabilen telemetri ayrıntıları aşağıdaki tablolarda belgelenmiştir. Lütfen belirli bir veritabanı özelliği için hangi günlüklerin desteklendiğini anlamak için bkz. [desteklenen tanılama günlüğü](#supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases) -Azure SQL Single, havuza alınmış veya örnek veritabanı.

### <a name="resource-usage-stats-for-managed-instance"></a>Yönetilen örnek için kaynak kullanım istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure|
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her ResourceUsageStats |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her MANAGEDINSTANCES |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Yönetilen örnek adı |
|RESOURCEID|Kaynak URI 'SI |
|SKU_s|Yönetilen örnek Ürün SKU 'SU |
|virtual_core_count_s|Kullanılabilir sanal çekirdek sayısı |
|avg_cpu_percent_s|Ortalama CPU yüzdesi |
|reserved_storage_mb_s|Yönetilen örnekteki ayrılmış depolama kapasitesi |
|storage_space_used_mb_s|Yönetilen örnekte kullanılan depolama alanı |
|io_requests_s|IOPS sayısı |
|io_bytes_read_s|IOPS bayt okuma |
|io_bytes_written_s|Bayt yazılan ıOPS |

### <a name="query-store-runtime-statistics"></a>Sorgu deposu çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her QueryStoreRuntimeStatistics |
|OperationName|İşlemin adı. Her QueryStoreRuntimeStatisticsEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|query_hash_s|Sorgu karması |
|query_plan_hash_s|Sorgu planı karması |
|statement_sql_handle_s|Ekstre SQL tanıtıcısı |
|interval_start_time_d|1900-1-1 arasındaki onay işareti sayısında aralığın DateTimeOffset değerini Başlat |
|interval_end_time_d|1900-1-1 'den onay işareti sayısı cinsinden aralığın bitiş aralığı. |
|logical_io_writes_d|Toplam mantıksal GÇ yazma sayısı |
|max_logical_io_writes_d|Yürütme başına en fazla mantıksal GÇ yazma sayısı |
|physical_io_reads_d|Toplam fiziksel GÇ okuma sayısı |
|max_physical_io_reads_d|Yürütme başına en fazla mantıksal GÇ okuma sayısı |
|logical_io_reads_d|Toplam mantıksal GÇ okuma sayısı |
|max_logical_io_reads_d|Yürütme başına en fazla mantıksal GÇ okuma sayısı |
|execution_type_d|Yürütme türü |
|count_executions_d|Sorgunun yürütmelerinin sayısı |
|cpu_time_d|Sorgu tarafından mikrosaniye cinsinden tüketilen toplam CPU süresi |
|max_cpu_time_d|Mikrosaniye cinsinden tek bir yürütme ile en fazla CPU süresi tüketicisi |
|dop_d|Paralellik derecenin toplamı |
|max_dop_d|Tek yürütme için kullanılan en yüksek paralellik derecesi |
|rowcount_d|Döndürülen toplam satır sayısı |
|max_rowcount_d|Tek yürütmede döndürülen en fazla satır sayısı |
|query_max_used_memory_d|KB cinsinden kullanılan toplam bellek miktarı |
|max_query_max_used_memory_d|KB cinsinden tek bir yürütme tarafından kullanılan maksimum bellek miktarı |
|duration_d|Mikrosaniye cinsinden toplam yürütme süresi |
|max_duration_d|Tek yürütmenin en fazla yürütme süresi |
|num_physical_io_reads_d|Toplam fiziksel okuma sayısı |
|max_num_physical_io_reads_d|Yürütme başına en fazla fiziksel okuma sayısı |
|log_bytes_used_d|Kullanılan günlük baytlarının toplam miktarı |
|max_log_bytes_used_d|Yürütme başına kullanılan günlük baytlarının maksimum miktarı |
|query_id_d|Sorgu deposundaki sorgunun KIMLIĞI |
|plan_id_d|Sorgu deposundaki planın KIMLIĞI |

[Sorgu deposu çalışma zamanı istatistikleri verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql)hakkında daha fazla bilgi edinin.

### <a name="query-store-wait-statistics"></a>Sorgu deposu bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her QueryStoreWaitStatistics |
|OperationName|İşlemin adı. Her QueryStoreWaitStatisticsEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|wait_category_s|Bekleme kategorisi |
|is_parameterizable_s|Sorgu parametreleştirilebilir mi |
|statement_type_s|Deyimin türü |
|statement_key_hash_s|Ekstre anahtarı karması |
|exec_type_d|Yürütme türü |
|total_query_wait_time_ms_d|Belirli bir bekleme kategorisindeki sorgunun toplam bekleme süresi |
|max_query_wait_time_ms_d|Belirli bir bekleme kategorisinde tek tek yürütmede sorgunun en fazla bekleme süresi |
|query_param_type_d|0 |
|query_hash_s|Sorgu deposunda sorgu karması |
|query_plan_hash_s|Sorgu deposunda sorgu planı karması |
|statement_sql_handle_s|Sorgu deposundaki ekstre tanıtıcısı |
|interval_start_time_d|1900-1-1 arasındaki onay işareti sayısında aralığın DateTimeOffset değerini Başlat |
|interval_end_time_d|1900-1-1 'den onay işareti sayısı cinsinden aralığın bitiş aralığı. |
|count_executions_d|Sorgunun yürütmelerinin sayısı |
|query_id_d|Sorgu deposundaki sorgunun KIMLIĞI |
|plan_id_d|Sorgu deposundaki planın KIMLIĞI |

[Sorgu deposu bekleme istatistikleri verileri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql)hakkında daha fazla bilgi edinin.

### <a name="errors-dataset"></a>Hatalar veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her Hatalar |
|OperationName|İşlemin adı. Her ErrorEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|`Message`|Düz metinde hata iletisi |
|user_defined_b|Hata Kullanıcı tanımlı biti |
|error_number_d|Hata kodu |
|severity|Hatanın önem derecesi |
|state_d|Hatanın durumu |
|query_hash_s|Varsa, başarısız sorgunun sorgu karması |
|query_plan_hash_s|Varsa, başarısız sorgunun sorgu planı karması |

[SQL Server hata iletileri](https://msdn.microsoft.com/library/cc645603.aspx)hakkında daha fazla bilgi edinin.

### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her DatabaseWaitStatistics |
|OperationName|İşlemin adı. Her DatabaseWaitStatisticsEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|wait_type_s|Bekleme türünün adı |
|start_utc_date_t [UTC]|Ölçülen dönem başlangıç zamanı |
|end_utc_date_t [UTC]|Ölçülen dönem bitiş zamanı |
|delta_max_wait_time_ms_d|Yürütme başına en fazla beklenen süre |
|delta_signal_wait_time_ms_d|Toplam sinyal bekleme süresi |
|delta_wait_time_ms_d|Dönemdeki toplam bekleme süresi |
|delta_waiting_tasks_count_d|Bekleyen görev sayısı |

[Veritabanı bekleme istatistikleri](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)hakkında daha fazla bilgi edinin.

### <a name="time-outs-dataset"></a>Zaman aşımları veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her Zaman Aşımları |
|OperationName|İşlemin adı. Her TimeoutEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|error_state_d|Hata durum kodu |
|query_hash_s|Varsa sorgu karması |
|query_plan_hash_s|Varsa sorgu planı karması |

### <a name="blockings-dataset"></a>Blok veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her Öbekleri |
|OperationName|İşlemin adı. Her BlockEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|lock_mode_s|Sorgu tarafından kullanılan kilit modu |
|resource_owner_type_s|Kilidin sahibi |
|blocked_process_filtered_s|Engellenen işlem raporu XML 'i |
|duration_d|Mikrosaniye cinsinden kilit süresi |

### <a name="deadlocks-dataset"></a>Kilitlenmeler veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC] |Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her Kilitlenmeler |
|OperationName|İşlemin adı. Her DeadlockEvent |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|deadlock_xml_s|Kilitlenme rapor XML 'i |

### <a name="automatic-tuning-dataset"></a>Otomatik ayarlama veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Her Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Type|Her AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Her MICROSOFT. SQL |
|Category|Kategorinin adı. Her Otomatik olarak ayarlama |
|Resource|Kaynağın adı |
|KaynakTürü|Kaynak türünün adı. Her SUNUCULAR/VERITABANLARI |
|SubscriptionId|Veritabanı için abonelik GUID 'SI |
|ResourceGroup|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|LogicalDatabaseName_s|Veritabanının adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|RESOURCEID|Kaynak URI 'SI |
|RecommendationHash_s|Otomatik ayarlama önerisinin benzersiz karması |
|OptionName_s|Otomatik ayarlama işlemi |
|Schema_s|Veritabanı şeması |
|Table_s|Etkilenen tablo |
|IndexName_s|Dizin adı |
|IndexColumns_s|Sütun adı |
|IncludedColumns_s|Dahil edilen sütunlar |
|EstimatedImpact_s|Otomatik ayarlama önerisi JSON ' inin tahmini etkisi |
|Event_s|Otomatik ayarlama olayının türü |
|Timestamp_t|Son güncelleme zaman damgası |

### <a name="intelligent-insights-dataset"></a>Akıllı İçgörüler veri kümesi

[Akıllı içgörüler günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Günlüğe kaydetmeyi etkinleştirme ve çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini anlama hakkında bilgi edinmek için bkz.:

- [Microsoft Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md)

Event Hubs hakkında bilgi edinmek için şunu okuyun:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Log Analytics 'ten telemetri temelinde uyarı ayarlamayı öğrenmek için bkz.:

- [SQL veritabanı ve yönetilen örnek için uyarı oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts)
