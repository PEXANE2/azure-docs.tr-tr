---
title: Azure Service Fabric birincil düğüm türünü büyütme
description: Düğüm türü ekleyerek bir Service Fabric kümesini ölçeklendirmeyi öğrenin.
ms.topic: article
ms.date: 08/06/2020
ms.author: pepogors
ms.openlocfilehash: 5cabe7e377c29812252074336d7c5e9c9d3ba259
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031990"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Service Fabric kümesi birincil düğüm türünün ölçeğini artırma
Bu makalede, kümeye ek bir düğüm türü ekleyerek bir Service Fabric kümesi birincil düğüm türünün nasıl ölçeklenebileceğinizi açıklamaktadır. Service Fabric küme, mikro hizmetlerinizin dağıtıldığı ve yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bir kümenin parçası olan makine veya VM, düğüm olarak adlandırılır. Sanal Makine Ölçek Kümeleri, bir sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır](service-fabric-cluster-nodetypes.md). Her düğüm türü ayrıca yönetilebilir.

Aşağıdaki öğreticideki örnek şablonlar şurada bulunabilir: [Service Fabric birincil düğüm türü ölçeklendirme örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Primary-NodeType-Scaling-Sample)

> [!WARNING]
> Küme durumu sağlıksız ise, birincil düğüm türü ölçeği artırma yordamını denemeyin, çünkü bu durum yalnızca kümenin daha fazla kararlı hale gelmesine sebep olur.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="process-to-upgrade-the-size-and-operating-system-of-the-primary-node-type"></a>Birincil düğüm türünün boyutunu ve işletim sistemini yükseltmek için işlem
Aşağıda, birincil düğüm türü VM 'lerinin VM boyutunu ve işletim sistemini güncelleştirme işlemi verilmiştir.  Yükseltmeden sonra birincil düğüm türü VM 'Ler boyut standardı D4_V2 ve kapsayıcılarla Windows Server 2019 Datacenter çalıştırıyor.

> [!WARNING]
> Bir üretim kümesinde bu yordamı denemeden önce, örnek şablonları araştırmayı ve işlemi bir test kümesine karşı doğrulamanızı öneririz. Küme kısa bir süre için de kullanılamayabilir. 

### <a name="deploy-the-initial-service-fabric-cluster"></a>İlk Service Fabric kümesini dağıtma 
Örnekle birlikte izlemek isterseniz, ilk kümeyi tek bir birincil düğüm türü ve tek bir ölçek kümesi [Service Fabric-Ilk küme](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-1.json)ile dağıtın. Zaten dağıtılmış olan bir Service Fabric kümeniz varsa, bu adımı atlayabilirsiniz. 

1. Azure hesabınızda oturum açın. 
```powershell
# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId "<your subscription ID>"
```
2. Yeni bir kaynak grubu oluşturma. 
```powershell
# create a resource group for your cluster deployment
$resourceGroupName = "myResourceGroup"
$location = "WestUS"

New-AzResourceGroup `
    -Name $resourceGroupName
    -Location $location
```
3. Şablon dosyalarındaki parametre değerlerini girin. 
4. Kümeyi adım 2 ' de oluşturulan kaynak grubuna dağıtın. 
```powershell
# deploy the template files to the resource group created above
$templateFilePath = "C:\AzureDeploy-1.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

### <a name="add-a-new-primary-node-type-to-the-cluster"></a>Kümeye yeni bir birincil düğüm türü ekleyin
> [!Note]
> Aşağıdaki adımlarda oluşturulan kaynaklar, ölçeklendirme işlemi tamamlandıktan sonra kümenizde yeni birincil düğüm türü olacaktır. İlk alt ağ, genel IP, Load Balancer, sanal makine ölçek kümesi ve düğüm türünden benzersiz olan adlar kullandığınızdan emin olun. 

Aşağıdaki adımların tümünü içeren bir şablon bulabilirsiniz: [Service Fabric-yeni düğüm türü küme](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-2.json). Aşağıdaki adımlarda, yeni kaynaklardaki değişiklikleri vurgulayan kısmi kaynak parçacıkları bulunur.  

