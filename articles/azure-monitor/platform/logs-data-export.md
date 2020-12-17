---
title: Azure Izleyici 'de çalışma alanı verilerini dışarı aktarma Log Analytics (Önizleme)
description: Log Analytics veri dışa aktarma, seçili tabloların verilerini Log Analytics çalışma alanınızdan Azure depolama hesabına veya Azure Event Hubs toplandığından sürekli olarak dışarı aktaralmanıza olanak sağlar.
ms.subservice: logs
ms.topic: conceptual
ms.custom: references_regions, devx-track-azurecli
author: bwren
ms.author: bwren
ms.date: 10/14/2020
ms.openlocfilehash: 3b29245aed1b2c7767c340cbe8cd35dfa38610b9
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97656691"
---
# <a name="log-analytics-workspace-data-export-in-azure-monitor-preview"></a>Azure Izleyici 'de çalışma alanı verilerini dışarı aktarma Log Analytics (Önizleme)
Azure Izleyici 'de Log Analytics çalışma alanı verileri dışarı aktarma işlemi, Log Analytics çalışma alanınızdaki seçili tablolardan verileri sürekli olarak bir Azure depolama hesabına veya Azure Event Hubs toplanarak dışarı aktaralmanıza olanak sağlar. Bu makalede, bu özellik hakkında ayrıntılar ve çalışma alanlarınızdaki veri dışarı aktarmayı yapılandırma adımları sağlanmaktadır.

## <a name="overview"></a>Genel Bakış
Log Analytics çalışma alanınız için veri dışa aktarma yapılandırıldıktan sonra, çalışma alanındaki seçili tablolara gönderilen tüm yeni veriler otomatik olarak depolama hesabınıza veya Olay Hub 'ınıza neredeyse gerçek zamanlı olarak dışa aktarılabilir.

![Veri dışarı aktarmaya genel bakış](media/logs-data-export/data-export-overview.png)

Dahil edilen tablolardaki tüm veriler bir filtre olmadan verilir. Örneğin, *Securityevent* tablosu için bir veri dışa aktarma kuralı yapılandırdığınızda, *securityevent* tablosuna gönderilen tüm veriler yapılandırma zamanından itibaren dışarı aktarılabilir.


## <a name="other-export-options"></a>Diğer dışarı aktarma seçenekleri
Log Analytics çalışma alanı verileri dışarı aktarma bir Log Analytics çalışma alanından sürekli olarak veri aktarır. Belirli senaryolara yönelik verileri dışarı aktarmaya yönelik diğer seçenekler şunlardır:

