---
title: Azure Hizmet Kumaş düğümü türünü ölçeklendir
description: Sanal Makine Ölçeği Kümesi ekleyerek Service Fabric kümesini nasıl ölçeklendireceklerini öğrenin.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: 33d535cb093eeb95e0ce95bdd5722bfd21150a40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464233"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Service Fabric kümesi birincil düğüm türünün ölçeğini artırma
Bu makalede, sanal makine kaynaklarını artırarak hizmet kumaşı küme birincil düğüm türünü nasıl ölçeklendirilir. Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine ye veya VM'ye düğüm denir. Sanal makine ölçek kümeleri, sanal makine koleksiyonunu küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure bilgi işlem kaynağıdır. Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır.](service-fabric-cluster-nodetypes.md) Her düğüm türü daha sonra ayrı ayrı yönetilebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, küme düğümü türünü dikey olarak ölçeklendirebilir (düğümlerin kaynaklarını değiştirebilir) veya düğüm türü VM'lerin işletim sistemini yükseltebilirsiniz.  Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

> [!WARNING]
> Küme durumu sağlıksızsa, birincil düğüm tipi VM SKU'yu değiştirmeye başlamayın. Küme durumu sağlıksızsa, VM SKU'yu değiştirmeye çalışırsanız, kümenin yalnızca daha da kararsızolduğunuz.
>
> [Silver dayanıklılıkta veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster)bir ölçekte çalışmadığı sürece ölçek kümesi/düğümü türünün VM SKU'yu değiştirmemenizi öneririz. VM SKU Boyutunu değiştirmek, yerinde veri yıkıcı bir altyapı işlemidir. Bu değişikliği geciktirme veya izleme yeteneği olmadan, işlemin devlet tarafından sunulan hizmetler için veri kaybına veya devlet siz iş yüklerinde bile öngörülemeyen diğer operasyonel sorunlara neden olması mümkündür. Bu, devlet hizmeti kumaş sistem hizmetlerini çalıştıran birincil düğüm türünüz veya durum lu uygulama çalışma yüklerinizi çalıştıran herhangi bir düğüm türü anlamına gelir.
>


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Birincil düğüm türü VM'lerin boyutunu ve işletim sistemini yükseltme
Burada birincil düğüm türü VM'lerin VM boyutunu ve işletim sistemini güncelleştirme işlemi ve işlemdir.  Yükseltmeden sonra, birincil düğüm türü VM'ler boyut Standart D4_V2 ve Windows Server 2016 Datacenter'ı Kapsayıcılarla çalıştırıyor.

> [!WARNING]
> Bir üretim kümesinde bu yordamı denemeden önce, örnek şablonları incelemenizi ve işlemi bir test kümesine karşı doğrulamanızı öneririz. Küme bir süre için de kullanılamaz. Aynı NodeType olarak bildirilen birden fazla VMSS'de paralel olarak değişiklik yapamazsınız; her NodeType VMSS'e tek tek değişiklik uygulamak için ayrı dağıtım işlemleri gerçekleştirmeniz gerekir.

1. Bu örnek [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) ve [parametre](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) dosyalarını kullanarak iki düğüm türü ve iki ölçek kümesi (düğüm türü başına bir ölçek kümesi) ile ilk kümeyi dağıtın.  Her iki ölçek kümeleri boyutu Standart D2_V2 ve Windows Server 2012 R2 Datacenter çalıştıran vardır.  Kümenin taban çizgisi yükseltmesini tamamlamasını bekleyin.   
2. İsteğe bağlı olarak, kümeye durumlu bir örnek dağıtın.
3. Birincil düğüm türü VM'leri yükseltmeye karar verdikten sonra, birincil düğüm türünü kullanarak bu örnek [şablonu](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) ve [parametreleri](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) kullanarak yeni bir ölçek kümesi ekleyin, böylece birincil düğüm türü artık iki ölçek kümesine sahip olur.  Sistem hizmetleri ve kullanıcı uygulamaları iki farklı ölçek kümesinde VM'ler arasında geçiş yapabiliyor.  Yeni ölçek kümesi VM'ler boyut Standart D4_V2 ve Windows Server 2016 Datacenter'ı Kapsayıcılarla çalıştırın.  Yeni ölçek kümesiile yeni bir yük dengeleyicisi ve genel IP adresi de eklenir.  
    Şablonda ayarlanan yeni ölçeği bulmak için *vmNodeType2Name* parametresi tarafından adlandırılan "Microsoft.Compute/virtualMachineScaleSets" kaynağını arayın.  Yeni ölçek kümesi özellikleri >virtualMachineProfile->extensionProfil->uzantıları->özellikleri->ayarları->düğümTypeRef ayarı kullanılarak birincil düğüm türüne eklenir.