1. Var olan sanal ağınızda yeni bir alt ağ oluşturun.
```json
{
    "name": "[variables('subnet1Name')]",
    "properties": {
        "addressPrefix": "[variables('subnet1Prefix')]"
    }
}
```
2. Benzersiz bir domainNameLabel ile yeni bir genel IP kaynağı oluşturun. 
```json
{
    "apiVersion": "[variables('publicIPApiVersion')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))]",
    "location": "[variables('computeLocation')]",
    "properties": {
    "dnsSettings": {
        "domainNameLabel": "[concat(variables('dnsName'),'-','nt2')]"
    },
    "publicIPAllocationMethod": "Dynamic"
    },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
    }
}
```
3. Yukarıda oluşturulan genel IP 'ye bağlı yeni bir Load Balancer kaynağı oluşturun. 
```json
"dependsOn": [
    "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType1Name')))]"
]
```
4. Yeni VM SKU 'SU ve ölçeğini genişletmek istediğiniz işletim sistemi SKU 'su kullanan yeni bir sanal makine ölçek kümesi oluşturun. 

Düğüm türü başvurusu 
```json
"nodeTypeRef": "[variables('vmNodeType1Name')]"
```

VM SKU
```json
"sku": {
    "name": "[parameters('vmNodeType1Size')]",
    "capacity": "[parameters('nt1InstanceCount')]",
    "tier": "Standard"
}
```

IŞLETIM SISTEMI SKU 'SU 
```json
"imageReference": {
    "publisher": "[parameters('vmImagePublisher1')]",
    "offer": "[parameters('vmImageOffer1')]",
    "sku": "[parameters('vmImageSku1')]",
    "version": "[parameters('vmImageVersion1')]"
}
```
5. Kümeye yeni bir düğüm türü ekleyin, bu, yukarıda oluşturulan sanal makine ölçek kümesine başvurur. Bu düğüm türündeki **ısprımary** özelliği true olarak ayarlanmalıdır. 
```json
"name": "[variables('vmNodeType1Name')]",
"applicationPorts": {
    "endPort": "[variables('nt0applicationEndPort')]",
    "startPort": "[variables('nt0applicationStartPort')]"
},
"clientConnectionEndpointPort": "[variables('nt0fabricTcpGatewayPort')]",
"durabilityLevel": "Bronze",
"ephemeralPorts": {
    "endPort": "[variables('nt0ephemeralEndPort')]",
    "startPort": "[variables('nt0ephemeralStartPort')]"
},
"httpGatewayEndpointPort": "[variables('nt0fabricHttpGatewayPort')]",
"isPrimary": true,
"reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
"vmInstanceCount": "[parameters('nt1InstanceCount')]"
```
6. Güncelleştirilmiş ARM şablonunu dağıtın. 
```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-2.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

Dağıtım tamamlandığında Service Fabric kümesi artık iki düğüm türüne sahip olur. 

### <a name="remove-the-existing-node-type"></a>Var olan düğüm türünü kaldır 
Kaynakların dağıtımı tamamlandıktan sonra, özgün birincil düğüm türündeki düğümleri devre dışı bırakmayı deneyebilirsiniz. Düğümler devre dışı bırakıldığı için, sistem hizmetleri Yukarıdaki adımda dağıtılan yeni birincil düğüm türüne geçirilir.

1. Service Fabric küme kaynağındaki birincil düğüm türü özelliğini false olarak ayarlayın. 
```json
{
    "name": "[variables('vmNodeType0Name')]",
    "applicationPorts": {
        "endPort": "[variables('nt0applicationEndPort')]",
        "startPort": "[variables('nt0applicationStartPort')]"
    },
    "clientConnectionEndpointPort": "[variables('nt0fabricTcpGatewayPort')]",
    "durabilityLevel": "Bronze",
    "ephemeralPorts": {
        "endPort": "[variables('nt0ephemeralEndPort')]",
        "startPort": "[variables('nt0ephemeralStartPort')]"
    },
    "httpGatewayEndpointPort": "[variables('nt0fabricHttpGatewayPort')]",
    "isPrimary": false,
    "reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
    "vmInstanceCount": "[parameters('nt0InstanceCount')]"
}
```
2. Şablonu orijinal düğüm türündeki güncelleştirilmiş IsPrimary özelliği ile dağıtın. Birincil bayrağın asıl düğüm türünde false olarak ayarlanmış bir şablon bulabilirsiniz: [Service Fabric-birincil düğüm türü false](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-3.json).

```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-3.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

