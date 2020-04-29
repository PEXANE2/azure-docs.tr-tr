---
title: Platform ölçümlerini ve günlüklerini toplamak için tanılama ayarlarını kullanın ve Azure 'da
description: Azure izleyici platformu ölçümlerini ve günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs bir tanılama ayarı kullanarak gönderin.
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 04/15/2020
ms.subservice: logs
ms.openlocfilehash: edb34b1456efae4d06465cfa2e64e546f621c6da
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681233"
---
# <a name="create-diagnostic-setting-to-collect-resource-logs-and-metrics-in-azure"></a>Azure 'da kaynak günlüklerini ve ölçümleri toplamak için tanılama ayarı oluştur

Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](platform-logs-overview.md) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. [Platform ölçümleri](data-platform-metrics.md) varsayılan olarak toplanır ve genellikle Azure izleyici ölçümleri veritabanında depolanır.

Bu makalede, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için tanılama ayarlarını oluşturma ve yapılandırma hakkında ayrıntılı bilgi verilmektedir.

> [!IMPORTANT]
> Etkinlik günlüğünü toplamak üzere bir tanılama ayarı oluşturmadan önce, önce eski yapılandırmaları devre dışı bırakmanız gerekir. Ayrıntılar için bkz. [eski ayarlarla Azure etkinlik günlüğü toplama](diagnostic-settings-legacy.md) .

Her Azure kaynağı, aşağıdaki ölçütleri tanımlayan kendi tanılama ayarını gerektirir:

- Ayarda tanımlanan hedeflere gönderilen günlük kategorileri ve ölçüm verileri. Kullanılabilir Kategoriler farklı kaynak türleri için farklılık gösterir.
- Günlükleri göndermek için bir veya daha fazla hedef. Geçerli hedeflere Log Analytics çalışma alanı, Event Hubs ve Azure Storage dahildir.

Tek bir tanılama ayarı, hedeflerin her birinden fazlasını tanımlayabilir. Belirli bir hedef türünden birine (örneğin, iki farklı Log Analytics çalışma alanı) birden fazla veri göndermek istiyorsanız, daha sonra birden çok ayar oluşturun. Her kaynak en fazla 5 tanılama ayarlarına sahip olabilir.

> [!NOTE]
> [Platform ölçümleri](metrics-supported.md) , [Azure izleyici ölçümlerine](data-platform-metrics.md)otomatik olarak toplanır. Tanılama ayarları, belirli Azure hizmetleri için Azure Izleyici günlüklerine yönelik ölçümleri, [günlük sorguları](../log-query/log-query-overview.md)kullanılarak diğer izleme verileriyle analiz edilmek üzere toplamak için kullanılabilir.

## <a name="destinations"></a>Hedefler

Platform günlükleri ve ölçümleri aşağıdaki tablodaki hedeflere gönderilebilir. Bu hedefe veri gönderme hakkındaki ayrıntılar için aşağıdaki tablodaki her bir bağlantıyı izleyin.

