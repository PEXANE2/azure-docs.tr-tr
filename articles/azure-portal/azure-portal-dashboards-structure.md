---
title: Azure Panolarının yapısı | Microsoft Dokümanlar
description: Örnek bir pano kullanarak Azure Panosu'nun JSON yapısında yürüyün. Kaynak özelliklerine başvuru içerir.
services: azure-portal
documentationcenter: ''
author: adamabmsft
manager: mtillman
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/20/2019
ms.author: mblythe
ms.openlocfilehash: 18125e119e7ffdd2f8fa8ca3c5c1b12c8c9a94e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75640372"
---
# <a name="the-structure-of-azure-dashboards"></a>Azure Panolarının yapısı
Bu belge, örnek olarak aşağıdaki panoyu kullanarak Azure panosunun yapısında yürür:

![örnek pano](./media/azure-portal-dashboards-structure/sample-dashboard.png)

Paylaşılan [Azure panoları kaynak olduğundan,](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bu pano JSON olarak temsil edilebilir.  Aşağıdaki JSON, yukarıda görselleştirilmiş panoyu temsil eder.

```json

{
    "properties": {
        "lenses": {
            "0": {
                "order": 0,
                "parts": {
                    "0": {
                        "position": {
                            "x": 0,
                            "y": 0,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "## Azure Virtual Machines Overview\r\nNew team members should watch this video to get familiar with Azure Virtual Machines.",
                                        "title": "",
                                        "subtitle": ""
                                    }
                                }
                            }
                        }
                    },
                    "1": {
                        "position": {
                            "x": 3,
                            "y": 0,
                            "rowSpan": 4,
                            "colSpan": 8
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
                                        "title": "Test VM Dashboard",
                                        "subtitle": "Contoso"
                                    }
                                }
                            }
                        }
                    },
                    "2": {
                        "position": {
                            "x": 0,
                            "y": 2,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/VideoPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "title": "",
                                        "subtitle": "",
                                        "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
                                        "autoplay": false
                                    }
                                }
                            }
                        }
                    },
                    "3": {
                        "position": {
                            "x": 0,
                            "y": 4,
                            "rowSpan": 3,
                            "colSpan": 11
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Percentage CPU",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "4": {
                        "position": {
                            "x": 0,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "5": {
                        "position": {
                            "x": 3,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "6": {
                        "position": {
                            "x": 6,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Network In",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Network Out",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "7": {
                        "position": {
                            "x": 9,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 2
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "id",
                                    "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart",
                            "asset": {
                                "idInputName": "id",
                                "type": "VirtualMachine"
                            },
                            "defaultMenuItemId": "overview"
                        }
                    }
                }
            }
        },
        "metadata": {
            "model": {
                "timeRange": {
                    "value": {
                        "relative": {
                            "duration": 24,
                            "timeUnit": 1
                        }
                    },
                    "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
                }
            }
        }
    },
    "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/dashboards/providers/Microsoft.Portal/dashboards/aa9786ae-e159-483f-b05f-1f7f767741a9",
    "name": "aa9786ae-e159-483f-b05f-1f7f767741a9",
    "type": "Microsoft.Portal/dashboards",
    "location": "eastasia",
    "tags": {
        "hidden-title": "Created via API"
    }
}

```

## <a name="common-resource-properties"></a>Ortak kaynak özellikleri

JSON'ın ilgili bölümlerini parçalayalım.  Üst düzey özellikler, __id,__ __ad,__ __tür,__ __konum__ve __etiket__ özellikleri tüm Azure kaynak türlerinde paylaşılır. Diğer bir diğer nokta, pano içeriğiyle pek bir ilgisi yoktur.

### <a name="the-id-property"></a>Kimlik özelliği

Azure kaynak kimliği, [Azure kaynaklarının adlandırma sözleşmelerine](/azure/architecture/best-practices/resource-naming)tabidir. Portal bir pano oluşturduğunda genellikle kılavuz şeklinde bir kimlik seçer, ancak bunları programlı bir şekilde oluşturduğunuzda geçerli bir ad kullanmakta özgürüz. 

### <a name="the-name-property"></a>Ad özelliği
Ad, kaynak kimliğinin abonelik, kaynak türü veya kaynak grubu bilgilerini içermeyen kesimidir. Esasen, kaynak kimliğinin son bölümüdür.

### <a name="the-type-property"></a>Tür özelliği
Tüm panolar __Microsoft.Portal/pano__türündedir.

### <a name="the-location-property"></a>Konum özelliği
Diğer kaynakların aksine, panolarda çalışma zamanı bileşeni yoktur.  Panolar için konum, panonun JSON temsilini depolayan birincil coğrafi konumu gösterir. Değer, [abonelikkaynağındaki konumlar API'si](https://docs.microsoft.com/rest/api/resources/subscriptions)kullanılarak getirilebilecek konum kodlarından biri olmalıdır.

### <a name="the-tags-property"></a>Etiketler özelliği
Etiketler, Azure kaynaklarının, kaynağınızı rasgele ad değeri çiftleri tarafından düzenlemenize izin veren ortak bir özelliğidir. Panolar için __gizli başlık__adı verilen özel bir etiket vardır. Panonuzda bu özellik doldurulmuşsa, portaldaki panonuzun görüntü adı olarak kullanılır. Azure kaynak Kimlikleri yeniden adlandırılamaz, ancak etiketler ilerleyebilir. Bu etiket, panonuz için yeniden adlanabilir bir görüntü adı olması için bir yol sağlar.

`"tags": { "hidden-title": "Created via API" }`

### <a name="the-properties-object"></a>Özellikler nesnesi
Özellikleri nesne iki özellik, __lensler__ ve __meta veri__içerir. __Lensler__ özelliği pano üzerinde fayans hakkında bilgi içerir.  __Meta veri__ özelliği gelecekteki olası özellikler için vardır.

### <a name="the-lenses-property"></a>Lensler özelliği
__Lensler__ özelliği pano içerir. Bu örnekteki lensler nesnesinin "0" adlı tek bir özellik içerdiğini unutmayın. Lensler şu anda panolarda uygulanmayan bir gruplama konseptidir. Şimdilik, tüm panoları lens nesnesi üzerinde bu tek özelliği var, yine, "0" olarak adlandırılan.

### <a name="the-lens-object"></a>Lens nesnesi
"0"ın altındaki nesne iki özellik, __sıra__ ve __parça__içerir.  Panoların geçerli __sürümünde, sıra__ her zaman 0'dır. __Parça__ özelliği, panodaki tek tek parçaları (kutucuk olarak da adlandırılır) tanımlayan bir nesne içerir.

__Parçalar__ nesnesi, özelliğin adının bir sayı olduğu her parça için bir özellik içerir. Bu sayı önemli değildir. 

### <a name="the-part-object"></a>Parça nesnesi
Her bir parça nesnesi bir __konuma__ve __meta veriye__sahiptir.

### <a name="the-position-object"></a>Konum nesnesi
__Konum__ özelliği __x,__ __y__, __rowSpan__ve __colSpan__olarak ifade edilen parçanın boyut ve konum bilgilerini içerir. Değerler ızgara birimleri açısından dır. Bu ızgara birimleri, pano burada gösterildiği gibi özelleştirme modundayken görünür. Bir döşemenin iki ızgara birimi genişliğine, bir ızgara biriminin yüksekliğe ve panonun sol üst köşesindeki bir konuma sahip olmasını istiyorsanız, konum nesnesi aşağıdaki gibi görünür:

`location: { x: 0, y: 0, rowSpan: 2, colSpan: 1 }`

![ızgara birimleri](./media/azure-portal-dashboards-structure/grid-units.png)

### <a name="the-metadata-object"></a>Meta veri nesnesi
Her parçanın bir meta veri özelliği vardır, bir nesnenin __türü__olarak adlandırılan yalnızca bir gerekli özelliği vardır. Bu dize portala hangi döşemenin gösterilen olduğunu söyler. Örnek panomuz bu tür döşemeleri kullanır:


1. `Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart`– İzleme ölçümlerini göstermek için kullanılır
1. `Extension[azure]/HubsExtension/PartType/MarkdownPart`– Listeler, bağlantılar vb. için temel biçimlendirmeiçeren metin veya resimlerle birlikte göstermek için kullanılır.
1. `Extension[azure]/HubsExtension/PartType/VideoPart`– YouTube, Channel9 ve HTML video etiketinde çalışan diğer video türlerinden videolar göstermek için kullanılır.
1. `Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart`– Azure sanal makinesinin adını ve durumunu göstermek için kullanılır.

Her parça türünün kendi yapılandırması vardır. Olası yapılandırma özellikleri __girişler,__ __ayarlar__ve __varlık__olarak adlandırılır. 

### <a name="the-inputs-object"></a>Giriş nesnesi
Girişnesnesi genellikle bir kutucuğu kaynak örneğine bağlayan bilgiler içerir.  Örnek panomuzdaki sanal makine bölümü, bağlamayı ifade etmek için Azure kaynak kimliğini kullanan tek bir giriş içerir.  Bu kaynak kimliği biçimi tüm Azure kaynaklarında tutarlıdır.

```json
"inputs":
[
    {
        "name": "id",
        "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
    }
]

```
Ölçüm grafiği bölümü, bağlaştırılaca kaynağı ifade eden tek bir girdinin yanı sıra görüntülenen metrik(ler) hakkındaki bilgilere sahiptir. Ağ Giriş ve Ağ Çıkış ölçümlerini gösteren döşemenin girişi aşağıda veda eder.

```json
“inputs”:
[
    {
        "name": "queryInputs",
        "value": 
        {
            "timespan": 
            {
                "duration": "PT1H",
                "start": null,
                "end": null
           },
            "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
            "chartType": 0,
            "metrics": 
            [
                {
                    "name": "Network In",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                },
                {
                    "name": "Network Out",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                }
            ]
        }
    }
]

```

### <a name="the-settings-object"></a>Ayarlar nesnesi
Ayarlar nesnesi bir parçanın yapılandırılabilir öğelerini içerir.  Örnek panomuzda, Markdown bölümü özel işaretleme içeriğinin yanı sıra yapılandırılabilir bir başlık ve altyazıyı depolamak için ayarları kullanır.

```json
"settings": 
{
    "content": 
    {
        "settings": 
        {
            "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
            "title": "Test VM Dashboard",
            "subtitle": "Contoso"
        }
    }
}

```

Benzer şekilde, video döşemesinin oynatmak için video işaretçisi, otomatik oynatma ayarı ve isteğe bağlı başlık bilgileri içeren kendi ayarları vardır.

```json
"settings": 
{
   "content": 
    {
        "settings": 
        {
            "title": "",
            "subtitle": "",
            "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
            "autoplay": false
        }
    }
}

```

### <a name="the-asset-object"></a>Varlık nesnesi
Birinci sınıf yönetilebilir portal nesnelerine (varlıklar olarak adlandırılır) bağlanan kutucuklar, bu ilişkiyi varlık nesnesi aracılığıyla ifade eder.  Örnek panomuzda, sanal makine döşemesi bu varlık açıklamasını içerir.  __idInputName__ özelliği portala, kimlik girişinin varlık için benzersiz tanımlayıcıyı, bu durumda kaynak kimliğini içerdiğini söyler. Azure kaynak türlerinin çoğunda portalda tanımlanmış varlıklar vardır.

`"asset": {    "idInputName": "id",    "type": "VirtualMachine"    }`
