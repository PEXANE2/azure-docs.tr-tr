---
title: Azure'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturma
description: Azure platform günlüklerini Azure Monitör Günlükleri, Azure depolama alanı veya Azure Etkinlik Hub'larına iletmek için tanı lama ayarları oluşturun.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: fb2f9ff5af68575d9f9d29e9a6aca83d603395b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672421"
---
# <a name="create-diagnostic-setting-to-collect-platform-logs-and-metrics-in-azure"></a>Azure'da platform günlüklerini ve ölçümlerini toplamak için tanılama ayarı oluşturun
Azure Etkinliği günlüğü ve kaynak günlükleri de dahil olmak üzere Azure'daki [platform günlükleri,](platform-logs-overview.md) Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar. Bu makalede, farklı hedeflere platform günlükleri göndermek için tanılama ayarları oluşturma ve yapılandırma hakkında ayrıntılar sağlar.

> [!IMPORTANT]
> Etkinlik günlüğünü toplamak için bir tanılama ayarı oluşturmadan önce, önce eski yapılandırmayı devre dışı bırakmanız gerekir. Ayrıntılar için [eski ayarları içeren Azure Etkinliği Topla günlüğe](diagnostic-settings-legacy.md) bakın.

Her Azure kaynağı, aşağıdakileri tanımlayan kendi tanılama ayarı gerektirir:

- Ayarda tanımlanan hedeflere gönderilen günlük kategorileri ve metrik veriler. Kullanılabilir kategoriler farklı kaynak türleri için değişir.
- Günlükleri göndermek için bir veya daha fazla hedef. Geçerli varış noktaları arasında Günlük Analizi çalışma alanı, Etkinlik Hub'ları ve Azure Depolama yer almaktadır.
 
Tek bir tanılama ayarı, her hedeften birden birini tanımlayabilir. Belirli bir hedef türünden birine (örneğin, iki farklı Log Analytics çalışma alanı) veri göndermek istiyorsanız, birden çok ayar oluşturun. Her kaynağın en fazla 5 tanılama ayarı olabilir.