- Mantıksal uygulama kullanılarak günlük sorgusundan zamanlanan dışarı aktarma. Bu, veri dışa aktarma özelliğine benzerdir, ancak filtrelenmiş veya toplanmış verileri Azure depolama 'ya göndermenizi sağlar. Bu yöntem, [günlük sorgusu sınırlarına](../service-limits.md#log-analytics-workspaces)  tabidir, [Logic App kullanarak Log Analytics çalışma alanındaki arşiv verilerini Azure depolama 'ya](logs-export-logic-app.md)görün.
- Bir mantıksal uygulama kullanarak bir kerelik dışarı aktarma. [Logic Apps ve güç otomatikleştirme için bkz. Azure Izleyici günlükleri Bağlayıcısı](logicapp-flow-connector.md).
- PowerShell betiği kullanılarak yerel makineye bir kerelik dışarı aktarma. Bkz. [Invoke-Azoperationalınsightsqueryexport](https://www.powershellgallery.com/packages/Invoke-AzOperationalInsightsQueryExport).


## <a name="current-limitations"></a>Geçerli sınırlamalar

- Yapılandırma Şu anda yalnızca CLı veya REST istekleri kullanılarak gerçekleştirilebilir. Azure portal veya PowerShell kullanamazsınız.
- ```--export-all-tables```CLI ve REST seçenekleri desteklenmez ve kaldırılacak. Dışarı aktarma kurallarında Tablo listesini açıkça sağlamanız gerekir.
- Desteklenen tablolar şu anda aşağıdaki [desteklenen tablolar](#supported-tables) bölümünde yer aldığı sınırlandırıldı. Veri dışa aktarma kuralı desteklenmeyen bir tablo içeriyorsa, işlem başarılı olur, ancak bu tablo için hiçbir veri aktarılmaz. Veri dışa aktarma kuralı mevcut olmayan bir tablo içeriyorsa, hata ile başarısız olur ```Table <tableName> does not exist in the workspace.```
- Log Analytics çalışma alanınız aşağıdakiler dışında herhangi bir bölgede olabilir:
  - İsviçre Kuzey
  - İsviçre Batı
  - Azure Kamu bölgeleri
- Hedef depolama hesabı veya Olay Hub 'ı Log Analytics çalışma alanıyla aynı bölgede olmalıdır.
- Aktarılacak tabloların adları, bir depolama hesabı için 60 karakterden uzun ve bir olay hub 'ına en fazla 47 karakterden oluşabilir. Daha uzun adlara sahip tablolar verilmeyecektir.

> [!NOTE]
> Log Analytics veri dışa aktarma verileri, şu anda Azure Data Lake Storage 2. için önizleme aşamasında olan Append blobu olarak yazar. Bu depolama alanına dışarı aktarmayı yapılandırmadan önce bir destek isteği açmalısınız. Bu istek için aşağıdaki ayrıntıları kullanın.
> - Sorun türü: Teknik
> - Abonelik: Aboneliğiniz
> - Hizmet: Data Lake Storage 2.
> - Kaynak: kaynak adınız
> - Özet: Log Analytics verileri dışarı aktarma işleminden verileri kabul etmek için abonelik kaydı Isteniyor.
> - Sorun türü: bağlantı
> - Sorun alt türü: bağlantı sorunu

## <a name="data-completeness"></a>Veri bütünlüğü
Verilerin dışarı aktarılması, hedefin kullanılamaz durumda olması durumunda 30 dakikaya kadar veri göndermeyi yeniden denemeye devam edecektir. 30 dakika sonra hala kullanılamıyorsa, veriler hedef kullanılabilir olana kadar atılır.

## <a name="cost"></a>Cost
Şu anda veri dışa aktarma özelliği için ek ücret alınmaz. Veri dışa aktarma fiyatlandırması gelecekte duyurulacaktır ve faturalandırma başlamadan önce bir uyarı verilir. Bildirim süresinden sonra veri dışa aktarmayı kullanmaya devam etmeyi seçerseniz, ilgili ücret üzerinden faturalandırılırsınız.

## <a name="export-destinations"></a>Hedefleri dışarı aktar

### <a name="storage-account"></a>Depolama hesabı
Veriler her saat için depolama hesaplarına gönderilir. Veri dışa aktarma yapılandırması, depolama hesabındaki her tablo için, adı *ve ardından tablonun adını taşıyan* bir kapsayıcı oluşturur. Örneğin, *securityevent* tablosu, *har-securityevent* adlı bir kapsayıcıya gönderilir.

Depolama hesabı blob yolu *WorkspaceResourceId =/Subscriptions/Subscription-id/ResourceGroups/ \<resource-group\> /providers/Microsoft.operationalinsights/Workspaces/ \<workspace\> /y = \<four-digit numeric year\> /m = \<two-digit numeric month\> /d = \<two-digit numeric day\> /h = \<two-digit 24-hour clock hour\> /m = 00/PT1H.js'* dir. Ekleme Blobları depolama alanındaki 50K yazmaları ile sınırlı olduğundan, eklenen BLOB sayısı yüksek ise, eklenen Blobların sayısı uzatabilirler. Bu tür bir örnekte Blobların adlandırma deseninin PT1H_ #. JSON olması, burada #, artımlı blob sayısıdır.

Depolama hesabı veri biçimi [JSON hatdır](./resource-logs-blob-format.md). Bu, her kaydın, dış kayıt dizisi olmadan ve JSON kayıtları arasında hiçbir virgül olmadan bir yeni satır tarafından sınırlandırılır. 

[![Depolama örneği verileri](media/logs-data-export/storage-data.png)](media/logs-data-export/storage-data.png#lightbox)

Log Analytics veri dışa aktarma, zaman tabanlı bekletme ilkelerinde *allowProtectedAppendWrites* ayarı etkinse, sabit depolama hesaplarına ekleme Blobları yazabilir. Bu, bir ekleme blobuna yeni blokların yazılmasına izin verir, bu sayede koruma ve uyumluluğu koruma sağlar. Bkz. [korumalı ekleme Blobları yazmasına Izin ver](../../storage/blobs/storage-blob-immutable-storage.md#allow-protected-append-blobs-writes).

### <a name="event-hub"></a>Olay hub'ı
Veriler, Azure Izleyici 'ye ulaştığında neredeyse gerçek zamanlı olarak olay hub 'ınıza gönderilir. Bir olay hub 'ı, adı " *ve ardından tablonun adı ile* dışarı aktarma yaptığınız her veri türü için oluşturulur. Örneğin, *securityevent* tablosu, *har-securityevent* adlı bir olay hub 'ına gönderilir. Dışarı aktarılmış verilerin belirli bir olay hub 'ına ulaşmasını istiyorsanız veya 47 karakter sınırını aşan bir ada sahip bir tablonuz varsa, kendi olay hub 'ınızın adını girip tanımlanmış tablolar için tüm verileri buna aktarabilirsiniz.

Dikkat edilmesi gerekenler:
1. ' Temel ' Olay Hub 'ı SKU, daha düşük olay boyutu [sınırını](../../event-hubs/event-hubs-quotas.md#basic-vs-standard-tiers) destekler ve çalışma alanınızdaki bazı Günlükler onu aşabilir ve bırakılamaz. Dışarı aktarma hedefi olarak ' Standard ' veya ' adanmış ' Olay Hub 'ı kullanmanızı öneririz.
2. İçe aktarılmış verilerin hacmi genellikle zaman içinde artar ve daha büyük aktarım hızlarını işlemek ve kısıtlama senaryolarından ve veri gecikmesinden kaçınmak için Olay Hub 'ı ölçeğinin artması gerekir. İşleme birimlerinin sayısını otomatik olarak artırmak ve kullanım ihtiyaçlarını karşılamak için Event Hubs otomatik Şişir özelliğini kullanmanız gerekir. Ayrıntılar için bkz. [Azure Event Hubs üretilen iş birimlerini otomatik olarak ölçeklendirme](../../event-hubs/event-hubs-auto-inflate.md) .

## <a name="prerequisites"></a>Önkoşullar
Log Analytics veri dışarı aktarma yapılandırmadan önce tamamlanması gereken önkoşullar aşağıda verilmiştir.

- Depolama hesabının ve Olay Hub 'ının zaten oluşturulması ve Log Analytics çalışma alanıyla aynı bölgede olması gerekir. Verilerinizi diğer depolama hesaplarına çoğaltmanız gerekiyorsa [Azure depolama yedekliliği seçeneklerinin](../../storage/common/storage-redundancy.md)herhangi birini kullanabilirsiniz.  
- Depolama hesabı StorageV1 veya StorageV2 olmalıdır. Klasik depolama desteklenmiyor  
- Depolama hesabınızı seçili ağlardan erişime izin verecek şekilde yapılandırdıysanız, Azure Izleyici 'nin depolama alanına yazmasını sağlamak için depolama hesabı ayarlarınıza bir özel durum eklemeniz gerekir.

## <a name="enable-data-export"></a>Veri vermeyi etkinleştir
Log Analytics verilerinin dışarı aktarılmasını etkinleştirmek için aşağıdaki adımları uygulamanız gerekir. Hakkında daha fazla bilgi için aşağıdaki bölümlere bakın.

- Kaynak sağlayıcısını kaydedin.
- Güvenilen Microsoft hizmetlerine izin verin.
- Dışarı aktarılacak tabloları ve bunların hedefini tanımlayan bir veya daha fazla veri dışa aktarma kuralı oluşturun.

### <a name="register-resource-provider"></a>Kaynak sağlayıcısını kaydetme
Log Analytics verilerini dışarı aktarmaya olanak tanımak için aşağıdaki Azure Kaynak sağlayıcısının aboneliğinize kayıtlı olması gerekir. 

- Microsoft. Insights

Bu kaynak sağlayıcısı muhtemelen çoğu Azure Izleyici kullanıcısı için zaten kayıtlı olacak. Doğrulamak için Azure portal **abonelikler** ' e gidin. Aboneliğinizi seçin ve ardından menüdeki **Ayarlar** bölümünde **kaynak sağlayıcıları** ' na tıklayın. **Microsoft. Insights**'ı bulun. Durumu **kayıtlıysa**, zaten kayıtlı olur. Aksi takdirde, kaydetmek için **Kaydet** ' e tıklayın.

[Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/management/resource-providers-and-types.md)bölümünde açıklandığı gibi, bir kaynak sağlayıcısını kaydetmek için de kullanılabilir yöntemlerden birini kullanabilirsiniz. Aşağıda PowerShell kullanan örnek bir komut verilmiştir:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.insights
```

### <a name="allow-trusted-microsoft-services"></a>Güvenilen Microsoft hizmetlerine izin ver
Depolama hesabınızı seçili ağlardan erişime izin verecek şekilde yapılandırdıysanız, Azure Izleyici 'nin hesaba yazmasına izin vermek için bir özel durum eklemeniz gerekir. Depolama Hesabınıza yönelik **güvenlik duvarları ve sanal ağlardan** , **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**' i seçin.

[![Depolama hesabı güvenlik duvarları ve sanal ağlar](media/logs-data-export/storage-account-vnet.png)](media/logs-data-export/storage-account-vnet.png#lightbox)


### <a name="create-or-update-data-export-rule"></a>Veri dışarı aktarma kuralı oluştur veya güncelleştir
Veri dışa aktarma kuralı, bir tablo kümesi için tek bir hedefe verilecek verileri tanımlar. Her hedef için bir kural oluşturabilirsiniz.


# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

Yok

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Yok

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Çalışma alanınızdaki tabloları görüntülemek için aşağıdaki CLı komutunu kullanın. İstediğiniz tabloları kopyalayıp veri dışa aktarma kuralına dahil etmek için yardımcı olabilir.

```azurecli
az monitor log-analytics workspace table list --resource-group resourceGroupName --workspace-name workspaceName --query [].name --output table
```

CLı kullanarak bir depolama hesabına veri dışarı aktarma kuralı oluşturmak için aşağıdaki komutu kullanın.

```azurecli
$storageAccountResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $storageAccountResourceId
```

CLı kullanarak bir olay hub 'ına veri dışarı aktarma kuralı oluşturmak için aşağıdaki komutu kullanın. Her tablo için ayrı bir olay hub 'ı oluşturulur.

```azurecli
$eventHubsNamespacesResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.EventHub/namespaces/namespaces-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $eventHubsNamespacesResourceId
```

CLı kullanarak belirli bir olay hub 'ına veri dışarı aktarma kuralı oluşturmak için aşağıdaki komutu kullanın. Tüm tablolar, belirtilen olay hub 'ının adına verilir. 

```azurecli
$eventHubResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.EventHub/namespaces/namespaces-name/eventHubName/eventhub-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $eventHubResourceId
```

# <a name="rest"></a>[REST](#tab/rest)

REST API kullanarak bir veri dışarı aktarma kuralı oluşturmak için aşağıdaki isteği kullanın. İstek, taşıyıcı belirteç yetkilendirmesi ve içerik türü uygulama/JSON kullanmalıdır.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

İsteğin gövdesi, tabloların hedefini belirtir. Aşağıda, bir depolama hesabı için REST isteğine yönelik bir örnek gövde verilmiştir.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
            "table1",
            "table2" 
        ],
        "enable": true
    }
}
```

Aşağıda bir olay hub 'ı için REST isteğinin örnek bir gövdesi verilmiştir.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
}
```

Aşağıda, Olay Hub 'ı adının sağlandığı bir olay hub 'ı için REST isteği için örnek bir gövde verilmiştir. Bu durumda, tüm aktarılmış veriler bu olay hub 'ına gönderilir.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name",
            "metaData": {
                "EventHubName": "eventhub-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
  }
}
```
---

## <a name="view-data-export-rule-configuration"></a>Veri dışa aktarma kuralı yapılandırmasını görüntüle

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

Yok

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Yok

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak bir veri dışa aktarma kuralının yapılandırmasını görüntülemek için aşağıdaki komutu kullanın.

```azurecli
az monitor log-analytics workspace data-export show --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

# <a name="rest"></a>[REST](#tab/rest)

REST API kullanarak bir veri dışarı aktarma kuralının yapılandırmasını görüntülemek için aşağıdaki isteği kullanın. İstek, taşıyıcı belirteç yetkilendirmesi kullanmalıdır.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```
---

## <a name="disable-an-export-rule"></a>Dışarı aktarma kuralını devre dışı bırak

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

Yok

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Yok

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Sınama gerçekleştirilirken belirli bir süre için verileri tutmanız gerekmiyorsa dışarı aktarma kuralları devre dışı bırakılabilir. CLı kullanarak bir veri dışarı aktarma kuralını devre dışı bırakmak için aşağıdaki komutu kullanın.

```azurecli
az monitor log-analytics workspace data-export update --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --enable false
```

# <a name="rest"></a>[REST](#tab/rest)

REST API kullanarak bir veri dışarı aktarma kuralını devre dışı bırakmak için aşağıdaki isteği kullanın. İstek, taşıyıcı belirteç yetkilendirmesi kullanmalıdır.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
"table1",
    "table2" 
        ],
        "enable": false
    }
}
```
---

## <a name="delete-an-export-rule"></a>Dışarı aktarma kuralını silme

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

Yok

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Yok

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak bir veri dışarı aktarma kuralını silmek için aşağıdaki komutu kullanın.

```azurecli
az monitor log-analytics workspace data-export delete --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

