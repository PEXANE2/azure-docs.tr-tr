---
title: Günlük alımı için müşterinin sahip olduğu depolama hesapları
description: Azure Izleyici 'de bir Log Analytics çalışma alanına günlük verilerinin alımı için kendi depolama hesabınızı kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/20/2020
ms.openlocfilehash: 0c9982fd4aa6459cdcbd715077f08092075a9776
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84610075"
---
# <a name="customer-owned-storage-accounts-for-log-ingestion-in-azure-monitor"></a>Azure Izleyici 'de günlük alımı için müşterinin sahip olduğu depolama hesapları

Azure Izleyici, [özel Günlükler](data-sources-custom-logs.md) ve bazı [Azure günlükleri](azure-storage-iis-table.md)gibi bazı veri türlerinin alma işlemindeki depolama hesaplarını kullanır. Alma işlemi sırasında, Günlükler önce bir depolama hesabına gönderilir ve daha sonra Log Analytics veya Application Insights. Alma sırasında verileriniz üzerinde denetim istiyorsanız, hizmet tarafından yönetilen depolama yerine kendi depolama hesaplarınızı kullanabilirsiniz. Kendi depolama hesabınızı kullanmak, alma sırasında günlüklerin erişim, içerik, şifreleme ve bekletme işlemi üzerinde denetim sahibi olmanızı sağlar. Bu, kendi depolama alanınızı veya KCG 'LERI getir olarak adlandırdık. 

KCG gerektiren bir senaryo, özel bağlantılar aracılığıyla ağ yalıtımına sahiptir. VNet kullanılırken, ağ yalıtımı genellikle gereksinimdir ve genel internet erişimi sınırlıdır. Bu gibi durumlarda, günlük alma için Azure Izleyici hizmeti depolamaya erişmek tamamen engelleniyor veya hatalı bir uygulama olarak kabul edilir. Bunun yerine, günlüklerin VNet içindeki bir müşterinin sahip olduğu veya kolayca erişilebilen bir depolama hesabı aracılığıyla kullanılması gerekir.

Başka bir senaryo, müşteri tarafından yönetilen anahtarlarla günlüklerin şifrelenme (CMK). Müşteriler, günlükleri depolayan kümeler üzerinde CMK kullanarak günlüğe kaydedilmiş verileri şifreleyebilir. Aynı anahtar, alma işlemi sırasında günlükleri şifrelemek için de kullanılabilir.

## <a name="data-types-supported"></a>Desteklenen veri türleri

Bir depolama hesabından alınan veri türleri şunlardır. Bu türlerin alımı hakkında daha fazla bilgi için bkz. [Azure tanılama uzantısından Azure Izleyici günlüklerine veri toplama](azure-storage-iis-table.md) .

| Tür | Tablo bilgileri |
|:-----|:------------------|
| IIS günlükleri | Blob: wad-IIS-LogFiles|
|Windows olay günlükleri | Tablo: WADWindowsEventLogsTable |
| Syslog | Tablo: LinuxsyslogVer2v0 |
| Windows ETW günlükleri | Tablo: Wadelenebilir Venttable|
| Service Fabric | Tablo: WADServiceFabricSystemEventTable <br/> WADServiceFabricReliableActorEventTable<br/> WADServiceFabricReliableServicEventTable |
| Özel günlükler | yok |
| Azure Güvenlik Merkezi Watson döküm dosyaları | yok|  

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri 
Depolama hesabının aşağıdaki gereksinimleri karşılaması gerekir:

- VNet 'iniz üzerinde günlükleri depolama alanına yazan kaynaklar için erişilebilir.
- Bağlandığı çalışma alanıyla aynı bölgede olmalıdır.
- Açık olarak, *GÜVENILEN MS hizmetlerinin bu depolama hesabına erişmesine izin ver*' i seçerek depolama hesabından günlükleri okumasına izin Log Analytics.

## <a name="process-to-configure-customer-owned-storage"></a>Müşteriye ait depolamayı yapılandırma işlemi
Alma için kendi depolama hesabınızı kullanmanın temel süreci aşağıdaki gibidir:

1. Bir depolama hesabı oluşturun veya var olan bir hesabı seçin.
2. Depolama hesabını bir Log Analytics çalışma alanına bağlayın.
3. Yük ve bekletmeyi inceleyerek, beklendiği gibi çalıştığından emin olmak için depolamayı yönetin.

Bağlantıların oluşturulması ve kaldırılması için kullanılabilecek tek yöntem REST API. Her işlem için gerekli olan belirli API isteği hakkındaki ayrıntılar aşağıdaki bölümlerde verilmiştir.

## <a name="api-request-values"></a>API istek değerleri

#### <a name="datasourcetype"></a>dataSourceType 

- AzureWatson-bu değeri Azure Güvenlik Merkezi Azure Watson döküm dosyaları için kullanın.
- CustomLogs – bu değeri aşağıdaki veri türleri için kullanın:
  - Özel günlükler
  - IIS Günlükleri
  - Olaylar (Windows)
  - Syslog (Linux)
  - ETW günlükleri
  - Service Fabric olaylar
  - Değerlendirme verileri  

#### <a name="storage_account_resource_id"></a>storage_account_resource_id
Bu değer aşağıdaki yapıyı kullanır:

```
subscriptions/{subscriptionId}/resourcesGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName1}
```



## <a name="get-current-links"></a>Geçerli bağlantıları al

### <a name="get-linked-storage-accounts-for-a-specific-data-source-type"></a>Belirli bir veri kaynağı türü için bağlantılı depolama hesapları al

#### <a name="api-request"></a>API isteği

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