3. Düğüm türü 0 içindeki düğümleri devre dışı bırakın. 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterConnectionEndpoint `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"


$nodeType = "nt1vm" # specify the name of node type
$nodes = Get-ServiceFabricNode 

Write-Host "Disabling nodes..."
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Disable-ServiceFabricNode -Intent RemoveNode -NodeName $node.NodeName -Force
  }
}
```
* Bronz dayanıklılık için tüm düğümlerin devre dışı duruma gelmesini bekleyin.
* Gümüş ve altın dayanıklılık için bazı düğümler devre dışı bırakılacak ve geri kalan durum devre dışı olacaktır. Devre dışı bırakma durumundaki düğümlerin Ayrıntılar sekmesini kontrol edin. Bunlar, altyapı hizmeti bölümleri için çekirdek sağlamaya açık durumdaysa devam etmek güvenli hale gelir.

> [!Note]
> Bu adımın tamamlanması biraz zaman alabilir. 

4. Düğüm türü 0 üzerindeki verileri durdur. 
```powershell
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
  }
}
```
5. Özgün sanal makine ölçek kümesindeki düğümleri serbest bırakma 
```powershell
$scaleSetName="nt1vm"
$scaleSetResourceType="Microsoft.Compute/virtualMachineScaleSets"

Remove-AzResource -ResourceName $scaleSetName -ResourceType $scaleSetResourceType -ResourceGroupName $resourceGroupName -Force
```
> [!Note]
> Standart SKU genel IP ve standart SKU yük dengeleyicisi kullanıyorsanız 6. ve 7. adım isteğe bağlıdır. Bu durumda, aynı yük dengeleyici altında birden fazla sanal makine ölçek kümesi/düğüm türüne sahip olabilirsiniz. 

6. Artık özgün IP 'yi ve Load Balancer kaynakları silebilirsiniz. Bu adımda DNS adını da güncelleşolursunuz. 

```powershell
$lbname="LB-cluster-name-nt1vm"
$lbResourceType="Microsoft.Network/loadBalancers"
$ipResourceType="Microsoft.Network/publicIPAddresses"
$oldPublicIpName="PublicIP-LB-FE-nt1vm"
$newPublicIpName="PublicIP-LB-FE-nt2vm"

$oldprimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $resourceGroupName
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzResource -ResourceName $lbname -ResourceType $lbResourceType -ResourceGroupName $resourceGroupName -Force
Remove-AzResource -ResourceName $oldPublicIpName -ResourceType $ipResourceType -ResourceGroupName $resourceGroupName -Force

$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $resourceGroupName
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
``` 

