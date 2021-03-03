---
title: Şablonlarla koşullu dağıtım
description: Azure Resource Manager şablonunda bir kaynağın koşullu olarak nasıl dağıtılacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 409d258d7dfe3ed186e5cf97cc0dbe6dc149b849
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741183"
---
# <a name="conditional-deployment-in-arm-templates"></a>ARM şablonlarında koşullu dağıtım

Bazen bir Azure Resource Manager şablonunda (ARM şablonunda) veya Bıcep dosyasında bir kaynağı dağıtmanız gerekebilir. JSON şablonları için, `condition` kaynağın dağıtılıp dağıtılmayacağını belirtmek için öğesini kullanın. Bıcep için, `if` kaynağın dağıtılıp dağıtılmadığını belirtmek için anahtar sözcüğünü kullanın. Koşulun değeri true veya false olarak çözümlenmektedir. Değer true olduğunda kaynak oluşturulur. Değer false olduğunda kaynak oluşturulmaz. Değer yalnızca kaynağın tamamına uygulanabilir.

> [!NOTE]
> Koşullu dağıtım, [alt kaynaklara](child-resource-name-type.md)basamaklandırmaz. Bir kaynağı ve alt kaynaklarını koşullu olarak dağıtmak istiyorsanız, her kaynak türüne aynı koşulu uygulamanız gerekir.

## <a name="deploy-condition"></a>Dağıtım koşulu

Bir kaynağın dağıtılıp dağıtılmadığını gösteren bir parametre değeri geçirebilirsiniz. Aşağıdaki örnek, bir DNS bölgesini koşullu olarak dağıtır.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "deployZone": {
      "type": "bool"
    }
  },
  "functions": [],
  "resources": [
    {
      "condition": "[parameters('deployZone')]",
      "type": "Microsoft.Network/dnsZones",
      "apiVersion": "2018-05-01",
      "name": "myZone",
      "location": "global"
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param deployZone bool

resource dnsZone 'Microsoft.Network/dnszones@2018-05-01' = if (deployZone) {
  name: 'myZone'
  location: 'global'
}
```

---

Daha karmaşık bir örnek için bkz. [Azure SQL mantıksal sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="new-or-existing-resource"></a>Yeni veya mevcut kaynak

Koşullu dağıtımı, yeni bir kaynak oluşturmak veya var olan bir kaynağı kullanmak için kullanabilirsiniz. Aşağıdaki örnek, yeni bir depolama hesabının nasıl dağıtılacağını ya da mevcut bir depolama hesabını nasıl kullanacağınızı gösterir.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string"
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "newOrExisting": {
      "type": "string",
      "defaultValue": "new",
      "allowedValues": [
        "new",
        "existing"
      ]
    }
  },
  "functions": [],
  "resources": [
    {
      "condition": "[equals(parameters('newOrExisting'), 'new')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param storageAccountName string
param location string = resourceGroup().location

@allowed([
  'new'
  'existing'
])
param newOrExisting string = 'new'

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = if (newOrExisting == 'new') {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

---

Parametre `newOrExisting` **Yeni** olarak ayarlandığında, koşul true olarak değerlendirilir. Depolama hesabı dağıtılır. Ancak, `newOrExisting` **var** olarak ayarlandığında, koşul yanlış olarak değerlendirilir ve depolama hesabı dağıtılmaz.

Öğesini kullanan bir örnek şablon için `condition` , bkz. [Yeni veya var olan sanal ağ, depolama ve genel IP ile VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="runtime-functions"></a>Çalışma zamanı işlevleri

Koşullu olarak dağıtılan bir kaynakla [başvuru](template-functions-resource.md#reference) veya [liste](template-functions-resource.md#list) işlevi kullanıyorsanız, işlev, kaynak dağıtılmasa bile değerlendirilir. İşlev mevcut olmayan bir kaynağa başvuruyorsa bir hata alırsınız.

İşlevin yalnızca kaynak dağıtıldığında koşullara göre değerlendirildiğinden emin olmak için [IF](template-functions-logical.md#if) işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla ve kullanan örnek bir şablon için [IF işlevine](template-functions-logical.md#if) bakın `if` `reference` .

Bir kaynağı koşullu kaynağa [bağlı olarak](define-resource-dependency.md) , diğer tüm kaynaklar gibi ayarlarsınız. Koşullu bir kaynak dağıtıldığında Azure Resource Manager, gerekli bağımlılıklardan otomatik olarak kaldırır.

## <a name="complete-mode"></a>Mod Tamam

Bir şablonu, [tamamen modu](deployment-modes.md) ile dağıtırsanız ve bir kaynak dağıtılmamışsa `condition` , false olarak değerlendirilirse, sonuç, şablonu dağıtmak için kullandığınız REST API sürümüne bağlıdır. 2019-05-10 'den önceki bir sürümü kullanıyorsanız, kaynak **silinmez**. 2019-05-10 veya sonraki bir sürümü kullanarak kaynak **silinir**. Azure PowerShell ve Azure CLı 'nın en son sürümleri, koşul false olduğunda kaynağı siler.

## <a name="next-steps"></a>Sonraki adımlar

* Koşullu dağıtımı içeren Microsoft Learn bir modül için bkz. [GELIŞMIŞ ARM şablon özelliklerini kullanarak karmaşık bulut dağıtımlarını yönetme](/learn/modules/manage-deployments-advanced-arm-template-features/).
* Şablon oluşturma hakkında öneriler için bkz. [ARM şablonu en iyi uygulamaları](template-best-practices.md).
* Bir kaynağın birden çok örneğini oluşturmak için, bkz. [ARM şablonlarında kaynak yinelemesi](copy-resources.md).
