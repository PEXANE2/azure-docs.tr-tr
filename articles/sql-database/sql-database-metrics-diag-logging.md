---
title: Ölçümler ve tanılama günlükleri
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
ms.date: 02/24/2020
ms.openlocfilehash: dead8b95446009880c36f97a095aee4aaae0579d
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78365456"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL veritabanı ölçümleri ve tanılama günlüğü

Bu makalede, Azure SQL veritabanları için Azure portal, PowerShell, Azure CLı, REST API ve Azure Resource Manager şablonu aracılığıyla tanılama telemetrisine yönelik günlüğü nasıl etkinleştireceğinizi ve yapılandıracağınızı öğreneceksiniz. Tek veritabanları, havuza alınmış veritabanları, elastik havuzlar, yönetilen örnekler ve örnek veritabanları, ölçümleri ve tanılama günlüklerini aşağıdaki Azure kaynaklarından birine kayükleyebilir:

- **Azure SQL Analytics**: performans raporları, uyarılar ve risk azaltma önerilerini içeren veritabanlarınızı akıllı olarak izlemeyi alın
- **Azure Event Hubs**: veritabanı telemetrisini özel izleme çözümlerinizle veya etkin işlem hatlarınız ile tümleştirin
- **Azure depolama**: fiyatın bir bölümü için büyük miktarlarda telemetri Arşivle

Bu Tanılamalar, daha kolay performans izleme için kaynak kullanımı ve sorgu yürütme istatistiklerini ölçmek için kullanılabilir.

![Mimari](./media/sql-database-metrics-diag-logging/architecture.png)

Çeşitli Azure hizmetleri tarafından desteklenen ölçümler ve günlük kategorileri hakkında daha fazla bilgi için bkz.:

- [Microsoft Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)

## <a name="enable-logging-of-diagnostics-telemetry"></a>Tanılama telemetrinin günlüğe kaydedilmesini etkinleştir

Aşağıdaki yöntemlerden birini kullanarak ölçümler ve tanılama telemetri günlüğünü etkinleştirebilir ve yönetebilirsiniz:

- Azure portalı
- PowerShell
- Azure CLI
- Azure Izleyici REST API
- Azure Resource Manager şablonu

Ölçümleri ve tanılama günlüğünü etkinleştirdiğinizde, tanılama telemetrisini toplamak için Azure Kaynak hedefini belirtmeniz gerekir. Kullanılabilir seçenekler şunlardır:

