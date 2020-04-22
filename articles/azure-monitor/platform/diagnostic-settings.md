---
title: Platform ölçümlerini ve günlüklerini toplamak için tanılama ayarlarını kullanın ve Azure'da
description: Azure Monitor platform ölçümlerini ve günlüklerini tanı lama ayarını kullanarak Azure Monitör Günlükleri, Azure depolama alanı veya Azure Etkinlik Hub'larına gönderin.
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 04/15/2020
ms.subservice: logs
ms.openlocfilehash: edb34b1456efae4d06465cfa2e64e546f621c6da
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681233"
---
# <a name="create-diagnostic-setting-to-collect-resource-logs-and-metrics-in-azure"></a>Azure'da kaynak günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturma

Azure Etkinliği günlüğü ve kaynak günlükleri de dahil olmak üzere Azure'daki [platform günlükleri,](platform-logs-overview.md) Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar. [Platform ölçümleri](data-platform-metrics.md) varsayılan olarak toplanır ve genellikle Azure Monitor ölçümleri veritabanında depolanır.

Bu makalede, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için tanılama ayarları oluşturma ve yapılandırma hakkında ayrıntılar sağlar.

> [!IMPORTANT]
> Etkinlik günlüğünü toplamak için bir tanılama ayarı oluşturmadan önce, önce eski yapılandırmayı devre dışı bırakmanız gerekir. Ayrıntılar için [eski ayarları içeren Azure Etkinliği Topla günlüğe](diagnostic-settings-legacy.md) bakın.

Her Azure kaynağı, aşağıdaki ölçütleri tanımlayan kendi tanılama ayarını gerektirir:

- Ayarda tanımlanan hedeflere gönderilen günlük kategorileri ve metrik veriler. Kullanılabilir kategoriler farklı kaynak türleri için değişir.
- Günlükleri göndermek için bir veya daha fazla hedef. Geçerli varış noktaları arasında Günlük Analizi çalışma alanı, Etkinlik Hub'ları ve Azure Depolama yer almaktadır.

Tek bir tanılama ayarı, her hedeften birden birini tanımlayabilir. Belirli bir hedef türünden birine (örneğin, iki farklı Log Analytics çalışma alanı) veri göndermek istiyorsanız, birden çok ayar oluşturun. Her kaynağın en fazla 5 tanılama ayarı olabilir.