4. Küme durumunu denetleyin ve tüm düğümlerin sağlıklı olduğunu doğrulayın.
5. Düğümü kaldırmak amacıyla birincil düğüm türünün eski ölçek kümesindeki düğümleri devre dışı bırak. Hepsini aynı anda devre dışı atabilirsiniz ve işlemler sıraya alınır. Tüm düğümler devre dışı bırakılana kadar bekleyin, bu da biraz zaman alabilir.  Düğüm türündeki eski düğümler devre dışı bırakıldığından, sistem hizmetleri ve tohum düğümleri birincil düğüm türünde ayarlanan yeni ölçeğin VM'lerine taşınır.
6. Birincil düğüm türünden eski ölçek kümesini kaldırın.
7. Eski ölçek kümesiyle ilişkili yük dengeleyicisini çıkarın. Küme, yeni genel IP adresi ve yük dengeleyicisi yeni ölçek kümesi için yapılandırılırken kullanılamaz.  
8. Bir değişkende ayarlanan eski birincil düğüm türü ölçeğiyle ilişkili genel IP adresinin DNS ayarlarını depolayın ve bu ortak IP adresini kaldırın.
9. Silinen genel IP adresinin DNS ayarlarıyla yeni birincil düğüm türü ölçeği kümesiyle ilişkili genel IP adresinin DNS ayarlarını değiştirin.  Kümeye artık yeniden ulaşılabilir.
10. Düğüm düğüm durumunu kümeden kaldırın.  Eski ölçek kümesinin dayanıklılık düzeyi gümüş veya altın ise, bu adım sistem tarafından otomatik olarak yapılır.
11. Durum lattığı uygulamayı önceki bir adımda dağıttıysanız, uygulamanın işlevsel olduğunu doğrulayın.

```powershell
# Variables.
$groupname = "sfupgradetestgroup"
$clusterloc="southcentralus"  
$subscriptionID="<your subscription ID>"

# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $subscriptionID 

# Create a new resource group for your deployment and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Deploy the two node type cluster.
New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.json" -Verbose

# Connect to the cluster and check the cluster health.
$ClusterName= "sfupgradetest.southcentralus.cloudapp.azure.com:19000"
$thumb="F361720F4BD5449F6F083DDE99DC51A86985B25B"

Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Get-ServiceFabricClusterHealth

# Deploy a new scale set into the primary node type.  Create a new load balancer and public IP address for the new scale set.
New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.json" -Verbose

# Check the cluster health again. All 15 nodes should be healthy.
Get-ServiceFabricClusterHealth

# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}

Write-Host "Checking node status..."
foreach($name in $nodeNames){
 
    $state = Get-ServiceFabricNode -NodeName $name 

    $loopTimeout = 50

    do{
        Start-Sleep 5
        $loopTimeout -= 1
        $state = Get-ServiceFabricNode -NodeName $name
        Write-Host "$name state: " $state.NodeDeactivationInfo.Status
    }

    while (($state.NodeDeactivationInfo.Status -ne "Completed") -and ($loopTimeout -ne 0))
    

    if ($state.NodeStatus -ne [System.Fabric.Query.NodeStatus]::Disabled)
    {
        Write-Error "$name node deactivation failed with state" $state.NodeStatus
        exit
    }
}

# Remove the scale set
$scaleSetName="NTvm1"
Remove-AzVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force
Write-Host "Removed scale set $scaleSetName"

$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP

# Check the cluster health
Get-ServiceFabricClusterHealth

# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

## <a name="next-steps"></a>Sonraki adımlar
* [Kümeye düğüm türü eklemeyi](virtual-machine-scale-set-scale-node-type-scale-out.md) öğrenin
* Uygulama [ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Azure kümesini içinde veya dışında ölçeklendirin.](service-fabric-tutorial-scale-cluster.md)
* Akıcı Azure işlem SDK'sını kullanarak [bir Azure kümesini programlı olarak](service-fabric-cluster-programmatic-scaling.md) ölçeklendirin.
* [Bağımsız bir kümeyi içinde veya dışında ölçeklendirin.](service-fabric-cluster-windows-server-add-remove-nodes.md)

