---
title: Windows Azure Tanılama ile performans izleme
description: Azure Hizmet Kumaş ı kümeleriniz için performans sayaçları toplamak için Windows Azure Diagnostics'i kullanın.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 0819ca02d088aeb9ada5de1269467f70242bbcca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609919"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Windows Azure Tanılama uzantısı ile performans izleme

Bu belge, Windows kümeleri için Windows Azure Tanılama (WAD) uzantısı aracılığıyla performans sayaçları koleksiyonu ayarlamak için gereken adımları kapsar. Linux kümeleri için, düğümleriniz için performans sayaçları toplamak için [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) ayarlayın. 

 > [!NOTE]
> Bu adımların sizin için çalışması için WAD uzantısı kümenizde dağıtılmalıdır. Ayarlı değilse, Windows Azure [Tanılama'yı kullanarak Olay toplama ve toplama'ya](service-fabric-diagnostics-event-aggregation-wad.md)gidin.  


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="collect-performance-counters-via-the-wadcfg"></a>WadCfg ile performans sayaçlarını topla

WAD aracılığıyla performans sayaçları toplamak için, kümenizin Kaynak Yöneticisi şablonundaki yapılandırmayı uygun şekilde değiştirmeniz gerekir. Şablonunuza toplamak istediğiniz bir performans sayacı eklemek ve Kaynak Yöneticisi kaynak yükseltmesi çalıştırmak için aşağıdaki adımları izleyin.

1. Kümenizin şablonundaki WAD yapılandırmasını `WadCfg`bulun - bulun. Altında toplamak için performans sayaçları `DiagnosticMonitorConfiguration`ekleyerek olacak.

2. Aşağıdaki bölümü ekleyerek performans sayaçlarını toplamak için yapılandırmanızı `DiagnosticMonitorConfiguration`ayarlayın. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    Toplanan `scheduledTransferPeriod` sayaçların değerlerinin Azure depolama tablonuza ve yapılandırılan herhangi bir lavaboya ne sıklıkta aktarıldığını tanımlar. 

3. Toplamak istediğiniz performans sayaçlarını önceki adımda bildirilenperformans `PerformanceCounterConfiguration` sayaçlarını ekleyin. Toplamak istediğiniz her sayaç , , `counterSpecifier` `sampleRate` `unit` `annotation`, , ve `sinks`ilgili herhangi bir .

