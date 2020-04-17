---
title: Azure Panolarını programlı bir şekilde oluşturun
description: Azure Panoları'nı programlı bir şekilde oluşturmak için Şablon olarak Azure portalındaki bir pano kullanın. JSON başvuru içerir.
services: azure-portal
documentationcenter: ''
author: adamabmsft
manager: mtillman
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/23/2020
ms.author: mblythe
ms.openlocfilehash: 9ec9a4daad139a4930174ba9e3445e1cda1f8c54
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461319"
---
# <a name="programmatically-create-azure-dashboards"></a>Azure Panolarını programlı bir şekilde oluşturun

Bu makale, Azure panolarının programlanabilir şekilde oluşturulması ve yayımlanması sürecinde size yol gösterin. Aşağıda gösterilen pano, belge boyunca başvurulur.

![örnek pano](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Genel Bakış

[Azure portalındaki](https://portal.azure.com) paylaşılan panolar, sanal makineler ve depolama hesapları gibi [kaynaklardır.](../azure-resource-manager/management/overview.md) [Azure Kaynak Yöneticisi REST API'lerini,](/rest/api/)Azure [CLI'yi](/cli/azure)ve [Azure PowerShell komutlarını](/powershell/azure/get-started-azureps)kullanarak kaynakları programlı olarak yönetebilirsiniz.

Kaynak yönetimini kolaylaştırmak için birçok özellik bu API'ler üzerine inşa edilir. Bu API'lerin ve araçların her biri kaynak oluşturma, listele, alma, değiştirme ve silme yolları sunar. Panolar kaynak olduğundan, en sevdiğiniz API'yi veya kullanılacak aracı seçebilirsiniz.

Programlı bir pano oluşturmak için hangi araçları kullanırsanız kullanın, pano nesnenizin JSON gösterimini oluşturursunuz. Bu nesne, panodaki döşemeler hakkında bilgi içerir. Boyutlar, konumlar, bağlı oldukları kaynaklar ve kullanıcı özelleştirmeleri içerir.

Bu JSON belgesini oluşturmanın en pratik yolu Azure portalını kullanmaktır. Kutucuklarınızı etkileşimli olarak ekleyebilir ve konumlandırabilirsiniz. Ardından JSON'u dışa aktarın ve komut dosyaları, programlar ve dağıtım araçlarında daha sonra kullanmak üzere sonuçtan bir şablon oluşturun.

## <a name="create-a-dashboard"></a>Pano oluşturma

Pano oluşturmak için [Azure portalı](https://portal.azure.com) menüsünden **Pano'yu** seçin ve ardından **Yeni pano'yu**seçin.

![yeni pano komutu](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

Kutucukları bulmak ve eklemek için kutucuk galerisini kullanın. Fayans sürükleyerek ve bırakarak eklenir. Bazı döşemeler sürükleme tutamacı kullanarak yeniden boyutlandırmayı destekler.

![boyutu değiştirmek için sürükleme tutamacı](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

Diğerleri kendi bağlam menüsünde seçim için sabit boyutları vardır.

![boyutu değiştirmek için boyutlar bağlam menüsü](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Panoyu paylaşma

Panoyu yapılandırdıktan sonra, bir sonraki **adım, Paylaş** komutunu kullanarak panoyu yayımlamaktır.

![pano paylaşma](./media/azure-portal-dashboards-create-programmatically/share-command.png)

**Paylaş'ı** seçmek, hangi abonelik ve kaynak grubuna yayınlayacağınızı seçmenizi ister. Seçtiğiniz abonelik ve kaynak grubuna yazma erişiminiz olmalıdır. Daha fazla bilgi için Azure [RBAC ve Azure portalını kullanarak rol atamaları ekle veya kaldır'a](../role-based-access-control/role-assignments-portal.md)bakın.

![paylaşım ve erişimde değişiklik yapma](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>Gösterge panelinin JSON temsilini getirin

Yayımlama yalnızca birkaç saniye sürer. Bittiğinde, bir sonraki adım **İndir** komutunu kullanarak JSON'ı almaktır.

![json temsilindir](./media/azure-portal-dashboards-create-programmatically/download-command.png)

## <a name="create-a-template-from-the-json"></a>JSON'dan şablon oluşturma

Bir sonraki adım bu JSON bir şablon oluşturmaktır. Bu şablonu, uygun kaynak yönetimi API'leri, komut satırı araçları veya portal içinde programlı olarak kullanın.

Şablon oluşturmak için pano JSON yapısını tam olarak anlamanız gerekmez. Çoğu durumda, her döşemenin yapısını ve yapılandırmasını korumak istiyorsunuz. Ardından, kutucukların işaret ettiği Azure kaynakları kümesini parametreleştirin. Dışa aktarılan JSON panonuza bakın ve Azure kaynak taliminin tüm oluşumlarını bulun. Örnek panomuzda, tek bir Azure sanal makineye işaret eden birden çok kutucuk bulunur. Çünkü panomuz sadece bu tek kaynağa bakıyor. Belgenin sonunda yer alan örnek JSON'da "/abonelikler" için arama yaparsa, bu kimliğin çeşitli oluşumlarını bulursunuz.

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Gelecekte herhangi bir sanal makine için bu panoyayım, JSON içinde bu dize her oluşumu parametreize.

Azure'da kaynak oluşturan API'ler için iki yaklaşım vardır:

* Zorunlu API'ler aynı anda tek bir kaynak oluşturur. Daha fazla bilgi için [Kaynaklar'a](/rest/api/resources/resources)bakın.
* Tek bir API çağrısıyla birden çok, bağımlı kaynak oluşturan şablon tabanlı dağıtım sistemi. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../azure-resource-manager/resource-group-template-deploy.md)bakın.

Şablon tabanlı dağıtım parametrelendirmeve templating destekler. Bu makalede bu yaklaşımı kullanıyoruz.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Şablon dağıtımını kullanarak şablonunuzdan programlı bir pano oluşturun

Azure, birden çok kaynağın dağıtımını düzenleme olanağı sunar. Dağıtılacak kaynak kümesini ve aralarındaki ilişkileri ifade eden bir dağıtım şablonu oluşturursunuz.  Her kaynağın JSON biçimi, bunları tek tek oluşturuyormuşsunuz gibi aynıdır. Fark, şablon dili değişkenler, parametreler, temel işlevler ve daha fazlası gibi birkaç kavram ekleyip ekler. Bu genişletilmiş sözdizimi yalnızca şablon dağıtımı bağlamında desteklenir. Daha önce tartışılan zorunlu API'lerle kullanıldığında çalışmaz. Daha fazla bilgi için bkz. Azure [Kaynak Yöneticisi şablonlarının yapısını ve sözdizimini anlayın.](../azure-resource-manager/resource-group-authoring-templates.md)

Parametrelendirme şablonun parametre sözdizimi kullanılarak yapılmalıdır.  Burada gösterildiği gibi daha önce bulduğumuz kaynak kimliğinin tüm örneklerini değiştirirsiniz.

Örnek JSON özelliği, sabit kodlu kaynak kimliğine sahip:

```json
id: "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
```

Örnek JSON özelliği şablon parametrelerine göre parametreli bir sürüme dönüştürüldü

```json
id: "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
```

Gerekli şablon meta verilerini ve JSON şablonunun üst kısmındaki parametreleri şu şekilde bildirin:

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
Şablonunuzu yapılandırdıktan sonra, aşağıdaki yöntemlerden birini kullanarak dağıtın:

* [REST API'leri](/rest/api/resources/deployments)
* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](/cli/azure/group/deployment#az-group-deployment-create)
* [Azure portalı şablondağıtım sayfası](https://portal.azure.com/#create/Microsoft.Template)

Daha sonra örnek panomuz JSON'un iki versiyonunu görürsünüz. Bunlardan ilki, zaten bir kaynağa bağlı olan portaldan dışa aktardığımız sürümdür. İkincisi, herhangi bir sanal makineye programlı olarak bağlanabilen ve Azure Kaynak Yöneticisi kullanılarak dağıtılabilen şablon sürümüdür.

### <a name="json-representation-of-our-example-dashboard-before-templating"></a>Cazip önce bizim örnek pano JSON temsili

Bu örnek, bu makaleyle birlikte izleseydiniz görmeyi bekleyebileceğinizleri gösterir. Yönergeler, zaten dağıtılan bir pano JSON gösterimini dışa aktarıyor. Sabit kodlanmış kaynak tanımlayıcıları, bu panobelirli bir Azure sanal makinesini işaret ettiğini gösterir.

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

### <a name="template-representation-of-our-example-dashboard"></a>Örnek panomuzun şablon gösterimi

Panonun şablon sürümü `virtualMachineName`, ve `virtualMachineResourceGroup`. `dashboardName`  Parametreler, her dağıtımıyaptığınızda bu panoyu farklı bir Azure sanal makinesine doğrultmanızı sağlar. Bu pano, herhangi bir Azure sanal makinesini işaret etmek için programlı olarak yapılandırılabilir ve dağıtılabilir. Bu özelliği sınamak için aşağıdaki şablonu kopyalayın ve [Azure portalı şablondağıtım sayfasına](https://portal.azure.com/#create/Microsoft.Template)yapıştırın.

Bu örnek bir panoyu tek başına dağıtır, ancak şablon dili birden çok kaynak dağıtmanızı ve bir veya daha fazla panoyu bunların yanına dağıtmanızı sağlar.

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

Bir pano dağıtmak için parametreli şablon kullanma nın bir örneğini gördüğünüze göre, [Azure Kaynak Yöneticisi REST API'lerini](/rest/api/), Azure [CLI](/cli/azure)veya [Azure PowerShell komutlarını](/powershell/azure/get-started-azureps)kullanarak şablonu dağıtmayı deneyebilirsiniz.
