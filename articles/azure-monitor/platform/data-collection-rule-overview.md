---
title: Azure Izleyici 'de veri toplama kuralları (Önizleme)
description: Azure Izleyici 'de içerik ve yapısını ve bunlarla nasıl oluşturabileceğiniz ve bunlarla nasıl çalışacağınıza ilişkin veri toplama kurallarına (CCR 'ler) genel bakış.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/10/2020
ms.openlocfilehash: af5324373359cea643a3e31b6bb94e614ddb7e36
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88083180"
---
# <a name="data-collection-rules-in-azure-monitor-preview"></a>Azure Izleyici 'de veri toplama kuralları (Önizleme)
Veri toplama kuralları (DCR), Azure Izleyici 'ye gelen verileri tanımlar ve verilerin nereye gönderileceğini veya depolanacağını belirtir. Bu makalede, içerik ve yapısı ve bunlarla nasıl oluşturabileceğiniz ve bunlarla çalışma gibi veri toplama kurallarına genel bir bakış sunulmaktadır.

## <a name="input-sources"></a>Giriş kaynakları
Veri toplama kuralları şu anda aşağıdaki giriş kaynaklarını desteklemektedir:

- Azure Izleyici aracısına sahip Azure sanal makinesi. Bkz. [Azure izleyici Aracısı (Önizleme) için veri toplamayı yapılandırma](data-collection-rule-azure-monitor-agent.md).



## <a name="components-of-a-data-collection-rule"></a>Bir veri toplama kuralının bileşenleri
Bir veri toplama kuralı aşağıdaki bileşenleri içerir.

| Bileşen | Açıklama |
|:---|:---|
| Veri kaynakları | Kendi biçimi ve kendi verilerini açığa çıkaran yöntemi olan, izleme verilerinin benzersiz kaynağı. Bir veri kaynağına örnek olarak Windows olay günlüğü, performans sayaçları ve Syslog dahildir. Her veri kaynağı, aşağıda açıklandığı gibi belirli bir veri kaynağı türüyle eşleşir. |
| Akışlar | Tek bir tür olarak dönüştürülecek ve şemaya ayarlanacak bir veri kaynakları kümesini açıklayan benzersiz tanıtıcı. Her veri kaynağı bir veya daha fazla akış gerektirir ve birden çok veri kaynağı tarafından bir akış kullanılabilir. Akıştaki tüm veri kaynakları ortak bir şemayı paylaşır. Aynı Log Analytics çalışma alanında birden çok tabloya belirli bir veri kaynağı göndermek istediğinizde, örneğin birden çok akışı kullanın. |
| Hedefler | Verilerin gönderilmesi gereken hedef kümesi. Log Analytics çalışma alanı, Azure Izleyici ölçümleri ve Azure Event Hubs sayılabilir. | 
| Veri akışları | Hangi akışların hangi hedeflere gönderilmesi gerektiğini gösteren tanım. | 

Aşağıdaki diyagramda bir veri toplama kuralının ve bunların ilişkilerinin bileşenleri gösterilmektedir