- [Azure SQL Analizi](#stream-diagnostic-telemetry-into-sql-analytics)
- [Azure Event Hubs](#stream-diagnostic-telemetry-into-event-hubs)
- [Azure Depolama](#stream-diagnostic-telemetry-into-azure-storage)

Yeni bir Azure kaynağı temin edebilir veya var olan bir kaynağı seçebilirsiniz. **Tanılama ayarları** seçeneğini kullanarak bir kaynak seçtikten sonra toplanacak verileri belirtin.

## <a name="supported-diagnostic-logging-for-azure-sql-databases"></a>Azure SQL veritabanları için desteklenen tanılama günlüğü

Azure SQL veritabanlarını aşağıdaki tanılama telemetrisini toplayacak şekilde ayarlayabilirsiniz:

| Veritabanları için telemetri izleme | Tek veritabanı ve havuza alınmış veritabanı desteği | Yönetilen örnek veritabanı desteği |
| :------------------- | ----- | ----- |
| [Temel ölçümler](#basic-metrics): DTU/CPU YÜZDESI, DTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturum yüzdesi, çalışan yüzdesi, depolama, depolama alanı yüzdesi ve XTP depolama yüzdesi içerir. | Yes | Hayır |
| [Örnek ve uygulama gelişmiş](#advanced-metrics): tempdb sistem veritabanı verilerini ve günlük dosyası boyutunu ve kullanılan tempdb yüzde günlük dosyasını içerir. | Yes | Hayır |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics): CPU kullanımı ve sorgu süresi istatistikleri gibi sorgu çalışma zamanı istatistikleri hakkındaki bilgileri içerir. | Yes | Yes |
| [Querystorewaitstatistics](#query-store-wait-statistics): CPU, günlük ve kilitleme gibi sorgu bekleme istatistikleri (sorgularınızın ne kadar bekledikleriniz) hakkındaki bilgileri içerir. | Yes | Yes |
| [Hatalar](#errors-dataset): BIR veritabanındaki SQL hatalarıyla ilgili bilgileri içerir. | Yes | Yes |
| [Databasewaitstatistics](#database-wait-statistics-dataset): bir veritabanının farklı bekleme türlerini beklerken ne kadar zaman harcadığını gösteren bilgiler içerir. | Yes | Hayır |
| [Zaman aşımları](#time-outs-dataset): bir veritabanındaki zaman aşımları hakkında bilgi içerir. | Yes | Hayır |
| [Bloklar](#blockings-dataset): bir veritabanındaki olayları engelleme hakkında bilgi içerir. | Yes | Hayır |
| [Kilitlenmeler](#deadlocks-dataset): bir veritabanındaki kilitlenme olayları hakkında bilgi içerir. | Yes | Hayır |
| Otomatik [ayarlama: bir](#automatic-tuning-dataset)veritabanı için otomatik ayarlama önerileri hakkındaki bilgileri içerir. | Yes | Hayır |
| [Sqlinsıghts](#intelligent-insights-dataset): bir veritabanının performansına akıllı içgörüler içerir. Daha fazla bilgi için bkz. [akıllı içgörüler](sql-database-intelligent-insights.md). | Yes | Yes |

> [!IMPORTANT]
> Elastik havuzlar ve yönetilen örneklerin içerdikleri veritabanlarından kendi ayrı tanılama telemetrisi vardır. Tanılama telemetrisi bu kaynakların her biri için ayrı olarak yapılandırıldığından bu, dikkat edilmesi açısından önemlidir.
>
> Denetim günlüğü akışını etkinleştirmek için bkz. [Azure izleyici günlüklerinde ve azure Event Hubs](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242), [Veritabanınız için denetim ayarlama](sql-database-auditing.md#subheading-2) ve günlükleri denetleme.
>
> Tanılama ayarları, ana, msdb, model, kaynak ve tempdb veritabanları gibi **sistem veritabanları**için yapılandırılamaz.

## <a name="configure-streaming-of-diagnostic-telemetry"></a>Tanılama telemetrinin akışını yapılandırma

Tanılama telemetrinin akışını etkinleştirmek ve yapılandırmak için Azure portal **Tanılama ayarları** menüsünü kullanabilirsiniz. Ayrıca, tanılama telemetrinin akışını yapılandırmak için PowerShell, Azure CLı, [REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)ve [Kaynak Yöneticisi şablonlarını](../azure-monitor/platform/diagnostic-settings-template.md) kullanabilirsiniz. Tanılama telemetrisini akışa almak için aşağıdaki hedefleri ayarlayabilirsiniz: Azure depolama, Azure Event Hubs ve Azure Izleyici günlükleri.

> [!IMPORTANT]
> Tanılama telemetrinin günlüğe kaydedilmesi varsayılan olarak etkin değildir.

# <a name="azure-portal"></a>[Azure portalında](#tab/azure-portal)

### <a name="elastic-pools"></a>Esnek havuzlar

Aşağıdaki tanılama telemetrisini toplamak için bir elastik havuz kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Elastik havuz** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) EDTU/CPU yüzdesi, EDTU/CPU sınırı, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, depolama sınırı ve XTP depolama yüzdesi içerir. |

Elastik havuzlar ve havuza alınmış veritabanları için tanılama telemetrinin akışını yapılandırmak için her birini ayrı ayrı yapılandırmanız gerekir:

- Elastik havuz için tanılama telemetrinin akışını etkinleştirme
- Elastik havuzdaki her veritabanı için tanılama telemetrinin akışını etkinleştirme

Elastik havuz kapsayıcısının, tek tek havuza alınmış her veritabanı telemetrisinden ayrı bir telemetri vardır.

Esnek havuz kaynağı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure portal ' deki **elastik havuz** kaynağına gidin.
2. **Tanılama ayarları**' nı seçin.
3. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.

   ![Elastik havuzlar için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış Tanılama verileri için bir hedef kaynak seçin: **depolama hesabına Arşivle**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
6. Log Analytics için, **Yapılandır** ' ı seçin ve yeni çalışma **alanı oluştur ' u seçerek yeni**bir çalışma alanı oluşturun veya mevcut bir çalışma alanı seçin.
7. Elastik havuz tanılama telemetrisi için onay kutusunu seçin: **temel** ölçümler.
   Elastik havuzlar için tanılamayı yapılandırma ![](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

8. **Kaydet**’i seçin.
9. Ayrıca, sonraki bölümde açıklanan adımları izleyerek izlemek istediğiniz elastik havuzda bulunan her bir veritabanı için tanılama telemetrinin akışını yapılandırın.

> [!IMPORTANT]
> Esnek havuz için tanılama telemetrisini yapılandırmanın yanı sıra, elastik havuzdaki her bir veritabanı için tanılama telemetrisini de yapılandırmanız gerekir.

### <a name="single-or-pooled-database"></a>Tek veya havuza alınmış veritabanı

Aşağıdaki tanılama telemetrisini toplamak için, tek veya havuza alınmış bir veritabanı kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Tek veya havuza alınmış veritabanı** | [Temel ölçümler](sql-database-metrics-diag-logging.md#basic-metrics) DTU YÜZDESI, DTU kullanımı, DTU SıNıRı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturum yüzdesi, çalışan yüzdesi, depolama, depolama alanı yüzdesi, XTP depolama yüzdesi ve kilitlenmeleri içerir. |

Tek veya havuza alınmış bir veritabanı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure **SQL veritabanı** kaynağına gidin.
2. **Tanılama ayarları**' nı seçin.
3. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin. Akış tanılama telemetrisine en fazla üç paralel bağlantı oluşturabilirsiniz.
4. Tanılama verilerinin paralel akışını birden çok kaynağa yapılandırmak için **Tanılama ayarı Ekle** ' yi seçin.

   ![Tek ve havuza alınmış veritabanları için tanılamayı etkinleştirme](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

5. Kendi başvurunuz için bir ayar adı girin.
6. Akış Tanılama verileri için bir hedef kaynak seçin: **depolama hesabına Arşivle**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
7. Standart, olay tabanlı izleme deneyimi için, veritabanı tanılama günlüğü telemetrisi için aşağıdaki onay kutularını seçin: **Sqlinsıghts**, **automatictuning**, **QueryStoreRuntimeStatistics**, **querystorewaitstatistics**, **hatalar**, **databasewaitstatistics**, **zaman aşımları**, **bloklar**ve **kilitlenmeler**.
8. Gelişmiş, tek dakikalık tabanlı izleme deneyimi için **temel** ölçümler onay kutusunu seçin.

   ![Tek, havuza alınmış veya örnek veritabanları için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)
9. **Kaydet**’i seçin.
10. İzlemek istediğiniz her veritabanı için bu adımları tekrarlayın.

> [!TIP]
> İzlemek istediğiniz her bir tek ve havuza alınmış veritabanı için bu adımları yineleyin.

### <a name="managed-instance"></a>Yönetilen örnek

Aşağıdaki tanılama telemetrisini toplamak için bir yönetilen örnek kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Yönetilen örnek** | [Resourceusagestats](#resource-usage-stats-for-managed-instances) sanal çekirdekler sayısını, ortalama CPU YÜZDESINI, GÇ isteklerini, okunan/yazılan bayt, ayrılmış depolama alanını ve kullanılan depolama alanını içerir. |

Yönetilen örnek ve örnek veritabanları için tanılama telemetrinin akışını yapılandırmak için, her birini ayrı ayrı yapılandırmanız gerekir:

- Yönetilen örnek için tanılama telemetrinin akışını etkinleştir
- Her örnek veritabanı için tanılama telemetrinin akışını etkinleştir

Yönetilen örnek kapsayıcısının her bir örnek veritabanının telemetrisinden ayrı kendi telemetrisi vardır.

Yönetilen örnek kaynağı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Azure portal **yönetilen örnek** kaynağına gidin.
2. **Tanılama ayarları**' nı seçin.
3. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.

   ![Yönetilen örnek için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış Tanılama verileri için bir hedef kaynak seçin: **depolama hesabına Arşivle**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
6. Log Analytics için, **Yapılandır** ' ı seçin ve yeni çalışma **alanı oluştur ' u seçerek yeni**bir çalışma alanı oluşturun veya mevcut bir çalışma alanını kullanın.
7. Örnek tanılama telemetrisi: **Resourceusagestats**onay kutusunu seçin.

   ![Yönetilen örnek için tanılamayı yapılandırma](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)

8. **Kaydet**’i seçin.
9. Ayrıca, sonraki bölümde açıklanan adımları izleyerek, izlemek istediğiniz yönetilen örnek içindeki her örnek veritabanı için tanılama telemetrinin akışını yapılandırın.

> [!IMPORTANT]
> Yönetilen bir örnek için tanılama telemetrisini yapılandırmaya ek olarak, her örnek veritabanı için de tanılama telemetrisini yapılandırmanız gerekir.

### <a name="instance-database"></a>Örnek veritabanı

Aşağıdaki tanılama telemetrisini toplamak için bir örnek veritabanı kaynağı ayarlayabilirsiniz:

| Kaynak | Telemetri izleme |
| :------------------- | ------------------- |
| **Örnek veritabanı** | [Resourceusagestats](#resource-usage-stats-for-managed-instances) sanal çekirdekler sayısını, ortalama CPU YÜZDESINI, GÇ isteklerini, okunan/yazılan bayt, ayrılmış depolama alanını ve kullanılan depolama alanını içerir. |

Bir örnek veritabanı için tanılama telemetrinin akışını etkinleştirmek için şu adımları izleyin:

1. Yönetilen örnek içinde **örnek veritabanı** kaynağına gidin.
2. **Tanılama ayarları**' nı seçin.
3. Önceki ayarlar yoksa **tanılamayı aç** ' ı seçin veya önceki bir ayarı düzenlemek Için **ayarı Düzenle** ' yi seçin.
   - Akış tanılama telemetrisine kadar en fazla üç (3) paralel bağlantı oluşturabilirsiniz.
   - Tanılama verilerinin paralel akışını birden çok kaynağa yapılandırmak için **+ Tanılama ayarı Ekle** ' yi seçin.

   ![Örnek veritabanları için tanılamayı etkinleştir](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. Kendi başvurunuz için bir ayar adı girin.
5. Akış Tanılama verileri için bir hedef kaynak seçin: **depolama hesabına Arşivle**, **bir olay hub 'ına akış**veya **Log Analytics gönderme**.
6. Veritabanı tanılama telemetrisi için onay kutularını seçin: **Sqlinsıghts**, **QueryStoreRuntimeStatistics**, **Querystorewaitstatistics**ve **hatalar**.
   örnek veritabanları için tanılamayı yapılandırma ![](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)
7. **Kaydet**’i seçin.
8. İzlemek istediğiniz her örnek veritabanı için bu adımları tekrarlayın.

> [!TIP]
> İzlemek istediğiniz her örnek veritabanı için bu adımları tekrarlayın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

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

**Birden çok Azure kaynağını yapılandırmak için**

Birden çok aboneliği desteklemek için PowerShell komut dosyasını kullanarak PowerShell 'i [kullanarak Azure Kaynak ölçümleri günlüğünü etkinleştirin](https://blogs.technet.microsoft.com/msoms/20../../enable-azure-resource-metrics-logging-using-powershell/).

Birden çok kaynaktan tanılama verilerini çalışma alanına göndermek için komut dosyası `Enable-AzureRMDiagnostics.ps1` yürütürken $WSID\> \<çalışma alanı kaynak KIMLIĞINI bir parametre olarak belirtin.

- Tanılama verilerinize yönelik hedefin $WSID\> \<çalışma alanı KIMLIĞINI almak için aşağıdaki betiği kullanın:

    ```powershell
    $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

   \<Subıd\> abonelik KIMLIĞIYLE değiştirin, kaynak grubu adıyla \<RG_NAME\> ve \<WS_NAME çalışma alanı adıyla değiştirin.\>

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak ölçümleri ve tanılama günlük kaydını etkinleştirebilirsiniz.

> [!IMPORTANT]
> Tanılama günlüğünü etkinleştirme betikleri Azure CLı v 1.0 için desteklenir. Azure CLı v 2.0 şu anda desteklenmiyor.

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

---

## <a name="stream-diagnostic-telemetry-into-sql-analytics"></a>SQL Analytics 'e akış tanılama telemetrisi

Azure SQL Analytics, tek veritabanlarının, elastik havuzların ve havuza alınan veritabanlarının performansını ve yönetilen örnekleri ve örnek veritabanlarını ölçeklendirmekte ve birden çok aboneliğe göre izleyen bir bulut çözümüdür. Azure SQL veritabanı performans ölçümlerini toplamanıza ve görselleştirmenize yardımcı olabilir ve performans sorunlarını gidermek için yerleşik zekaya sahiptir.

![Azure SQL Analytics genel bakış](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

SQL veritabanı ölçümleri ve tanılama günlükleri, Azure portal Tanılama Ayarları sekmesindeki yerleşik **Log Analytics gönder** seçeneği kullanılarak Azure SQL Analytics akışı yapılabilir. Log Analytics 'i PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla tanılama ayarlarını kullanarak da etkinleştirebilirsiniz.

### <a name="installation-overview"></a>Yüklemeye genel bakış

Aşağıdaki adımları gerçekleştirerek, Azure SQL veritabanlarının bir koleksiyonunu Azure SQL Analytics izleyebilirsiniz:

1. Azure Marketi 'nden bir Azure SQL Analytics çözümü oluşturun.
2. Çözümde bir izleme çalışma alanı oluşturun.
3. Tanılama telemetrisini çalışma alanına akışa almak için veritabanlarını yapılandırın.

Elastik havuzlar veya yönetilen örnekler kullanıyorsanız, bu kaynaklardan tanılama telemetri akışını da yapılandırmanız gerekir.

### <a name="create-an-azure-sql-analytics-resource"></a>Azure SQL Analytics kaynağı oluşturma

1. Azure Market 'te Azure SQL Analytics arayın ve seçin.

   ![Portalda Azure SQL Analytics arama](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)

2. Çözümün genel bakış ekranında **Oluştur** ' u seçin.

3. Azure SQL Analytics formunu, gereken ek bilgilerle girin: çalışma alanı adı, abonelik, kaynak grubu, konum ve fiyatlandırma katmanı.

   ![Portalda Azure SQL Analytics yapılandırma](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Onaylamak için **Tamam** ' ı seçin ve ardından **Oluştur**' u seçin.

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Ölçümleri ve tanılama günlüklerini kaydetmek için veritabanlarını yapılandırma

Veritabanlarının, ölçüm kayıtlarını nerede Azure portal kullanarak yapılandırmanın en kolay yolu. Azure portal veritabanı kaynağına gidin ve **Tanılama ayarları**' nı seçin.

### <a name="use-azure-sql-analytics-for-monitoring-and-alerting"></a>İzleme ve uyarma için Azure SQL Analytics kullanma

SQL veritabanı kaynaklarınızı görüntülemek için SQL Analytics 'i hiyerarşik bir pano olarak kullanabilirsiniz.

- Azure SQL Analytics nasıl kullanacağınızı öğrenmek için bkz. SQL [Analytics kullanarak SQL veritabanını izleme](../log-analytics/log-analytics-azure-sql.md).
- SQL Analytics 'te için uyarıları ayarlamayı öğrenmek için bkz. [veritabanı, elastik havuzlar ve yönetilen örnekler için uyarı oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts).

## <a name="stream-diagnostic-telemetry-into-event-hubs"></a>Tanılama telemetrisini Event Hubs olarak akışa ekleyin

SQL veritabanı ölçümleri ve tanılama günlüklerini, Azure portal **bir olay hub 'ı** seçeneğinde yerleşik akışı kullanarak Event Hubs ' a akışını sağlayabilirsiniz. Ayrıca, PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla tanılama ayarlarını kullanarak Service Bus kuralı KIMLIĞINI etkinleştirebilirsiniz.

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Event Hubs ölçümler ve tanılama günlükleri ile ne yapmalı

Seçili veriler Event Hubs akışa alındıktan sonra, Gelişmiş izleme senaryolarına olanak tanımak için bir adım daha yakınına sahip olursunuz. Event Hubs, bir olay işlem hattının ön kapısı olarak davranır. Veriler bir olay hub 'ına toplandıktan sonra, gerçek zamanlı bir analiz sağlayıcısı veya bir depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs, bir olay akışının üretimini bu olayların kullanımından ayırır. Bu şekilde olay tüketicileri, olaylara kendi zamanlamalarıyla erişebilirler. Event Hubs hakkında daha fazla bilgi için bkz.

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Event Hubs için akan ölçümleri kullanabilirsiniz:

- **Power BI, etkin yol verilerini akışa alarak hizmet durumunu görüntüleyin**

   Event Hubs, Stream Analytics ve Power BI kullanarak ölçümler ve tanılama verilerinizi Azure hizmetlerinizden neredeyse gerçek zamanlı içgörüler halinde kolayca dönüştürebilirsiniz. Bir olay hub 'ı ayarlama, Stream Analytics ile verileri işleme ve çıkış olarak Power BI kullanma hakkında genel bilgi için bkz. [Stream Analytics ve Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

- **Üçüncü taraf günlüğe kaydetme ve telemetri akışlarına yönelik akış günlükleri**

   Event Hubs akışı kullanarak, ölçümlerinizi ve tanılama günlüklerinizi çeşitli üçüncü taraf izleme ve Log Analytics çözümlerinde bulabilirsiniz.

- **Özel telemetri ve günlüğe kaydetme platformu oluşturma**

   Zaten özel olarak oluşturulmuş bir telemetri platformudur veya bir tane oluşturmayı düşünülüyor musunuz? Yüksek düzeyde ölçeklenebilir yayımla-abone ol Event Hubs, tanılama günlüklerini esnek bir şekilde almanızı sağlar. [Küresel ölçekli bir telemetri platformunda Event Hubs kullanmak için bkz. Dan Rosanova Kılavuzu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-diagnostic-telemetry-into-azure-storage"></a>Azure depolama 'da tanılama telemetrisini akış

Azure portal **bir depolama hesabı** seçeneğinde yerleşik Arşiv ' i kullanarak ölçümleri ve tanılama günlüklerini Azure Storage 'da saklayabilirsiniz. Ayrıca, PowerShell cmdlet 'leri, Azure CLı veya Azure Izleyici REST API aracılığıyla tanılama ayarlarını kullanarak depolamayı etkinleştirebilirsiniz.

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

Event Hubs veya depolama hesabı ' nı seçerseniz bir bekletme ilkesi belirleyebilirsiniz. Bu ilke seçilen bir zaman aralığından daha eski olan verileri siler. Log Analytics belirtirseniz, bekletme ilkesi seçili fiyatlandırma katmanına bağlıdır. Bu durumda, sunulan ücretsiz veri alma birimleri her ay çeşitli veritabanlarının ücretsiz olarak izlenmesini etkinleştirebilir. Her türlü ücretsiz birimi aşan tanılama telemetrisine ilişkin tüm tüketimler maliyette bulunabilir. 

> [!IMPORTANT]
> Yoğun iş yüklerine sahip etkin veritabanları, boşta veritabanlarından daha fazla veri alma. Daha fazla bilgi için bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).

Azure SQL Analytics kullanıyorsanız, Azure SQL Analytics gezinti menüsünde **OMS çalışma alanı** ' nı ve ardından **kullanım** ve **tahmini maliyetler**' i seçerek veri alımı kullanımınızı izleyebilirsiniz.

## <a name="metrics-and-logs-available"></a>Ölçümler ve Günlükler kullanılabilir

Tek veritabanları, havuza alınmış veritabanları, elastik havuzlar, yönetilen örnek ve örnek veritabanları için kullanılabilen izleme telemetrisi, makalenin bu bölümünde belgelenmiştir. SQL Analytics içinde toplanan izleme telemetrisi, [Azure izleyici günlük sorguları](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) dilini kullanarak kendi özel analiz ve uygulama geliştirme için kullanılabilir.

### <a name="basic-metrics"></a>Temel ölçümler

Kaynağa göre temel ölçümler hakkında daha fazla bilgi için aşağıdaki tablolara bakın.

> [!NOTE]
> Temel ölçümler seçeneği önceden tüm ölçümler olarak bilinirdi. Yapılan değişiklik yalnızca adlandırma amaçlıdır ve izlenen ölçümlerde hiçbir değişiklik yapılmadı. Bu değişiklik, gelecekte ek ölçüm kategorilerinin tanıtılmasıyla izin verecek şekilde başlatıldı.

#### <a name="basic-metrics-for-elastic-pools"></a>Elastik havuzlar için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Elastik havuz|eDTU yüzdesi, eDTU kullanımı, eDTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, depolama sınırı, XTP depolama yüzdesi |

#### <a name="basic-metrics-for-single-and-pooled-databases"></a>Tek ve havuza alınmış veritabanları için temel ölçümler

|**Kaynak**|**Ölçümler**|
|---|---|
|Tek ve havuza alınmış veritabanı|DTU yüzdesi, DTU kullanımı, DTU sınırı, CPU yüzdesi, fiziksel veri okuma yüzdesi, günlük yazma yüzdesi, başarılı/başarısız/engellenen güvenlik duvarı bağlantıları, oturum yüzdesi, çalışan yüzdesi, depolama, depolama yüzdesi, XTP depolama yüzdesi ve kilitlenmeler |

### <a name="advanced-metrics"></a>Gelişmiş ölçümler

Gelişmiş ölçümler hakkındaki ayrıntılar için aşağıdaki tabloya bakın.

|**Ölçüm**|**Ölçüm görünen adı**|**Açıklama**|
|---|---|---|
|tempdb_data_size| Tempdb veri dosyası boyutu kilobayt |Tempdb veri dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir. Bu ölçüm, 2 sanal çekirdek ve üzeri ile vCore satın alma modeli, DTU tabanlı satın alma modelleri için 200 DTU ve üzeri kullanan veritabanlarında kullanılabilir. Bu ölçüm, hiper ölçekli veritabanları için şu anda kullanılamıyor.|
|tempdb_log_size| Tempdb günlük dosyası boyutu kilobayt |Tempdb günlük dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir. Bu ölçüm, 2 sanal çekirdek ve üzeri ile vCore satın alma modeli, DTU tabanlı satın alma modelleri için 200 DTU ve üzeri kullanan veritabanlarında kullanılabilir. Bu ölçüm, hiper ölçekli veritabanları için şu anda kullanılamıyor.|
|tempdb_log_used_percent| Kullanılan tempdb günlüğü yüzdesi |Kullanılan tempdb günlüğü yüzdesi. Veri ambarlarında geçerli değildir. Bu ölçüm, 2 sanal çekirdek ve üzeri ile vCore satın alma modeli, DTU tabanlı satın alma modelleri için 200 DTU ve üzeri kullanan veritabanlarında kullanılabilir. Bu ölçüm, hiper ölçekli veritabanları için şu anda kullanılamıyor.|

### <a name="basic-logs"></a>Temel Günlükler

Tüm Günlükler için kullanılabilen telemetri ayrıntıları aşağıdaki tablolarda belgelenmiştir. Belirli bir veritabanı türü-Azure SQL tek, havuza alınmış veya örnek veritabanı için hangi günlüklerin desteklendiğini anlamak üzere [desteklenen tanılama günlüğü](#supported-diagnostic-logging-for-azure-sql-databases) ' ne bakın.

#### <a name="resource-usage-stats-for-managed-instances"></a>Yönetilen örnekler için kaynak kullanım istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure|
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: ResourceUsageStats |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: MANAGEDıNSTANCES |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Yönetilen örnek adı |
|ResourceId|Kaynak URI 'SI |
|SKU_s|Yönetilen örnek Ürün SKU 'SU |
|virtual_core_count_s|Kullanılabilir sanal çekirdek sayısı |
|avg_cpu_percent_s|Ortalama CPU yüzdesi |
|reserved_storage_mb_s|Yönetilen örnekteki ayrılmış depolama kapasitesi |
|storage_space_used_mb_s|Yönetilen örnekte kullanılan depolama alanı |
|io_requests_s|IOPS sayısı |
|io_bytes_read_s|IOPS bayt okuma |
|io_bytes_written_s|Bayt yazılan ıOPS |

#### <a name="query-store-runtime-statistics"></a>Sorgu deposu çalışma zamanı istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: QueryStoreRuntimeStatistics |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: QueryStoreRuntimeStatisticsEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
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

#### <a name="query-store-wait-statistics"></a>Sorgu deposu bekleme istatistikleri

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: QueryStoreWaitStatistics |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: QueryStoreWaitStatisticsEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
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

#### <a name="errors-dataset"></a>Hatalar veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: hatalar |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: ErrorEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
|İleti|Düz metinde hata iletisi |
|user_defined_b|Hata Kullanıcı tanımlı biti |
|error_number_d|Hata kodu |
|Severity|Hatanın önem derecesi |
|state_d|Hatanın durumu |
|query_hash_s|Varsa, başarısız sorgunun sorgu karması |
|query_plan_hash_s|Varsa, başarısız sorgunun sorgu planı karması |

[SQL Server hata iletileri](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors?view=sql-server-ver15)hakkında daha fazla bilgi edinin.

#### <a name="database-wait-statistics-dataset"></a>Veritabanı bekleme istatistikleri veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: DatabaseWaitStatistics |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: DatabaseWaitStatisticsEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
|wait_type_s|Bekleme türünün adı |
|start_utc_date_t [UTC]|Ölçülen dönem başlangıç zamanı |
|end_utc_date_t [UTC]|Ölçülen dönem bitiş zamanı |
|delta_max_wait_time_ms_d|Yürütme başına en fazla beklenen süre |
|delta_signal_wait_time_ms_d|Toplam sinyal bekleme süresi |
|delta_wait_time_ms_d|Dönemdeki toplam bekleme süresi |
|delta_waiting_tasks_count_d|Bekleyen görev sayısı |

[Veritabanı bekleme istatistikleri](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)hakkında daha fazla bilgi edinin.

#### <a name="time-outs-dataset"></a>Zaman aşımları veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: zaman aşımları |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: TimeoutEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
|error_state_d|Hata durum kodu |
|query_hash_s|Varsa sorgu karması |
|query_plan_hash_s|Varsa sorgu planı karması |

#### <a name="blockings-dataset"></a>Blok veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: bloklar |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: BlockEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
|lock_mode_s|Sorgu tarafından kullanılan kilit modu |
|resource_owner_type_s|Kilidin sahibi |
|blocked_process_filtered_s|Engellenen işlem raporu XML 'i |
|duration_d|Mikrosaniye cinsinden kilit süresi |

#### <a name="deadlocks-dataset"></a>Kilitlenmeler veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC] |Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: kilitlenmeler |
|için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir|İşlemin adı. Always: DeadlockEvent |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
|deadlock_xml_s|Kilitlenme rapor XML 'i |

#### <a name="automatic-tuning-dataset"></a>Otomatik ayarlama veri kümesi

|Özellik|Açıklama|
|---|---|
|TenantId|Kiracı KIMLIĞINIZ |
|SourceSystem|Always: Azure |
|TimeGenerated [UTC]|Günlüğün kaydedildiği zaman damgası |
|Tür|Always: AzureDiagnostics |
|ResourceProvider|Kaynak sağlayıcının adı. Always: MICROSOFT. SQL |
|Kategori|Kategorinin adı. Always: otomatik olarak ayarlama |
|Kaynak|Kaynağın adı |
|ResourceType|Kaynak türünün adı. Always: sunucular/VERITABANLARı |
|kaynak grubundaki|Veritabanı için abonelik GUID 'SI |
|adlı yönetilen örnek,|Veritabanı için kaynak grubunun adı |
|LogicalServerName_s|Veritabanı için sunucu adı |
|LogicalDatabaseName_s|Veritabanının adı |
|ElasticPoolName_s|Veritabanı için elastik havuzun adı (varsa) |
|DatabaseName_s|Veritabanının adı |
|ResourceId|Kaynak URI 'SI |
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

#### <a name="intelligent-insights-dataset"></a>Akıllı İçgörüler veri kümesi

[Akıllı içgörüler günlük biçimi](sql-database-intelligent-insights-use-diagnostics-log.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Günlüğe kaydetmeyi etkinleştirme ve çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini anlama hakkında bilgi edinmek için bkz.:

- [Microsoft Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)

Event Hubs hakkında bilgi edinmek için şunu okuyun:

- [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Log Analytics 'ten telemetri temelinde uyarıları ayarlamayı öğrenmek için bkz.:

- [SQL veritabanı ve yönetilen örnek için uyarı oluşturma](../azure-monitor/insights/azure-sql.md#analyze-data-and-create-alerts)
