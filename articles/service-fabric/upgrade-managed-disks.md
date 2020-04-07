---
title: Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme
description: Varolan bir Hizmet Kumaşı kümesini, kümenizin çok az veya hiç kapalı kalma süresi olmayan Azure yönetilen diskleri kullanmak için şu şekilde yükseltebilirsiniz.
ms.topic: how-to
ms.date: 3/01/2020
ms.openlocfilehash: 2bda5572eda5579bb31c3613b220885f27220d99
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758059"
---
# <a name="upgrade-cluster-nodes-to-use-azure-managed-disks"></a>Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme

[Azure yönetilen diskler,](../virtual-machines/windows/managed-disks-overview.md) verilerin kalıcı olarak depolanması için Azure sanal makineleriyle kullanılmak üzere önerilen disk depolama teklifidir. Yönetilen diskleri kullanmak için düğüm tiplerinizin altında yatan sanal makine ölçek kümelerini yükselterek Service Fabric iş yüklerinizin esnekliğini artırabilirsiniz. Varolan bir Hizmet Kumaşı kümesini, kümenizin çok az veya hiç kapalı kalma süresi olmayan Azure yönetilen diskleri kullanmak için şu şekilde yükseltebilirsiniz.

Yönetilen diskleri kullanmak için Bir Hizmet Kumaşı küme düğümyükseltme için genel strateji:

