---
title: Azure’da Service Fabric kümesi ölçeklendirme
description: Bu eğitimde, Azure'da bir Hizmet Kumaşı kümesini nasıl ölçeklendirdiğinizi ve kalan kaynakları nasıl temizleyesiniz öğrenirsiniz.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: f1b813576a94541cdc2ab0a67fea71b6f49696c5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78251806"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure'daki bir Service Fabric kümesini ölçeklendirme

Bu öğretici, bir serinin üçüncü bölümüdür ve varolan kümenizi nasıl ölçeklendirdiğinizi ve içinde nasıl ölçeklendirilebildiğinizi gösterir. Tamamladığınızda, kümenizin nasıl ölçekleneceğini ve kalan kaynakların nasıl temizleneceğini öğrenmiş olacaksınız.  Azure'da çalışan bir kümeyi ölçekleme hakkında daha fazla bilgi için [Ölçekleme Hizmeti Kumaş kümelerini](service-fabric-cluster-scaling.md)okuyun.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Düğümekleme ve kaldırma (ölçeklendirin ve ölçeklendirin)
> * Düğüm türlerini ekleme ve kaldırma (ölçeklendirin ve ölçeklendirin)
> * Düğüm kaynaklarını artırma (ölçeklendirin)

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oluşturma
> * [Bir kümeyi izleme](service-fabric-tutorial-monitor-cluster.md)
> * Bir kümenin ölçeğini daraltma veya genişletme
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps) veya [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
* Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oluşturma

## <a name="important-considerations-and-guidelines"></a>Önemli hususlar ve yönergeler

Uygulama iş yükleri zaman içinde değişir, varolan hizmetlerinizin daha fazla (veya daha az) kaynağa ihtiyacı var mı?  Küme kaynaklarını artırmak veya azaltmak için düğümleri düğüm türünden ekleyin veya [kaldırın.](#add-nodes-to-or-remove-nodes-from-a-node-type)

Kümenize 100'den fazla düğüm eklemeniz gerekiyor mu?  Tek bir Hizmet Kumaş düğüm türü/ölçek kümesi 100'den fazla düğüm/VM içeremez.  Bir kümeyi 100 düğümün ötesine ölçeklendirmek için [ek düğüm türleri ekleyin.](#add-nodes-to-or-remove-nodes-from-a-node-type)

Uygulamanızın birden çok hizmeti var mı ve bunlardan herhangi birinin herkese açık veya internetle karşı karşıya olması gerekiyor mu?  Tipik uygulamalar, istemciden giriş alan bir ön uç ağ geçidi hizmeti ve ön uç hizmetleriyle iletişim sağlayan bir veya daha fazla arka uç hizmeti içerir. Bu durumda, kümeye [en az iki düğüm türü eklemenizi](#add-nodes-to-or-remove-nodes-from-a-node-type) öneririz.  

Hizmetlerinizin daha fazla RAM veya daha yüksek CPU döngüleri gibi farklı altyapı gereksinimleri var mı? Örneğin, uygulamanız bir ön uç hizmeti ve bir arka uç hizmeti içerir. Ön uç hizmeti, bağlantı noktaları internete açık olan daha küçük VM'lerde (D2 gibi VM boyutları) çalıştırılabilir. Ancak arka uç hizmeti, hesaplama yoğundur ve internete dönük olmayan daha büyük VM'lerde (D4, D6, D15 gibi VM boyutlarında) çalışması gerekir. Bu durumda, kümenize [iki veya daha fazla düğüm türü eklemenizi](#add-nodes-to-or-remove-nodes-from-a-node-type) tavsiye ettik. Bu, her düğüm türünün internet bağlantısı veya VM boyutu gibi farklı özelliklere sahip olmasını sağlar. VM sayısı da bağımsız olarak ölçeklendirilebilir.

Bir Azure kümesini ölçeklerken aşağıdaki yönergeleri göz önünde bulundurun:

* Tek bir Hizmet Kumaş düğüm türü/ölçek kümesi 100'den fazla düğüm/VM içeremez.  Bir kümeyi 100 düğümün ötesine ölçeklendirmek için ek düğüm türleri ekleyin.
* Üretim iş yüklerini çalıştıran birincil düğüm türleri Altın veya Gümüş [dayanıklılık düzeyine][durability] sahip olmalı ve her zaman beş veya daha fazla düğüme sahip olmalıdır.
* Durum lu üretim iş yüklerini çalıştıran birincil olmayan düğüm türlerinin her zaman beş veya daha fazla düğümü olmalıdır.
* Durum dışı üretim iş yüklerini çalıştıran birincil olmayan düğüm türlerinin her zaman iki veya daha fazla düğümü olmalıdır.
* Altın veya Gümüş [dayanıklılık düzeyi][durability] herhangi bir düğüm türü her zaman beş veya daha fazla düğüm olmalıdır.
* Birincil düğüm türünde ölçekleme (düğümleri kaldırma) varsa, örnek sayısını hiçbir zaman [güvenilirlik düzeyinin][reliability] gerektirdiğinden daha aza düşürmemelisiniz.

Daha fazla bilgi için [küme kapasitesi kılavuzunu](service-fabric-cluster-capacity.md)okuyun.

## <a name="export-the-template-for-the-resource-group"></a>Kaynak grubu için şablonu dışarı aktarma

Güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oluşturduktan ve kaynak grubunuzu başarıyla ayarladıktan sonra, kaynak grubu için Kaynak Yöneticisi şablonunu dışa aktarın. Şablonu dışa aktarmak, şablonun tüm altyapısını içerdiğinden kümenin ve kaynaklarının gelecekteki dağıtımlarını otomatikleştirmenize olanak tanır.  Şablonları dışa aktarma hakkında daha fazla bilgi için Azure [portalını kullanarak Azure Kaynak Yöneticisi kaynak gruplarını yönet'i](/azure/azure-resource-manager/manage-resource-groups-portal)okuyun.

1. Azure [portalında,](https://portal.azure.com)kümeyi içeren kaynak grubuna **(sfclustertutorialgroup**, bu öğreticiyi takip ediyorsanız) gidin. 

2. Sol **bölmede, Dağıtımlar'ı**seçin veya **Dağıtımlar**altındaki bağlantıyı seçin. 

3. Listeden en son başarılı dağıtımı seçin.

4. Sol bölmede **Şablon'u** seçin ve ardından şablonu ZIP dosyası olarak dışa aktarmak için **İndir'i** seçin.  Şablonu ve parametreleri yerel bilgisayarınıza kaydedin.

## <a name="add-nodes-to-or-remove-nodes-from-a-node-type"></a>Düğüm türüne düğüm ekleme veya düğümleri kaldırma

Ölçekleme girip çıkma veya yatay ölçekleme kümedeki düğüm sayısını değiştirir. Ölçeklendirildiğinde veya çıktığınızda, ölçek kümesine daha fazla sanal makine örneği eklersiniz. Bu örnekler, Service Fabric tarafından kullanılan düğümler haline gelir. Service Fabric, ölçek kümesine ne zaman daha fazla örnek eklendiğini bilir (ölçek genişletilerek) ve otomatik olarak tepki verir. Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.

### <a name="update-the-template"></a>Şablonu güncelleştirme

En son dağıtım için kaynak grubundan [bir şablon ve parametre dosyası dışa](#export-the-template-for-the-resource-group) aktarın.  *Parametreler.json* dosyasını açın.  Kümeyi bu öğreticideki [örnek şablonu][template] kullanarak dağıttıysanız, kümede üç düğüm türü ve her düğüm türü için düğüm sayısını ayarlayan üç parametre vardır: *nt0InstanceCount*, *nt1InstanceCount*ve *nt2InstanceCount*.  Örneğin *nt1InstanceCount* parametresi, ikinci düğüm türü için örnek sayısını ayarlar ve ilişkili sanal makine ölçeği kümesindeki VM sayısını ayarlar.

Bu nedenle, *nt1InstanceCount* değerini güncelleyerek ikinci düğüm türünde düğüm sayısını değiştirirsiniz.  Unutmayın, bir düğüm türünü 100'den fazla düğüme ölçeklendiremezsiniz.  Durum lu üretim iş yüklerini çalıştıran birincil olmayan düğüm türlerinin her zaman beş veya daha fazla düğümü olmalıdır. Durum dışı üretim iş yüklerini çalıştıran birincil olmayan düğüm türlerinin her zaman iki veya daha fazla düğümü olmalıdır.

Eğer ölçekleme, düğümleri kaldırarak, Bronz [dayanıklılık düzeyi][durability] bir düğüm türü el ile [bu düğümlerin durumunu kaldırmanız](service-fabric-cluster-scale-up-down.md#manually-remove-vms-from-a-node-typevirtual-machine-scale-set)gerekir.  Gümüş ve Altın dayanıklılık katmanı için bu adımlar platform tarafından otomatik olarak yapılır.

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
*Template.json* ve *parameters.json* dosyalarındaki değişiklikleri kaydedin.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ChangingInstanceCount"
```
Veya aşağıdaki Azure CLI komutu:
```azurecli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="add-a-node-type-to-the-cluster"></a>Kümeye düğüm türü ekleme

Azure'da çalışan bir Hizmet Kumaşı kümesinde tanımlanan her düğüm türü ayrı bir [sanal makine ölçek kümesi](service-fabric-cluster-nodetypes.md)olarak ayarlanır. Her düğüm türü daha sonra ayrı ayrı yönetilebilir. Bağımsız olarak her düğüm türünü yukarı veya aşağı ölçeklendirebilir, farklı bağlantı noktaları kümelerini açabilir ve farklı kapasite ölçümleri kullanabilirsiniz. Ayrıca, her küme düğümünde çalışan işletim sistemi SKU'yu bağımsız olarak değiştirebilirsiniz, ancak örnek kümede çalışan Windows ve Linux karışımına sahip olamazsınız. Tek bir düğüm türü/ölçek kümesi 100'den fazla düğüm içeremez.  Ek düğüm türleri/ölçek kümeleri ekleyerek bir kümeyi yatay olarak 100'den fazla düğüme ölçeklendirebilirsiniz. Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.

### <a name="update-the-template"></a>Şablonu güncelleştirme

En son dağıtım için kaynak grubundan [bir şablon ve parametre dosyası dışa](#export-the-template-for-the-resource-group) aktarın.  *Parametreler.json* dosyasını açın.  Bu öğreticideki [örnek şablonu][template] kullanarak kümeyi dağıttıysanız, kümede üç düğüm türü vardır.  Bu bölümde, Kaynak Yöneticisi şablonu güncelleştirerek ve dağıtarak dördüncü bir düğüm türü eklersiniz. 

Yeni düğüm türüne ek olarak, ilişkili sanal makine ölçeği kümesini (sanal ağın ayrı bir alt ağında çalışır) ve ağ güvenlik grubunu da eklersiniz.  Yeni ölçek kümesi için yeni veya varolan genel IP adresi ve Azure yük dengeleyici kaynakları eklemeyi seçebilirsiniz.  Yeni düğüm türü Gümüş [dayanıklılık düzeyine][durability] ve "Standard_D2_V2" boyutuna sahiptir.

*template.json* dosyasına aşağıdaki yeni parametreleri ekleyin:
```json
"nt3InstanceCount": {
    "defaultValue": 5,
    "type": "Int",
    "metadata": {
        "description": "Instance count for node type"
    }
},
"vmNodeType3Size": {
    "defaultValue": "Standard_D2_V2",
    "type": "String"
},
```

*template.json* dosyasına aşağıdaki yeni değişkenleri ekleyin:
```json
"lbID3": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
"lbIPConfig3": "[concat(variables('lbID3'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
"lbPoolID3": "[concat(variables('lbID3'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
"lbProbeID3": "[concat(variables('lbID3'),'/probes/FabricGatewayProbe')]",
"lbHttpProbeID3": "[concat(variables('lbID3'),'/probes/FabricHttpGatewayProbe')]",
"lbNatPoolID3": "[concat(variables('lbID3'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
"vmNodeType3Name": "[toLower(concat('NT4', variables('vmName')))]",
"vmStorageAccountName3": "[toLower(concat(uniqueString(resourceGroup().id), '1', '3' ))]",
"nt3applicationStartPort": "20000",
"nt3applicationEndPort": "30000",
"nt3ephemeralStartPort": "49152",
"nt3ephemeralEndPort": "65534",
"nt3fabricTcpGatewayPort": "19000",
"nt3fabricHttpGatewayPort": "19080",
"nt3reverseProxyEndpointPort": "19081",
"subnet3Name": "Subnet-3",
"subnet3Prefix": "10.0.3.0/24",
"subnet3Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet3Name'))]",
```

*template.json* dosyasında, sanal ağ kaynağına yeni bir alt ağ ekleyin:
```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            ...
            {
                "name": "[variables('subnet3Name')]",
                "properties": {
                    "addressPrefix": "[variables('subnet3Prefix')]",
                    "networkSecurityGroup": {
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', variables('subnet3Name')))]"
                    }
                }
            }
        ]
    },
    "dependsOn": [
        ...
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', variables('subnet3Name')))]"
    ]
},
```

*template.json* dosyasına, yeni genel IP adresi ve yük bakiyesi kaynakları ekleyin:
```json
{
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "dnsSettings": {
            "domainNameLabel": "[concat(variables('dnsName'),'-','nt4')]"
        },
        "publicIPAllocationMethod": "Dynamic"
    }
},
        {
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "frontendIPConfigurations": [
            {
                "name": "LoadBalancerIPConfig",
                "properties": {
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
                    }
                }
            }
        ],
        "backendAddressPools": [
            {
                "name": "LoadBalancerBEAddressPool",
                "properties": {}
            }
        ],
        "loadBalancingRules": [
            {
                "name": "LBRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "LBHttpRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbHttpProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule1",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort1')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort1')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe1')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule2",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort2')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort2')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe2')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            {
                "name": "FabricGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricTcpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "FabricHttpGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricHttpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe1",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort1')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe2",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort2')]",
                    "protocol": "tcp"
                }
            }
        ],
        "inboundNatPools": [
            {
                "name": "LoadBalancerBEAddressNatPool",
                "properties": {
                    "backendPort": "3389",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPortRangeEnd": "4500",
                    "frontendPortRangeStart": "3389",
                    "protocol": "tcp"
                }
            }
        ]
    },
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
    ]
},
```

*template.json* dosyasında, yeni ağ güvenlik grubu ve sanal makine ölçeği kümesi kaynakları ekleyin.  Sanal makine ölçeğinin Hizmet Kumaşı uzantısı özellikleri içindeki NodeTypeRef özelliği, belirtilen düğüm türünü ölçek kümesiyle eşler.

```json
{
    "type": "Microsoft.Network/networkSecurityGroups",
    "name": "[concat('nsg', variables('subnet3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[resourceGroup().location]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "securityRules": [
            {
                "name": "allowSvcFabSMB",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "445",
                    "direction": "Inbound",
                    "priority": 3950,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow SMB traffic within the net, used by fabric to move packages around"
                }
            },
            {
                "name": "allowSvcFabCluser",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "1025-1027",
                    "direction": "Inbound",
                    "priority": 3920,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow ports within vnet that are used by the fabric to talk between nodes"
                }
            },
            {
                "name": "allowSvcFabEphemeral",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3ephemeralStartPort'), '-', variables('nt3ephemeralEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3930,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow fabric ephemeral ports within the vnet"
                }
            },
            {
                "name": "allowSvcFabPortal",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricHttpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3900,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster web portal"
                }
            },
            {
                "name": "allowSvcFabClient",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricTcpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3910,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used by the fabric client (includes powershell)"
                }
            },
            {
                "name": "allowSvcFabApplication",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3applicationStartPort'), '-', variables('nt3applicationEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3940,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow fabric application ports within the vnet"
                }
            },
            {
                "name": "blockAll",
                "properties": {
                    "access": "Deny",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "*",
                    "direction": "Inbound",
                    "priority": 4095,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "block all traffic except what we've explicitly allowed"
                }
            },
            {
                "name": "allowVNetRDP",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "3389",
                    "direction": "Inbound",
                    "priority": 3960,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow RDP within the net"
                }
            },
            {
                "name": "allowSvcFabReverseProxy",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3reverseProxyEndpointPort')]",
                    "direction": "Inbound",
                    "priority": 3980,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster using reverse proxy"
                }
            },
            {
                "name": "allowAppPort1",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort1')]",
                    "direction": "Inbound",
                    "priority": 2001,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 1"
                }
            },
            {
                "name": "allowAppPort2",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort2')]",
                    "direction": "Inbound",
                    "priority": 2002,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 2"
                }
            }
        ]
    }
},
{
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "sku": {
        "name": "[parameters('vmNodeType3Size')]",
        "capacity": "[parameters('nt3InstanceCount')]",
        "tier": "Standard"
    },
    "name": "[variables('vmNodeType3Name')]",
    "apiVersion": "2018-10-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "overprovision": "[variables('overProvision')]",
        "upgradePolicy": {
            "mode": "Automatic"
        },
        "virtualMachineProfile": {
            "extensionProfile": {
                "extensions": [
                    {
                        "name": "[concat(variables('vmNodeType3Name'),'OMS')]",
                        "properties": {
                            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                            "type": "MicrosoftMonitoringAgent",
                            "typeHandlerVersion": "1.0",
                            "autoUpgradeMinorVersion": true,
                            "settings": {
                                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                            },
                            "protectedSettings": {
                                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                            }
                        }
                    },
                    {
                        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "ServiceFabricNode",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                            },
                            "publisher": "Microsoft.Azure.ServiceFabric",
                            "settings": {
                                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                "nodeTypeRef": "[variables('vmNodeType3Name')]",
                                "dataPath": "D:\\SvcFab",
                                "durabilityLevel": "Silver",
                                "enableParallelJobs": true,
                                "nicPrefixOverride": "[variables('subnet3Prefix')]",
                                "certificate": {
                                    "thumbprint": "[parameters('certificateThumbprint')]",
                                    "x509StoreName": "[parameters('certificateStoreValue')]"
                                }
                            },
                            "typeHandlerVersion": "1.0"
                        }
                    },
                    {
                        "name": "[concat('VMDiagnosticsVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "IaaSDiagnostics",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "storageAccountName": "[variables('applicationDiagnosticsStorageAccountName')]",
                                "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                "storageAccountEndPoint": "https://core.windows.net/"
                            },
                            "publisher": "Microsoft.Azure.Diagnostics",
                            "settings": {
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
                                        }
                                    }
                                },
                                "StorageAccount": "[variables('applicationDiagnosticsStorageAccountName')]"
                            },
                            "typeHandlerVersion": "1.5"
                        }
                    },
                    {
                        "name": "[concat('VMIaaSAntimalware','_vmNodeType3Name')]",
                        "properties": {
                            "publisher": "Microsoft.Azure.Security",
                            "type": "IaaSAntimalware",
                            "typeHandlerVersion": "1.5",
                            "settings": {
                                "AntimalwareEnabled": "true",
                                "Exclusions": {
                                    "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                                    "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
                                },
                                "RealtimeProtectionEnabled": "true",
                                "ScheduledScanSettings": {
                                    "isEnabled": "true",
                                    "scanType": "Quick",
                                    "day": "7",
                                    "time": "120"
                                }
                            },
                            "protectedSettings": null
                        }
                    }
                ]
            },
            "networkProfile": {
                "networkInterfaceConfigurations": [
                    {
                        "name": "[concat(variables('nicName'), '-2')]",
                        "properties": {
                            "ipConfigurations": [
                                {
                                    "name": "[concat(variables('nicName'),'-',2)]",
                                    "properties": {
                                        "loadBalancerBackendAddressPools": [
                                            {
                                                "id": "[variables('lbPoolID3')]"
                                            }
                                        ],
                                        "loadBalancerInboundNatPools": [
                                            {
                                                "id": "[variables('lbNatPoolID3')]"
                                            }
                                        ],
                                        "subnet": {
                                            "id": "[variables('subnet3Ref')]"
                                        }
                                    }
                                }
                            ],
                            "primary": true
                        }
                    }
                ]
            },
            "osProfile": {
                "adminPassword": "[parameters('adminPassword')]",
                "adminUsername": "[parameters('adminUsername')]",
                "computernamePrefix": "[variables('vmNodeType3Name')]",
                "secrets": [
                    {
                        "sourceVault": {
                            "id": "[parameters('sourceVaultValue')]"
                        },
                        "vaultCertificates": [
                            {
                                "certificateStore": "[parameters('certificateStoreValue')]",
                                "certificateUrl": "[parameters('certificateUrlValue')]"
                            }
                        ]
                    }
                ]
            },
            "storageProfile": {
                "imageReference": {
                    "publisher": "[parameters('vmImagePublisher')]",
                    "offer": "[parameters('vmImageOffer')]",
                    "sku": "[parameters('vmImageSku')]",
                    "version": "[parameters('vmImageVersion')]"
                },
                "osDisk": {
                    "caching": "ReadOnly",
                    "createOption": "FromImage",
                    "managedDisk": {
                        "storageAccountType": "[parameters('storageAccountType')]"
                    }
                }
            }
        }
    },
    "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
    ]
},
```

*template.json* dosyasında küme kaynağını güncelleştirin ve yeni bir düğüm türü ekleyin:
```json
{
    "type": "Microsoft.ServiceFabric/clusters",
    "name": "[parameters('clusterName')]",
    "apiVersion": "2018-02-01",
    "location": "[parameters('clusterLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "nodeTypes": [
            ...
            {
                "name": "[variables('vmNodeType3Name')]",
                "applicationPorts": {
                    "endPort": "[variables('nt3applicationEndPort')]",
                    "startPort": "[variables('nt3applicationStartPort')]"
                },
                "clientConnectionEndpointPort": "[variables('nt3fabricTcpGatewayPort')]",
                "durabilityLevel": "Silver",
                "ephemeralPorts": {
                    "endPort": "[variables('nt3ephemeralEndPort')]",
                    "startPort": "[variables('nt3ephemeralStartPort')]"
                },
                "httpGatewayEndpointPort": "[variables('nt3fabricHttpGatewayPort')]",
                "isPrimary": false,
                "reverseProxyEndpointPort": "[variables('nt3reverseProxyEndpointPort')]",
                "vmInstanceCount": "[parameters('nt3InstanceCount')]"
            }
        ],    
    }
}                
```

*parameters.json* dosyasına aşağıdaki yeni parametreleri ve değerleri ekleyin:
```json
"nt3InstanceCount": {
    "Value": 5    
},
"vmNodeType3Size": {
    "Value": "Standard_D2_V2"
},
```

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
*Template.json* ve *parameters.json* dosyalarındaki değişiklikleri kaydedin.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "AddingNodeType"
```
Veya aşağıdaki Azure CLI komutu:
```azurecli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="remove-a-node-type-from-the-cluster"></a>Kümeden bir düğüm türünü kaldırma
Hizmet Kumaşı kümesi oluşturduktan sonra, düğüm türünü (sanal makine ölçek kümesi) ve tüm düğümlerini kaldırarak kümeyi yatay olarak ölçeklendirebilirsiniz. Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz. Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

> [!WARNING]
> Bir üretim kümesinden bir düğüm türünü kaldırmak için Remove-AzServiceFabricNodeType'ın kullanılması sık olarak kullanılması önerilmez. Düğüm türünün arkasındaki sanal makine ölçeği ayarlı kaynağı sildiği için tehlikeli bir komutdur. 

Düğüm türünü kaldırmak için [Remove-AzServiceFabricNodeType](/powershell/module/az.servicefabric/remove-azservicefabricnodetype) cmdlet'i çalıştırın.  Düğüm türü Gümüş veya Altın [dayanıklılık düzeyi][durability] olmalıdır Cmdlet düğüm türüile ilişkili ölçek kümesini siler ve tamamlanması biraz zaman alır.  Ardından kaldırılacak düğümlerin her birinde [kaldır-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) cmdlet'i çalıştırın, bu da düğüm durumunu siler ve düğümleri kümeden kaldırır. Düğümlerde hizmetler varsa, hizmetler önce başka bir düğüme taşınır. Küme yöneticisi yineleme/hizmet için bir düğüm bulamazsa, işlem geciktirilir/engellenir.

```powershell
$groupname = "sfclustertutorialgroup"
$nodetype = "nt4vm"
$clustername = "mysfcluster123"

Remove-AzServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My

$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="increase-node-resources"></a>Düğüm kaynaklarını artırma 
Hizmet Kumaşı kümesi oluşturduktan sonra, küme düğümü türünü dikey olarak ölçeklendirebilir (düğümlerin kaynaklarını değiştirebilir) veya düğüm türü VM'lerin işletim sistemini yükseltebilirsiniz.  

> [!WARNING]
> Silver dayanıklılıkveya daha büyük bir ölçek kümesi/düğümü türündeki VM SKU'yu değiştirmemenizi öneririz. VM SKU boyutunu değiştirmek, yerinde veri yıkıcı bir altyapı işlemidir. Bu değişikliği geciktirme veya izleme yeteneği olmadan, işlemin devlet tarafından sunulan hizmetler için veri kaybına veya devlet siz iş yüklerinde bile öngörülemeyen diğer operasyonel sorunlara neden olması mümkündür.

> [!WARNING]
> Tehlikeli bir işlem olan ve desteklenmeyen birincil düğüm türünün VM SKU'yu değiştirmemenizi öneririz.  Daha fazla küme kapasitesine ihtiyacınız varsa, daha fazla VM örneği veya ek düğüm türü ekleyebilirsiniz.  Bu mümkün değilse, eski kümenizden yeni bir küme oluşturabilir ve uygulama durumunu (varsa) [geri yükleyebilirsiniz.](service-fabric-reliable-services-backup-restore.md)  Bu mümkün değilse, [birincil düğüm türünün VM SKU'yu değiştirebilirsiniz.](service-fabric-scale-up-node-type.md)

### <a name="update-the-template"></a>Şablonu güncelleştirme

En son dağıtım için kaynak grubundan [bir şablon ve parametre dosyası dışa](#export-the-template-for-the-resource-group) aktarın.  *Parametreler.json* dosyasını açın.  Bu öğreticideki [örnek şablonu][template] kullanarak kümeyi dağıttıysanız, kümede üç düğüm türü vardır.  

İkinci düğüm türündeki VM'lerin boyutu *vmNodeType1Size* parametresinde ayarlanır.  *VmNodeType1Size* parametre değerini Standard_D2_V2'dan [Standard_D3_V2'a](../virtual-machines/dv2-dsv2-series.md)değiştirin ve bu da her VM örneğinin kaynaklarını ikikatına çıkar.

Her üç düğüm türü için VM SKU *vmImageSku* parametre ayarlanır.  Yine, bir düğüm türünün VM SKU'su değiştirilmelidir ve birincil düğüm türü için önerilmez.

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
*Template.json* ve *parameters.json* dosyalarındaki değişiklikleri kaydedin.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ScaleUpNodeType"
```
Veya aşağıdaki Azure CLI komutu:
```azurecli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Düğümekleme ve kaldırma (ölçeklendirin ve ölçeklendirin)
> * Düğüm türlerini ekleme ve kaldırma (ölçeklendirin ve ölçeklendirin)
> * Düğüm kaynaklarını artırma (ölçeklendirin)

Ardından, kümenin çalışma zamanının nasıl yükseltileceğini öğrenmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster
[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json

> * Düğüm türlerini ekleme ve kaldırma (ölçeklendirin ve ölçeklendirin)
> * Düğüm kaynaklarını artırma (ölçeklendirin)

Ardından, kümenin çalışma zamanının nasıl yükseltileceğini öğrenmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster
[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
