---
title: Bir çalışma alanı oluşturmak için Azure Resource Manager şablonu kullanma
titleSuffix: Azure Machine Learning service
description: Yeni bir Azure Machine Learning hizmeti çalışma alanı oluşturmak için Azure Resource Manager şablonu kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 07/16/2019
ms.custom: seoapril2019
ms.openlocfilehash: 89c24512e50dedbf10d145088ec77c2e6e303d1e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69873160"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için bir çalışma alanı oluşturmak üzere bir Azure Resource Manager şablonu kullanma

Bu makalede Azure Resource Manager şablonları kullanarak Azure Machine Learning hizmet çalışma alanı oluşturmanın birkaç yolunu öğreneceksiniz. Kaynak Yöneticisi şablonu, kaynakları tek ve eşgüdümlü bir işlem olarak oluşturmayı kolaylaştırır. Şablon, bir dağıtım için gereken kaynakları tanımlayan bir JSON belgesidir. Ayrıca, dağıtım parametrelerini de belirtebilir. Parametreler, şablon kullanılırken giriş değerleri sağlamak için kullanılır.

Daha fazla bilgi için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. Bunlardan birine sahip değilseniz, [Azure Machine Learning hizmetinin ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* CLı 'dan bir şablon kullanmak için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) ya da [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerekir.

## <a name="resource-manager-template"></a>Resource Manager şablonu

Aşağıdaki Kaynak Yöneticisi şablonu, Azure Machine Learning bir hizmet çalışma alanı ve ilişkili Azure kaynakları oluşturmak için kullanılabilir:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Bu şablon aşağıdaki Azure hizmetlerini oluşturur:

* Azure kaynak grubu
* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning çalışma alanı

Kaynak grubu, Hizmetleri tutan kapsayıcıdır. Azure Machine Learning çalışma alanı için çeşitli hizmetler gereklidir.

Örnek şablonda iki parametre vardır:

* Kaynak grubunun ve hizmetlerin oluşturulacağı **konum** .

    Şablon, çoğu kaynak için seçtiğiniz konumu kullanacaktır. Özel durum, diğer hizmetlerin bulunduğu tüm konumlarda kullanılamayan Application Insights hizmetidir. Kullanılabilir olmayan bir konum seçerseniz, hizmet Orta Güney ABD konumunda oluşturulur.

* Azure Machine Learning çalışma alanının kolay adı olan **çalışma alanı adı**.

    Diğer hizmetlerin adları rastgele oluşturulur.

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager şablonları yaz](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonlarıyla uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft. MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Özel şablondan kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)bölümündeki adımları izleyin. __Şablonu Düzenle__ ekranına geldiğinizde, şablonu bu belgeden yapıştırın.
1. Şablonu kullanmak için __Kaydet__ ' i seçin. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul edin:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: Hizmetleri içerecek bir kaynak grubu seçin veya oluşturun.
   * Çalışma alanı adı: Oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3 ile 33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve '-' içerebilir.
   * Konumuna Kaynakların oluşturulacağı konumu seçin.

Daha fazla bilgi için bkz. [özel şablondan kaynak dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Bu örnekte, şablonu geçerli dizinde adlı `azuredeploy.json` bir dosyaya kaydettiğiniz varsayılır:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure POWERSHELL](../../azure-resource-manager/resource-group-template-deploy.md) [SAS belirteci ve Azure PowerShell ile özel kaynak yöneticisi şablonu dağıtma](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI kullanma

Bu örnekte, şablonu geçerli dizinde adlı `azuredeploy.json` bir dosyaya kaydettiğiniz varsayılır:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus
```

Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) ve [SAS BELIRTECI ve Azure clı ile özel kaynak yöneticisi şablonu dağıtma](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault erişim ilkesi ve Azure Resource Manager şablonları

Çalışma alanını ve ilişkili kaynakları (Azure Key Vault dahil) oluşturmak için bir Azure Resource Manager şablonu kullandığınızda, birden çok kez. Örneğin, şablonu bir sürekli tümleştirme ve dağıtım işlem hattının bir parçası ile aynı parametrelerle birden çok kez kullanmak.

Şablonlar aracılığıyla kaynak oluşturma işlemlerinin çoğu ıdempotent, ancak Key Vault şablon her kullanıldığında erişim ilkelerini temizler. Erişim ilkelerinin temizlenmesi, onu kullanan var olan bir çalışma alanının Key Vault erişimini keser. Örneğin, Azure Notebooks VM 'yi durdur/oluştur işlevleri başarısız olabilir.  

Bu sorundan kaçınmak için aşağıdaki yaklaşımlardan birini öneririz:

*  Aynı parametreler için şablonu birden çok kez dağıtmayın. Ya da yeniden oluşturmak için şablonu kullanmadan önce mevcut kaynakları silin.
  
* Key Vault erişim ilkelerini inceleyin ve sonra şablonun accessPolicies özelliğini ayarlamak için bu ilkeleri kullanın.
* Key Vault kaynağının zaten var olup olmadığını denetleyin. Varsa, şablon aracılığıyla yeniden oluşturmayın. Örneğin, zaten varsa Key Vault kaynağı oluşturmayı devre dışı bırakmanızı sağlayan bir parametre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıtın](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Visual Studio aracılığıyla Azure Kaynak grupları oluşturma ve dağıtma](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
