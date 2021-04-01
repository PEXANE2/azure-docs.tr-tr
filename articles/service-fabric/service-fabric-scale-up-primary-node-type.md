---
title: Azure Service Fabric birincil düğüm türünü büyütme
description: Yeni bir düğüm türü ekleyerek ve öncekini kaldırarak Service Fabric kümenizi dikey olarak ölçeklendirin.
ms.date: 12/11/2020
ms.author: pepogors
ms.topic: how-to
ms.openlocfilehash: 325ece761481077171a670c52e9d98071237601a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98251190"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Service Fabric kümesi birincil düğüm türünün ölçeğini artırma

Bu makalede, en az kapalı kalma süresine sahip Service Fabric kümesi birincil düğüm türünün nasıl ölçeklenebileceğinizi açıklamaktadır. Service Fabric küme düğümü türünü yükseltmek için genel strateji şu şekilde yapılır:

1. Service Fabric kümenize, yükseltilen (veya değiştirdiğiniz) sanal makine ölçek kümesi SKU 'SU ve yapılandırmanızla desteklenen yeni bir düğüm türü ekleyin. Bu adım Ayrıca ölçek kümesi için yeni bir yük dengeleyici, alt ağ ve genel IP ayarlamayı da içerir.

1. Hem özgün hem de yükseltilen ölçek kümeleri yan yana çalışırken, sistem hizmetlerinin (veya durum bilgisi olan hizmetlerin çoğaltmalarının) yeni ölçek kümesine geçişini sağlamak için özgün düğüm örneklerini tek seferde devre dışı bırakın.

1. Kümenin ve yeni düğümlerin sağlıklı olduğunu doğrulayın, sonra silinen düğümlerin orijinal ölçek kümesini (ve ilgili kaynakları) ve düğüm durumunu kaldırın.

Aşağıda [, beş](service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster)düğümlü tek bir ölçek kümesiyle desteklenen bir örnek kümenin birincil düğüm türü VM 'lerinin VM boyutunu ve işletim sistemini güncelleştirme işleminde size kılavuzluk edilecek. Birincil düğüm türünü yükseltiyoruz:

- VM boyutundan *Standard_D2_V2* *Standart D4_V2* ve
- Kapsayıcılarla *Windows server 2019 Datacenter*'a KAPSAYıCı içeren VM Işletim sistemi *Windows Server 2016 Datacenter* 'dan.

> [!WARNING]
> Bir üretim kümesinde bu yordamı denemeden önce, örnek şablonları araştırmayı ve işlemi bir test kümesine karşı doğrulamanızı öneririz. Küme kısa bir süre için de kullanılamayabilir.
>
> Küme durumu sağlıksız ise, birincil düğüm türü ölçeği artırma yordamını denemeyin, çünkü bu durum yalnızca kümenin daha fazla kararlı hale gelmesine sebep olur.

Bu örnek yükseltme senaryosunu tamamlamaya yönelik olarak kullanacağınız adım adım Azure dağıtım şablonları aşağıda verilmiştir: https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade

## <a name="set-up-the-test-cluster"></a>Test kümesini ayarlama

İlk Service Fabric test kümesini ayarlayalim. İlk olarak, bu senaryoyu gerçekleştirmek için kullanacağımız Azure Resource Manager örnek şablonları [indirin](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade) .

Ardından, Azure hesabınızda oturum açın.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