Burada *Toplam İşlemci Süresi* (CPU işleme işlemleri için kullanılan süre miktarı) ve *İkinci başına Hizmet Kumaş Aktör Yöntemi Çağrıları*için sayaç ile bir yapılandırma örneği , Hizmet Kumaş özel performans sayaçlarından biri. Service Fabric özel perf sayaçlarının tam listesi için [Güvenilir Aktör Performans Sayaçları](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) ve [Güvenilir Servis Performans Sayaçları'na](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) bakın.

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 Sayaç için örnek oran ihtiyaçlarınıza göre değiştirilebilir. Bunun için biçimi `PT<time><unit>`, bu yüzden sayaç her saniye toplanan istiyorsanız, `"sampleRate": "PT15S"`o zaman ayarlamanız gerekir .

 Arm şablonunuzda, işlem başına performans sayaçları topladığınızda kullanışlı olabilecek bir dizi performans sayacı toplamak için de değişkenler kullanabilirsiniz. Aşağıdaki örnekte, işlem başına işlemci zamanı ve çöp toplayıcı zamanı ve daha sonra tüm değişkenleri kullanarak düğümler üzerinde 2 performans sayaçları topluyoruz. 

 ```json
"variables": {
  "copy": [
      {
        "name": "processorTimeCounters",
        "count": "[length(parameters('monitoredProcesses'))]",
        "input": {
          "counterSpecifier": "\\Process([parameters('monitoredProcesses')[copyIndex('processorTimeCounters')]])\\% Processor Time",
          "sampleRate": "PT1M",
          "unit": "Percent",
          "sinks": "applicationInsights",
          "annotation": [
            {
              "displayName": "[concat(parameters('monitoredProcesses')[copyIndex('processorTimeCounters')],' Processor Time')]",
              "locale": "en-us"
            }
          ]
        }
      },
      {
        "name": "gcTimeCounters",
        "count": "[length(parameters('monitoredProcesses'))]",
        "input": {
          "counterSpecifier": "\\.NET CLR Memory([parameters('monitoredProcesses')[copyIndex('gcTimeCounters')]])\\% Time in GC",
          "sampleRate": "PT1M",
          "unit": "Percent",
          "sinks": "applicationInsights",
          "annotation": [
            {
              "displayName": "[concat(parameters('monitoredProcesses')[copyIndex('gcTimeCounters')],' Time in GC')]",
              "locale": "en-us"
            }
          ]
        }
      }
    ],
    "machineCounters": [
      {
        "counterSpecifier": "\\Memory\\Available Bytes",
        "sampleRate": "PT1M",
        "unit": "KB",
        "sinks": "applicationInsights",
        "annotation": [
          {
            "displayName": "Memory Available Kb",
            "locale": "en-us"
          }
        ]
      },
      {
        "counterSpecifier": "\\Memory\\% Committed Bytes In Use",
        "sampleRate": "PT15S",
        "unit": "percent",
        "annotation": [
          {
            "displayName": "Memory usage",
            "locale": "en-us"
          }
        ]
      }
    ]
  }
....
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
      "overallQuotaInMB": "50000",
      "Metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', variables('vmNodeTypeApp2Name'))]"
      },
      "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": "[concat(variables ('processorTimeCounters'), variables('gcTimeCounters'),  variables('machineCounters'))]"
      },
....
```

1. Toplanması gereken uygun performans sayaçlarını ekledikten sonra, bu değişikliklerin çalışan kümenize yansıtılması için küme kaynağınızı yükseltmeniz gerekir. Modifiye `template.json` edilmiş telefonunuzu kaydedin ve PowerShell'i açın. Kümenizi kullanarak `New-AzResourceGroupDeployment`yükseltebilirsiniz. Arama, kaynak grubunun adını, güncelleştirilmiş şablon dosyasını ve parametreler dosyasını gerektirir ve Kaynak Yöneticisi'nden güncellediğiniz kaynaklarda uygun değişiklikleri yapmasıistenir. Hesabınızda oturum açıp doğru abonelikte olduktan sonra yükseltmeyi çalıştırmak için aşağıdaki komutu kullanın:

    ```sh
    New-AzResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

1. Yükseltme işlemi tamamlandığında (ilk dağıtım ve kaynak grubunuzun boyutuna bağlı olarak 15-45 dakika sürer), WAD performans sayaçlarını toplamalı ve bunları adlı tabloya göndermelidir WADPerformanceCountersTable küme ile ilişkili depolama hesabında. [Kaynak Yöneticisi şablonuna AI Lavabosu ekleyerek](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template)Uygulama Öngörüleri'ndeki performans sayaçlarınızı görün.

## <a name="next-steps"></a>Sonraki adımlar
* Kümeniz için daha fazla performans sayacı toplayın. Toplamanız gereken sayaçların listesi için [Performans ölçümlerine](service-fabric-diagnostics-event-generation-perf.md) bakın.
* [Windows VM ve Azure Kaynak Yöneticisi şablonlarıyla izleme ve tanılama](../virtual-machines/windows/extensions-diagnostics-template.md) yı `WadCfg`kullanarak tanılama verilerini göndermek için ek depolama hesaplarını yapılandırmak da dahil olmak üzere daha fazla değişiklik yapın.
* Sıfırdan bir şablon oluşturmak ve sözdiziminizin doğru olduğundan emin olmak için [WadCfg oluşturucuyu](https://azure.github.io/azure-diagnostics-tools/config-builder/) ziyaret edin. (https://azure.github.io/azure-diagnostics-tools/config-builder/) sıfırdan bir şablon oluşturmak ve sözdiziminizin doğru olduğundan emin olmak için.