[![DCR diyagramı](media/data-collection-rule/data-collection-rule-components.png)](media/data-collection-rule/data-collection-rule-components.png#lightbox)

### <a name="data-source-types"></a>Veri kaynağı türleri
Her veri kaynağının bir veri kaynağı türü vardır. Her tür, her veri kaynağı için belirtilmesi gereken benzersiz bir özellikler kümesi tanımlar. Şu anda kullanılabilir veri kaynağı türleri aşağıdaki tabloda gösterilmiştir.

| Veri kaynağı türü | Açıklama | 
|:---|:---|
| uzantı | VM Uzantısı tabanlı veri kaynağı |
| performanceCounters | Hem Windows hem de Linux için performans sayaçları |
| syslog | Linux sanal makinesinde Syslog olayları |
| windowsEventLogs | Windows olay günlüğü |


## <a name="limits"></a>Sınırlar
Aşağıdaki tabloda, her bir veri toplama kuralı için geçerli olan sınırlar listelenmektedir.

| Sınır | Değer |
|:---|:---|
| Maksimum veri kaynağı sayısı | 10 |
| Performans için en fazla sayaç belirticileri | 100 |
| SysLog 'da maksimum tesis adları | 20 |
| Olay günlüğüne en fazla XPath sorgusu | 100 |
| En fazla veri akışı | 10 |
| En fazla veri akışı | 10 |
| En fazla uzantı sayısı | 10 |
| Uzantı ayarlarının en büyük boyutu | 32 KB |
| Maksimum Log Analytics çalışma alanı | 10 |


## <a name="create-a-dcr"></a>DCR oluşturma
Şu anda bir DCR oluşturmak için kullanılabilecek iki yöntem vardır:

- Bir veri toplama kuralı oluşturmak ve bir veya daha fazla sanal makineyle ilişkilendirilmesini sağlamak için [Azure Portal kullanın](data-collection-rule-azure-monitor-agent.md) .
- JSON 'daki veri toplama kuralını doğrudan düzenleyin ve REST API kullanarak gönderebilirsiniz.

## <a name="sample-data-collection-rule"></a>Örnek veri toplama kuralı
Aşağıdaki örnek veri toplama kuralı, Azure Yönetim aracısına sahip sanal makineler içindir ve aşağıdaki ayrıntılara sahiptir:

- Performans verileri
  - 15 saniyede bir belirli Işlemci, bellek, mantıksal disk ve fiziksel disk sayaçlarını toplar ve dakikada bir karşıya yükler.
  - Her 30 saniyede bir belirli Işlem sayaçlarını toplar ve 5 dakikada bir karşıya yükler.
- Windows olayları
  - Windows Güvenlik olaylarını toplar ve dakikada bir karşıya yükler.
  - Windows uygulaması ve sistem olaylarını toplar ve 5 dakikada bir karşıya yükler.
- Syslog
  - Cron tesisinden hata ayıklama, kritik ve acil durum olaylarını toplar.
  - Syslog tesisinden uyarı, kritik ve acil durum olaylarını toplar.
- Hedefler
  - Tüm verileri, merkezileştirme Teamworkspace adlı bir Log Analytics çalışma alanına gönderir.
  - Geçerli abonelikteki Azure Izleyici ölçümlerine performans verileri gönderir.

```json
{
    "location": "eastus",
    "properties": {
      "dataSources": {
        "performanceCounters": [
          {
            "name": "cloudTeamCoreCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT1M",
            "samplingFrequencyInSeconds": 15,
            "counterSpecifiers": [
              "\\Processor(_Total)\\% Processor Time",
              "\\Memory\\Committed Bytes",
              "\\LogicalDisk(_Total)\\Free Megabytes",
              "\\PhysicalDisk(_Total)\\Avg. Disk Queue Length"
            ]
          },
          {
            "name": "appTeamExtraCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT5M",
            "samplingFrequencyInSeconds": 30,
            "counterSpecifiers": [
              "\\Process(_Total)\\Thread Count"
            ]
          }
        ],
        "windowsEventLogs": [
          {
            "name": "cloudSecurityTeamEvents",
            "streams": [
              "Microsoft-WindowsEvent"
            ],
            "scheduledTransferPeriod": "PT1M",
            "xPathQueries": [
              "Security!*"
            ]
          },
          {
            "name": "appTeam1AppEvents",
            "streams": [
              "Microsoft-WindowsEvent"
            ],
            "scheduledTransferPeriod": "PT5M",
            "xPathQueries": [
              "System!*[System[(Level = 1 or Level = 2 or Level = 3)]]",
              "Application!*[System[(Level = 1 or Level = 2 or Level = 3)]]"
            ]
          }
        ],
        "syslog": [
          {
            "name": "cronSyslog",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "cron"
            ],
            "logLevels": [
              "Debug",
              "Critical",
              "Emergency"
            ]
          },
          {
            "name": "sylogBase",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "syslog"
            ],
            "logLevels": [
              "Alert",
              "Critical",
              "Emergency"
            ]
          }
        ]
      },
      "destinations": {
        "logAnalytics": [
          {
            "workspaceResourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.OperationalInsights/workspaces/my-workspace",
            "name": "centralWorkspace"
          }
        ]
      },
      "dataFlows": [
        {
          "streams": [
            "Microsoft-Perf",
            "Microsoft-Syslog",
            "Microsoft-WindowsEvent"
          ],
          "destinations": [
            "centralWorkspace"
          ]
        }
      ]
    }
  }
```


## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici aracısını kullanarak bir sanal makineden bir [veri toplama kuralı](data-collection-rule-azure-monitor-agent.md) ve buna bir ilişki oluşturun.