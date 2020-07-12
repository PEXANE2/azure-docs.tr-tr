---
title: Azure Service Fabric düğüm türünü büyütme
description: Bir sanal makine ölçek kümesi ekleyerek bir Service Fabric kümesini ölçeklendirmeyi öğrenin.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: a42e33fa87b6cf7966368481ef6d3920511919e3
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260453"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Service Fabric kümesi birincil düğüm türünün ölçeğini artırma
Bu makalede, sanal makine kaynaklarını artırarak Service Fabric kümesi birincil düğüm türünün nasıl ölçeklenebileceğinizi açıklamaktadır. Service Fabric küme, mikro hizmetlerinizin dağıtıldığı ve yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bir kümenin parçası olan makine veya VM, düğüm olarak adlandırılır. Sanal Makine Ölçek Kümeleri, bir sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır](service-fabric-cluster-nodetypes.md). Her düğüm türü ayrıca yönetilebilir. Service Fabric kümesi oluşturduktan sonra, küme düğümü türünü dikey olarak ölçeklendirebilir (düğümlerin kaynaklarını değiştirebilir) veya düğüm türü VM 'lerinin işletim sistemini yükseltebilirsiniz.  Küme üzerinde iş yükleri çalışırken bile kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklenirken uygulamalarınız da otomatik olarak ölçeklendirilir.

> [!WARNING]
> Küme durumu sağlıksız ise, birincil düğüm türü ölçeği artırma yordamını denemeyin, çünkü bu durum yalnızca kümenin daha fazla kararlı hale gelmesine sebep olur.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="process-to-upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Birincil düğüm türü VM 'lerinin boyutunu ve işletim sistemini yükseltme işlemi
Birincil düğüm türü VM 'lerinin sanal makine boyutunu ve işletim sistemini güncelleştirme süreci aşağıda verilmiştir.  Yükseltmeden sonra birincil düğüm türü VM 'Ler boyut standardı D4_V2 ve kapsayıcılarla Windows Server 2016 Datacenter çalıştırıyor.

> [!WARNING]
> Bir üretim kümesinde bu yordamı denemeden önce, örnek şablonları araştırmayı ve işlemi bir test kümesine karşı doğrulamanızı öneririz. Küme bir süre için de kullanılamaz. Paralel olarak aynı NodeType olarak belirtilen birden fazla VMSS üzerinde değişiklik yapamazsınız; Her NodeType VMSS 'de yapılan değişiklikleri ayrı ayrı uygulamak için ayrılmış dağıtım işlemleri gerçekleştirmeniz gerekir.

1. Bu örnek [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) ve [parametre](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) dosyalarını kullanarak, ilk kümeyi iki düğüm türü ve iki ölçek kümesi (düğüm başına bir ölçek kümesi) dağıtın.  Her iki ölçek kümesi de boyut standardı D2_V2 ve Windows Server 2012 R2 Datacenter çalıştırıyor.  Kümenin temel yükseltme işleminin tamamlanmasını bekleyin.   
2. İsteğe bağlı-kümeye durum bilgisi olan bir örnek dağıtın.
3. Birincil düğüm türü VM 'Leri yükseltmeye karar verdikten sonra, bu örnek [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) ve [parametre](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) dosyalarını kullanarak birincil düğüm türüne yeni bir ölçek kümesi ekleyin, böylece birincil düğüm türünün artık iki ölçek kümesi vardır.  Sistem Hizmetleri ve kullanıcı uygulamaları iki farklı ölçek kümesindeki VM 'Ler arasında geçiş yapabilir.  Yeni ölçek kümesi VM 'Leri boyut standardı D4_V2 ve kapsayıcılarla Windows Server 2016 Datacenter çalıştırır.  Yeni ölçek kümesiyle yeni bir yük dengeleyici ve genel IP adresi de eklenir.  
    Şablonda yeni ölçek kümesini bulmak için, *vmNodeType2Name* parametresi tarafından adlandırılan "Microsoft. COMPUTE/virtualMachineScaleSets" kaynağını arayın.  Yeni ölçek kümesi, >virtualMachineProfile->extensionProfile->uzantıları->özellikleri->ayarlar->nodeTypeRef ayarı kullanılarak birincil düğüm türüne eklenir.