# <a name="rest"></a>[REST](#tab/rest)

REST API kullanarak bir veri dışarı aktarma kuralını silmek için aşağıdaki isteği kullanın. İstek, taşıyıcı belirteç yetkilendirmesi kullanmalıdır.

```rest
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```
---

## <a name="view-all-data-export-rules-in-a-workspace"></a>Çalışma alanındaki tüm veri dışarı aktarma kurallarını görüntüleme

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

Yok

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Yok

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak bir çalışma alanındaki tüm veri dışarı aktarma kurallarını görüntülemek için aşağıdaki komutu kullanın.

```azurecli
az monitor log-analytics workspace data-export list --resource-group resourceGroupName --workspace-name workspaceName
```

# <a name="rest"></a>[REST](#tab/rest)

REST API kullanarak bir çalışma alanındaki tüm veri dışarı aktarma kurallarını görüntülemek için aşağıdaki isteği kullanın. İstek, taşıyıcı belirteç yetkilendirmesi kullanmalıdır.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports?api-version=2020-08-01
```
---

## <a name="unsupported-tables"></a>Desteklenmeyen tablolar
Veri dışa aktarma kuralı desteklenmeyen bir tablo içeriyorsa, yapılandırma başarılı olur, ancak bu tablo için hiçbir veri aktarılmaz. Tablo daha sonra destekleniyorsa, verileri o anda dışarıya kaydedilir.

Veri dışa aktarma kuralı mevcut olmayan bir tablo içeriyorsa, "tablo <tableName> çalışma alanında yok" hatasıyla başarısız olur.


## <a name="supported-tables"></a>Desteklenen tablolar
Desteklenen tablolar şu anda aşağıda belirtilenler ile sınırlıdır. Kısıtlama belirtilmediği takdirde tablodaki tüm veriler verilecek. Bu liste, ek tablolara yönelik destek eklendikçe güncelleştirilecektir.


| Tablo | Sınırlamalar |
|:---|:---|
| AADDomainServicesAccountLogon | |
| AADDomainServicesAccountManagement | |
| AADDomainServicesDirectoryServiceAccess | |
| AADDomainServicesLogonLogoff | |
| AADDomainServicesPolicyChange | |
| AADDomainServicesPrivilegeUse | |
| Aadmanagedıdentitysignınlogs | |
| Aadnonınteractiveusersignınlogs | |
| AADProvisioningLogs | |
| Aadserviceprincipalsignınlogs | |
| Adassessmentönerisi | |
| ADFActivityRun | |
| Adfardışık düzen eylemsizlik | |
| ADFTriggerRun | |
| ADReplicationResult | |
| Adsecurityassessmentönerisi | |
| ADTDigitalTwinsOperation | |
| ADTEventRoutesOperation | |
| ADTModelsOperation | |
| ADTQueryOperation | |
| ADXCommand | |
| ADXQuery | |
| Ayumurdeliveryfailurelogs | |
| Ayumurpublishfailurelogs | |
| Uyarı |Kısmi destek. Bu tabloya ait verilerden bazıları depolama hesabı üzerinden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| Anomalileri | |
| ApiManagementGatewayLogs | |
| Appcenterhatası | |
| AppPlatformSystemLogs | |
| AppServiceAppLogs | |
| AppServiceAuditLogs | |
| AppServiceConsoleLogs | |
| AppServiceFileAuditLogs | |
| AppServiceHTTPLogs | |
| AppServiceIPSecAuditLogs | |
| AppServicePlatformLogs | |
| AuditLogs | |
| AutoscaleEvaluationsLog | |
| Oto Scalescaleactionslog | |
| Awscses Izi | |
| Azureassessmentönerisi | |
| AzureDevOpsAuditing | |
| BehaviorAnalytics | |
| BlockchainApplicationLog | |
| BlockchainProxyLog | |
| BlockchainProxyLog | |
| CommonSecurityLog | |
| CommonSecurityLog | |
| ComputerGroup | |
| ConfigurationData | Kısmi destek. Bazı veriler dışarı aktarma için desteklenmeyen iç hizmetlerden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| Containerımageınventory | |
| Containerınventory | |
| ContainerLog | |
| Containernodeınventory | |
| ContainerServiceLog | |
| CoreAzureBackup | |
| DatabricksAccounts | |
| DatabricksClusters | |
| DatabricksDBFS | |
| Databricksınstancepools | |
| DatabricksJobs | |
| DatabricksNotebook | |
| Databricksgizlilikler | |
| Databricksstopermissions | |
| DatabricksSSH | |
| DatabricksWorkspace | |
| DnsEvents | |
| DnsInventory | |
| Dynamics365Activity | |
| Olay | Kısmi destek. Bu tabloya ait verilerden bazıları depolama hesabı üzerinden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| ExchangeAssessmentRecommendation | |
| FailedIngestion | |
| FunctionAppLogs | |
| HDInsightAmbariClusterAlerts | |
| HDInsightAmbariSystemMetrics | |
| HDInsightGatewayAuditLogs | |
| HDInsightHadoopAndYarnLogs | |
| HDInsightHadoopAndYarnMetrics | |
| HDInsightHBaseLogs | |
| HDInsightHBaseMetrics | |
| HDInsightHiveAndLLAPLogsSample | |
| HDInsightKafkaLogs | |
| HDInsightKafkaMetrics | |
| HDInsightOozieLogs | |
| HDInsightSecurityLogs | |
| HDInsightSparkApplicationEvents | |
| HDInsightSparkBlockManagerEvents | |
| HDInsightSparkEnvironmentEvents | |
| HDInsightSparkEventsLog | |
| HDInsightSparkExecutorEvents | |
| HDInsightSparkExtraEvents | |
| HDInsightSparkJobEvents | |
| HDInsightSparkLogs | |
| HDInsightSparkSQLExecutionEvents | |
| HDInsightSparkStageEvents | |
| HDInsightSparkStageTaskAccumulables | |
| HDInsightSparkTaskEvents | |
| HDInsightStormLogs | |
| HDInsightStormMetrics | |
| HDInsightStormTopologyMetrics | |
| Sinyal | |
| HuntingBookmark | |
| Insightsölçümlerini | Kısmi destek. Bazı veriler dışarı aktarma için desteklenmeyen iç hizmetlerden alınır. Bu bölüm şu anda dışa aktarma içinde yok. |
| Intuneauditlogs | |
| Intunedevicekarmaşıkanceorg | |
| Intuneoperationallogs | |
| KubeEvents | |
| KubeHealth | |
| KubeMonAgentEvents | |
| Kubenodeınventory | |
| KubePodInventory | |
| KubeServices | |
| KubeServices | |
| LAQueryLogs 'lar | |
| McasShadowItReporting | |
| MicrosoftAzureBastionAuditLogs | |
| Microsoftdatasharerecei, Snapshotlog | |
| MicrosoftDataShareSentSnapshotLog | |
| MicrosoftDataShareShareLog | |
| Microsofthealthumererererce | |
| NWConnectionMonitorDestinationListenerResult | |
| NWConnectionMonitorDNSResult | |
| NWConnectionMonitorPathResult | |
| NWConnectionMonitorPathResult | |
| NWConnectionMonitorTestResult | |
| NWConnectionMonitorTestResult | |
| Officeetkinliği | Kısmi destek. Office 365 ' den Web kancaları aracılığıyla alınacak verilerden bazıları Log Analytics. Bu veriler şu anda dışarıya aktarılmamış. |
| Çalışma | Kısmi destek. Bazı veriler dışarı aktarma için desteklenmeyen iç hizmetlerden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| Perf | Kısmi destek. Şu anda yalnızca Windows performans verileri destekleniyor. Linux performans verileri şu anda dışarıya aktarılmamış. |
| ProtectionStatus | |
| Sccmassessmentönerisi | |
| Scomassessmentönerisi | |
| SecurityAlert | |
| SecurityBaseline | |
| SecurityBaselineSummary | |
| SecurityDetection | |
| SecurityEvent | |
| Securityıncident | |
| Securityıotrawevent | |
| Securitynestedönerisi | |
| Securityönerisi | |
| Sfbassessmentönerisi | |
| SfBOnlineAssessmentRecommendation | |
| SharePointOnlineAssessmentRecommendation | |
| Signalrservicediagnoyapışlogs | |
| Signınlogs | |
| Spassessmentönerisi | |
| Sqlassessmentönerisi | |
| SucceededIngestion | |
| SynapseGatewayEvents | |
| SynapseRBACEvents | |
| Syslog | Kısmi destek. Bu tabloya ait verilerden bazıları depolama hesabı üzerinden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| Threatıntelligenceındicator | |
| Güncelleştir | Kısmi destek. Bazı veriler dışarı aktarma için desteklenmeyen iç hizmetlerden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| UpdateRunProgress | |
| UpdateSummary | |
| Kullanım | |
| UserAccessAnalytics | |
| UserPeerAnalytics | |
| Listem | |
| WindowsEvent | |
| WindowsFirewall | |
| Iletilen veri | Kısmi destek. Bazı veriler dışarı aktarma için desteklenmeyen iç hizmetlerden alınır. Bu veriler şu anda dışarıya aktarılmamış. |
| WorkloadMonitoringPerf | |
| WVDAgentHealthStatus | |
| WVDCheckpoints | |
| WVDConnections | |
| WVDErrors | |
| WVDFeeds | |
| WVDManagement | |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gezgini 'den aktarılmış verileri sorgulayın](azure-data-explorer-query-storage.md).
