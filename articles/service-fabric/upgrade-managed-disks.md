---
title: Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme
description: Mevcut bir Service Fabric kümesini, kümenizde çok az veya kapalı kalma süresi olmadan Azure yönetilen diskleri kullanacak şekilde yükseltme.
ms.topic: how-to
ms.date: 4/07/2020
ms.openlocfilehash: 1ca85af86df28691e2194c40e1cdde1abd7c8a4d
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192298"
---
# <a name="upgrade-cluster-nodes-to-use-azure-managed-disks"></a>Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme

[Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md) , verilerin kalıcı olarak depolanması için Azure sanal makineler ile kullanılmak üzere önerilen disk depolama sunumudur. Yönetilen diskleri kullanmak için düğüm türlerinizi içeren sanal makine ölçek kümelerini yükselterek Service Fabric iş yüklerinizin dayanıklılığını geliştirebilirsiniz. Mevcut bir Service Fabric kümesini, kümenizde çok az veya kapalı kalma süresi olmadan Azure yönetilen diskleri kullanacak şekilde yükseltme.

Yönetilen diskleri kullanmak için bir Service Fabric küme düğümünü yükseltmeye yönelik genel strateji şu şekilde yapılır:

1. Bu düğüm türü için, ancak sanal makine ölçek kümesi dağıtım şablonunun bölümüne eklenen [Manageddisk](/azure/templates/microsoft.compute/2019-07-01/virtualmachinescalesets/virtualmachines#ManagedDiskParameters) nesnesi ile, başka bir şekilde yinelenen bir sanal makine ölçek kümesi dağıtın `osDisk` . Yeni ölçek kümesi, müşterilerinizin geçiş sırasında hizmet kesintisi yaşamamasını sağlamak için, orijinalle aynı yük dengeleyiciye/IP 'ye bağlanmalıdır.

2. Hem özgün hem de yükseltilen ölçek kümeleri yan yana çalışırken, sistem hizmetlerinin (veya durum bilgisi olan hizmetlerin çoğaltmalarının) yeni ölçek kümesine geçişini sağlamak için özgün düğüm örneklerini tek seferde devre dışı bırakın.

3. Kümenin ve yeni düğümlerin sağlıklı olduğunu doğrulayın, sonra silinen düğümlerin orijinal ölçek kümesini ve düğüm durumunu kaldırın.

Bu makale, yönetilen diskleri kullanmak için örnek bir kümenin birincil düğüm türünü yükseltme adımlarında size yol gösterir, ancak herhangi bir küme kapalı kalma süresini önleyerek (aşağıdaki nota bakın). Örnek test kümesinin ilk durumu, beş düğümlü tek bir ölçek kümesi tarafından desteklenen [gümüş dayanıklılığa](service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster)sahip bir düğüm türünden oluşur.

> [!NOTE]
> Temel SKU yük dengeleyicinin sınırlamaları, ek ölçek kümesinin eklenmesini engeller. Bunun yerine standart SKU yük dengeleyiciyi kullanmanızı öneririz. Daha fazla bilgi için bkz. [Iki SKU 'nun karşılaştırması](/azure/load-balancer/skus).

> [!CAUTION]
> Bu yordamı yalnızca küme DNS ( [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)erişimi gibi) üzerinde bağımlılıklara sahipseniz bir kesinti yaşanacaktır. [Ön uç hizmetleri için mimari en iyi uygulaması](/azure/architecture/microservices/design/gateway) , düğüm değiştirmeyi kesinti olmadan mümkün hale getirmek için düğüm türlerinizin önünde bazı tür [yük dengeleyiciler](/azure/architecture/guide/technology-choices/load-balancing-overview) içermelidir.

Yükseltme senaryosunu tamamlaması için kullanacağımız Azure Resource Manager [Şablonlar ve cmdlet 'leri](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage) aşağıda bulabilirsiniz. Şablon değişiklikleri, aşağıdaki [birincil düğüm türü için yükseltilen ölçek kümesi dağıtma](#deploy-an-upgraded-scale-set-for-the-primary-node-type)  bölümünde açıklanacaktır.

## <a name="set-up-the-test-cluster"></a>Test kümesini ayarlama

İlk Service Fabric test kümesini ayarlayalim. İlk olarak, bu senaryoyu gerçekleştirmek için kullanacağımız Azure Resource Manager örnek şablonları [indirin](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage) .

Ardından, Azure hesabınızda oturum açın.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

Aşağıdaki komutlar, otomatik olarak imzalanan yeni bir sertifika oluşturma ve test kümesini dağıtma konusunda size rehberlik edecektir. Kullanmak istediğiniz bir sertifikanız zaten varsa, [kümeyi dağıtmak için mevcut bir sertifikayı kullanmak](#use-an-existing-certificate-to-deploy-the-cluster)üzere atlayın.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Kendinden imzalı bir sertifika oluşturma ve kümeyi dağıtma

İlk olarak, Service Fabric küme dağıtımı için gereken değişkenleri atayın. `resourceGroupName` `certSubjectName` `parameterFilePath` `templateFilePath` Belirli hesabınız ve ortamınız için,,, ve değerlerini ayarlayın:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$templateFilePath = "C:\Initial-1NodeType-UnmanagedDisks.json"
$parameterFilePath = "C:\Initial-1NodeType-UnmanagedDisks.parameters.json"
```

> [!NOTE]
> `certOutputFolder`Yeni bir Service Fabric kümesi dağıtmak için komutunu çalıştırmadan önce, konumun yerel makinenizde mevcut olduğundan emin olun.

Sonra, [*Initial-1NodeType-UnmanagedDisks.parameters.jsdosya üzerinde*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json) açın ve değerlerini, `clusterName` `dnsName` PowerShell 'de ayarladığınız dinamik değerlere karşılık gelecek şekilde ayarlayın ve değişikliklerinizi kaydedin.

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

Bu işlem, [yeni kümeye bağlanmak](#connect-to-the-new-cluster-and-check-health-status) ve sistem durumunu denetlemek için kullanacağınız sertifika parmak izini döndürür. (Küme dağıtımına alternatif bir yaklaşım olan aşağıdaki bölümü atlayın.)

### <a name="use-an-existing-certificate-to-deploy-the-cluster"></a>Kümeyi dağıtmak için var olan bir sertifikayı kullan

Ayrıca, test kümesini dağıtmak için mevcut bir Azure Key Vault sertifikayı da kullanabilirsiniz. Bunu yapmak için, Key Vault ve sertifika parmak [izinizdeki başvuruları edinmeniz](#obtain-your-key-vault-references) gerekir.

```powershell
# Key Vault variables
$certUrlValue = "https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
$sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
```

Dosyasında [*Initial-1NodeType-UnmanagedDisks.parameters.js*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json) açın ve değerlerini `clusterName` ve `dnsName` benzersiz bir şekilde değiştirin.

Son olarak, küme için bir kaynak grubu adı belirleyin ve `templateFilePath` `parameterFilePath` *Ilk-1Nodetype-unmanageddisks* dosyalarınızın ve konumlarını ayarlayın:

> [!NOTE]
> Belirtilen kaynak grubu zaten var olmalıdır ve Key Vault aynı bölgede yer almalıdır.

```powershell
# Deploy the new scale set (upgraded to use managed disks) into the primary node type.
$resourceGroupName = "sftestupgradegroup"
$templateFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.json"
$parameterFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json"
```

Son olarak, ilk test kümesini dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
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

Kümeye bağlanın ve düğümlerin tümünün sağlam olduğundan emin olun ( `clusterName` `thumb` kümeniz için ve değişkenlerini değiştirerek):

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

## <a name="deploy-an-upgraded-scale-set-for-the-primary-node-type"></a>Birincil düğüm türü için yükseltilmiş bir ölçek kümesi dağıtma

Bir düğüm türünü yükseltmek veya *dikey olarak ölçeklendirmek*için, `nodeTypeRef` `subnet` `loadBalancerBackendAddressPools` istenen yükseltme/değişiklik ve kendi ayrı alt ağını ve gelen NAT adres havuzunu içermesi dışında, bu düğüm türünün sanal makine ölçek kümesinin bir kopyasını dağıtmalı (aynı, ve ' a başvuru dahil olmak üzere). Birincil düğüm türünü yükselttireceğiz, yeni ölçek kümesi `isPrimary: true` yalnızca özgün ölçek kümesi gibi birincil () olarak işaretlenir. (Birincil olmayan düğüm türü yükseltmeleri için bunu atlayın.)

Kolaylık olması için, *yükseltme-1NodeType-2ScaleSets-ManagedDisks* [şablonunda](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.json) ve [Parameters](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json) dosyalarında sizin için gerekli değişiklikler zaten yapıldı.

Aşağıdaki bölümler, şablon değişikliklerinin ayrıntılı olarak açıklanacaktır. İsterseniz, açıklamayı atlayabilir ve [yükseltme yordamının bir sonraki adımında](#obtain-your-key-vault-references)devam edebilirsiniz.

### <a name="update-the-cluster-template-with-the-upgraded-scale-set"></a>Küme şablonunu yükseltilen ölçek kümesiyle Güncelleştir

Birincil düğüm türü için yükseltilmiş ölçek kümesi eklemek üzere özgün küme dağıtım şablonunun bölüm tarafından yapılan değişiklikleri aşağıda verilmiştir.

#### <a name="parameters"></a>Parametreler

Yeni ölçek kümesinin örnek adı, sayısı ve boyutu için parametreler ekleyin. `vmNodeType1Name`Yeni ölçek kümesinin benzersiz olduğunu, sayı ve boyut değerlerinin ise orijinal ölçek kümesiyle aynı olduğunu unutmayın.

**Şablon dosyası**

```json
"vmNodeType1Name": {
    "type": "string",
    "defaultValue": "NTvm2",
    "maxLength": 9
},
"nt1InstanceCount": {
    "type": "int",
    "defaultValue": 5,
    "metadata": {
        "description": "Instance count for node type"
    }
},
"vmNodeType1Size": {
    "type": "string",
    "defaultValue": "Standard_D2_v2"
},
```

**Parametre dosyası**

```json
"vmNodeType1Name": {
    "value": "NTvm2"
},
"nt1InstanceCount": {
    "value": 5
},
"vmNodeType1Size": {
    "value": "Standard_D2_v2"
}
```

### <a name="variables"></a>Değişkenler

Dağıtım şablonu `variables` bölümünde, yeni ölçek kümesinin gelen NAT adres havuzu için bir giriş ekleyin.

**Şablon dosyası**

```json
"lbNatPoolID1": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool1')]", 
```

### <a name="resources"></a>Kaynaklar

Dağıtım şablonu *kaynakları* bölümünde, yeni sanal makine ölçek kümesini ekleyerek şunları göz önünde bulundurun:

* Yeni ölçek kümesi özgün ile aynı düğüm türüne başvuruyor:

    ```json
    "nodeTypeRef": "[parameters('vmNodeType0Name')]",
    ```

* Yeni ölçek kümesi aynı yük dengeleyici arka uç adresine ve alt ağına başvuruyor (ancak farklı yük dengeleyici gelen NAT havuzu kullanır):

   ```json
    "loadBalancerBackendAddressPools": [
        {
            "id": "[variables('lbPoolID0')]"
        }
    ],
    "loadBalancerInboundNatPools": [
        {
            "id": "[variables('lbNatPoolID1')]"
        }
    ],
    "subnet": {
        "id": "[variables('subnet0Ref')]"
    }
   ```

* Orijinal ölçek kümesi gibi, yeni ölçek kümesi birincil düğüm türü olarak işaretlenir. (Birincil olmayan düğüm türlerini yükseltirken bu değişikliği atlayın.)

    ```json
    "isPrimary": true,
    ```

* Orijinal ölçek kümesinden farklı olarak, yeni ölçek kümesi yönetilen diskleri kullanacak şekilde yükseltilir.

    ```json
    "managedDisk": {
        "storageAccountType": "[parameters('storageAccountType')]"
    }
    ```

Şablon ve parametre dosyalarınızda tüm değişiklikleri yaptıktan sonra, Key Vault başvurularını almak ve güncelleştirmeleri kümenize dağıtmak için sonraki bölüme ilerleyin.

### <a name="obtain-your-key-vault-references"></a>Key Vault başvurularınızı alın

Güncelleştirilmiş yapılandırmayı dağıtmak için öncelikle Key Vault depolanan küme sertifikanız için birkaç başvuru elde edersiniz. Bu değerleri bulmanın en kolay yolu Azure portal kullanmaktır. Şunlara ihtiyacınız var:

* **Küme sertifikanızın Key Vault URL 'SI.** Azure Portal ' Key Vault, **Certificates**  >  *istediğiniz sertifika*  >  **gizli tanımlayıcı tanımlarınızı**seçin:

    ```powershell
    $certUrlValue="https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
    ```

* **Küme sertifikanızın parmak izi.** (Bu durum muhtemelen, sistem durumunu denetlemek için [ilk kümeye bağlandıysanız](#connect-to-the-new-cluster-and-check-health-status) zaten var.) Azure Portal ' de aynı sertifika dikey**Certificates**penceresinden (  >  *istediğiniz sertifika*sertifikaları), **X. 509.440 SHA-1 parmak izini (onaltılı)** kopyalayın:

    ```powershell
    $thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
    ```

* **Key Vault kaynak KIMLIĞI.** Azure Portal Key Vault **Özellikler**  >  **kaynak kimliği**' ni seçin.

    ```powershell
    $sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
    ```

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma

`parameterFilePath`Ve `templateFilePath` gereken şekilde ayarlayın ve ardından aşağıdaki komutu çalıştırın:

```powershell
# Deploy the new scale set (upgraded to use managed disks) into the primary node type.
$templateFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.json"
$parameterFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Dağıtım tamamlandığında, küme sistem durumunu yeniden kontrol edin ve tüm on düğümlerin (orijinal ve beş yeni ölçek kümesinde beş) sağlıklı olduğundan emin olun.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-seed-nodes-to-the-new-scale-set"></a>Çekirdek düğümlerini yeni ölçek kümesine geçirme

Artık orijinal ölçek kümesinin düğümlerini devre dışı bırakmaya başlamaya hazırsınız. Bu düğümler devre dışı bırakıldığı için, sistem hizmetleri ve çekirdek düğümleri, birincil düğüm türü olarak da işaretlendiğinden, yeni ölçek kümesinin VM 'lerine geçirilir.

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Çekirdek düğümlerin yeni ölçek kümesine geçişini ve orijinal ölçek kümesindeki düğümlerin ilerlemesini *devre dışı* *durumuna geçirmek için* Service Fabric Explorer kullanın.

![Devre dışı bırakılan düğümlerin durumunu gösteren Service Fabric Explorer](./media/upgrade-managed-disks/service-fabric-explorer-node-status.png)

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

![Devre dışı bırakılan düğümleri hata durumunda göstermek Service Fabric Explorer](./media/upgrade-managed-disks/service-fabric-explorer-disabled-nodes-error-state.png)

Küme sistem durumunu *Tamam*olarak geri yüklemek için Service Fabric kümeden kullanılmayan düğümleri kaldırın.

```powershell
# Remove node states for the deleted scale set
foreach($name in $nodeNames){
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

![Hata durumundaki düğümleri aşağı Service Fabric Explorer kaldırıldı](./media/upgrade-managed-disks/service-fabric-explorer-healthy-cluster.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, işlem sırasında hizmet kesintilerine karşı bir Service Fabric kümesinin sanal makine ölçek kümelerini yönetilen diskleri kullanacak şekilde yükseltmeyi öğrendiniz. İlgili konular hakkında daha fazla bilgi için aşağıdaki kaynaklara göz atın.

Şunları nasıl yapacağınızı öğrenin:

* [Service Fabric kümesi birincil düğüm türünün ölçeğini artırma](service-fabric-scale-up-node-type.md)

* [Ölçek kümesi şablonunu yönetilen diskleri kullanacak şekilde dönüştürme](../virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md.md)

* [Service Fabric düğüm türünü kaldır](service-fabric-how-to-remove-node-type.md)

Ayrıca bkz:

* [Örnek: Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage)

* [Dikey ölçeklendirme konuları](service-fabric-best-practices-capacity-scaling.md#vertical-scaling-considerations)