4. Küme durumunu denetleyin ve tüm düğümlerin sağlıklı olduğunu doğrulayın.
5. Düğüm kaldırma amacına sahip birincil düğüm türünün eski ölçek kümesindeki düğümleri devre dışı bırakın. Tümünü tek seferde devre dışı bırakabilirsiniz ve işlemler sıraya alınır. Tüm düğümler devre dışı bırakılıncaya kadar bekleyin, bu işlem biraz zaman alabilir.  Düğüm türündeki eski düğümler devre dışı bırakıldığı için, sistem hizmetleri ve çekirdek düğümleri, birincil düğüm türünde yeni ölçek kümesinin VM 'lerine geçirilir.
6. Eski ölçek kümesini birincil düğüm türünden kaldırın. (5. adımda düğümler devre dışı bırakıldıktan sonra, Azure portal sanal makine ölçek kümesi dikey penceresinde, düğümleri eski düğüm türünden tek tek serbest bırakın.)
7. Eski ölçek kümesiyle ilişkili yük dengeleyiciyi kaldırın. Yeni genel IP adresi ve yük dengeleyici yeni ölçek kümesi için yapılandırıldığında küme kullanılamaz.  
8. Bir değişkende eski birincil düğüm türü ölçek kümesiyle ilişkili genel IP adresinin DNS ayarlarını depolayın ve bu genel IP adresini kaldırın.
9. Yeni birincil düğüm türü ölçek kümesiyle ilişkili genel IP adresinin DNS ayarlarını, silinen genel IP adresinin DNS ayarları ile değiştirin.  Kümeye artık yeniden ulaşılabilir.
10. Düğümlerin düğüm durumunu kümeden kaldırın.  Eski ölçek kümesinin dayanıklılık düzeyi gümüş veya altın ise, bu adım sistem tarafından otomatik olarak gerçekleştirilir.
11. Durum bilgisi olan uygulamayı önceki bir adımda dağıttıysanız, uygulamanın işlevsel olduğunu doğrulayın.

## <a name="set-up-the-test-cluster"></a>Test kümesini ayarlama

Bu öğretici için gereken iki dosya kümesini indirerek başlayın, önce [şablon]() ve [Parametreler]() ve sonra [şablondan]() ve [parametrelerden]()sonra.

Ardından, Azure hesabınızda oturum açın.

```powershell
# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId "<your subscription ID>"
```

