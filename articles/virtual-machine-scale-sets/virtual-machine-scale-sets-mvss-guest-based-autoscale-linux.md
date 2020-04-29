---
title: Linux ölçek kümesi şablonunda Konuk ölçümleriyle Azure otomatik ölçeklendirmeyi kullanma
description: Linux sanal makine ölçek kümesi şablonunda Konuk ölçümleri kullanarak otomatik ölçeklendirmeyi nasıl kullanacağınızı öğrenin
author: mimckitt
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: mimckitt
ms.openlocfilehash: 8021b7b8feb6dc06fb2e48bc4e825200a1baad33
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81273656"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Linux ölçek kümesi şablonunda Konuk ölçümleri kullanarak otomatik ölçeklendirme

Azure 'da VM 'Ler ve ölçek kümelerinden toplanan iki kapsamlı ölçüm türü vardır: konak ölçümleri ve konuk ölçümleri. Yüksek düzeyde, Standart CPU, disk ve ağ ölçümleri kullanmak isterseniz, ana bilgisayar ölçümleri iyi bir uyum sağlar. Ancak, daha büyük bir ölçüm seçimine ihtiyaç duyuyorsanız, Konuk ölçümlerinin bakımış olması gerekir.

Konak ölçümleri konak VM tarafından toplandığı için ek kurulum gerektirmez, ancak Konuk ölçümler [Windows Azure tanılama uzantısını](../virtual-machines/windows/extensions-diagnostics-template.md) veya [Linux Azure tanılama uzantısını](../virtual-machines/linux/diagnostic-extension.md) Konuk VM 'ye yüklemenizi gerektirir. Konuk ölçümlerinin tek bir yaygın nedeni, ana bilgisayar ölçümlerinin yerine Konuk ölçümlerinin daha büyük bir kullanımını konak ölçümlerine göre sağlar. Bu tür bir örnek, yalnızca Konuk ölçümler aracılığıyla kullanılabilen bellek tüketim ölçümleridir. Desteklenen konak ölçümleri [burada](../azure-monitor/platform/metrics-supported.md)listelenir ve yaygın olarak kullanılan Konuk ölçümler [burada](../azure-monitor/platform/autoscale-common-metrics.md)listelenir. Bu makalede, Linux ölçek kümelerinin Konuk ölçümlerine göre otomatik ölçeklendirme kurallarını kullanmak için [temel uygun ölçek kümesi şablonunun](virtual-machine-scale-sets-mvss-start.md) nasıl değiştirileceği gösterilmektedir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme

Önceki bir [makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturduk. Artık bu önceki şablonu kullanacağız ve konuk ölçümü tabanlı otomatik ölçeklendirme ile Linux ölçek kümesi dağıtan bir şablon oluşturacak şekilde değiştirirsiniz.

İlk olarak, ve `storageAccountName` `storageAccountSasToken`için parametreleri ekleyin. Tanılama Aracısı, ölçüm verilerini bu depolama hesabındaki bir [tabloda](../cosmos-db/table-storage-how-to-use-dotnet.md) depolar. Linux Tanılama Aracısı sürüm 3,0 itibariyle, depolama erişim anahtarı kullanmak artık desteklenmemektedir. Bunun yerine bir [SAS belirteci](../storage/common/storage-dotnet-shared-access-signature-part-1.md)kullanın.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

Sonra, ölçek kümesini `extensionProfile` tanılama uzantısını içerecek şekilde değiştirin. Bu yapılandırmada, ölçümlerin toplanacağı ölçek kümesinin kaynak KIMLIĞINI, Ayrıca ölçümleri depolamak için kullanılacak depolama hesabını ve SAS belirtecini belirtin. Ölçümlerin ne sıklıkta toplanduğunu (Bu durumda, her dakika) ve hangi ölçümlerin izleneceğini (Bu örnekte, kullanılan bellek yüzdesini) belirtin. Kullanılan bu yapılandırma ve ölçümler hakkında daha ayrıntılı bilgi için [Bu belgelere](../virtual-machines/linux/diagnostic-extension.md)bakın.

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Son olarak, bu `autoscaleSettings` ölçümlere göre otomatik ölçeklendirmeyi yapılandırmak için bir kaynak ekleyin. Bu kaynak, ölçek `dependsOn` kümesinin otomatik ölçeklendirmeyi denemeden önce mevcut olduğundan emin olmak için ölçek kümesine başvuran bir yan tümcesine sahiptir. Otomatik ölçeklendirme için farklı bir ölçüm seçerseniz, ' yi otomatik ölçeklendirme yapılandırmasındaki olarak `counterSpecifier` `metricName` tanılama uzantısı yapılandırmasından kullanırsınız. Otomatik ölçeklendirme yapılandırması hakkında daha fazla bilgi için bkz. [Otomatik ölçeklendirme en iyi uygulamaları](../azure-monitor/platform/autoscale-best-practices.md) ve [Azure izleyici REST API başvuru belgeleri](/rest/api/monitor/autoscalesettings).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
