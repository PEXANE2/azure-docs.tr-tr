---
title: Azure Resource Manager şablonu kullanarak Service Fabric kümesi oluşturma
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak bir Azure Service Fabric test kümesi oluşturacaksınız.
author: erikadoyle
ms.service: service-fabric
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: edoyle
ms.date: 04/24/2020
ms.openlocfilehash: 60771d5a188df5dfeca3530a551a116c870e63f5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82149336"
---
# <a name="quickstart-create-a-service-fabric-cluster-using-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Service Fabric kümesi oluşturma

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric *küme* , mikro hizmetlerinizin dağıtıldığı ve yönetildiği, ağa bağlı bir sanal makineler kümesidir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu makalede, Azure 'da Kaynak Yöneticisi kullanarak Service Fabric test kümesinin nasıl dağıtılacağı açıklanır. Bu beş düğümlü Windows kümesi, otomatik olarak imzalanan bir sertifikayla güvenli hale getirilir ve bu nedenle yalnızca eğitim amaçlarıyla (üretim iş yükleri yerine) yöneliktir.

Şablonu dağıtmak için Azure PowerShell kullanacağız. Azure PowerShell ek olarak, Azure portal, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-portal.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

### <a name="install-service-fabric-sdk-and-powershell-modules"></a>Service Fabric SDK ve PowerShell modülleri 'ni yükler

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:

* [SERVICE fabrıc SDK ve PowerShell modülünü](service-fabric-get-started.md)yükler.

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps)'i yükler.

### <a name="download-the-sample-template-and-certificate-helper-script"></a>Örnek şablonu ve sertifika Yardımcısı betiğini indirin

[Azure Resource Manager hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates) deposunu kopyalayın veya indirin. Alternatif olarak, *Service-Fabric-Secure-Cluster-5-node-1-NodeType* klasöründen kullanacağınız dosyaları yerel olarak aşağı kopyalayın:

* [New-ServiceFabricClusterCertificate. ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/service-fabric-secure-cluster-5-node-1-nodetype/New-ServiceFabricClusterCertificate.ps1)
* [azuredeploy. JSON](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/service-fabric-secure-cluster-5-node-1-nodetype/azuredeploy.json)
* [azuredeploy.parameters.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/service-fabric-secure-cluster-5-node-1-nodetype/azuredeploy.parameters.json)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure 'da oturum açın ve Service Fabric kümenizi oluşturmak için kullanılacak aboneliği belirleyin.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

### <a name="create-a-self-signed-certificate-stored-in-key-vault"></a>Key Vault depolanan kendinden imzalı bir sertifika oluşturun

Service Fabric, [bir kümeyi güvenli hale](./service-fabric-cluster-security.md) getirmek ve uygulama güvenlik özellikleri sağlamak ve bu sertifikaları yönetmek Için [Key Vault](../key-vault/general/overview.md) X. 509.440 sertifikalarını kullanır. Başarılı küme oluşturma, düğümden düğüme iletişimi etkinleştirmek için bir küme sertifikası gerektirir. Bu hızlı başlangıç testi kümesini oluşturmak amacıyla, küme kimlik doğrulaması için otomatik olarak imzalanan bir sertifika oluşturacağız. Üretim iş yükleri doğru yapılandırılmış bir Windows Server sertifika hizmeti kullanılarak veya onaylanmış bir sertifika yetkilisinden (CA) oluşturulmuş sertifikalar gerektirir.

```powershell
# Designate unique (within cloudapp.azure.com) names for your resources
$resourceGroupName = "SFQuickstartRG"
$keyVaultName = "SFQuickstartKV"

# Create a new resource group for your Key Vault and Service Fabric cluster
New-AzResourceGroup -Name $resourceGroupName -Location SouthCentralUS

# Create a Key Vault enabled for deployment
New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $resourceGroupName -Location SouthCentralUS -EnabledForDeployment

# Generate a certificate and upload it to Key Vault
.\New-ServiceFabricClusterCertificate.ps1
```

Komut dosyası sizden aşağıdakileri ister ( *Certdnsname* ve *keyvaultname* değerlerini aşağıdaki örnek değerlerden değiştirdiğinizden emin olun):

* **Parola:** Parola! 1
* **Certdnsname:** *sfquickstart*. southcentralus.cloudapp.Azure.com
* **Keyvaultname:** *sfquickstartkv*
* **Keyvaultsecretname:** clustercert

Tamamlandıktan sonra betik, şablon dağıtımı için gerekli parametre değerlerini sağlar. Bunları aşağıdaki değişkenlerde depoladığınızdan emin olun, çünkü küme şablonunuzu dağıtmak için gerekecektir:

```powershell
$sourceVaultId = "<Source Vault Resource Id>"
$certUrlValue = "<Certificate URL>"
$certThumbprint = "<Certificate Thumbprint>"
```

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype). Bu makalenin şablonu burada görüntülenemeyecek kadar uzun. Şablonu görüntülemek için bkz https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/azuredeploy.json..

Şablonda birden çok Azure kaynağı tanımlanmış:

* [Microsoft. Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft. Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft. Network/Publicıpaddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft. Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers)
* [Microsoft.Compute/virtualMachineScaleSets](/azure/templates/microsoft.compute/virtualmachinescalesets)
* [Microsoft. ServiceFabric/kümeler](/azure/templates/microsoft.servicefabric/clusters)

Azure Service Fabric ile ilgili daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?sort=Popular&term=service+fabric).

### <a name="customize-the-parameters-file"></a>Parametreler dosyasını özelleştirme

*Azuredeploy. Parameters. JSON* ' u açın ve parametre değerlerini düzenleyerek şunları yapabilirsiniz:

* **clusterName** , küme sertifikanızı oluştururken *certdnsname* için sağladığınız değerle eşleşir
* **AdminUserName** varsayılan *Gen-UNIQUE* Token dışında bir değerdir
* **adminPassword** varsayılan *genel parola* belirtecinden farklı bir değerdir
* **certificateThumbprint**, **Sourcevaultresourceıd**ve **certificateurlvalue** tüm boş dizlardır (`""`)

Örneğin:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "value": "sfquickstart"
    },
    "adminUsername": {
      "value": "testadm"
    },
    "adminPassword": {
      "value": "Password#1234"
    },
    "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultResourceId": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    }
  }
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablon ve parametre dosyalarınızın yollarını değişkenlerde depolayın, sonra şablonu dağıtın.

```powershell
$templateFilePath = "<full path to azuredeploy.json>"
$parameterFilePath = "<full path to azuredeploy.parameters.json>"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $certThumbprint `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultResourceId $sourceVaultId `
    -Verbose
```

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtım tamamlandıktan sonra, çıktıdaki `managementEndpoint` değeri bulun ve bir web tarayıcısında [Service Fabric Explorer](./service-fabric-visualizing-your-cluster.md)kümenizi görüntülemek için adresi açın.

![Yeni kümeyi gösteren Service Fabric Explorer](./media/quickstart-cluster-template/service-fabric-explorer.png)

Ayrıca, Azure portal içindeki hizmet Gezgini kaynak dikey penceresinden Service Fabric Explorer uç noktasını bulabilirsiniz.

![Service Fabric Explorer uç noktasını gösteren Service Fabric kaynak dikey penceresi](./media/quickstart-cluster-template/service-fabric-explorer-endpoint-azure-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubundaki kaynakları silen kaynak grubunu silin.

```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Özel bir Azure Service Fabric küme şablonu oluşturma hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Service Fabric kümesi Kaynak Yöneticisi şablonu oluşturma](service-fabric-cluster-creation-create-template.md)
