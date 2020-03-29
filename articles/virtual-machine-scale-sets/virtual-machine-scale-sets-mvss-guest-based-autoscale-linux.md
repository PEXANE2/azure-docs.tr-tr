---
title: Linux ölçeği kümesi şablonundaki konuk ölçümleri ile Azure otomatik ölçeklendirmeyi kullanma
description: Linux Sanal Makine Ölçeği Seti şablonundaki konuk metriklerini kullanarak otomatik ölçeklendirme yapmayı öğrenin
author: mayanknayar
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 88f839b281e4d345b012a7e6acff3770dc536045
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76271944"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Linux ölçeği kümesi şablonundaki konuk metriklerini kullanarak otomatik ölçeklendirme

Azure'da VM'lerden ve ölçek kümelerinden toplanan iki geniş ölçüm türü vardır: Ana bilgisayar ölçümleri ve Konuk ölçümleri. Yüksek düzeyde, standart CPU, disk ve ağ ölçümleri kullanmak istiyorsanız, ana bilgisayar ölçümleri iyi bir uyum sağlar. Ancak, daha büyük bir metrik seçimine ihtiyacınız varsa, konuk ölçümleri araştırılmalıdır.

Ana bilgisayar ölçümleri, ana bilgisayar VM tarafından toplandığı için ek kurulum gerektirmez, konuk ölçümleri ise konuk VM'de [Windows Azure Tanılama uzantısını](../virtual-machines/windows/extensions-diagnostics-template.md) veya [Linux Azure Tanılama uzantısını](../virtual-machines/linux/diagnostic-extension.md) yüklemenizi gerektirir. Ana bilgisayar ölçümleri yerine konuk ölçümleri kullanmanın yaygın nedenlerinden biri, konuk ölçümlerin ana bilgisayar ölçümlerinden daha büyük bir ölçüm seçimi sağlamasıdır. Bu tür bir örnek, yalnızca konuk ölçümleri aracılığıyla kullanılabilen bellek tüketimi ölçümleridir. Desteklenen ana bilgisayar ölçümleri [burada](../azure-monitor/platform/metrics-supported.md)listelenir ve yaygın olarak kullanılan konuk ölçümleri [burada](../azure-monitor/platform/autoscale-common-metrics.md)listelenir. Bu makalede, Linux ölçek kümeleri için konuk ölçümleri dayalı otomatik ölçek kurallarını kullanmak için [temel uygulanabilir ölçek kümesi şablonu](virtual-machine-scale-sets-mvss-start.md) nasıl değiştirilen gösterir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme

Önceki [bir makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturmuştuk. Şimdi bu önceki şablonu kullanacağız ve konuk metrik tabanlı otomatik ölçekle bir Linux ölçeği kümesi dağıtan bir şablon oluşturmak için değiştireceğiz.

İlk olarak, `storageAccountName` parametreleri ekleyin ve `storageAccountSasToken`. Tanılama aracısı, metrik verileri bu depolama hesabındaki bir [tabloda](../cosmos-db/table-storage-how-to-use-dotnet.md) depolar. Linux Diagnostics Agent sürüm 3.0 itibariyle, bir depolama erişim anahtarı kullanmak artık desteklenmez. Bunun yerine, bir [SAS Belirteci](../storage/common/storage-dotnet-shared-access-signature-part-1.md)kullanın.

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

Ardından, tanılama `extensionProfile` uzantısını içerecek şekilde ölçek kümesini değiştirin. Bu yapılandırmada, ölçümleri toplamak için ölçek kümesinin kaynak kimliğinin yanı sıra ölçümleri depolamak için kullanılacak depolama hesabı ve SAS belirteci belirtin. Ölçümlerin ne sıklıkta toplandığı (bu durumda, her dakika) ve hangi ölçümlerin izlenir (bu durumda, kullanılan bellek yüzdesi) belirtin. Bu yapılandırma ve kullanılan bellek yüzdesi dışındaki ölçümler hakkında daha ayrıntılı bilgi için [bu belgelere](../virtual-machines/linux/diagnostic-extension.md)bakın.

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

Son olarak, `autoscaleSettings` bu ölçümleri temel alan otomatik ölçeklendirmeyi yapılandırmak için bir kaynak ekleyin. Bu kaynağın, ölçek kümesini otomatik ölçeklendirmeye çalışmadan önce var olduğundan emin olmak için ölçek kümesine başvuran bir `dependsOn` yan tümcesi vardır. Otomatik ölçeklendirme için farklı bir metrik seçerseniz, tanılama uzantısı `counterSpecifier` yapılandırmasından otomatik ölçek `metricName` yapılandırmasında ki gibi kullanırsınız. Otomatik ölçek yapılandırması hakkında daha fazla bilgi için [otomatik ölçeklendirme en iyi uygulamaları](../azure-monitor/platform/autoscale-best-practices.md) ve Azure Monitor REST [API başvuru belgelerine](/rest/api/monitor/autoscalesettings)bakın.

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