> [!NOTE]
> [Platform ölçümleri](metrics-supported.md) otomatik olarak [Azure Monitör Ölçümleri'ne](data-platform-metrics.md)toplanır. Tanılama ayarları, [günlük sorgularını](../log-query/log-query-overview.md)kullanarak diğer izleme verileriyle analiz için azure monitör günlüklerinde belirli Azure hizmetlerinin ölçümlerini toplamak için kullanılabilir.

## <a name="destinations"></a>Hedefler 
Platform günlükleri aşağıdaki tablodaki hedeflere gönderilebilir. Her hedef için yapılandırma, bu makalede açıklanan tanı ayarları oluşturmak için aynı işlem kullanılarak gerçekleştirilir. Bu hedefe veri gönderme yle ilgili ayrıntılar için aşağıdaki tablodaki her bağlantıyı izleyin.

| Hedef | Açıklama |
|:---|:---|
| [Log Analytics çalışma alanı](resource-logs-collect-workspace.md) | Günlükleri bir Log Analytics çalışma alanında toplamak, bunları azure monitor tarafından toplanan diğer izleme verileriyle güçlü günlük sorgularını kullanarak analiz etmenizi ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Monitor özelliklerinden yararlanmanızı sağlar. |
| [Olay hub’ları](resource-logs-stream-event-hubs.md) | Günlükleri Olay Hub'larına göndermek, üçüncü taraf SIEM'ler ve diğer günlük analiz çözümleri gibi harici sistemlere veri akışı yapmanızı sağlar. |
| [Azure depolama hesabı](resource-logs-collect-storage.md) | Bir Azure depolama hesabında günlükleri arşivlemek denetim, statik çözümleme veya yedekleme için yararlıdır. |

## <a name="create-diagnostic-settings-in-azure-portal"></a>Azure portalında tanı lama ayarları oluşturma
Azure portalındaki tanı ayarlarını Azure Monitörü menüsünden veya kaynak menüsünden yapılandırabilirsiniz.

1. Azure portalında tanılama ayarlarını yapılandırdığınız yer kaynağa bağlıdır.

   - Tek bir kaynak için, kaynak menüsünde **Monitör'ün** altındaki **Tanılama ayarlarını** tıklatın.

        ![Tanılama ayarları](media/diagnostic-settings/menu-resource.png)

    - Bir veya daha fazla kaynak için Azure Monitörü menüsünde **Ayarlar** altında **Tanılama ayarlarını** tıklatın ve ardından kaynağa tıklayın.
    
        ![Tanılama ayarları](media/diagnostic-settings/menu-monitor.png)

    - Etkinlik günlüğü için **Azure Monitörü** menüsünde **Etkinlik günlüğü** ve ardından **Tanılama ayarlarını**tıklatın. Etkinlik günlüğü için eski yapılandırmayı devre dışı bıraktığından emin olun. Ayrıntılar için [varolan ayarları devre dışı dışı](diagnostic-settings-legacy.md#disable-existing-settings) kındırı(

        ![Tanılama ayarları](media/diagnostic-settings/menu-activity-log.png)

2. Seçtiğiniz kaynakta ayar yoksa, bir ayar oluşturmanız istenir. **Tanı ayarını ekle'yi**tıklatın.

   ![Tanılama ayarı ekleme - varolan ayar yok](media/diagnostic-settings/add-setting.png)

   Kaynakta varolan ayarlar varsa, önceden yapılandırılmış ayarların listesini görürsünüz. Yeni bir ayar eklemek için **tanı ayar Ekle'yi** veya varolan bir ayarı yeniden ayarlamak için **ayarını edin'i** tıklatın. Her ayar, hedef türlerinin her birinden birden fazla olamaz.

   ![Tanılama ayarı ekleme - varolan ayarlar](media/diagnostic-settings/edit-setting.png)

3. Ayarınızda zaten bir ad yoksa bir ad verin.
4. Günlükleri göndermek için her hedef için kutuyu işaretleyin. Aşağıdaki tabloda açıklandığı gibi ayarlarını belirtmek için **Yapıya'yı** tıklatın.

    | Ayar | Açıklama |
    |:---|:---|
    | Log Analytics çalışma alanı | Çalışma alanının adı. |
    | Depolama hesabı | Depolama hesabının adı. |
    | Olay hub’ı ad alanı | Olay hub'ınoluşturulduğu ad alanı (bu günlükleri ilk kez akışınız saise) veya akış (bu günlük kategorisine zaten akış yapan kaynaklar varsa).
    | Olay hub'ı adı | İsteğe bağlı olarak, ayarındaki tüm verileri göndermek için bir olay hub adı belirtin. Bir ad belirtmezseniz, her günlük kategorisi için bir olay merkezi oluşturulur. Birden çok kategori gönderiyorsanız, oluşturulan olay hub'larının sayısını sınırlamak için bir ad belirtmek isteyebilirsiniz. Ayrıntılar için [Azure Etkinlik Hubkotaları kotalarına ve sınırlarına](../../event-hubs/event-hubs-quotas.md) bakın. |
    | Olay merkezi ilkesi adı | Akış mekanizmasının sahip olduğu izinleri tanımlar. |

    ![Tanılama ayarı ekleme - varolan ayarlar](media/diagnostic-settings/setting-details.png)

5. Belirtilen hedeflere gönderilecek veri kategorilerinin her biri için kutuyu işaretleyin. Kategorilerlistesi her Azure hizmeti için değişir.

   > [!NOTE]
   > Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
   >
   > *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.

6. **Kaydet**'e tıklayın.

Birkaç dakika sonra, yeni ayar bu kaynağın ayarlar listenizde görünür ve günlükleri yeni olay verileri oluşturulurken belirtilen hedeflere akışı sağlar. Bir olayın yayımlandığında niçin log [analitiği çalışma alanında görünmesi](data-ingestion-time.md)arasında on beş dakikaya kadar çıkabileceğini unutmayın.



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

* [Azure platformu Günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md)