Bu öğreticide, kendinden imzalı bir sertifika oluşturma senaryosunda adım adım gösterilmektedir. Azure Key Vault var olan bir sertifikayı kullanmak için aşağıdaki adımı atlayın ve bunun yerine [kümeyi dağıtmak için mevcut bir sertifikayı kullanma](./upgrade-managed-disks.md#use-an-existing-certificate-to-deploy-the-cluster)bölümündeki adımları yansıtın.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Kendinden imzalı bir sertifika oluşturma ve kümeyi dağıtma

İlk olarak, Service Fabric küme dağıtımı için gereken değişkenleri atayın. `resourceGroupName` `certSubjectName` `parameterFilePath` `templateFilePath` Belirli hesabınız ve ortamınız için,,, ve değerlerini ayarlayın:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$templateFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.parameters.json"
```

> [!NOTE]
> `certOutputFolder`Yeni bir Service Fabric kümesi dağıtmak için komutunu çalıştırmadan önce, konumun yerel makinenizde mevcut olduğundan emin olun.

Sonra, *Deploy-2NodeTypes-2ScaleSets.parameters.jsdosya üzerinde* açın ve değerlerini, `clusterName` `dnsName` PowerShell 'de ayarladığınız dinamik değerlere karşılık gelecek şekilde ayarlayın ve değişikliklerinizi kaydedin.

Sonra Service Fabric test kümesini dağıtın:

```powershell
# Deploy the initial test cluster
New-AzServiceFabricCluster `
    -ResourceGroupName $resourceGroupName `
    -CertificateOutputFolder $certOutputFolder `
    -CertificatePassword $certPassword `
    -CertificateSubjectName $certSubjectName `
    -TemplateFile $templateFilePath `
    -ParameterFile $parameterFilePath
```

Dağıtım tamamlandıktan sonra, yerel makinenizde *. pfx* dosyasını () bulun `$certPfx` ve sertifika deponuza içeri aktarın:

```powershell
cd c:\certificates
$certPfx = ".\sftestupgradegroup20200312121003.pfx"

Import-PfxCertificate `
     -FilePath $certPfx `
     -CertStoreLocation Cert:\CurrentUser\My `
     -Password (ConvertTo-SecureString Password!1 -AsPlainText -Force)
```

Bu işlem, yeni kümeye bağlanmak ve sistem durumunu denetlemek için kullanacağınız sertifika parmak izini döndürür.

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Yeni kümeye bağlanma ve sistem durumunu denetleme

Kümeye bağlanın ve tüm düğümlerinin sağlıklı olduğundan emin olun ( `clusterName` `thumb` kümeniz için ve değişkenlerini değiştirerek):

```powershell
# Connect to the cluster
$clusterName = "sftestupgrade.southcentralus.cloudapp.azure.com:19000"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"

Connect-ServiceFabricCluster `
    -ConnectionEndpoint $clusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My

# Check cluster health
Get-ServiceFabricClusterHealth
```

Yükseltme yordamına başlamaya hazırsınız.

## <a name="upgrade-the-primary-node-type-vms"></a>Birincil düğüm türü VM 'Leri yükseltme

Birincil düğüm türü VM 'Leri yükseltmeye karar verdikten sonra birincil düğüm türüne yeni bir ölçek kümesi ekleyerek birincil düğüm türünün artık iki ölçek kümesi vardır. Gerekli değişiklikleri göstermek için örnek bir [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) ve [parametre](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) dosyaları verilmiştir. Yeni ölçek kümesinin VM 'Leri boyut standardı D4_V2 ve kapsayıcılarla Windows Server 2016 Datacenter çalıştırır. Yeni ölçek kümesiyle yeni bir yük dengeleyici ve genel IP adresi de eklenir. 

Şablonda yeni ölçek kümesini bulmak için, vmNodeType2Name parametresi tarafından adlandırılan "Microsoft. COMPUTE/virtualMachineScaleSets" kaynağını arayın. Yeni ölçek kümesi, >virtualMachineProfile->extensionProfile->uzantıları->özellikleri->ayarlar->nodeTypeRef ayarı kullanılarak birincil düğüm türüne eklenir.

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma

`parameterFilePath`Ve `templateFilePath` gereken şekilde ayarlayın ve ardından aşağıdaki komutu çalıştırın:

```powershell
# Deploy the new scale set into the primary node type along with a new load balancer and public IP
$templateFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Dağıtım tamamlandığında, küme durumunu yeniden denetleyin ve tüm düğümlerin (orijinal ve yeni ölçek kümesinde) sağlıklı olduğundan emin olun.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-nodes-to-the-new-scale-set"></a>Düğümleri yeni ölçek kümesine geçirme

Artık orijinal ölçek kümesinin düğümlerini devre dışı bırakmaya başlamaya hazırsınız. Bu düğümler devre dışı bırakıldığı için, sistem hizmetleri ve çekirdek düğümleri, birincil düğüm türü olarak da işaretlendiğinden, yeni ölçek kümesinin VM 'lerine geçirilir.

Birincil olmayan düğüm türlerini ölçeklendirmek için, bu adımda hizmet yerleştirme kısıtlamasını yeni sanal makine ölçek kümesi/düğüm türünü içerecek şekilde değiştirir ve ardından eski sanal makine ölçek kümesi örnek sayısını sıfıra, tek seferde bir düğüm (düğüm kaldırma işleminin küme güvenilirliğini etkilememesini sağlamak için) azaltabilirsiniz.

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Çekirdek düğümlerin yeni ölçek kümesine geçişini ve orijinal ölçek kümesindeki düğümlerin ilerlemesini *devre dışı* *durumuna geçirmek için* Service Fabric Explorer kullanın.

> [!NOTE]
> Özgün ölçek kümesinin tüm düğümlerinde devre dışı bırakma işleminin tamamlanması biraz zaman alabilir. Veri tutarlılığını güvence altına almak için, tek seferde yalnızca bir çekirdek düğüm değiştirilebilir. Her çekirdek düğümü değişikliği bir küme güncelleştirmesi gerektirir; Bu nedenle, çekirdek düğümü değiştirmek için iki küme yükseltmesi gerekir (her biri düğüm ekleme ve kaldırma için bir tane). Bu örnek senaryoda beş tohum düğümünü yükseltmek, on küme yükseltmelerine neden olur.

## <a name="remove-the-original-scale-set"></a>Özgün ölçek kümesini kaldır

Devre dışı bırakma işlemi tamamlandıktan sonra ölçek kümesini kaldırın.

```powershell
# Remove the original scale set
$scaleSetName = "NTvm1"

Remove-AzVmss `
    -ResourceGroupName $resourceGroupName `
    -VMScaleSetName $scaleSetName `
    -Force

Write-Host "Removed scale set $scaleSetName"
```

Service Fabric Explorer, kaldırılan düğümler (ve dolayısıyla *küme sistem durumu*) artık *hata* durumunda görünür.

## <a name="remove-the-old-load-balancer-and-update-dns-settings"></a>Eski yük dengeleyiciyi kaldırın ve DNS ayarlarını güncelleştirin

Artık, yük dengeleyiciden ve eski genel IP 'den başlayarak eski birincil düğüm türüyle ilgili kaynakları kaldırabiliriz. 

```powershell
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
```

Ardından, yeni genel IP 'nin DNS ayarlarını, eski birincil düğüm türünün genel IP 'sinden ayarları yansıtacak şekilde güncelleştiririz.

```powershell
# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
```

Daha fazla bilgi için, küme durumunu kontrol edin

```powershell
# Check the cluster health
Get-ServiceFabricClusterHealth
```

Son olarak, ilgili düğümlerin her biri için düğüm durumunu kaldırın. Eski ölçek kümesinin dayanıklılık düzeyi gümüş veya altın ise, bu otomatik olarak gerçekleşir.

```powershell
# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

Kümenin birincil düğüm türü şimdi yükseltildi. Dağıtılan tüm uygulamaların düzgün çalıştığını ve küme durumunun tamam olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Kümeye düğüm türü eklemeyi](virtual-machine-scale-set-scale-node-type-scale-out.md) öğrenin
* [Uygulama ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Bir Azure kümesini içinde veya dışarı ölçeklendirin](service-fabric-tutorial-scale-cluster.md).
* Akıcı Azure işlem SDK 'sını kullanarak [bir Azure kümesini programlı bir şekilde ölçeklendirin](service-fabric-cluster-programmatic-scaling.md) .
* [Tek başına kümeyi içinde veya dışarı ölçeklendirin](service-fabric-cluster-windows-server-add-remove-nodes.md).
