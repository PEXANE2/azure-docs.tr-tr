---
title: Farklı hedeflere platform günlükleri ve ölçümleri göndermek için Tanılama ayarları oluşturma
description: Azure izleyici platformu ölçümlerini ve günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs bir tanılama ayarı kullanarak gönderin.
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2020
ms.subservice: logs
ms.openlocfilehash: 0a9eaeb9b77c7b4dd7e0b2347c66de3a325a66ee
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505185"
---
# <a name="create-diagnostic-settings-to-send-platform-logs-and-metrics-to-different-destinations"></a>Farklı hedeflere platform günlükleri ve ölçümleri göndermek için Tanılama ayarları oluşturma
Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](platform-logs-overview.md) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. [Platform ölçümleri](data-platform-metrics.md) varsayılan olarak toplanır ve genellikle Azure izleyici ölçümleri veritabanında depolanır. Bu makalede, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için tanılama ayarlarını oluşturma ve yapılandırma hakkında ayrıntılı bilgi verilmektedir.

> [!IMPORTANT]
> Etkinlik günlüğü için bir tanılama ayarı oluşturmadan önce, önce eski yapılandırmaları devre dışı bırakmanız gerekir. Ayrıntılar için bkz. [eski koleksiyon yöntemleri](activity-log.md#legacy-collection-methods) .

Her Azure kaynağı, aşağıdaki ölçütleri tanımlayan kendi tanılama ayarını gerektirir:

- Ayarda tanımlanan hedeflere gönderilen günlük kategorileri ve ölçüm verileri. Kullanılabilir Kategoriler farklı kaynak türleri için farklılık gösterir.
- Günlükleri göndermek için bir veya daha fazla hedef. Geçerli hedeflere Log Analytics çalışma alanı, Event Hubs ve Azure Storage dahildir.

Tek bir tanılama ayarı, hedeflerin her birinden fazlasını tanımlayabilir. Belirli bir hedef türünden birine (örneğin, iki farklı Log Analytics çalışma alanı) birden fazla veri göndermek istiyorsanız, daha sonra birden çok ayar oluşturun. Her kaynak en fazla 5 tanılama ayarlarına sahip olabilir.

Aşağıdaki videoda, platform günlüklerine tanılama ayarlarıyla yönlendirme adımları gösterilmektedir.
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4AvVO]

> [!NOTE]
> [Platform ölçümleri](metrics-supported.md) , [Azure izleyici ölçümlerine](data-platform-metrics.md)otomatik olarak gönderilir. Tanılama ayarları belirli Azure hizmetleri için Azure Izleyici günlüklerine yönelik ölçümleri, belirli sınırlamalara sahip [günlük sorgularını](../log-query/log-query-overview.md) kullanarak diğer izleme verileriyle analiz edilmek üzere göndermek için kullanılabilir. 
>  
>  
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır. *Örneğin*: bir blok zincirindeki ' ıoreadbytes ' ölçümü, düğüm düzeyinde araştırılabilir ve bu şekilde grafiklenebilir. Ancak, Tanılama ayarları aracılığıyla verildiğinde, tüm düğümler için tüm okuma baytları temsil eder. Ayrıca, iç sınırlamalar nedeniyle tüm ölçümler Azure Izleyici günlüklerine/Log Analytics dışa aktarılabilir. Daha fazla bilgi için bkz. [dışa aktarılabilir ölçümler listesi](metrics-supported-export-diagnostic-settings.md). 
>  
>  
> Belirli ölçümler için bu sınırlamaları ortadan kaldırmak amacıyla, [ölçüm REST API](/rest/api/monitor/metrics/list) kullanarak bunları el ile ayıklamanızı ve [Azure IZLEYICI veri toplayıcı API](data-collector-api.md)'Sini kullanarak bunları Azure izleyici günlüklerine almanızı öneririz.  


## <a name="destinations"></a>Hedefler

Platform günlükleri ve ölçümleri aşağıdaki tablodaki hedeflere gönderilebilir. Bu hedefe veri gönderme hakkındaki ayrıntılar için aşağıdaki tablodaki her bir bağlantıyı izleyin.

