---
title: Program aracılığıyla Azure panoları oluşturma | Microsoft Docs
description: Program aracılığıyla Azure panoları oluşturmak için Azure portal bir panoyu şablon olarak kullanabilirsiniz. JSON başvurusunu içerir.
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
ms.date: 09/01/2017
ms.author: mblythe
ms.openlocfilehash: 498e0255cfa289f7d8ccb93040980c362cf510a0
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640355"
---
# <a name="programmatically-create-azure-dashboards"></a>Program aracılığıyla Azure panoları oluşturma

Bu belge, Azure panoları programlı bir şekilde oluşturma ve yayımlama sürecinde yol gösterir. Aşağıda gösterilen panoya belge boyunca başvurulur.

![örnek pano](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Genel Bakış

Azure 'daki paylaşılan panolar, sanal makineler ve depolama hesapları gibi [kaynaklardır](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) .  Bu nedenle, Kaynak yönetiminin daha kolay olması için [Azure Resource Manager REST API 'leri](/rest/api/), [Azure CLI](https://docs.microsoft.com/cli/azure), [Azure PowerShell komutları](https://docs.microsoft.com/powershell/azure/get-started-azureps)ve birçok [Azure Portal](https://portal.azure.com) özelliği, bu API 'lerin üzerine inşa ederek programlı bir şekilde yönetilebilir.  

Bu API 'lerin ve araçların her biri, kaynakları oluşturma, listeleme, alma, değiştirme ve silme gibi yollar sunar.  Panolar kaynak olduğundan, kullanmak için en sevdiğiniz API/aracı seçebilirsiniz.

Kullandığınız araçlardan bağımsız olarak, herhangi bir kaynak oluşturma API 'sini çağırabilmeniz için Pano nesnenizin bir JSON temsilini oluşturmanız gerekir. Bu nesne parçalar hakkında bilgiler içerir (deyişle Kutucuklar) Pano üzerinde. Bu, ilişkili oldukları boyutları, konumları, kaynakları ve Kullanıcı özelleştirmelerini içerir.

Bu JSON belgesini oluşturmanın en pratik yolu, portalınızı etkileşimli olarak eklemek ve konumlandırmak için [portalı](https://portal.azure.com/) kullanmaktır. Ardından JSON 'ı dışarı aktarabilirsiniz. Son olarak, komut dosyalarında, programlarda ve dağıtım araçlarında daha sonra kullanmak üzere sonuçtan bir şablon oluşturursunuz.

## <a name="create-a-dashboard"></a>Pano oluşturma

Yeni bir pano oluşturmak için portalın ana ekranındaki yeni Pano komutunu kullanın.

![Yeni Pano komutu](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

Daha sonra kutucuk galerisini kullanarak kutucukları bulabilir ve ekleyebilirsiniz. Kutucuklar, sürükleyip bırakarak eklenir. Bazı Kutucuklar, bir sürükleme tutamacı aracılığıyla yeniden boyutlandırmayı destekler, diğerleri ise bağlam menüsünde görünebilecek olan düzeltmeleri destekler.

### <a name="drag-handle"></a>Sürükleme tutamacı
![Sürükleme tutamacı](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

### <a name="fixed-sizes-via-context-menu"></a>Bağlam menüsü ile sabit Boyutlar
![Boyutlar bağlam menüsü](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Panoyu paylaşma

Panoyu dilediğiniz şekilde yapılandırdıktan sonra, bir sonraki adım panoyu yayımlamaktır (Share komutunu kullanarak) ve ardından JSON 'ı getirmek için kaynak Gezgini 'ni kullanın.

![Share komutu](./media/azure-portal-dashboards-create-programmatically/share-command.png)

Share komutuna tıkladığınızda, hangi aboneliğin ve kaynak grubunun yayımlanacağını seçmenizi isteyen bir iletişim kutusu gösterilir. Seçtiğiniz aboneliğe ve kaynak grubuna [yazma erişiminizin olması](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) gerektiğini aklınızda bulundurun.

![paylaşım ve erişim](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>Panonun JSON temsilini getir

Yayımlama yalnızca birkaç saniye sürer.  İşlem tamamlandığında, bir sonraki adım JSON 'u getirmek için [Kaynak Gezgini](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) gidemez.

![Kaynak Gezgini 'ne gözatamıyorum](./media/azure-portal-dashboards-create-programmatically/browse-resource-explorer.png)

Kaynak Gezgini ' nden seçtiğiniz abonelik ve kaynak grubuna gidin. Sonra, JSON 'u göstermek için yeni yayınlanan Pano kaynağına tıklayın.

![Kaynak Gezgini JSON](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json.png)

## <a name="create-a-template-from-the-json"></a>JSON 'dan şablon oluşturma

Sonraki adım, bu JSON 'dan bir şablon oluşturmaktır, böylece uygun kaynak yönetimi API 'Leri, komut satırı araçlarıyla veya portalda programlı bir şekilde yeniden kullanılabilir.

Şablon oluşturmak için Pano JSON yapısını tam olarak anlamak gerekli değildir. Çoğu durumda, her kutucuğun yapısını ve yapılandırmasını korumak ve işaret ettikleri Azure kaynakları kümesini parametreleştirmek istersiniz. İçe aktarılmış JSON panonuza bakın ve tüm Azure Kaynak kimliklerinin oluşumlarını bulun. Örnek panonuz, hepsi tek bir Azure sanal makinesine işaret eden birden çok kutucuğa sahiptir. Bunun nedeni, panonuzun yalnızca bu tek kaynağa baktığı için. "/Abonelikler" için örnek JSON (belgenin sonuna dahildir) arıyorsanız, bu kimliğin birkaç tekrarı bulabilirsiniz.

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Bu panoyu gelecekte herhangi bir sanal makine için yayımlamak üzere JSON içinde bu dizenin her geçtiği her zaman için Parametreleştirme yapmanız gerekir. 

Azure 'da kaynak oluşturan API 'lerin iki türleri vardır. Tek seferde bir kaynak oluşturan zorunlu [API 'ler](https://docs.microsoft.com/rest/api/resources/resources) ve tek bir API çağrısıyla birden çok bağımlı kaynak oluşturulmasını düzenleyebilecek [şablon tabanlı bir dağıtım](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) sistemi. İkinci olarak Parametreleştirme ve şablon oluşturma desteği, bizim örneğimizde bunu kullanacağız.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Şablon dağıtımını kullanarak şablonınızdan program aracılığıyla pano oluşturma

Azure, birden fazla kaynağın dağıtımını yönetme olanağı sunar. Dağıtılacak kaynak kümesini ve bunlar arasındaki ilişkileri ifade eden bir dağıtım şablonu oluşturursunuz.  Her bir kaynağın JSON biçimi, bunları bir tane oluşturup oluşturuyoruz. Fark, [şablon dilinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) değişkenler, parametreler, temel işlevler ve daha fazlası gibi birkaç kavram eklemesi gerektiğidir. Bu genişletilmiş sözdizimi yalnızca bir şablon dağıtımı bağlamında desteklenir ve daha önce ele alınan zorunlu API 'lerle birlikte kullanılırsa çalışmaz.

Bu rotayı izleyecekseniz, parametrenin parametre sözdizimi kullanılarak Parametreleştirme yapılmalıdır.  Daha önce bulduğumuz kaynak kimliğinin tüm örneklerini burada gösterildiği gibi değiştirirsiniz.

### <a name="example-json-property-with-hard-coded-resource-id"></a>Sabit kodlanmış kaynak kimliği ile örnek JSON özelliği
`id: "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"`

### <a name="example-json-property-converted-to-a-parameterized-version-based-on-template-parameters"></a>Örnek JSON özelliği, şablon parametrelerine göre parametreli bir sürüme dönüştürüldü

`id: "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"`

Ayrıca, bazı gerekli şablon meta verilerini ve aşağıdaki gibi JSON şablonunun en üstünde bulunan parametreleri bildirmeniz gerekir:

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

__Bu belgenin sonunda, çalışma şablonunu tam olarak görebilirsiniz.__

Şablonunuzu oluşturduktan sonra [REST API 'leri](https://docs.microsoft.com/rest/api/resources/deployments), [POWERSHELL](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), [Azure CLI](https://docs.microsoft.com/cli/azure/group/deployment#az-group-deployment-create)veya [portalın şablon dağıtımı sayfasını](https://portal.azure.com/#create/Microsoft.Template)kullanarak dağıtabilirsiniz.

Örnek panonuzun JSON 'imizin iki sürümü aşağıda verilmiştir. Birincisi, portaldan, bir kaynağa zaten bağlanan sürümdür. İkincisi, program aracılığıyla herhangi bir VM 'ye bağlanabilir ve Azure Resource Manager kullanılarak dağıtılabilir şablon sürümüdür.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>Örnek panonuzun JSON temsili (şablon oluşturma öncesi)

Daha önce dağıtılmış olan bir panonun JSON temsilini getirmek için önceki yönergeleri izleyip izleyeceğinizi, daha önce bu adımları izlemeniz beklendiğinizi görürsünüz. Bu panonun belirli bir Azure sanal makinesini işaret ettiğinden emin olan sabit kodlanmış kaynak tanımlayıcılarını unutmayın.

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

Panonun şablon sürümü __Virtualmachinename__, __virtualMachineResourceGroup__ve __dashboardName__adlı üç parametre tanımladı.  Parametreler, her dağıttığınız zaman bu panoyu farklı bir Azure sanal makinesinde işaret etmenize olanak tanır. Parametreli kimlikler, bu panonun, herhangi bir Azure sanal makinesini işaret etmek üzere programlı bir şekilde yapılandırılıp dağıtılabileceğini göstermek için vurgulanır. Bu özelliği sınamanın en kolay yolu, aşağıdaki şablonu kopyalayıp [Azure Portal şablon dağıtım sayfasına](https://portal.azure.com/#create/Microsoft.Template)yapıştırmaktır. 

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