7. Kümedeki yönetim uç noktasını yeni IP 'ye başvuracak şekilde güncelleştirin. 
```json
  "managementEndpoint": "[concat('https://',reference(concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))).dnsSettings.fqdn,':',variables('nt0fabricHttpGatewayPort'))]",
```
8. Düğüm durumu 0 olan düğüm türünden kaldır.
```powershell
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
  }
}
```
9. ARM şablonundaki Service Fabric kaynağından özgün düğüm türü başvurusunu kaldırın. 
```json
"name": "[variables('vmNodeType0Name')]",
"applicationPorts": {
    "endPort": "[variables('nt0applicationEndPort')]",
    "startPort": "[variables('nt0applicationStartPort')]"
},
"clientConnectionEndpointPort": "[variables('nt0fabricTcpGatewayPort')]",
"durabilityLevel": "Bronze",
"ephemeralPorts": {
    "endPort": "[variables('nt0ephemeralEndPort')]",
    "startPort": "[variables('nt0ephemeralStartPort')]"
},
"httpGatewayEndpointPort": "[variables('nt0fabricHttpGatewayPort')]",
"isPrimary": true,
"reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
"vmInstanceCount": "[parameters('nt0InstanceCount')]"
```
Yalnızca gümüş ve daha yüksek dayanıklılık kümelerinde, şablondaki küme kaynağını güncelleştirin ve küme kaynak özellikleri altına aşağıda verilen şekilde applicationDeltaHealthPolicies ekleyerek yapı:/sistem uygulaması durumu ' nu yok saymaya yönelik sistem durumu ilkelerini yapılandırın. Aşağıdaki ilke var olan hataları yoksayacak, ancak yeni sistem durumu hatalarına izin vermez.
```json
"upgradeDescription":  
{ 
 "forceRestart": false, 
 "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807", 
 "healthCheckWaitDuration": "00:05:00", 
 "healthCheckStableDuration": "00:05:00", 
 "healthCheckRetryTimeout": "00:45:00", 
 "upgradeTimeout": "12:00:00", 
 "upgradeDomainTimeout": "02:00:00", 
 "healthPolicy": { 
   "maxPercentUnhealthyNodes": 100, 
   "maxPercentUnhealthyApplications": 100 
 }, 
 "deltaHealthPolicy":  
 { 
   "maxPercentDeltaUnhealthyNodes": 0, 
   "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0, 
   "maxPercentDeltaUnhealthyApplications": 0, 
   "applicationDeltaHealthPolicies":  
   { 
       "fabric:/System":  
       { 
           "defaultServiceTypeDeltaHealthPolicy":  
           { 
                   "maxPercentDeltaUnhealthyServices": 0 
           } 
       } 
   } 
 } 
}
```
10. ARM şablonundan orijinal düğüm türüyle ilişkili diğer tüm kaynakları kaldırın. Bu özgün kaynakların tümü kaldırılmış olan bir şablon için [Service Fabric-yeni düğüm türü kümesi](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-4.json) ' ne bakın.

11. Değiştirilen Azure Resource Manager şablonunu dağıtın. * * Bu adım genellikle iki saate kadar sürer. Bu yükseltme ayarları InfrastructureService olarak değiştirecek, bu nedenle bir düğümün yeniden başlatılması gerekiyor. Bu durumda forceRestart yoksayıldı. Yükseltilebilir Dereperepsetchecktimeout parametresi, Service Fabric bir bölümün güvenli bir durumda olmasını bekleyeceği en uzun süreyi belirtir, zaten güvenli bir durumda değildir. Güvenlik denetimleri bir düğümdeki tüm bölümler için başarılı olduktan sonra, bu düğümdeki yükseltmeye devam eder Service Fabric. UpgradeTimeout parametresi için değer 6 saate indirgenecek, ancak maxhayvan güvenliği 12 saat kullanılmalıdır.
Ardından, portalda Service Fabric kaynağının, ' ın hazırlandığını doğrulayın. 

```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-4.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

Kümenin birincil düğüm türü şimdi yükseltildi. Dağıtılan tüm uygulamaların düzgün çalıştığını ve küme durumunun tamam olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Kümeye düğüm türü eklemeyi](virtual-machine-scale-set-scale-node-type-scale-out.md) öğrenin
* [Uygulama ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Bir Azure kümesini içinde veya dışarı ölçeklendirin](service-fabric-tutorial-scale-cluster.md).
* Akıcı Azure işlem SDK 'sını kullanarak [bir Azure kümesini programlı bir şekilde ölçeklendirin](service-fabric-cluster-programmatic-scaling.md) .
* [Tek başına kümeyi içinde veya dışarı ölçeklendirin](service-fabric-cluster-windows-server-add-remove-nodes.md).
