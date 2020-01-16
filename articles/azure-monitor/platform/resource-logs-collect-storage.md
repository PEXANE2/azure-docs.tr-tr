---
title: Azure Kaynak günlüklerini depolama hesabına arşivleme | Microsoft Docs
description: Depolama hesabında uzun süreli saklama için Azure Kaynak günlüklerinizi arşivlemeyi öğrenin.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: e99a0e8e42bcfb5c7967a9cb1c91631bacbea53a
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980069"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>Azure Kaynak günlüklerini depolama hesabına arşivleme
Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](platform-logs-overview.md) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar.  Bu makalede, verilerin arşivlenmek üzere korunması için Azure depolama hesabına platform günlüklerinin toplanması açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Henüz yoksa bir [Azure depolama hesabı oluşturmanız](../../storage/common/storage-account-create.md) gerekir. Ayarı yapılandıran kullanıcının her iki aboneliğe de uygun RBAC erişimi olması koşuluyla, depolama hesabının, kaynak gönderme günlükleri ile aynı abonelikte olması gerekmez.


> [!IMPORTANT]
> Azure Data Lake Storage 2. hesapları, Azure portal geçerli bir seçenek olarak listelenseler bile, şu anda Tanılama ayarları için hedef olarak desteklenmemektedir.


Verilere erişimi daha iyi denetleyebilmeniz için, içinde depolanan diğer, izleme olmayan verileri olan mevcut bir depolama hesabını kullanmamalısınız. Aynı şekilde etkinlik günlüğü ve kaynak günlüklerini birlikte arşivlerken, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı tercih edebilirsiniz.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluştur
Azure kaynağı için bir tanılama ayarı oluşturarak, platform günlüklerini depolama ve diğer hedeflere gönderin. Ayrıntılar için bkz. [Azure 'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturma](diagnostic-settings.md) .


## <a name="collect-data-from-compute-resources"></a>İşlem kaynaklarından veri toplama
Tanılama ayarları, Azure işlem kaynakları için, Konuk işletim sistemleri veya iş yükleri değil, diğer kaynaklar gibi kaynak günlüklerini toplar. Bu verileri toplamak için [Windows Azure tanılama aracısını](diagnostics-extension-overview.md)yüklersiniz. Ayrıntılar için bkz. [Azure Storage 'da tanılama verilerini depolama ve görüntüleme](diagnostics-extension-to-storage.md) .


## <a name="schema-of-platform-logs-in-storage-account"></a>Depolama hesabındaki platform günlüklerinin şeması

Tanılama ayarını oluşturduktan sonra, etkin günlük kategorilerinden birinde bir olay meydana geldiğinde depolama hesabında bir depolama kapsayıcısı oluşturulur. Kapsayıcı içindeki Bloblar aşağıdaki adlandırma kuralını kullanır:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir ağ güvenlik grubunun blobu aşağıdakine benzer bir ada sahip olabilir:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Her PT1H.json blobu, blob URL’sinde belirtilen saat (örneğin, h=12) içinde gerçekleşen bir JSON olay blobu içerir. Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Kaynak günlüğü olayları saat başına ayrı blob 'lara bölündüğü için, dakika değeri (e = 00) her zaman 00 ' dır.

PT1H. json dosyasında, her olay aşağıdaki biçimde depolanır. Bu, ortak bir en üst düzey şemayı kullanır, ancak [kaynak günlükleri şeması](diagnostic-logs-schema.md) ve [etkinlik günlüğü şeması](activity-log-schema.md)bölümünde açıklandığı gibi her bir Azure hizmeti için benzersizdir.

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Platform günlükleri, her olayın bir satır olduğu ve yeni bir olayı gösterdiği [JSON satırları](http://jsonlines.org/)kullanılarak BLOB depolama alanına yazılır. Bu biçim Kasım 2018 ' de uygulandı. Bu tarihten önce, [Azure izleyici platformu günlüklerine yapılan biçim değişikliği Için hazırlama](resource-logs-blob-format.md)bölümünde açıklandığı gibi bir JSON dizisi olarak blob depolamaya yazılan günlükler bir depolama hesabına arşivlendi.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md).
* [Azure 'da günlüklerin ve ölçümlerin toplanması için tanılama ayarı oluşturun](diagnostic-settings.md).
* [Analiz için Blobları indirin](../../storage/blobs/storage-quickstart-blobs-dotnet.md).
* [Azure izleyici ile Azure Active Directory günlüklerini arşivleyin](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md).