1. Bu düğüm türünden yinelenen bir sanal makine ölçeği kümesini dağıtın, `osDisk` ancak yönetilen [Disk](https://docs.microsoft.com/azure/templates/microsoft.compute/2019-07-01/virtualmachinescalesets/virtualmachines#ManagedDiskParameters) nesnesi sanal makine ölçeği dağıtım şablonu bölümüne eklenir. Yeni ölçek kümesi, müşterilerinizin geçiş sırasında bir hizmet kesintisi yaşamaması için orijinalle aynı yük dengeleyicisine /IP'ye bağlanmalıdır.

2. Özgün ve yükseltilmiş ölçek kümeleri yan yana çalıştırDıktan sonra, sistem hizmetlerinin (veya devlet hizmetlerinin yinelemelerinin) yeni ölçek kümesine geçiş yapabilmesi için özgün düğüm örneklerini birer birer devre dışı bırakın.

3. Kümeve yeni düğümlerin sağlıklı olduğunu doğrulayın, ardından silinen düğümler için özgün ölçek kümesini ve düğüm durumunu kaldırın.

Bu makalede, yönetilen diskleri kullanmak için bir örnek kümenin birincil düğüm türünü yükseltme adımlarında size yol gösterirken, herhangi bir küme kapalı kalma süresini önlersiniz (aşağıdaki nota bakın). Örnek test kümesinin ilk durumu, beş düğümlü tek bir ölçek kümesi tarafından desteklenen bir düğüm gümüş [dayanıklılık](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster)türünden oluşur.

> [!CAUTION]
> Bu yordamda yalnızca DNS kümesine bağımlılıklarıvarsa [(Örneğin, Hizmet Kumaş Gezgini'ne](service-fabric-visualizing-your-cluster.md)erişirken) bir kesinti yaşarsınız. Ön uç hizmetleri için mimari [en iyi uygulama,](https://docs.microsoft.com/azure/architecture/microservices/design/gateway) düğüm türünüzün önünde bir tür [yük dengeleyicisi](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) olması ve bir kesinti olmadan düğüm takasını mümkün kılmaktır.

Azure Kaynak Yöneticisi için yükseltme senaryosunu tamamlamak için kullanacağımız [şablonlar ve cmdletler](https://github.com/erikadoyle/service-fabric-scripts-and-templates/tree/managed-disks/templates/nodetype-upgrade-no-outage) aşağıda verilmiştir. Şablon değişiklikleri, aşağıdaki [birincil düğüm türü için yükseltilmiş bir ölçek kümesinde](#deploy-an-upgraded-scale-set-for-the-primary-node-type) açıklanacaktır.

## <a name="set-up-the-test-cluster"></a>Test kümesini ayarlama

İlk Service Fabric test kümesini ayarlayalım. İlk olarak, bu senaryoyu tamamlamak için kullanacağımız Azure kaynak yöneticisi örnek şablonlarını [indirin.](https://github.com/erikadoyle/service-fabric-scripts-and-templates/tree/managed-disks/templates/nodetype-upgrade-no-outage)

Ardından Azure hesabınızda oturum açın.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

Aşağıdaki komutlar, yeni bir kendi imzalı sertifika oluşturma ve test kümesini dağıtma konusunda size yol gösterecektir. Kullanmak istediğiniz bir sertifika zaten varsa, kümeyi dağıtmak için [varolan bir sertifikayı kullan'ı](#use-an-existing-certificate-to-deploy-the-cluster)atlayın.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Kendi imzalı bir sertifika oluşturun ve kümeyi dağıtın

İlk olarak, Service Fabric küme dağıtımı için gereken değişkenleri atayın. Belirli bir `resourceGroupName`hesap `certSubjectName` `parameterFilePath`ve `templateFilePath` ortam için ve değerleri ayarlayın:

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
> Yeni bir `certOutputFolder` Hizmet Kumaşı kümesini dağıtmak için komutu çalıştırmadan önce konumun yerel makinenizde bulunduğundan emin olun.

Ardından [*Initial-1NodeType-UnmanagedDisks.parameters.json*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json) dosyasını açın ve `clusterName` `dnsName` PowerShell'de belirlediğiniz dinamik değerlere karşılık gelen değerleri ayarlayın ve değişikliklerinizi kaydedin.

Ardından Service Fabric test kümesini dağıtın:

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

Dağıtım tamamlandıktan sonra, *.pfx* dosyasını`$certPfx`yerel makinenizde bulun ve sertifika deponuza aktarın:

```powershell
cd c:\certificates
$certPfx = ".\sftestupgradegroup20200312121003.pfx"

Import-PfxCertificate `
     -FilePath $certPfx `
     -CertStoreLocation Cert:\CurrentUser\My `
     -Password (ConvertTo-SecureString Password!1 -AsPlainText -Force)
```

İşlem, [yeni kümeye bağlanmak](#connect-to-the-new-cluster-and-check-health-status) ve sistem durumunu denetlemek için kullanacağınız sertifika parmak izini döndürecek. (Küme dağıtımına alternatif bir yaklaşım olan aşağıdaki bölümü atlayın.)

### <a name="use-an-existing-certificate-to-deploy-the-cluster"></a>Kümeyi dağıtmak için varolan bir sertifikayı kullanma

Test kümesini dağıtmak için varolan bir Azure Anahtar Kasası sertifikasını da kullanabilirsiniz. Bunu yapmak [için, Anahtar Vault](#obtain-your-key-vault-references) ve sertifika parmak izi referansları almak gerekir.

```powershell
# Key Vault variables
$certUrlValue = "https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
$sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
```

[*Initial-1NodeType-UnmanagedDisks.parameters.json*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json) dosyasını açın ve `clusterName` benzersiz `dnsName` bir şey için ve değerleri değiştirin.

Son olarak, küme için bir kaynak `templateFilePath` grubu `parameterFilePath` adı belirleyin ve *İlk-1NodeType-UnmanagedDisks* dosyalarınızın konumlarını ve konumlarını ayarlayın:

> [!NOTE]
> Atanan kaynak grubunun zaten var olması ve Key Vault'unuzla aynı bölgede bulunması gerekir.

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

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Yeni kümeye bağlanın ve sistem durumu durumunu kontrol edin

Kümeye bağlanın ve düğümlerinin beşinin de sağlıklı olduğundan `clusterName` `thumb` emin olun (kümeniz için değişkenleri ve değişkenleri değiştirin):

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

Bununla birlikte, yükseltme prosedürüne başlamaya hazırız.

## <a name="deploy-an-upgraded-scale-set-for-the-primary-node-type"></a>Birincil düğüm türü için yükseltilmiş ölçek kümesini dağıtma

Yükseltme yapmak veya *dikey olarak*ölçeklendirmek için, bu düğüm türüne ait sanal makine ölçeği kümesinin bir kopyasını dağıtmamız gerekir, bu da orijinal `nodeTypeRef` `subnet`ölçek `loadBalancerBackendAddressPools`kümesiyle aynıdır (aynı , ve ) istenilen yükseltme/değişiklikleri ve kendi ayrı alt netini ve gelen NAT adres havuzunu içermesi dışında. Birincil düğüm türünü yükselttiğimiz için, yeni ölçek kümesi özgün ölçek`isPrimary: true`kümesi gibi birincil olarak işaretlenir. (Birincil olmayan düğüm türü yükseltmeleri için, sadece bu atlayın.)

Kolaylık sağlamak *için, Yükseltme-1NodeType-2ScaleSets-ManagedDisks* [şablonu](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.json) ve [parametreler](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json) dosyalarında sizin için gerekli değişiklikler zaten yapılmıştır.

Aşağıdaki bölümlerde şablon değişiklikleri ayrıntılı olarak açıklanacaktır. İsterseniz, açıklamayı atlayabilir ve yükseltme [yordamının bir sonraki adımına devam edebilirsiniz.](#obtain-your-key-vault-references)

### <a name="update-the-cluster-template-with-the-upgraded-scale-set"></a>Yükseltilmiş ölçek kümesiyle küme şablonu güncelleştirme

Burada, birincil düğüm türü için yükseltilmiş bir ölçek kümesi eklemek için özgün küme dağıtım şablonunun bölüm bölüm değişiklikleri verilmiştir.

#### <a name="parameters"></a>Parametreler

Yeni ölçek kümesinin örnek adı, sayısı ve boyutu için parametreler ekleyin. Sayım `vmNodeType1Name` ve boyut değerleri özgün ölçek kümesiyle aynıyken, yeni ölçek kümesine özgü olduğunu unutmayın.

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

**Parametreler dosyası**

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

Dağıtım *şablonu kaynakları* bölümünde, aşağıdakileri göz önünde bulundurarak yeni sanal makine ölçeği kümesini ekleyin:

* Yeni ölçek kümesi, özgünle aynı düğüm türüne başvurur:

    ```json
    "nodeTypeRef": "[parameters('vmNodeType0Name')]",
    ```

* Yeni ölçek kümesi aynı yük dengeleyici arka uç adresi ve alt net başvurular (ancak farklı bir yük dengeleyici gelen NAT havuzu kullanır):

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

* Özgün ölçek kümesi gibi, yeni ölçek kümesi birincil düğüm türü olarak işaretlenir. (Birincil olmayan düğüm türlerini yükseltirken, bu değişikliği atla.)

    ```json
    "isPrimary": true,
    ```

* Özgün ölçek kümesinin aksine, yeni ölçek kümesi yönetilen diskleri kullanmak üzere yükseltilir.

    ```json
    "managedDisk": {
        "storageAccountType": "[parameters('storageAccountType')]"
    }
    ```

Şablon ve parametre dosyalarınızdaki tüm değişiklikleri uyguladıktan sonra, Key Vault başvurularınızı almak ve güncelleştirmeleri kümenize dağıtmak için bir sonraki bölüme gidin.

### <a name="obtain-your-key-vault-references"></a>Key Vault referanslarınızı edinin

Güncelleştirilmiş yapılandırmayı dağıtmak için, önce Anahtar Kasanızda depolanan küme sertifikanıza birkaç başvuru alırsınız. Bu değerleri bulmanın en kolay yolu Azure portalıdır. Gerekenler:

* **Küme sertifikanızın Anahtar Kasa Sı.** Azure'daki Key Vault portalınızdan, **Sertifikaları** > *seçin Istediğiniz sertifika* > **Gizli Tanımlayıcı:**

    ```powershell
    $certUrlValue="https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
    ```

* **Küme sertifikanızın parmak izi.** (Sistem durumu kontrol etmek [için ilk kümeye bağlandıysanız,](#connect-to-the-new-cluster-and-check-health-status) büyük olasılıkla bu var.) Azure portalında aynı sertifika dan **(Sertifikalar** > *İstediğiniz sertifika)* **X.509 SHA-1 Thumbprint (hex)** kopyalayın:

    ```powershell
    $thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
    ```

* **Anahtar Kasanızın Kaynak Kimliği.** Azure portalınızdaki Key Vault'unuzdan **Özellikler** > **Kaynak Kimliği'ni**seçin:

    ```powershell
    $sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
    ```

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma

`parameterFilePath` Ve gerektiği `templateFilePath` gibi ayarlayın ve ardından aşağıdaki komutu çalıştırın:

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

Dağıtım tamamlandığında, küme durumunu yeniden denetleyin ve tüm on düğümün (orijinalde beş, yeni ölçek kümesinde beş) sağlıklı olduğundan emin olun.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-seed-nodes-to-the-new-scale-set"></a>Tohum düğümlerini yeni ölçek kümesine geçirin

Artık orijinal ölçek kümesinin düğümlerini devre dışı bırakmaya hazırız. Bu düğümler devre dışı bırakıldıkça, sistem hizmetleri ve tohum düğümleri, birincil düğüm türü olarak da işaretlendiği için yeni ölçek kümesinin VM'lerine taşınır.

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Tohum düğümlerinin yeni ölçek kümesine geçişini ve *disabling'den* *Devre Dışı Bırakma* durumuna ayarlanan orijinal ölçektedüğümlerin ilerlemesini izlemek için Hizmet Kumaş Gezgini'ni kullanın.

![Devre dışı bırakılan düğümlerin durumunu gösteren Hizmet Kumaş Gezgini](./media/upgrade-managed-disks/service-fabric-explorer-node-status.png)

> [!NOTE]
> Özgün ölçek kümesinin tüm düğümlerinde devre dışı bırakma işleminin tamamlanması biraz zaman alabilir. Veri tutarlılığını garanti etmek için, aynı anda yalnızca bir tohum düğümü değişebilir. Her tohum düğümü değişikliği bir küme güncelleştirmesi gerektirir; böylece bir tohum düğümü yerine iki küme yükseltmeleri (düğüm ekleme ve kaldırma için her biri) gerektirir. Bu örnek senaryodaki beş tohum düğümlerinin yükseltilmesi on küme yükseltmesi ile sonuçlanır.

## <a name="remove-the-original-scale-set"></a>Özgün ölçek kümesini kaldırma

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

Hizmet Kumaş Gezgini'nde, kaldırılan düğümler (ve böylece *Küme Durumu Durumu)* artık *Hata* durumunda görünür.

![Hata durumunda devre dışı bırakılan düğümleri gösteren Hizmet Kumaş Gezgini](./media/upgrade-managed-disks/service-fabric-explorer-disabled-nodes-error-state.png)

Küme Durumu *Durumunu Tamam'a*geri yüklemek için Hizmet Dokusu kümesinden eski düğümleri kaldırın.

```powershell
# Remove node states for the deleted scale set
foreach($name in $nodeNames){
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

![Hata durumunda aşağı düğümleri kaldırılan Hizmet Kumaş Gezgini](./media/upgrade-managed-disks/service-fabric-explorer-healthy-cluster.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu gözden geçirme de, işlem sırasında hizmet kesintilerini önlerken yönetilen diskleri kullanmak için Bir Hizmet Kumaşı kümesinin sanal makine ölçeği kümelerini yükseltmeyi öğrendiniz. İlgili konular hakkında daha fazla bilgi için aşağıdaki kaynaklara göz atın.

Şunları nasıl yapacağınızı öğrenin:

* [Service Fabric kümesi birincil düğüm türünün ölçeğini artırma](service-fabric-scale-up-node-type.md)

* [Yönetilen diskleri kullanmak için ölçek kümesi şablonu dönüştürme](../virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md.md)

* [Hizmet Kumaş düğümü türünü kaldırma](service-fabric-how-to-remove-node-type.md)

Ayrıca bkz:

* [Örnek: Azure yönetilen diskleri kullanmak için küme düğümlerini yükseltme](https://github.com/erikadoyle/service-fabric-scripts-and-templates/tree/managed-disks/templates/nodetype-upgrade-no-outage)

* [Dikey ölçekleme hususları](service-fabric-best-practices-capacity-scaling.md#vertical-scaling-considerations)