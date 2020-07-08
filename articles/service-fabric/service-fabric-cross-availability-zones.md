---
title: Kullanılabilirlik Alanları arasında küme dağıtma
description: Kullanılabilirlik Alanları arasında Azure Service Fabric kümesi oluşturmayı öğrenin.
author: peterpogorski
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: 54382e74899d2cbb56ccf424b0f39bd874e31630
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84259380"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Kullanılabilirlik Alanları arasında bir Azure Service Fabric kümesi dağıtma
Azure 'daki Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Bir kullanılabilirlik alanı, bir Azure bölgesi içinde bağımsız güç, soğutma ve ağ ile donatılmış benzersiz bir fiziksel konumdur.

Service Fabric, belirli bölgelere sabitlenmiş düğüm türlerini dağıtarak Kullanılabilirlik Alanları yayılmış kümeleri destekler. Bu, uygulamalarınızın yüksek kullanılabilirliğe sahip olmasını sağlayacaktır. Azure Kullanılabilirlik Alanları yalnızca belirli bölgelerde kullanılabilir. Daha fazla bilgi için bkz. [Azure kullanılabilirlik alanları genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

Örnek şablonlar mevcuttur: [Service Fabric çapraz kullanılabilirlik bölgesi şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Kullanılabilirlik Alanları arasında yayılan Azure Service Fabric kümelerinin birincil düğüm türü için önerilen topoloji
Kullanılabilirlik Alanları üzerinde dağıtılan bir Service Fabric kümesi, küme durumunun yüksek düzeyde kullanılabilir olmasını sağlar. Bölgeler arasında bir Service Fabric kümesini yaymak için bölgenin desteklediği her bir kullanılabilirlik alanında bir birincil düğüm türü oluşturmanız gerekir. Bu işlem, çekirdek düğümlerini birincil düğüm türlerinin her biri arasında eşit olarak dağıtır.

Birincil düğüm türü için önerilen topoloji, aşağıda özetlenen kaynakları gerektirir:

* Küme güvenilirlik düzeyi Platinum olarak ayarlanır.
* Birincil olarak işaretlenmiş üç düğüm türü.
    * Her düğüm türü, farklı bölgelerde bulunan kendi sanal makine ölçek kümesine eşlenmelidir.
    * Her sanal makine ölçek kümesi en az beş düğüm (gümüş dayanıklılık) içermelidir.
* Standart SKU kullanan tek bir genel IP kaynağı.
* Standart SKU kullanan tek bir Load Balancer kaynağı.
* Sanal makine ölçek kümelerinizi dağıttığınız alt ağ tarafından başvurulan bir NSG.

>[!NOTE]
> Service Fabric alanları kapsayan tek bir sanal makine ölçek kümesini desteklemediğinden, sanal makine ölçek kümesi tek yerleşim grubu özelliği true olarak ayarlanmalıdır.

 ![Azure Service Fabric kullanılabilirlik alanı mimarisi][sf-architecture]

## <a name="networking-requirements"></a>Ağ gereksinimleri
### <a name="public-ip-and-load-balancer-resource"></a>Genel IP ve Load Balancer kaynağı
Bir sanal makine ölçek kümesi kaynağında bölgeler özelliğini etkinleştirmek için, bu sanal makine ölçek kümesi tarafından başvurulan yük dengeleyici ve IP kaynağının her ikisi de *Standart* SKU kullanmalıdır. SKU özelliği olmadan bir yük dengeleyici veya IP kaynağı oluşturmak, Kullanılabilirlik Alanları desteklemeyen temel bir SKU oluşturur. Standart SKU yük dengeleyici, dışarıdaki tüm trafiği varsayılan olarak engeller; Dış trafiğe izin vermek için bir NSG 'nin alt ağa dağıtılması gerekir.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> Ortak IP ve yük dengeleyici kaynaklarında bir yerinde SKU değişikliği yapmak mümkün değildir. Temel SKU 'SU olan mevcut kaynaklardan geçiş yapıyorsanız, bu makalenin geçiş bölümüne bakın.

### <a name="virtual-machine-scale-set-nat-rules"></a>Sanal makine ölçek kümesi NAT kuralları
Yük dengeleyici gelen NAT kuralları, sanal makine ölçek kümesindeki NAT havuzlarıyla eşleşmelidir. Her sanal makine ölçek kümesinin benzersiz bir gelen NAT havuzu olmalıdır.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Standart SKU Load Balancer giden kuralları
Standart Load Balancer ve standart genel IP, temel SKU 'Ları kullanmaya kıyasla giden bağlantılara yeni yetenekler ve farklı davranışlar getirir. Standart SKU 'Lar ile çalışırken giden bağlantı isterseniz, standart genel IP adresleriyle veya standart ortak Load Balancer açıkça tanımlamanız gerekir. Daha fazla bilgi için bkz. [giden bağlantılar](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) ve [Azure Standart Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

>[!NOTE]
> Standart Şablon, tüm giden trafiğe varsayılan olarak izin veren bir NSG 'ye başvurur. Gelen trafik Service Fabric yönetim işlemleri için gerekli olan bağlantı noktalarıyla sınırlıdır. NSG kuralları gereksinimlerinizi karşılayacak şekilde değiştirilebilir.

>[!NOTE]
> Standart SKU SLB kullanarak her bir Service Fabric kümesi, bağlantı noktası 443 ' de giden trafiğe izin veren her düğüm türünün bir kurala sahip olduğundan emin olunması gerekir. Bu, küme kurulumunu tamamlaması için gereklidir ve bu tür bir kural olmadan herhangi bir dağıtım başarısız olur.


### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi üzerinde bölgeleri etkinleştirme
Bir bölgeyi etkinleştirmek için, bir sanal makine ölçek kümesinde, sanal makine ölçek kümesi kaynağına aşağıdaki üç değeri eklemeniz gerekir.

* İlk değer, sanal makine ölçek kümesinin hangi kullanılabilirlik bölgesine dağıtılacağını belirten **bölgeler** özelliğidir.
* İkinci değer, true olarak ayarlanması gereken "singlePlacementGroup" özelliğidir.
* Üçüncü değer, Service Fabric sanal makine ölçek kümesi uzantısında "faultDomainOverride" özelliğidir. Bu özelliğin değeri, bu sanal makine ölçek kümesinin yerleştirileceği bölgeyi ve bölgeyi içermelidir. Örnek: "faultDomainOverride": "eastus/AZ1" Azure Service Fabric kümelerinde çapraz bölge desteği olmadığından, tüm sanal makine ölçek kümesi kaynakları aynı bölgeye yerleştirilmelidir.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric.Test",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "eastus/az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağında birden çok birincil düğüm türü etkinleştiriliyor
Bir küme kaynağında birincil olarak bir veya daha fazla düğüm türü ayarlamak için "Isprımary" özelliğini "true" olarak ayarlayın. Kullanılabilirlik Alanları arasında Service Fabric kümesi dağıtımında, farklı bölgelerde üç düğüm türüne sahip olmanız gerekir.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType2Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt2applicationEndPort')]",
            "startPort": "[parameters('nt2applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt2ephemeralEndPort')]",
            "startPort": "[parameters('nt2ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Temel SKU Load Balancer ve temel SKU IP 'si kullanarak bir kümeden Kullanılabilirlik Alanları kullanmak için geçirme
Temel SKU ile Load Balancer ve IP kullanan bir kümeyi geçirmek için, önce standart SKU 'yu kullanarak tamamen yeni bir Load Balancer ve IP kaynağı oluşturmanız gerekir. Bu kaynakları yerinde güncelleştirmek mümkün değildir.

Yeni LB ve IP 'yi kullanmak istediğiniz yeni çapraz kullanılabilirlik bölgesi düğüm türlerinde başvurulmalıdır. Yukarıdaki örnekte, bölge 1, 2 ve 3 ' te üç yeni sanal makine ölçek kümesi kaynağı eklenmiştir. Bu sanal makine ölçek kümeleri, yeni oluşturulan LB ve IP 'ye başvurur ve Service Fabric küme kaynağında birincil düğüm türleri olarak işaretlenir.

Başlamak için, mevcut Kaynak Yöneticisi şablonunuza yeni kaynakları eklemeniz gerekir. Bu kaynaklar şunları içerir:
* Standart SKU kullanan genel bir IP kaynağı.
* Standart SKU kullanan bir Load Balancer kaynağı.
* Sanal makine ölçek kümelerinizi dağıttığınız alt ağ tarafından başvurulan bir NSG.
* Birincil olarak işaretlenmiş üç düğüm türü.
    * Her düğüm türü, farklı bölgelerde bulunan kendi sanal makine ölçek kümesine eşlenmelidir.
    * Her sanal makine ölçek kümesi en az beş düğüm (gümüş dayanıklılık) içermelidir.

[Örnek şablonda](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure)bu kaynaklara bir örnek bulabilirsiniz.

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Kaynakların dağıtımı tamamlandıktan sonra, birincil düğüm türündeki düğümleri orijinal kümeden devre dışı bırakmayı deneyebilirsiniz. Düğümler devre dışı bırakıldığı için, sistem hizmetleri Yukarıdaki adımda dağıtılan yeni birincil düğüm türüne geçirilir.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Düğümlerin tümü devre dışı bırakıldıktan sonra sistem hizmetleri, bölgeler arasında yayılan birincil düğüm türünde çalışır. Bundan sonra devre dışı bırakılmış düğümleri kümeden kaldırabilirsiniz. Düğümler kaldırıldıktan sonra, özgün IP, Load Balancer ve sanal makine ölçek kümesi kaynaklarını kaldırabilirsiniz.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

Daha sonra, bu kaynaklara ait başvuruları dağıttığınız Kaynak Yöneticisi şablondan kaldırmanız gerekir.

Son adım, DNS adının ve genel IP 'nin güncelleştirilmesini içerir.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