#### <a name="response"></a>Yanıt 

```json
{
    "properties":
    {
        "dataSourceType": "CustomLogs",
        "storageAccountIds  ": 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    },
    "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
    "name": "CustomLogs",
    "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
}
```

### <a name="get-all-linked-storage-accounts"></a>Tüm bağlı depolama hesaplarını al

#### <a name="api-request"></a>API isteği

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts?api-version=2019-08-01-preview  
```

#### <a name="response"></a>Yanıt

```json
{
    [
        {
            "properties":
            {
                "dataSourceType": "CustomLogs",
                "storageAccountIds  ": 
                [  
                    "<storage_account_resource_id_1>",
                    "<storage_account_resource_id_2>"
                ],
            },
            "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
            "name": "CustomLogs",
            "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
        },
        {
            "properties":
            {
                "dataSourceType": " AzureWatson "
                "storageAccountIds  ": 
                [  
                    "<storage_account_resource_id_3>",
                    "<storage_account_resource_id_4>"
                ],
            },
            "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/AzureWatson",
            "name": "AzureWatson",
            "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
        }
    ]
}
```

## <a name="create-or-modify-a-link"></a>Bağlantı oluşturma veya değiştirme

Bir depolama hesabını bir çalışma alanına bağladığınızda, Log Analytics hizmetin sahip olduğu depolama hesabı yerine onu kullanmaya başlayacaktır. Aynı anda bir depolama hesapları listesi kaydedebilir ve birden çok çalışma alanı için aynı depolama hesabını kullanabilirsiniz.

Çalışma alanınız hem VNet kaynaklarını hem de bir sanal ağın dışındaki kaynakları işlediğinde, internet 'ten gelen trafiği reddetmediğinden emin olmanız gerekir. Depolama alanınız, çalışma alanınız ile aynı ayarlara sahip olmalıdır ve sanal ağınızın dışındaki kaynaklar için kullanılabilir hale getirilir. 

### <a name="api-request"></a>API isteği

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

### <a name="payload"></a>Te

```json
{
    "properties":
    {
        "storageAccountIds  " : 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    }
}
```

### <a name="response"></a>Yanıt

```json
{
    "properties":
    {
        "dataSourceType": "CustomLogs"
        "storageAccountIds  ": 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    },
"id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
"name": "CustomLogs",
"type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
}
```


## <a name="unlink-a-storage-account"></a>Depolama hesabının bağlantısını kaldırma
Alma işlemi için bir depolama hesabı kullanmayı durdurmayı veya kullandığınız çalışma alanını değiştirmeyi seçerseniz, depolama alanının bağlantısını çalışma alanından kaldırmanız gerekir.

Tüm depolama hesaplarının bir çalışma alanından bağlantısı kesmek, alma, hizmet tarafından yönetilen depolama hesaplarına güvenmeye çalışacağı anlamına gelir. Aracılarınız internet 'e sınırlı erişimi olan bir VNet üzerinde çalışıyorsa, alma işlemi başarısız olması beklenir. Çalışma alanının, izlenen kaynaklarınızdan erişilebilen bağlı bir depolama hesabı olması gerekir.

Bir depolama hesabını silmeden önce, içerdiği tüm verilerin çalışma alanına alınmış olduğundan emin olmanız gerekir. Bir önlem olarak, alternatif bir depolama alanını bağlantıladıktan sonra depolama hesabınızı kullanılabilir durumda tutun. Yalnızca tüm içeriği alındıktan sonra silin ve yeni bağlanan depolama hesabına yeni veri yazıldığını görebilirsiniz.


### <a name="api-request"></a>API isteği
```
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

## <a name="replace-a-storage-account"></a>Bir depolama hesabını değiştirme

Alma işlemi için kullanılan bir depolama hesabını değiştirmek için, önce yeni depolama hesabı için bir bağlantı oluşturun. Günlüğe kaydetme aracıları güncelleştirilmiş yapılandırmayı alacak ve yeni depolama alanına veri göndermeye başlayacak.

Daha sonra aracıların kaldırılan hesaba yazmayı durdurması için eski depolama hesabının bağlantısını kaldırın. Alma işlemi, tüm içeri alınana kadar bu hesaptan veri okumayı sürdürür. Tüm Günlükler alınana kadar depolama hesabını silmeyin.

Aracı yapılandırması birkaç dakika sonra yenilenir ve yeni depolama alanına geçiş yapar.

## <a name="manage-storage-account"></a>Depolama hesabını Yönet

### <a name="load"></a>Yükleme

Depolama hesapları, istek azaltma isteklerini başlatmaya başlamadan önce belirli bir okuma ve yazma isteği yükünü işleyebilir. Azaltma, günlükleri almak için gereken süreyi etkiler ve kayıp verilere neden olabilir. Depolama alanınızı aşırı yüklenmişse, ek depolama hesaplarını kaydedin ve yükü aralarında yayın. 

### <a name="related-charges"></a>İlgili ücretler

Depolama hesapları, depolanan verilerin hacmine, depolama türlerine ve artıklık türüne göre ücretlendirilir. Ayrıntılar için bkz. [Blok Blobu fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/blobs/) ve [Tablo Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/tables/).

Çalışma alanınızın kayıtlı depolama hesabı başka bir bölgedeyse, bu [bant genişliği fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/bandwidth/)göre çıkış için ücretlendirilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- Özel bir bağlantı ayarlama hakkında daha fazla bilgi için bkz. [Azure özel bağlantı kullanarak ağları güvenli bir şekilde Azure izleyici 'ye bağlama](private-link-security.md)
