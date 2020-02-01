---
title: Program aracılığıyla Azure panoları oluşturma
description: Program aracılığıyla Azure panoları oluşturmak için Azure portal bir panoyu şablon olarak kullanın. JSON başvurusunu içerir.
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
ms.date: 01/29/2020
ms.author: mblythe
ms.openlocfilehash: 414427c722b3531c994bb99dbd5d1332c5253dfd
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900989"
---
# <a name="programmatically-create-azure-dashboards"></a>Program aracılığıyla Azure panoları oluşturma

Bu belge, Azure panoları programlı bir şekilde oluşturma ve yayımlama sürecinde yol gösterir. Aşağıda gösterilen panoya belge boyunca başvurulur.

![örnek Pano](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Genel Bakış

[Azure Portal](https://portal.azure.com) paylaşılan panolar, sanal makineler ve depolama hesapları gibi [kaynaklardır](../azure-resource-manager/management/overview.md) . [Azure Resource Manager REST API 'leri](/rest/api/), [Azure CLI](/cli/azure)ve [Azure PowerShell komutlarını](/powershell/azure/get-started-azureps)kullanarak kaynakları programlı bir şekilde yönetebilirsiniz.  

Birçok özellik, kaynak yönetimini kolaylaştırmak için bu API 'lerde derleme yapar. Bu API 'lerin ve araçların her biri, kaynakları oluşturma, listeleme, alma, değiştirme ve silme gibi yollar sunar. Panolar kaynak olduğundan, kullanmak için en sevdiğiniz API 'yi veya aracını seçebilirsiniz.

Hangi araçları kullanırsanız kullanın, pano nesnenizin bir JSON gösterimini oluşturmanız gerekir. Bu nesne, panodaki kutucuklar hakkında bilgiler içerir. Bu, hangi boyutları, konumları, bağlandıkları kaynakları ve Kullanıcı özelleştirmelerini içerir.

Bu JSON belgesini oluşturmanın en pratik yolu Azure portal kullanmaktır. Kutucuklarınızı etkileşimli bir şekilde ekleyebilir ve yerleştirebilirsiniz. Ardından JSON 'u dışarı aktarın ve daha sonra betiklerin, programların ve dağıtım araçlarında kullanmak üzere sonuçtan bir şablon oluşturun.

## <a name="create-a-dashboard"></a>Pano oluşturma

Bir pano oluşturmak için [Azure Portal](https://portal.azure.com) menüsünde **Pano** ' yı seçin ve ardından **Yeni Pano**' yı seçin.

![Yeni Pano komutu](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

Kutucuk bulmak ve eklemek için kutucuk galerisini kullanın. Kutucuklar, sürükleyip bırakarak eklenir. Bazı Kutucuklar, bir sürükleme tutamacı kullanılarak yeniden boyutlandırmayı destekler.

![boyutu değiştirmek için tutamacı sürükleyin](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

Başkalarının kendi bağlam menüsünde arasından seçim yapabileceğiniz sabit boyutları vardır.

![bağlam menüsünü boyut olarak boyutlandır](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Panoyu paylaşma

Panoyu yapılandırdıktan sonra, sonraki adımlar, Panoyu **Share** komutunu kullanarak yayımlamaktır ve sonra JSON 'ı getirmek için kaynak Gezgini kullanır.

![bir panoyu paylaşma](./media/azure-portal-dashboards-create-programmatically/share-command.png)

**Paylaşma** seçeneğinin belirlenmesi, hangi aboneliğin ve kaynak grubunun yayımlanacağını seçmenizi ister. Seçtiğiniz aboneliğe ve kaynak grubuna yazma erişiminizin olması gerekir. Daha fazla bilgi için bkz. [Azure RBAC kullanarak rol atamaları ekleme veya kaldırma ve Azure Portal](../role-based-access-control/role-assignments-portal.md).

![paylaşım ve erişim değişiklikleri yapın](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>Panonun JSON temsilini getir

Yayımlama yalnızca birkaç saniye sürer.  İşlem tamamlandığında, bir sonraki adım JSON 'u getirmek için [Kaynak Gezgini](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) gidemez.

![Kaynak Gezgini tarayın](./media/azure-portal-dashboards-create-programmatically/search-resource-explorer.png)

Kaynak Gezgini, seçtiğiniz aboneliğe ve kaynak grubuna gidin. Sonra, JSON 'u açığa çıkarmak için yeni yayınlanan Pano kaynağını seçin.

![Kaynak Gezgini JSON görüntüleme](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json-detail.png)

## <a name="create-a-template-from-the-json"></a>JSON 'dan şablon oluşturma

Sonraki adım, bu JSON 'dan bir şablon oluşturmaktır. Bu şablonu, uygun kaynak yönetimi API 'Leri, komut satırı araçları veya Portal içinde programlama yoluyla kullanın.

Şablon oluşturmak için Pano JSON yapısını tam olarak anlamanız gerekmez. Çoğu durumda, her kutucuğun yapısını ve yapılandırmasını korumak istersiniz. Ardından, kutucukların işaret eden Azure kaynakları kümesini parametreleştirin. İçe aktarılmış JSON panonuza bakın ve tüm Azure Kaynak kimliklerinin oluşumlarını bulun. Örnek panonuz, hepsi tek bir Azure sanal makinesine işaret eden birden çok kutucuğa sahiptir. Bunun nedeni, panonuzun yalnızca bu tek kaynağa baktığı için. Belgenin sonuna eklenen JSON örnek JSON ' u arıyorsanız, "/abonelikler" için bu KIMLIğIN birkaç tekrarı bulabilirsiniz.

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Bu panoyu gelecekte herhangi bir sanal makine için yayımlamak üzere JSON içinde bu dizenin her geçtiği her zaman parametreleştirin.

Azure 'da kaynak oluşturan API 'Ler için iki yaklaşım vardır:

* Zorunlu API 'Ler tek seferde bir kaynak oluşturur. Daha fazla bilgi için bkz. [kaynaklar](/rest/api/resources/resources).
* Tek bir API çağrısıyla birden çok bağımlı kaynak oluşturan şablon tabanlı bir dağıtım sistemi. Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure PowerShell ile kaynak dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

Şablon tabanlı dağıtım Parametreleştirme ve şablon oluşturmayı destekler. Bu yaklaşımı Bu makalede kullanırız.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Şablon dağıtımını kullanarak şablonınızdan program aracılığıyla pano oluşturma

Azure, birden fazla kaynağın dağıtımını yönetme olanağı sunar. Dağıtılacak kaynak kümesini ve bunlar arasındaki ilişkileri ifade eden bir dağıtım şablonu oluşturursunuz.  Her bir kaynağın JSON biçimi, bunları bir tane oluşturup oluşturuyoruz. Fark, şablon dilinin değişkenler, parametreler, temel işlevler ve daha fazlası gibi birkaç kavram eklemesi gerektiğidir. Bu genişletilmiş söz dizimi yalnızca bir şablon dağıtımı bağlamında desteklenir. Daha önce ele alınan zorunlu API 'lerle birlikte kullanılırsa çalışmaz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](../azure-resource-manager/resource-group-authoring-templates.md).

Parametreleştirme, şablonun parametre sözdizimi kullanılarak yapılmalıdır.  Daha önce bulduğumuz kaynak KIMLIĞININ tüm örneklerini burada gösterildiği gibi değiştirirsiniz.

Sabit kodlanmış kaynak KIMLIĞI ile örnek JSON özelliği:

```json
id: "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
```

Örnek JSON özelliği, şablon parametrelerine göre parametreli bir sürüme dönüştürüldü

```json
id: "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
```

Gerekli şablon meta verilerini ve şu şekilde JSON şablonunun en üstünde bulunan parametreleri bildirin:

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},

    ... rest of template omitted ...
```

Bu belgenin sonunda, çalışma şablonunu tam olarak görebilirsiniz.

Şablonunuzu yapılandırdıktan sonra, aşağıdaki yöntemlerden birini kullanarak dağıtın:

* [REST API'ler](/rest/api/resources/deployments)
* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](/cli/azure/group/deployment#az-group-deployment-create)
* [Azure portal şablonu dağıtım sayfası](https://portal.azure.com/#create/Microsoft.Template)

Örnek panonuzun JSON 'imizin iki sürümü aşağıda verilmiştir. Birincisi, portaldan, bir kaynağa zaten bağlanan sürümdür. İkincisi, bir sanal makineye programlı bir şekilde bağlanabilen ve Azure Resource Manager kullanılarak dağıtılan şablon sürümüdür.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>Şablon oluşturmadan önce örnek panonuzun JSON temsili

Bu örnek, bu makaleyle birlikte takip ettiğiniz ve neleri görmeniz gerektiğini gösterir. Yönergeler, zaten dağıtılmış olan bir panonun JSON temsilini verdi. Bu panonun belirli bir Azure sanal makinesini işaret ettiğinden emin olan sabit kodlanmış kaynak tanımlayıcıları.

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
        "metadata": { }
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

### <a name="template-representation-of-our-example-dashboard"></a>Örnek panonuzun şablon temsili

Panonun şablon sürümü `virtualMachineName`, `virtualMachineResourceGroup`ve `dashboardName`olarak adlandırılan üç parametre tanımladı.  Parametreler, her dağıttığınız zaman bu panoyu farklı bir Azure sanal makinesinde işaret etmenize olanak tanır. Bu pano programlı bir şekilde yapılandırılıp dağıtılabilir ve herhangi bir Azure sanal makinesini işaret edebilir. Bu özelliği test etmek için, aşağıdaki şablonu kopyalayın ve [Azure Portal şablonu dağıtım sayfasına](https://portal.azure.com/#create/Microsoft.Template)yapıştırın.

Bu örnek, bir panoyu kendisine dağıtır, ancak şablon dili birden fazla kaynak dağıtmanıza ve yan yana bir veya daha fazla pano paketlemenize olanak tanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Percentage CPU",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Network In",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Network Out",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                                            "value": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
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
                }
            },
            "metadata": { },
            "apiVersion": "2015-08-01-preview",
            "type": "Microsoft.Portal/dashboards",
            "name": "[parameters('dashboardName')]",
            "location": "westus",
            "tags": {
                "hidden-title": "[parameters('dashboardName')]"
            }
        }
    ]
}


```
