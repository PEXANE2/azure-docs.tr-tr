---
title: Kullanılabilirlik Bölgeleri arasında bir küme dağıtma
description: Kullanılabilirlik Bölgeleri arasında Azure Hizmet Kumaşı kümesini nasıl oluşturabilirsiniz öğrenin.
author: peterpogorski
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: 6da9517f822c9c157d26a1bda8dab2c694b08b12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609987"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Kullanılabilirlik Bölgeleri arasında Azure Hizmet Kumaşı kümesini dağıtma
Azure'daki Kullanılabilirlik Bölgeleri, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirlik teklifidir. Kullanılabilirlik Bölgesi, bir Azure bölgesinde bağımsız güç, soğutma ve ağ ile donatılmış benzersiz bir fiziksel konumdur.

Service Fabric, belirli bölgelere sabitlenmiş düğüm türlerini dağıtarak Kullanılabilirlik Bölgeleri arasında yayılan kümeleri destekler. Bu, uygulamalarınızın yüksek kullanılabilirliğini sağlayacaktır. Azure Kullanılabilirlik Bölgeleri yalnızca belirli bölgelerde kullanılabilir. Daha fazla bilgi için Azure [Kullanılabilirlik Bölgelerine Genel Bakış'a](https://docs.microsoft.com/azure/availability-zones/az-overview)bakın.

Örnek şablonlar mevcuttur: [Service Fabric çapraz kullanılabilirlik bölgesi şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Kullanılabilirlik Bölgeleri arasında yayılan Azure Hizmet Kumaşı kümelerinin birincil düğüm türü için önerilen topoloji
Kullanılabilirlik Bölgeleri arasında dağıtılan Hizmet Kumaşı kümesi, küme durumunun yüksek kullanılabilirliğini sağlar. Bölgeler arasında bir Hizmet Kumaşı kümesini kapamak için, bölge tarafından desteklenen her Kullanılabilirlik Bölgesinde birincil düğüm türü oluşturmanız gerekir. Bu, tohum düğümlerini birincil düğüm türlerinin her birine eşit olarak dağıtır.

Birincil düğüm türü için önerilen topoloji, aşağıda özetlenen kaynakları gerektirir:

* Küme güvenilirlik düzeyi Platin olarak ayarlanır.
* Birincil olarak işaretlenen üç Düğüm Türü.
    * Her Düğüm Türü, farklı bölgelerde bulunan kendi sanal makine ölçeği kümesine eşlenmelidir.
    * Her sanal makine ölçek kümesien az beş düğüm (Gümüş Dayanıklılık) olmalıdır.
* Standart SKU kullanan tek bir genel IP Kaynağı.
* Standart SKU kullanan tek yük dengeleyici kaynağı.
* Sanal makine ölçek kümelerinizi dağıttığınız alt ağ tarafından başvurulan bir NSG.

>[!NOTE]
> Service Fabric bölgeleri kapsayan tek bir sanal makine ölçeği kümesini desteklemediğinden, sanal makine ölçeği tek yerleşim grubu özelliği nin gerçek olarak ayarlanabilmesi gerekir.

 ![Azure Hizmet Kumaş Kullanılabilirlik Bölgesi Mimarisi][sf-architecture]

## <a name="networking-requirements"></a>Ağ gereksinimleri
### <a name="public-ip-and-load-balancer-resource"></a>Genel IP ve Yük Dengeleyici Kaynağı
Sanal makine ölçeği ayarlanmış kaynaktaki bölgeler özelliğini etkinleştirmek için, bu sanal makine ölçeği kümesi tarafından başvurulan yük dengeleyicisi ve IP kaynağının her ikisi de *standart* SKU kullanıyor olmalıdır. SKU özelliği olmadan bir yük dengeleyicisi veya IP kaynağı oluşturmak, Kullanılabilirlik Bölgelerini desteklemeyen bir Temel SKU oluşturur. Standart Bir SKU yük dengeleyicivarsayılan olarak dışarıdan tüm trafiği engelleyecektir; dış trafiğe izin vermek için, alt ağa bir NSG dağıtılmalıdır.

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
> Kamu IP ve yük dengeleyici kaynakları üzerinde SKU yerinde bir değişiklik yapmak mümkün değildir. Temel SKU'su olan varolan kaynaklardan geçiş yapıyorsunuz, bu makalenin geçiş bölümüne bakın.

### <a name="virtual-machine-scale-set-nat-rules"></a>Sanal makine ölçeği NAT kurallarını belirler
Yük dengeleyici gelen NAT kuralları sanal makine ölçeği kümesinden NAT havuzları eşleşmelidir. Her sanal makine ölçeği kümesinin benzersiz bir gelen NAT havuzu olmalıdır.

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

### <a name="standard-sku-load-balancer-outbound-rules"></a>Standart SKU Yük Dengeleyici giden kuralları
Standart Yük Dengeleyicisi ve Standart Genel IP, Temel STU'lar kullanılarak karşılaştırıldığında giden bağlantıya yeni yetenekler ve farklı davranışlar sunar. Standart SKUs'larla çalışırken giden bağlantı istiyorsanız, standart genel IP adresleri veya Standart genel yük dengeleyicisi ile açıkça tanımlamanız gerekir. Daha fazla bilgi [için, Giden bağlantılara](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) ve [Azure Standart Yük Dengeleyicisi'ne](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)bakın.

>[!NOTE]
> Standart şablon, varsayılan olarak tüm giden trafiğe izin veren bir NSG'ye başvurur. Gelen trafik, Service Fabric yönetim işlemleri için gerekli olan bağlantı noktalarıyla sınırlıdır. NSG kuralları gereksinimlerinizi karşılayacak şekilde değiştirilebilir.

### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesinde bölgeleri etkinleştirme
Bir bölgeyi etkinleştirmek için, sanal makine ölçeği kümesinde aşağıdaki üç değeri sanal makine ölçeği kümesi kaynağına eklemeniz gerekir.

* İlk değer, sanal makine ölçeği kümesinin hangi Kullanılabilirlik Bölgesine dağıtılaolacağını belirten **bölgeler** özelliğidir.
* İkinci değer, doğru olarak ayarlanılması gereken "singlePlacementGroup" özelliğidir.
* Üçüncü değer Service Fabric sanal makine ölçeği kümesi uzantısı "faultDomainOverride" özelliğidir. Bu özelliğin değeri, bu sanal makine ölçeği kümesinin yerleştirileceği bölge ve bölgeyi içermelidir. Örnek: "faultDomainOverride": "eastus/az1" Azure Hizmet Kumaş kümelerinin bölgeler arası desteği olmadığından, tüm sanal makine ölçeği kümesi kaynakları aynı bölgeye yerleştirilmelidir.

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

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Hizmet Kumaş Kümesi kaynağında birden çok birincil Düğüm Türünü etkinleştirme
Küme kaynağında bir veya daha fazla düğüm türünü birincil olarak ayarlamak için "isPrimary" özelliğini "true" olarak ayarlayın. Kullanılabilirlik Bölgeleri arasında bir Hizmet Kumaşı kümesi dağıtırken, farklı bölgelerde üç düğüm türüolmalıdır.

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

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Temel SKU Yük Dengeleyicisi ve Temel SKU IP kullanarak kümeden Kullanılabilirlik Bölgelerini kullanmaya geçiş
Temel bir SKU ile yük dengeleyicisi ve IP kullanan bir kümeyi geçirmek için, öncelikle standart SKU'yu kullanarak tamamen yeni bir Yük Dengeleyicisi ve IP kaynağı oluşturmanız gerekir. Bu kaynakları yerinde güncelleştirmek mümkün değildir.

Yeni LB ve IP, kullanmak istediğiniz yeni çapraz Kullanılabilirlik Bölgesi düğüm türlerinde başvurulmalıdır. Yukarıdaki örnekte, 1,2 ve 3 bölgelerine üç yeni sanal makine ölçeği kümesi kaynağı eklendi. Bu sanal makine ölçeği, yeni oluşturulan LB ve IP'ye başvuru rağvetler ve Hizmet Kumaş Küme Kaynağı'nda birincil düğüm türleri olarak işaretlenir.

Başlamak için, varolan Kaynak Yöneticisi şablonunuza yeni kaynaklar eklemeniz gerekir. Bu kaynaklar şunlardır:
* Standart SKU kullanan bir Genel IP Kaynağı.
* Standart SKU kullanan bir Yük Dengeleyici Kaynağı.
* Sanal makine ölçek kümelerinizi dağıttığınız alt ağ tarafından başvurulan bir NSG.
* Birincil olarak işaretlenmiş üç düğüm türü.
    * Her düğüm türü, farklı bölgelerde bulunan kendi sanal makine ölçeği kümesine eşlenmelidir.
    * Her sanal makine ölçek kümesien az beş düğüm (Gümüş Dayanıklılık) olmalıdır.

Bu kaynakların bir örneği örnek [şablonda](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure)bulunabilir.

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Kaynaklar dağıtımı tamamlandıktan sonra, özgün kümeden birincil düğüm türündeki düğümleri devre dışı kalmaya başlayabilirsiniz. Düğümler devre dışı bırakıldıkça, sistem hizmetleri yukarıdaki adımda dağıtılan yeni birincil düğüm türüne taşınır.

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

Düğümlerin tümü devre dışı bırakıldıktan sonra, sistem hizmetleri bölgelere yayılan birincil düğüm türünde çalışacaktır. Daha sonra devre dışı bırakılan düğümleri kümeden kaldırabilirsiniz. Düğümler kaldırıldıktan sonra, orijinal IP, Yük Dengeleyici ve sanal makine ölçeği ayar kaynaklarını kaldırabilirsiniz.

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

Daha sonra, dağıtmış olduğunuz Kaynak Yöneticisi şablonundan bu kaynaklara yapılan başvuruları kaldırmanız gerekir.

Son adım, DNS adının ve Genel IP'nin güncelleştirilmesini içerir.

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
