---
title: Azure Resource Manager şablondaki çıktılar
description: Azure Resource Manager şablonunda çıkış değerlerinin nasıl tanımlanacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 0ff1e3cb71bd1bf5ee947eb5204839d48103628b
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827939"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Azure Resource Manager şablondaki çıktılar

Bu makalede, Azure Resource Manager şablonunuzda çıkış değerlerinin nasıl tanımlanacağı açıklanmaktadır. Dağıtılan kaynaklardan değer döndürihtiyacınız olduğunda çıktıları kullanırsınız.

## <a name="define-output-values"></a>Çıkış değerlerini tanımla

Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ nasıl dönegösterdiğini göstermektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>Koşullu çıkış

Çıktılar bölümünde, koşullu bir değer döndürebilirsiniz. Genellikle, bir kaynağı [koşullu olarak dağıttığınız](conditional-resource-deployment.md) durumunda çıkışların durumunu kullanırsınız. Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ, yeni bir birinin dağıtılıp dağıtıldığına göre nasıl koşullu olarak döndürülüp döndürülmeyeceğini göstermektedir:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Koşullu çıkışın basit bir örneği için bkz. [koşullu çıkış şablonu](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

## <a name="linked-templates"></a>Bağlı şablonlar

Bağlı bir şablondan çıkış değerini almak için, üst şablondaki [başvuru](resource-group-template-functions-resource.md#reference) işlevini kullanın. Üst şablondaki sözdizimi şöyledir:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Bağlantılı şablondan çıkış özelliği alınırken, özellik adı bir tire içeremez.

Aşağıdaki örnek, bir yük dengeleyicide, bağlantılı şablondan bir değer alarak IP adresinin nasıl ayarlanacağını gösterir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

[İç içe bir şablonun](resource-group-linked-templates.md#nested-template)çıktılar bölümünde `reference` işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe yerleştirilmiş bir şablonda döndürmek için, iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

## <a name="get-output-values"></a>Çıkış değerlerini al

Dağıtım başarılı olduğunda, çıkış değerleri dağıtım sonuçlarında otomatik olarak döndürülür.

Dağıtım geçmişinden çıkış değerleri almak için, komut dosyası kullanabilirsiniz.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group deployment show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, çıkışları kullanmaya yönelik senaryolar gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
|[Değişkenleri Kopyala](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri verir. Hiçbir kaynak dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Genel bir IP adresi oluşturur ve kaynak KIMLIĞINI verir. |
|[Yük Dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Önceki şablonun bağlantıları. Yük dengeleyiciyi oluştururken çıktıda kaynak KIMLIĞINI kullanır. |

## <a name="next-steps"></a>Sonraki adımlar

* Çıktılar için kullanılabilen özellikler hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
* Çıktılar oluşturma hakkında öneriler için bkz. [en iyi yöntemler-çıktılar](template-best-practices.md#outputs).