Sonra, [*parameters.jsdosya üzerinde*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/parameters.json) açın ve değerini `clusterName` benzersiz bir şekilde (Azure 'da) güncelleştirin.

Aşağıdaki komutlar, otomatik olarak imzalanan yeni bir sertifika oluşturma ve test kümesini dağıtma konusunda size rehberlik edecektir. Kullanmak istediğiniz bir sertifikanız zaten varsa, [kümeyi dağıtmak için mevcut bir sertifikayı kullanmak](#use-an-existing-certificate-to-deploy-the-cluster)üzere atlayın.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Kendinden imzalı bir sertifika oluşturma ve kümeyi dağıtma

İlk olarak, Service Fabric küme dağıtımı için gereken değişkenleri atayın. `resourceGroupName` `certSubjectName` `parameterFilePath` `templateFilePath` Belirli hesabınız ve ortamınız için,,, ve değerlerini ayarlayın:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$parameterFilePath = "C:\parameters.json"
$templateFilePath = "C:\Initial-TestClusterSetup.json"
```

> [!NOTE]
> `certOutputFolder`Yeni bir Service Fabric kümesi dağıtmak için komutunu çalıştırmadan önce, konumun yerel makinenizde mevcut olduğundan emin olun.

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

Bu işlem, artık [yeni kümeye bağlanmak](#connect-to-the-new-cluster-and-check-health-status) ve sistem durumunu denetlemek için kullanabileceğiniz sertifika parmak izini döndürür. (Küme dağıtımına alternatif bir yaklaşım olan aşağıdaki bölümü atlayın.)

### <a name="use-an-existing-certificate-to-deploy-the-cluster"></a>Kümeyi dağıtmak için var olan bir sertifikayı kullan

Alternatif olarak, varolan bir Azure Key Vault sertifikayı test kümesini dağıtmak için kullanabilirsiniz. Bunu yapmak için, Key Vault ve sertifika parmak [izinizdeki başvuruları edinmeniz](#obtain-your-key-vault-references) gerekir.

```powershell
# Key Vault variables
$certUrlValue = "https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
$sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
```

Ardından, küme için bir kaynak grubu adı belirleyin ve `templateFilePath` ve `parameterFilePath` konumlarını ayarlayın:

> [!NOTE]
> Belirtilen kaynak grubu zaten var olmalıdır ve Key Vault aynı bölgede yer almalıdır.

```powershell
$resourceGroupName = "sftestupgradegroup"
$templateFilePath = "C:\Initial-TestClusterSetup.json"
$parameterFilePath = "C:\parameters.json"
```

Son olarak, ilk test kümesini dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
# Deploy the initial test cluster
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Yeni kümeye bağlanma ve sistem durumunu denetleme

Kümeye bağlanın ve düğümlerin tümünün sağlıklı olduğundan emin olun ( `clusterName` ve `thumb` değişkenlerini kendi değerlerinizle değiştirin):

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

Bununla birlikte, yükseltme yordamına başlamaya hazırız.

## <a name="deploy-a-new-primary-node-type-with-upgraded-scale-set"></a>Yükseltilmiş ölçek kümesiyle yeni bir birincil düğüm türü dağıtın

Bir düğüm türünü yükseltmek (dikey olarak ölçeklendirmek) için ilk olarak yeni bir ölçek kümesi ve destekleyici kaynaklar tarafından desteklenen yeni bir düğüm türü dağıtmanız gerekir. Yeni ölçek kümesi `isPrimary: true` , özgün ölçek kümesi gibi birincil () olarak işaretlenir (birincil olmayan bir düğüm türü yükseltmesi yapmadığınız sürece). Aşağıdaki bölümde oluşturulan kaynaklar sonunda kümenizdeki yeni birincil düğüm türü olur ve özgün birincil düğüm türü kaynakları silinir.

### <a name="update-the-cluster-template-with-the-upgraded-scale-set"></a>Küme şablonunu yükseltilen ölçek kümesiyle Güncelleştir

Yeni birincil düğüm türü ve destekleyici kaynakları eklemek için özgün küme dağıtım şablonunun bölüm tarafından yapılan değişiklikleri aşağıda verilmiştir.

Bu adım için gerekli değişiklikler, şablon dosyasında [*Step1-AddPrimaryNodeType.js*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/Step1-AddPrimaryNodeType.json) için zaten yapılmıştır ve aşağıdaki değişiklikler ayrıntılı olarak açıklanacaktır. İsterseniz, açıklamayı atlayıp [Key Vault başvurularınızı almaya](#obtain-your-key-vault-references) devam edebilir ve yeni bir birincil düğüm türü ekleyerek [güncelleştirilmiş şablonu dağıtabilirsiniz](#deploy-the-updated-template) .

> [!Note]
> Orijinal düğüm türü, ölçek kümesi, yük dengeleyici, genel IP ve özgün birincil düğüm türünün alt ağından benzersiz olan adlar kullandığınızdan emin olun, çünkü bu kaynaklar işlemdeki sonraki bir adımda silinir.

#### <a name="create-a-new-subnet-in-the-existing-virtual-network"></a>Var olan sanal ağda yeni bir alt ağ oluştur

```json
{
    "name": "[variables('subnet1Name')]",
    "properties": {
        "addressPrefix": "[variables('subnet1Prefix')]"
    }
}
```

#### <a name="create-a-new-public-ip-with-a-unique-domainnamelabel"></a>Benzersiz bir domainNameLabel ile yeni bir genel IP oluşturma

```json
{
    "apiVersion": "[variables('publicIPApiVersion')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))]",
    "location": "[variables('computeLocation')]",
    "properties": {
    "dnsSettings": {
        "domainNameLabel": "[concat(variables('dnsName'),'-','nt1')]"
    },
    "publicIPAllocationMethod": "Dynamic"
    },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
    }
}
```

#### <a name="create-a-new-load-balancer-for-the-public-ip"></a>Genel IP için yeni bir yük dengeleyici oluşturun

```json
"dependsOn": [
    "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType1Name')))]"
]
```

#### <a name="create-a-new-virtual-machine-scale-set-with-upgraded-vm-and-os-skus"></a>Yeni bir sanal makine ölçek kümesi oluşturma (yükseltilen VM ve işletim sistemi SKU 'Ları ile)

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

Ayrıca, iş yükünüz için gereken ek uzantıları da dahil edin.

#### <a name="add-a-new-primary-node-type-to-the-cluster"></a>Kümeye yeni bir birincil düğüm türü ekleyin

Artık yeni düğüm türünün (vmNodeType1Name) kendi adı, alt ağı, IP, yük dengeleyici ve ölçek kümesi olduğuna göre, özgün düğüm türünden diğer tüm değişkenleri ( `nt0applicationEndPort` , ve gibi) yeniden kullanabilir `nt0applicationStartPort` `nt0fabricTcpGatewayPort` :

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

Şablon ve parametre dosyalarınızda tüm değişiklikleri yaptıktan sonra, Key Vault başvurularını almak ve güncelleştirmeleri kümenize dağıtmak için sonraki bölüme ilerleyin.

### <a name="obtain-your-key-vault-references"></a>Key Vault başvurularınızı alın

Güncelleştirilmiş yapılandırmayı dağıtmak için Key Vault depolanan küme sertifikasına birkaç başvuru gerekir. Bu değerleri bulmanın en kolay yolu Azure portal kullanmaktır. Şunlara ihtiyacınız var:

* **Küme sertifikanızın Key Vault URL 'SI.** Azure Portal ' Key Vault,   >  *istediğiniz sertifika*  >  **gizli tanımlayıcı tanımlarınızı** seçin:

    ```powershell
    $certUrlValue="https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
    ```

* **Küme sertifikanızın parmak izi.** (Bu durum muhtemelen, sistem durumunu denetlemek için [ilk kümeye bağlandıysanız](#connect-to-the-new-cluster-and-check-health-status) zaten var.) Azure Portal ' de aynı sertifika dikeypenceresinden (  >  *istediğiniz sertifika* sertifikaları), **X. 509.440 SHA-1 parmak izini (onaltılı)** kopyalayın:

    ```powershell
    $thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
    ```

* **Key Vault kaynak KIMLIĞI.** Azure Portal Key Vault **Özellikler**  >  **kaynak kimliği**' ni seçin.

    ```powershell
    $sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
    ```

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma

`templateFilePath`Gereken şekilde ayarlayın ve aşağıdaki komutu çalıştırın:

```powershell
# Deploy the new node type and its resources
$templateFilePath = "C:\Step1-AddPrimaryNodeType.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Dağıtım tamamlandığında, küme durumunu yeniden denetleyin ve her iki düğüm türündeki tüm düğümlerin sağlıklı olduğundan emin olun.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-seed-nodes-to-the-new-node-type"></a>Çekirdek düğümlerini yeni düğüm türüne geçir

Artık özgün düğüm türünü birincil olmayan olarak güncelleştirmeye ve düğümlerini devre dışı bırakmaya başlamaya hazırız. Düğümler devre dışı olduğunda, kümenin sistem hizmetleri ve tohum düğümleri yeni ölçek kümesine geçirilir.

### <a name="unmark-the-original-node-type-as-primary"></a>Özgün düğüm türünün işaretini birincil olarak kaldır

İlk `isPrimary` olarak şablondaki belirtimi özgün düğüm türünden kaldırın.

```json
{
    "isPrimary": false,
}
```

Ardından şablonu güncelleştirmesiyle birlikte dağıtın. Bu, çekirdek düğümlerin yeni ölçek kümesine geçişini başlatır.

```powershell
$templateFilePath = "C:\Step2-UnmarkOriginalPrimaryNodeType.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

> [!Note]
> Yeni ölçek kümesine çekirdek düğümü geçişi işleminin tamamlanması biraz zaman alır. Veri tutarlılığını güvence altına almak için, tek seferde yalnızca bir çekirdek düğüm değiştirilebilir. Her çekirdek düğümü değişikliği bir küme güncelleştirmesi gerektirir; Bu nedenle, çekirdek düğümü değiştirmek için iki küme yükseltmesi gerekir (her biri düğüm ekleme ve kaldırma için bir tane). Bu örnek senaryoda beş tohum düğümünü yükseltmek, on küme yükseltmelerine neden olur.

Çekirdek düğümlerin yeni ölçek kümesine geçişini izlemek için Service Fabric Explorer kullanın. Özgün düğüm türünün (nt0vm) düğümleri, bu **çekirdek düğüm** sütununda tümü *false* olmalıdır ve yeni düğüm türü (nt1vm) *true* olur.

### <a name="disable-the-nodes-in-the-original-node-type-scale-set"></a>Özgün düğüm türü ölçek kümesindeki düğümleri devre dışı bırak

Tüm çekirdek düğümleri yeni ölçek kümesine geçirildikten sonra, orijinal ölçek kümesinin düğümlerini devre dışı bırakabilirsiniz.

```powershell
# Disable the nodes in the original scale set.
$nodeType = "nt0vm"
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

Özgün ölçek kümesindeki düğümlerin ilerlemesini *devre dışı* durumuna göre *izlemek için Service Fabric Explorer* kullanın.

:::image type="content" source="./media/scale-up-primary-node-type/service-fabric-explorer-node-status.png" alt-text="Devre dışı bırakılan düğümlerin durumunu gösteren Service Fabric Explorer":::

Gümüş ve altın dayanıklılık için bazı düğümler devre dışı durumuna geçer, diğerleri devre *dışı* durumda kalabilir. Service Fabric Explorer, devre dışı bırakma durumundaki düğümlerin **Ayrıntılar** sekmesini denetleyin. Bu kişiler, *Ensurepartitionquorem* türü için *bekleyen bir güvenlik denetimi* gösteriyorlarsa (altyapı hizmeti bölümleri için çekirdeği sağlama), devam etmek güvenlidir.

:::image type="content" source="./media/scale-up-primary-node-type/service-fabric-explorer-node-status-disabling.png" alt-text="' EnsurePartitionQuorum ' türünde bekleyen bir güvenlik denetimi varsa, verileri durdurma ve ' devre dışı bırakma ' durumunda kalmış düğümleri kaldırma işlemine devam edebilirsiniz.":::

Kümeniz daha fazla dayanıklılık içeriyorsa, tüm düğümlerin *devre dışı* durumuna ulaşmasını bekleyin.

### <a name="stop-data-on-the-disabled-nodes"></a>Devre dışı bırakılmış düğümlerdeki verileri durdur

Artık devre dışı bırakılmış düğümlerdeki verileri durdurabilirsiniz.

```powershell
# Stop data on the disabled nodes.
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
  }
}
```

## <a name="remove-the-original-node-type-and-cleanup-its-resources"></a>Özgün düğüm türünü kaldır ve kaynaklarını temizle

Orijinal düğüm türünü ve ilişkili kaynaklarını, dikey ölçeklendirme yordamını sonuçlandırmaya yönelik olarak kaldırmaya hazırız.

### <a name="remove-the-original-scale-set"></a>Özgün ölçek kümesini kaldır

Önce düğüm türünün yedekleme ölçek kümesini kaldırın.

```powershell
$scaleSetName = "nt0vm"
$scaleSetResourceType = "Microsoft.Compute/virtualMachineScaleSets"

Remove-AzResource -ResourceName $scaleSetName -ResourceType $scaleSetResourceType -ResourceGroupName $resourceGroupName -Force
```

### <a name="delete-the-original-ip-and-load-balancer-resources"></a>Özgün IP ve yük dengeleyici kaynaklarını silme

Artık özgün IP 'yi ve yük dengeleyici kaynaklarını silebilirsiniz. Bu adımda DNS adını da güncelleşolursunuz.

> [!Note]
> Zaten *Standart* SKU genel IP ve yük dengeleyici kullanıyorsanız bu adım isteğe bağlıdır. Bu durumda, aynı yük dengeleyici altında birden fazla ölçek kümesi/düğüm türüne sahip olabilirsiniz.

Gerektiğinde değeri değiştirerek aşağıdaki komutları çalıştırın `$lbname` .

```powershell
# Delete the original IP and load balancer resources
$lbName = "LB-sftestupgrade-nt0vm"
$lbResourceType = "Microsoft.Network/loadBalancers"
$ipResourceType = "Microsoft.Network/publicIPAddresses"
$oldPublicIpName = "PublicIP-LB-FE-nt0vm"
$newPublicIpName = "PublicIP-LB-FE-nt1vm"

$oldPrimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $resourceGroupName
$primaryDNSName = $oldPrimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldPrimaryPublicIP.DnsSettings.Fqdn

Remove-AzResource -ResourceName $lbName -ResourceType $lbResourceType -ResourceGroupName $resourceGroupName -Force
Remove-AzResource -ResourceName $oldPublicIpName -ResourceType $ipResourceType -ResourceGroupName $resourceGroupName -Force

$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $resourceGroupName
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
```

### <a name="remove-node-state-from-the-original-node-type"></a>Düğüm durumunu orijinal düğüm türünden kaldır

Özgün düğüm türü düğümleri artık **sistem durumları** için *hata* gösterir. Düğüm durumlarını kümeden kaldırın.

```powershell
# Remove state of the obsolete nodes from the cluster
$nodeType = "nt0vm"
$nodes = Get-ServiceFabricNode

Write-Host "Removing node state..."
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
  }
}
```

Service Fabric Explorer artık, yeni düğüm türünün (nt1vm) yalnızca beş düğümünü yansıtmalıdır, bu durum *Tamam*' a ait sağlık durumu değerleridir. Küme sistem durumlarınız yine de *hata* göstermeye devam eder. Şablonu, en son değişiklikleri yansıtacak ve yeniden dağıtmaya yönelik olarak güncelleştirerek bu ileri düzelteceğiz.

### <a name="update-the-deployment-template-to-reflect-the-newly-scaled-up-primary-node-type"></a>Dağıtım şablonunu, yeni ölçekli birincil düğüm türünü yansıtacak şekilde güncelleştirin

Bu adım için gerekli değişiklikler, şablon dosyasında [*Step3-CleanupOriginalPrimaryNodeType.js*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/Step3-CleanupOriginalPrimaryNodeType.json) için zaten yapılmıştır ve aşağıdaki bölümlerde bu şablon değişiklikleri ayrıntılı olarak açıklanacaktır. İsterseniz açıklamayı atlayabilir ve [güncelleştirilmiş şablonu dağıtmaya](#deploy-the-finalized-template) devam edebilir ve öğreticiyi tamamlayabilirsiniz.

#### <a name="update-the-cluster-management-endpoint"></a>Küme yönetim uç noktasını güncelleştirme

`managementEndpoint`Dağıtım şablonundaki kümeyi yenı IP 'ye başvuracak şekilde güncelleştirin ( *VmNodeType0Name* ile *vmNodeType1Name* güncelleyerek).

```json
  "managementEndpoint": "[concat('https://',reference(concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))).dnsSettings.fqdn,':',variables('nt0fabricHttpGatewayPort'))]",
```

#### <a name="remove-the-original-node-type-reference"></a>Özgün düğüm türü başvurusunu kaldır

Dağıtım şablonundaki Service Fabric kaynağından özgün düğüm türü başvurusunu kaldırın:

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

#### <a name="configure-health-policies-to-ignore-existing-errors"></a>Mevcut hataları yok saymak için sistem durumu ilkeleri yapılandırma

Yalnızca gümüş ve daha yüksek dayanıklılık kümelerinde, şablondaki küme kaynağını güncelleştirin ve `fabric:/System` küme kaynağı özellikleri altına aşağıda verilen şekilde *Applicationdeltahealthpolicies* ekleyerek uygulama durumunu yoksayacak şekilde sistem durumu ilkelerini yapılandırın. Aşağıdaki ilke var olan hataları yoksayacak, ancak yeni sistem durumu hatalarına izin vermez.

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

#### <a name="remove-supporting-resources-for-the-original-node-type"></a>Özgün düğüm türü için destekleme kaynaklarını kaldır

ARM şablonundan ve parametreler dosyasından orijinal düğüm türüyle ilişkili diğer tüm kaynakları kaldırın. Aşağıdakileri silin:

```json
    "vmImagePublisher": {
      "value": "MicrosoftWindowsServer"
    },
    "vmImageOffer": {
      "value": "WindowsServer"
    },
    "vmImageSku": {
      "value": "2016-Datacenter-with-Containers"
    },
    "vmImageVersion": {
      "value": "latest"
    },
```

#### <a name="deploy-the-finalized-template"></a>Son şablonu dağıtma

Son olarak, değiştirilen Azure Resource Manager şablonunu dağıtın.

```powershell
# Deploy the updated template file
$templateFilePath = "C:\Step3-CleanupOriginalPrimaryNodeType"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

> [!NOTE]
> Bu adım genellikle iki saate kadar sürer.

Yükseltme, ayarları *InfrastructureService* olarak değiştirecek; Bu nedenle, bir düğümün yeniden başlatılması gerekiyor. Bu durumda, *forcerestart* yoksayıldı. Parametresi, `upgradeReplicaSetCheckTimeout` Service Fabric bir bölümün güvenli durumda olmasını bekleyeceği en uzun süreyi belirtir, daha önceden güvenli bir durumda değildir. Güvenlik denetimleri bir düğümdeki tüm bölümler için başarılı olduktan sonra, bu düğümdeki yükseltmeye devam eder Service Fabric. Parametrenin değeri `upgradeTimeout` 6 saate indirgenmiş olabilir, ancak maxhayvan güvenliği 12 saat kullanılmalıdır.

Dağıtım tamamlandıktan sonra, Service Fabric kaynak durumunun Azure portal *doğrulayın.* Yeni Service Fabric Explorer uç noktasına ulaşabildiğinizi, **küme sistem durumunun** *Tamam* olduğunu ve dağıtılan tüm uygulamaların düzgün şekilde çalıştığını doğrulayın.

Bu şekilde, küme birincil düğüm türünü dikey olarak ölçeklendirdiniz!

## <a name="next-steps"></a>Sonraki adımlar

* [Kümeye düğüm türü eklemeyi](virtual-machine-scale-set-scale-node-type-scale-out.md) öğrenin
* [Uygulama ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Bir Azure kümesini içinde veya dışarı ölçeklendirin](service-fabric-tutorial-scale-cluster.md).
* Akıcı Azure işlem SDK 'sını kullanarak [bir Azure kümesini programlı bir şekilde ölçeklendirin](service-fabric-cluster-programmatic-scaling.md) .
* [Tek başına kümeyi içinde veya dışarı ölçeklendirin](service-fabric-cluster-windows-server-add-remove-nodes.md).