| Hedef | Description |
|:---|:---|
| [Log Analytics çalışma alanı](#log-analytics-workspace) | Log Analytics çalışma alanına Günlükler ve ölçümler gönderme, güçlü günlük sorguları ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Izleyici özelliklerinden yararlanmak için Azure Izleyici tarafından toplanan diğer izleme verileriyle analiz etmenizi sağlar. |
| [Olay hub’ları](#event-hub) | Event Hubs Günlükler ve ölçümler gönderme, üçüncü taraf SIG 'ler ve diğer Log Analytics çözümleri gibi dış sistemlere veri akışını sağlar. |
| [Azure depolama hesabı](#azure-storage) | Günlükleri ve ölçümleri bir Azure depolama hesabına arşivleme, denetim, statik analiz veya yedekleme için faydalıdır. Azure Izleyici günlükleri ve bir Log Analytics çalışma alanıyla karşılaştırıldığında, Azure Storage daha pahalı olur ve Günlükler sonsuza kadar tutulabilirler. |


## <a name="prerequisites"></a>Önkoşullar
Tanılama ayarının tüm hedefleri gereken izinlerle oluşturulmalıdır. Her hedefle ilgili önkoşul gereksinimleri için aşağıdaki bölümlere bakın.

### <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Henüz bir tane yoksa [Yeni bir çalışma alanı oluşturun](../learn/quick-create-workspace.md) . Ayarı yapılandıran kullanıcının her iki aboneliğe de uygun RBAC erişimi olduğundan, çalışma alanının kaynakla aynı abonelikte olması gerekmez.

### <a name="event-hub"></a>Olay hub'ı
Henüz bir tane yoksa [bir olay hub 'ı oluşturun](../../event-hubs/event-hubs-create.md) . Ayarı yapılandıran kullanıcının her iki aboneliğe ve her iki aboneliğe de aynı AAD kiracısına sahip olması koşuluyla, Event Hubs ad alanının, günlükleri yayan abonelikle aynı abonelikte olması gerekmez.

Ad alanı için paylaşılan erişim ilkesi, akış mekanizmanın sahip olduğu izinleri tanımlar. Event Hubs akışının yönetilmesi, gönderilmesi ve dinlemesi izinlerinin olması gerekir. Event Hubs ad alanınız için Yapılandır sekmesinin altındaki Azure portal paylaşılan erişim ilkeleri oluşturabilir veya değiştirebilirsiniz. Tanılama ayarını akış içerecek şekilde güncelleştirmek için, bu Event Hubs yetkilendirme kuralında ListKey izninizin olması gerekir. 


### <a name="azure-storage"></a>Azure depolama
Henüz yoksa bir [Azure depolama hesabı oluşturun](../../storage/common/storage-account-create.md) . Ayarı yapılandıran kullanıcının her iki aboneliğe de uygun RBAC erişimi olması koşuluyla, depolama hesabının, kaynak gönderme günlükleri ile aynı abonelikte olması gerekmez.

Verilere erişimi daha iyi denetleyebilmeniz için, içinde depolanan diğer, izleme olmayan verileri olan mevcut bir depolama hesabını kullanmamalısınız. Aynı şekilde etkinlik günlüğü ve kaynak günlüklerini birlikte arşivlerken, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı tercih edebilirsiniz.

Verileri sabit depolamaya göndermek için, depolama hesabının sabit ilkesini [BLOB depolama Için ayarlama ve yönetme kuralları](../../storage/blobs/storage-blob-immutability-policies-manage.md)bölümünde açıklandığı gibi ayarlayın. Bu makaledeki korumalı ekleme bloblarını yazmayı etkinleştirme dahil tüm adımları izlemeniz gerekir.

> [!NOTE]
> Azure portalında geçerli seçeneklerden biri olarak listeleniyor olsa bile, Azure Data Lake Storage 2. Nesil hesapları şu anda tanılama ayarlarının hedefi olarak desteklenmemektedir.



## <a name="create-in-azure-portal"></a>Azure portalda oluşturma

Azure portal tanılama ayarlarını Azure Izleyici menüsünden ya da kaynak menüsünden yapılandırabilirsiniz.

1. Azure portal tanılama ayarlarını yapılandırdığınız konum kaynağa bağlıdır.

   - Tek bir kaynak için kaynak menüsünde **izleyici** ' nin altındaki **Tanılama ayarları** ' na tıklayın.

        ![Tanılama ayarları](media/diagnostic-settings/menu-resource.png)

   - Bir veya daha fazla kaynak için, Azure Izleyici menüsünde **Ayarlar** altında **Tanılama ayarları** ' na tıklayın ve ardından kaynağa tıklayın.

      ![Tanılama ayarları](media/diagnostic-settings/menu-monitor.png)

   - Etkinlik günlüğü için, **Azure izleyici** menüsünde **etkinlik günlüğü** ' ne ve ardından **Tanılama ayarları**' na tıklayın. Etkinlik günlüğü için eski tüm yapılandırmaları devre dışı bıraktığınızdan emin olun. Ayrıntılar için bkz. [var olan ayarları devre dışı bırakma](./activity-log.md#legacy-collection-methods) .

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
       > Bu makalenin önceki kısımlarında yer alarak ölçümleri Azure Izleyici günlüklerine yönlendirme için bkz. limitatation.  


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

## <a name="create-using-powershell"></a>PowerShell kullanarak oluşturma

[Azure PowerShell](../samples/powershell-samples.md)bir tanılama ayarı oluşturmak için [set-azdiagnosticsetting](/powershell/module/az.monitor/set-azdiagnosticsetting) cmdlet 'ini kullanın. Bu cmdlet 'in parametrelerinin açıklamaları için belgelerine bakın.

> [!IMPORTANT]
> Azure etkinlik günlüğü için bu yöntemi kullanamazsınız. Bunun yerine, bir Kaynak Yöneticisi şablonu oluşturmak ve PowerShell ile dağıtmak için [Kaynak Yöneticisi şablonu kullanarak Azure izleyici 'de tanılama ayarı oluştur](diagnostic-settings-template.md) ' u kullanın.

Aşağıda, üç hedefi kullanarak bir tanılama ayarı oluşturan örnek bir PowerShell cmdlet 'i verilmiştir.

```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

## <a name="create-using-azure-cli"></a>Azure CLı kullanarak oluşturma

[Azure CLI](/cli/azure/monitor?view=azure-cli-latest)ile bir tanılama ayarı oluşturmak için [az Monitor Diagnostic-Settings Create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) komutunu kullanın. Parametrelerinin açıklamaları için bu komuta yönelik belgelere bakın.

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

## <a name="create-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak oluşturma
Tanılama ayarlarını bir Kaynak Yöneticisi şablonuyla oluşturmak veya güncelleştirmek için bkz. [Azure izleyici 'de Tanılama ayarları için Kaynak Yöneticisi şablonu örnekleri](../samples/resource-manager-diagnostic-settings.md) .

## <a name="create-using-rest-api"></a>REST API kullanarak oluşturma
[Azure izleyici REST API](/rest/api/monitor/)kullanarak tanılama ayarlarını oluşturmak veya güncelleştirmek için bkz. [Tanılama ayarları](/rest/api/monitor/diagnosticsettings) .

## <a name="create-using-azure-policy"></a>Azure Ilkesi kullanarak oluşturma
Her Azure kaynağı için bir tanılama ayarının oluşturulması gerektiğinden, her kaynak oluşturulduğu için Azure Ilkesi otomatik olarak bir tanılama ayarı oluşturmak üzere kullanılabilir. Ayrıntılar için bkz. [Azure izleyici 'yi kullanarak ölçekte Azure Izleyici dağıtma](deploy-scale.md) .


## <a name="next-steps"></a>Sonraki adımlar

- [Azure platformu günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md)