> [!NOTE]
> [Platform ölçümleri](metrics-supported.md) otomatik olarak [Azure Monitör Ölçümleri'ne](data-platform-metrics.md)toplanır. Tanılama ayarları, [günlük sorgularını](../log-query/log-query-overview.md)kullanarak diğer izleme verileriyle analiz için azure monitör günlüklerinde belirli Azure hizmetlerinin ölçümlerini toplamak için kullanılabilir.

## <a name="destinations"></a>Hedefler

Platform günlükleri ve ölçümleri aşağıdaki tablodaki hedeflere gönderilebilir. Bu hedefe veri gönderme yle ilgili ayrıntılar için aşağıdaki tablodaki her bağlantıyı izleyin.

| Hedef | Açıklama |
|:---|:---|
| [Log Analytics çalışma alanı](resource-logs-collect-workspace.md) | Günlükleri ve ölçümleri bir Log Analytics çalışma alanında toplamak, bunları azure monitor tarafından toplanan diğer izleme verileriyle güçlü günlük sorguları kullanarak analiz etmenizi ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Monitor özelliklerinden yararlanmanızı sağlar. |
| [Etkinlik hub'ları](resource-logs-stream-event-hubs.md) | Günlükleri ve ölçümleri Event Hub'larına göndermek, verileri üçüncü taraf SIEM'ler ve diğer günlük analizi çözümleri gibi harici sistemlere aktarmanızı sağlar. |
| [Azure depolama hesabı](resource-logs-collect-storage.md) | Günlükleri ve ölçümleri bir Azure depolama hesabına arşivlemek denetim, statik analiz veya yedekleme için yararlıdır. Azure Monitör Günlükleri ve Günlük Analizi çalışma alanıyla karşılaştırıldığında, Azure depolama alanı daha ucuzdur ve günlükler süresiz olarak orada tutulabilir. |

## <a name="create-diagnostic-settings-in-azure-portal"></a>Azure portalında tanı lama ayarları oluşturma

Azure portalındaki tanı ayarlarını Azure Monitörü menüsünden veya kaynak menüsünden yapılandırabilirsiniz.

1. Azure portalında tanılama ayarlarını yapılandırdığınız yer kaynağa bağlıdır.

   - Tek bir kaynak için, kaynak menüsünde **Monitör'ün** altındaki **Tanılama ayarlarını** tıklatın.

        ![Tanılama ayarları](media/diagnostic-settings/menu-resource.png)

   - Bir veya daha fazla kaynak için Azure Monitörü menüsünde **Ayarlar** altında **Tanılama ayarlarını** tıklatın ve ardından kaynağa tıklayın.

      ![Tanılama ayarları](media/diagnostic-settings/menu-monitor.png)

   - Etkinlik günlüğü için **Azure Monitörü** menüsünde **Etkinlik günlüğü** ve ardından **Tanılama ayarlarını**tıklatın. Etkinlik günlüğü için eski yapılandırmayı devre dışı bıraktığından emin olun. Ayrıntılar için [varolan ayarları devre dışı dışı](/azure/azure-monitor/platform/activity-log-collect#collecting-activity-log) kındırı(

        ![Tanılama ayarları](media/diagnostic-settings/menu-activity-log.png)

2. Seçtiğiniz kaynakta ayar yoksa, bir ayar oluşturmanız istenir. **Tanı ayarını ekle'yi**tıklatın.

   ![Tanılama ayarı ekleme - varolan ayar yok](media/diagnostic-settings/add-setting.png)

   Kaynakta varolan ayarlar varsa, önceden yapılandırılmış ayarların listesini görürsünüz. Yeni bir ayar eklemek için **tanı ayar Ekle'yi** veya varolan bir ayarı yeniden ayarlamak için **ayarını edin'i** tıklatın. Her ayar, hedef türlerinin her birinden birden fazla olamaz.

   ![Tanılama ayarı ekleme - varolan ayarlar](media/diagnostic-settings/edit-setting.png)

3. Ayarınızda zaten bir ad yoksa bir ad verin.

    ![Tanılama ayarı ekleme](media/diagnostic-settings/setting-new-blank.png)

4. **Kategori ayrıntıları (rotanedir)** - Daha sonra belirtilen hedeflere göndermek istediğiniz her veri kategorisi için kutuyu işaretleyin. Kategoriler listesi her Azure hizmeti için değişir.

     - **AllMetrics,** bir kaynağın platform ölçümlerini Azure Günlükleri deposuna, ancak günlük formunda yönlendirir. Bu ölçümler genellikle yalnızca Azure Monitor ölçümleri zaman serisi veritabanına gönderilir. Bunları Azure Monitor Günlükleri mağazasına göndermek (Log Analytics üzerinden aranabilir) bunları diğer günlüklerde arama yapan sorgulara entegre etmek için. Bu seçenek tüm kaynak türleri için kullanılamayabilir. Azure [Monitor desteklendiğinde,](metrics-supported.md) desteklenen ölçümler hangi kaynak türleri için hangi ölçümlerin toplandığını listeler.

       > [!NOTE]
       > Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
       >
       > *Örneğin:* Blockchain'deki 'IOReadBytes' ölçümü, düğüm başına olarak incelenebilir ve grafiklenebilir. Ancak, tanılama ayarları yla dışa aktarıldığında, dışa aktarılan metrik tüm düğümler için okunan baytlar olarak temsil eder.

     - **Günlükler,** kaynak türüne bağlı olarak kullanılabilen farklı kategorileri listeler. Bir hedefe yönlendirmek istediğiniz kategorileri denetleyin.

5. **Hedef ayrıntıları** - Her hedef için kutuyu işaretleyin. Her kutuyu işaretlediğinizde, seçenekler ek bilgi eklemenize olanak sağlar.

      ![Günlük Analitiği veya Etkinlik Hub'larına Gönder](media/diagnostic-settings/send-to-log-analytics-event-hubs.png)

    1. **Günlük Analizi** - Aboneliği ve çalışma alanını girin.  Çalışma alanınız yoksa, devam etmeden önce bir çalışma alanı [oluşturmanız](../learn/quick-create-workspace.md)gerekir.

    1. **Olay hub'ları** - Aşağıdaki ölçütleri belirtin:
       - Etkinlik merkezinin parçası olduğu abonelik
       - Olay hub ad alanı - Henüz bir tane yoksa, [bir tane oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir
       - Tüm verileri göndermek için bir Olay hub adı (isteğe bağlı). Bir ad belirtmezseniz, her günlük kategorisi için bir olay merkezi oluşturulur. Birden çok kategori gönderiyorsanız, oluşturulan olay hub'larının sayısını sınırlamak için bir ad belirtmek isteyebilirsiniz. Ayrıntılar için [Azure Etkinlik Hubkotaları kotalarına ve sınırlarına](../../event-hubs/event-hubs-quotas.md) bakın.
       - Olay Hub ilkesi (isteğe bağlı) Akış mekanizmasının sahip olduğu izinleri tanımlar. Daha fazla bilgi için [Olay hub'ları özelliklerine](../../event-hubs/event-hubs-features.md#publisher-policy)bakın.

    1. **Depolama** - Abonelik, depolama hesabı ve saklama ilkesini seçin.

        ![Depolamaya Gönder](media/diagnostic-settings/storage-settings-new.png)

        > [!TIP]
        > Bekletme ilkesini 0 olarak ayarlamayı ve gelecekte olası karışıklığı önlemek için zamanlanmış bir iş kullanarak verilerinizi depolama alanından el ile silmeyi düşünün.
        >
        > İlk olarak, depolama alanını arşivleme için kullanıyorsanız, verilerinizin genellikle 365 günden uzun bir süre etrafta dolaşmalarını istersiniz. İkinci olarak, 0'dan büyük bir bekletme ilkesi seçerseniz, son kullanma tarihi depolama sırasındaki günlüklere eklenir. Depolanan bu günlüklerin tarihini değiştiremezsiniz.
        >
        > Örneğin, *WorkflowRuntime* için bekletme ilkesini 180 gün olarak ayarlarsanız ve 24 saat sonra 365 güne ayarlarsanız, bu ilk 24 saat içinde depolanan günlükler 180 gün sonra otomatik olarak silinir ve bu türdeki sonraki tüm günlükler 365 gün sonra otomatik olarak silinir. Bekletme ilkesini daha sonra değiştirmek, günlüklerin ilk 24 saatinin 365 gün boyunca kalmasına neden olmaz.

6. **Kaydet**’e tıklayın.

Birkaç dakika sonra, yeni ayar bu kaynağın ayarlar listenizde görünür ve günlükleri yeni olay verileri oluşturulurken belirtilen hedeflere akışı sağlar. Bir olayın yayımlandığından bir [Log Analytics çalışma alanında görüntülenmeleri](data-ingestion-time.md)arasında 15 dakika kadar sürebilir.

## <a name="create-diagnostic-settings-using-powershell"></a>PowerShell'i kullanarak tanılama ayarları oluşturma

[Azure PowerShell](powershell-quickstart-samples.md)ile tanılama ayarı oluşturmak için [Set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) cmdlet'i kullanın. Parametrelerinin açıklamaları için bu cmdlet için belgelere bakın.

> [!IMPORTANT]
> Bu yöntemi Azure Etkinliği günlüğü için kullanamazsınız. Bunun yerine, Kaynak Yöneticisi şablonu oluşturmak ve PowerShell ile dağıtmak için [Kaynak Yöneticisi şablonu kullanarak Azure Monitor'da tanı ayarını oluştur'u](diagnostic-settings-template.md) kullanın.

Aşağıda, üç hedefi de kullanarak tanılama ayarı oluşturmak için örnek bir PowerShell cmdlet verilmiştir.

```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

## <a name="create-diagnostic-settings-using-azure-cli"></a>Azure CLI'yi kullanarak tanılama ayarları oluşturma

[Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest)ile bir tanılama ayarı oluşturmak için [az monitör tanılama ayarları oluşturma](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) komutunu kullanın. Parametrelerinin açıklamaları için bu komutun belgelerine bakın.

> [!IMPORTANT]
> Bu yöntemi Azure Etkinliği günlüğü için kullanamazsınız. Bunun yerine, Kaynak Yöneticisi şablonu oluşturmak ve CLI ile dağıtmak için [Kaynak Yöneticisi şablonu kullanarak Azure Monitor'da tanı ayarını oluştur'u](diagnostic-settings-template.md) kullanın.

Aşağıda, üç hedefi de kullanarak bir tanılama ayarı oluşturmak için örnek bir CLI komutu verilmiştir.

```azurecli
az monitor diagnostic-settings create  \
--name KeyVault-Diagnostics \
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault \
--logs    '[{"category": "AuditEvent","enabled": true}]' \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--storage-account /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace \
--event-hub-rule /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

### <a name="configure-diagnostic-settings-using-rest-api"></a>REST API'yi kullanarak tanılama ayarlarını yapılandırın

[Azure Monitor REST API'sini](https://docs.microsoft.com/rest/api/monitor/)kullanarak tanılama ayarlarını oluşturmak veya güncelleştirmek için [Tanılama Ayarları'na](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) bakın.

### <a name="configure-diagnostic-settings-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak tanılama ayarlarını yapılandırma

Bkz. Kaynak Yöneticisi şablonuyla tanılama ayarlarını oluşturmak veya güncelleştirmek için [Kaynak Yöneticisi şablonu kullanarak Azure Monitor'da tanılama ayarı](diagnostic-settings-template.md) oluştur' a bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure platformu Günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md)
