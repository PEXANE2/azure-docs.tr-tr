---
title: Azure kaynak günlüklerini depolama hesabına arşivleyin | Microsoft Dokümanlar
description: Bir depolama hesabında uzun süreli bekletme için Azure kaynak günlüklerinizi nasıl arşivlendireceklerini öğrenin.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 843c179826b2064a1be24d3cee84b398987b4aed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274222"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>Azure kaynak günlüğünü depolama hesabına arşivleme
Azure Etkinliği günlüğü ve kaynak günlükleri de dahil olmak üzere Azure'daki [platform günlükleri,](platform-logs-overview.md) Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar.  Bu makalede, arşivleme için veri tutmak için bir Azure depolama hesabında platform günlükleri toplama açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Zaten bir Azure depolama hesabınız yoksa [bir Azure depolama hesabı oluşturmanız](../../storage/common/storage-account-create.md) gerekir. Depolama hesabı, ayarı yapılandıran kullanıcının her iki aboneye de uygun RBAC erişimine sahip olması nedeniyle, kaynak gönderme günlükleri ile aynı abonelikte olması gerekmez.


> [!IMPORTANT]
> Azure Veri Gölü Depolama Gen2 hesapları, Azure portalında geçerli bir seçenek olarak listelenmiş olsalar bile, şu anda tanılama ayarları için bir hedef olarak desteklenmemektedir.


Verilere erişimi daha iyi denetlemek için içinde başka, izlemeyen veriler depolanan varolan bir depolama hesabı kullanmamalısınız. Etkinlik günlüğünü ve kaynak günlüklerini birlikte arşivliyorsanız, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı seçebilirsiniz.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma
Azure kaynağı için tanılama ayarı oluşturarak platform günlüklerini depolama alanına ve diğer hedeflere gönderin. Ayrıntılar [için Azure'da günlükleri ve ölçümleri toplamak için tanı lama ayarını oluştur'a](diagnostic-settings.md) bakın.


## <a name="collect-data-from-compute-resources"></a>Bilgi işlem kaynaklarından veri toplama
Tanılama ayarları, diğer kaynaklar gibi Azure bilgi işlem kaynakları için kaynak günlükleri toplar, ancak konuk işletim sistemleri veya iş yükleri toplanmaz. Bu verileri toplamak için [Windows Azure Tanılama aracısını](diagnostics-extension-overview.md)yükleyin. 


## <a name="schema-of-platform-logs-in-storage-account"></a>Depolama hesabında platform günlüklerinin şeması

Tanılama ayarını oluşturduktan sonra, etkin günlük kategorilerinden birinde bir olay oluşur oluşmaz depolama hesabında bir depolama kapsayıcısı oluşturulur. Kapsayıcı içindeki lekeler aşağıdaki adlandırma kuralını kullanır:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin, bir ağ güvenlik grubu için blob aşağıdakilere benzer bir ad olabilir:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Her PT1H.json blobu, blob URL’sinde belirtilen saat (örneğin, h=12) içinde gerçekleşen bir JSON olay blobu içerir. Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Kaynak günlüğü olayları saatte tek tek blobs bölündüğünden, dakika değeri (m=00) her zaman 00'dır.

PT1H.json dosyası içinde, her olay aşağıdaki biçimde depolanır. Bu ortak bir üst düzey şema kullanır, ancak Kaynak [günlükleri şeması](diagnostic-logs-schema.md) ve [Etkinlik günlük şema](activity-log-schema.md)açıklandığı gibi her Azure hizmetleri için benzersiz dir.

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Platform günlükleri, her olayın bir satır olduğu ve yeni satır karakterinin yeni bir olayı gösterdiği [JSON satırları](http://jsonlines.org/)kullanılarak blob depolamaalanına yazılır. Bu format Kasım 2018'de uygulanmıştır. Bu tarihten önce, günlükler, [bir depolama hesabına arşivlenmiş Azure Monitor platform günlüklerine biçim değişikliği için hazırla'da](resource-logs-blob-format.md)açıklandığı gibi bir json kayıt dizisi olarak blob depolamasına yazılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin.](platform-logs-overview.md)
* [Azure'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturun.](diagnostic-settings.md)
* [Analiz için blobs indirin.](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Azure Monitor ile Azure Etkin Dizin günlüklerini arşivleyin.](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
