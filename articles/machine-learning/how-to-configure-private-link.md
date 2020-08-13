---
title: Özel uç nokta yapılandırma (Önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çalışma alanınıza sanal bir ağdan güvenli bir şekilde erişmek için Azure özel bağlantısı 'nı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/28/2020
ms.openlocfilehash: bdb7ba30d9fa2d0bd1eff9368d6e30e516b53895
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192719"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace-preview"></a>Azure Machine Learning çalışma alanı için Azure özel bağlantısını yapılandırma (Önizleme)

Bu belgede, Azure özel bağlantısının Azure Machine Learning çalışma alanıyla nasıl kullanılacağını öğrenirsiniz. 

> [!IMPORTANT]
> Azure Machine Learning çalışma alanı ile Azure özel bağlantısı 'nın kullanımı Şu anda genel önizlemededir. Bu işlevsellik yalnızca **ABD Doğu** ve **ABD Batı 2** bölgelerinde kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure özel bağlantısı, özel bir uç nokta kullanarak çalışma alanınıza bağlanmanızı sağlar. Özel uç nokta, sanal ağınız içindeki özel IP adresleri kümesidir. Daha sonra çalışma alanınıza erişimi yalnızca özel IP adresleri üzerinden oluşacak şekilde sınırlayabilirsiniz. Özel bağlantı, veri taşması riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel bağlantı](/azure/private-link/private-link-overview) makalesi.

> [!IMPORTANT]
> Azure özel bağlantısı, çalışma alanını silme veya işlem kaynaklarını yönetme gibi Azure denetim düzlemi 'ni (yönetim işlemleri) etkilemez. Örneğin, bir işlem hedefi oluşturma, güncelleştirme veya silme. Bu işlemler, genel Internet üzerinden normal olarak gerçekleştirilir.
>
> Azure Machine Learning işlem örnekleri önizlemesi, özel bağlantının etkinleştirildiği bir çalışma alanında desteklenmez.
>
> Mozilla Firefox kullanıyorsanız, çalışma alanınızın özel uç noktasına erişmeye çalışırken sorunlarla karşılaşabilirsiniz. Bu sorun, Mozilla 'de HTTPS üzerinden DNS ile ilişkili olabilir. Google Chrome 'un Microsoft Edge 'i geçici çözüm olarak kullanmanızı öneririz.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Özel uç nokta kullanan bir çalışma alanı oluşturma

> [!IMPORTANT]
> Şu anda yalnızca yeni bir Azure Machine Learning çalışma alanı oluştururken özel bir uç noktanın etkinleştirilmesini destekliyoruz.

' De şablon, [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) özel bir uç nokta ile çalışma alanı oluşturmak için kullanılabilir.

Özel uç noktalar dahil bu şablonu kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md).

## <a name="using-a-workspace-over-a-private-endpoint"></a>Özel bir uç nokta üzerinde çalışma alanı kullanma

Çalışma alanına yönelik iletişime yalnızca sanal ağdan izin verildiğinden, çalışma alanını kullanan tüm geliştirme ortamları sanal ağın üyesi olmalıdır. Örneğin, sanal ağdaki bir sanal makine.

> [!IMPORTANT]
> Microsoft, bağlantının geçici kesintisini önlemek için özel bağlantı etkinleştirildikten sonra çalışma alanına bağlanan makinelerde DNS önbelleğini temizlemeye öneriyor. 

Azure sanal makineleri hakkında daha fazla bilgi için bkz. [sanal makineler belgeleri](/azure/virtual-machines/).


## <a name="using-azure-storage"></a>Azure Depolama’yı kullanma

Çalışma alanınız tarafından kullanılan Azure depolama hesabının güvenliğini sağlamak için sanal ağın içine yerleştirin.

Depolama hesabını sanal ağa yerleştirme hakkında daha fazla bilgi için bkz. [çalışma alanınız için bir depolama hesabı kullanma](how-to-enable-virtual-network.md#use-a-storage-account-for-your-workspace).

> [!WARNING]
> Azure Machine Learning, özel bağlantısı etkin olan bir Azure depolama hesabı kullanmayı desteklemez.

## <a name="using-azure-key-vault"></a>Azure Key Vault kullanma

Çalışma alanınız tarafından kullanılan Azure Key Vault güvenli hale getirmek için, sanal ağın içine yerleştirebilir veya özel bağlantıyı etkinleştirebilirsiniz.

Anahtar kasasını sanal ağa yerleştirme hakkında daha fazla bilgi için bkz. [çalışma alanınıza sahip bir Anahtar Kasası örneği kullanma](how-to-enable-virtual-network.md#key-vault-instance).

Anahtar Kasası için özel bağlantıyı etkinleştirme hakkında daha fazla bilgi için bkz. [Azure özel bağlantısı ile Key Vault tümleştirme](/azure/key-vault/private-link-service).

## <a name="using-azure-kubernetes-services"></a>Azure Kubernetes hizmetlerini kullanma

Çalışma alanınız tarafından kullanılan Azure Kubernetes hizmetlerini güvenli hale getirmek için, sanal bir ağ içine yerleştirin. Daha fazla bilgi için bkz. [Azure Kubernetes hizmetlerini çalışma alanım Ile kullanma](how-to-enable-virtual-network.md#aksvnet).

Azure Machine Learning artık özel bağlantısı etkin olan bir Azure Kubernetes hizmetini kullanmayı desteklemektedir.
Özel bir AKS kümesi oluşturmak için belgeleri [burada](https://docs.microsoft.com/azure/aks/private-clusters) izleyin

## <a name="azure-container-registry"></a>Azure Container Registry

Sanal ağ içinde Azure Container Registry güvenliğini sağlama hakkında bilgi için bkz. [Azure Container Registry kullanma](how-to-enable-virtual-network.md#azure-container-registry).

> [!IMPORTANT]
> Azure Machine Learning çalışma alanınız için özel bağlantı kullanıyorsanız ve çalışma alanınızın Azure Container Registry bir sanal ağa yerleştirirseniz, aşağıdaki Azure Resource Manager şablonunu da uygulamanız gerekir. Bu şablon, çalışma alanınızın özel bağlantı üzerinden ACR ile iletişim kurmasını sağlar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "keyVaultArmId": {
      "type": "string"
      },
      "workspaceName": {
      "type": "string"
      },
      "containerRegistryArmId": {
      "type": "string"
      },
      "applicationInsightsArmId": {
      "type": "string"
      },
      "storageAccountArmId": {
      "type": "string"
      },
      "location": {
      "type": "string"
      }
  },
  "resources": [
      {
      "type": "Microsoft.MachineLearningServices/workspaces",
      "apiVersion": "2019-11-01",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "identity": {
          "type": "SystemAssigned"
      },
      "sku": {
          "tier": "enterprise",
          "name": "enterprise"
      },
      "properties": {
          "sharedPrivateLinkResources":
  [{"Name":"Acr","Properties":{"PrivateLinkResourceId":"[concat(parameters('containerRegistryArmId'), '/privateLinkResources/registry')]","GroupId":"registry","RequestMessage":"Approve","Status":"Pending"}}],
          "keyVault": "[parameters('keyVaultArmId')]",
          "containerRegistry": "[parameters('containerRegistryArmId')]",
          "applicationInsights": "[parameters('applicationInsightsArmId')]",
          "storageAccount": "[parameters('storageAccountArmId')]"
      }
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning çalışma alanınızı güvenli hale getirme hakkında daha fazla bilgi için bkz. [Enterprise Security](concept-enterprise-security.md) article.