| Hedef | Açıklama |
|:---|:---|
| [Log Analytics çalışma alanı](resource-logs-collect-workspace.md) | Log Analytics çalışma alanında günlüklerin ve ölçümlerin toplanması, güçlü günlük sorguları ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Izleyici özelliklerinden yararlanmak üzere Azure Izleyici tarafından toplanan diğer izleme verileriyle analiz etmenizi sağlar. |
| [Olay Hub 'ları](resource-logs-stream-event-hubs.md) | Event Hubs Günlükler ve ölçümler gönderme, üçüncü taraf SIG 'ler ve diğer Log Analytics çözümleri gibi dış sistemlere veri akışını sağlar. |
| [Azure depolama hesabı](resource-logs-collect-storage.md) | Günlükleri ve ölçümleri bir Azure depolama hesabına arşivleme, denetim, statik analiz veya yedekleme için faydalıdır. Azure Izleyici günlükleri ve bir Log Analytics çalışma alanıyla karşılaştırıldığında, Azure Storage daha pahalı olur ve Günlükler sonsuza kadar tutulabilirler. |

## <a name="create-diagnostic-settings-in-azure-portal"></a>Azure portal Tanılama ayarları oluşturma

Azure portal tanılama ayarlarını Azure Izleyici menüsünden ya da kaynak menüsünden yapılandırabilirsiniz.

1. Azure portal tanılama ayarlarını yapılandırdığınız konum kaynağa bağlıdır.

   - Tek bir kaynak için kaynak menüsünde **izleyici** ' nin altındaki **Tanılama ayarları** ' na tıklayın.

        ![Tanılama ayarları](media/diagnostic-settings/menu-resource.png)

   - Bir veya daha fazla kaynak için, Azure Izleyici menüsünde **Ayarlar** altında **Tanılama ayarları** ' na tıklayın ve ardından kaynağa tıklayın.

      ![Tanılama ayarları](media/diagnostic-settings/menu-monitor.png)

   - Etkinlik günlüğü için, **Azure izleyici** menüsünde **etkinlik günlüğü** ' ne ve ardından **Tanılama ayarları**' na tıklayın. Etkinlik günlüğü için eski tüm yapılandırmaları devre dışı bıraktığınızdan emin olun. Ayrıntılar için bkz. [var olan ayarları devre dışı bırakma](/azure/azure-monitor/platform/activity-log-collect#collecting-activity-log) .

        ![Tanılama ayarları](media/diagnostic-settings/menu-activity-log.png)

2. Seçtiğiniz kaynakta hiçbir ayar yoksa, bir ayar oluşturmanız istenir. **Tanılama ayarı Ekle**' ye tıklayın.

   ![Tanılama ayarı Ekle-mevcut ayar yok](media/diagnostic-settings/add-setting.png)

   Kaynakta mevcut ayarlar varsa, önceden yapılandırılmış ayarların bir listesini görürsünüz. Varolan bir ayarı düzenlemek için yeni bir ayar veya **düzenleme ayarı** eklemek için **Tanılama ayarı Ekle** ' ye tıklayın. Her bir ayarın hedef türlerin her biri birden fazla olamaz.

   ![Tanılama ayarı ekleme-var olan ayarlar](media/diagnostic-settings/edit-setting.png)

3. Ayarınız yoksa, bir ad verin.

    ![Tanılama ayarı Ekle](media/diagnostic-settings/setting-new-blank.png)

4. **Kategori ayrıntıları (rota olarak)** -daha sonra belirtilen hedeflere göndermek istediğiniz her bir veri kategorisi için kutuyu işaretleyin. Kategorilerin listesi, her bir Azure hizmeti için farklılık gösterir.

     - **Allölçümleri** bir kaynağın platform ölçümlerini Azure günlükleri deposuna, ancak günlük biçiminde yönlendirir. Bu ölçümler genellikle yalnızca Azure Izleyici ölçümleri zaman serisi veritabanına gönderilir. Bunları Azure Izleyici günlükleri deposuna gönderme (Log Analytics ile aranabilir), bunları diğer günlüklerde aramak için sorgular halinde tümleştirmenizi sağlar. Bu seçenek, tüm kaynak türleri için kullanılamayabilir. Destekleniyorsa, [Azure izleyici desteklenen ölçümler](metrics-supported.md) , hangi kaynak türleri için toplanan ölçümleri listeler.

       > [!NOTE]
       > Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
       >
       > *Örneğin*: bir blok zincirindeki ' ıoreadbytes ' ölçümü, düğüm düzeyinde araştırılabilir ve bu şekilde grafiklenebilir. Ancak, Tanılama ayarları aracılığıyla verildiğinde, tüm düğümler için tüm okuma baytları temsil eder.

     - **Günlükler** , kaynak türüne bağlı olarak kullanılabilir farklı kategorileri listeler. Bir hedefe yönlendirmek istediğiniz kategorileri kontrol edin.

5. **Hedef ayrıntıları** -her bir hedefin kutusunu işaretleyin. Her bir kutuyu denetlediğinizde, ek bilgi eklemenize olanak sağlayan seçenekler görünür.

      ![Log Analytics veya Event Hubs gönderin](media/diagnostic-settings/send-to-log-analytics-event-hubs.png)

    1. **Log Analytics** abonelik ve çalışma alanını girin.  Çalışma alanınız yoksa [devam etmeden önce bir tane oluşturmanız](../learn/quick-create-workspace.md)gerekir.

    1. **Olay Hub 'ları** -aşağıdaki ölçütleri belirtin:
       - Olay Hub 'ının parçası olduğu abonelik
       - Olay Hub 'ı ad alanı-henüz yoksa [bir tane oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir
       - Tüm verilerin gönderileceği bir olay hub 'ı adı (isteğe bağlı). Bir ad belirtmezseniz, her günlük kategorisi için bir olay hub 'ı oluşturulur. Birden çok kategori gönderiyorsanız, oluşturulan olay hub 'larının sayısını sınırlandırmak için bir ad belirtmek isteyebilirsiniz. Ayrıntılar için bkz. [Azure Event Hubs kotaları ve limitleri](../../event-hubs/event-hubs-quotas.md) .
       - Bir olay hub 'ı ilkesi (isteğe bağlı) bir ilke, akış mekanizmanın sahip olduğu izinleri tanımlar. Daha fazla bilgi için bkz. [Event-hub 'lar-Özellikler](../../event-hubs/event-hubs-features.md#publisher-policy).

    1. **Depolama** -abonelik, depolama hesabı ve bekletme ilkesi seçin.

        ![Depolamaya gönder](media/diagnostic-settings/storage-settings-new.png)

        > [!TIP]
        > Daha sonra olası karışıklık oluşmasını önlemek için, bekletme ilkesini 0 olarak ayarlamayı ve zamanlanan bir iş kullanarak verilerinizi depolamadan el ile silmeyi düşünün.
        >
        > İlk olarak, arşivleme için depolama kullanıyorsanız, genellikle verilerinizi 365 günden daha uzun bir süre boyunca istersiniz. İkincisi, 0 ' dan büyük bir bekletme ilkesi seçerseniz, sona erme tarihi depolama sırasında günlüklere eklenir. Depolandıktan sonra bu günlüklerin tarihini değiştiremezsiniz.
        >
        > Örneğin, *WorkflowRuntime* için bekletme ilkesini 180 gün olarak ayarlarsanız ve daha sonra 24 saat sonra 365 gün olarak ayarlarsanız, ilk 24 saat boyunca depolanan Günlükler otomatik olarak 180 ve bu türden sonraki tüm Günlükler 365 gün sonra otomatik olarak silinir. Bekletme ilkesini daha sonra değiştirmek, ilk 24 saatin günlük 365 gün boyunca kalmasını engellemez.

6. **Kaydet**’e tıklayın.

Birkaç dakika sonra, yeni ayar bu kaynak için ayarlar listenizde görünür ve yeni olay verileri oluşturulmasıyla Günlükler belirtilen hedeflere akışla kaydedilir. Bir olayın bir [Log Analytics çalışma alanında göründüğü](data-ingestion-time.md)zaman arasında 15 dakika kadar sürebilir.

## <a name="create-diagnostic-settings-using-powershell"></a>PowerShell kullanarak tanılama ayarları oluşturma

[Azure PowerShell](powershell-quickstart-samples.md)bir tanılama ayarı oluşturmak için [set-azdiagnosticsetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) cmdlet 'ini kullanın. Bu cmdlet 'in parametrelerinin açıklamaları için belgelerine bakın.

> [!IMPORTANT]
> Azure etkinlik günlüğü için bu yöntemi kullanamazsınız. Bunun yerine, bir Kaynak Yöneticisi şablonu oluşturmak ve PowerShell ile dağıtmak için [Kaynak Yöneticisi şablonu kullanarak Azure izleyici 'de tanılama ayarı oluştur](diagnostic-settings-template.md) ' u kullanın.

Aşağıda, üç hedefi kullanarak bir tanılama ayarı oluşturan örnek bir PowerShell cmdlet 'i verilmiştir.

```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

## <a name="create-diagnostic-settings-using-azure-cli"></a>Azure CLı kullanarak tanılama ayarları oluşturma

[Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest)ile bir tanılama ayarı oluşturmak için [az Monitor Diagnostic-Settings Create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) komutunu kullanın. Parametrelerinin açıklamaları için bu komuta yönelik belgelere bakın.

> [!IMPORTANT]
> Azure etkinlik günlüğü için bu yöntemi kullanamazsınız. Bunun yerine, bir Kaynak Yöneticisi şablonu oluşturmak ve CLı ile dağıtmak için [Kaynak Yöneticisi şablonu kullanarak Azure izleyici 'de tanılama ayarı oluştur](diagnostic-settings-template.md) ' u kullanın.

Aşağıda, üç hedefi kullanarak bir tanılama ayarı oluşturmak için örnek bir CLı komutu verilmiştir.

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

### <a name="configure-diagnostic-settings-using-rest-api"></a>REST API kullanarak tanılama ayarlarını yapılandırma

[Azure izleyici REST API](https://docs.microsoft.com/rest/api/monitor/)kullanarak tanılama ayarlarını oluşturmak veya güncelleştirmek için bkz. [Tanılama ayarları](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) .

### <a name="configure-diagnostic-settings-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak tanılama ayarlarını yapılandırma

Kaynak Yöneticisi şablonuyla Tanılama ayarları oluşturmak veya güncelleştirmek için [Kaynak Yöneticisi şablonu kullanarak Azure izleyici 'de tanılama ayarı oluşturma](diagnostic-settings-template.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure platformu günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md)
