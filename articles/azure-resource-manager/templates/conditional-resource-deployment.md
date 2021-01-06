---
title: Şablonlarla koşullu dağıtım
description: Azure Resource Manager şablonunda bir kaynağın koşullu olarak nasıl dağıtılacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 12/17/2020
ms.openlocfilehash: 5650f7fb9f1483f2dc7059607732ecc68cbb7b9d
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934790"
---
# <a name="conditional-deployment-in-arm-templates"></a>ARM şablonlarında koşullu dağıtım

Bazen bir Azure Resource Manager şablonunda (ARM şablonu) bir kaynağı dağıtmanız gerekebilir. `condition`Kaynağın dağıtılıp dağıtılmayacağını belirtmek için öğesini kullanın. Bu öğenin değeri true veya false olarak çözümlenmektedir. Değer true olduğunda kaynak oluşturulur. Değer false olduğunda kaynak oluşturulmaz. Değer yalnızca kaynağın tamamına uygulanabilir.

> [!NOTE]
> Koşullu dağıtım, [alt kaynaklara](child-resource-name-type.md)basamaklandırmaz. Bir kaynağı ve alt kaynaklarını koşullu olarak dağıtmak istiyorsanız, her kaynak türüne aynı koşulu uygulamanız gerekir.

## <a name="new-or-existing-resource"></a>Yeni veya mevcut kaynak

Koşullu dağıtımı, yeni bir kaynak oluşturmak veya var olan bir kaynağı kullanmak için kullanabilirsiniz. Aşağıdaki örnek, `condition` Yeni bir depolama hesabı dağıtmak veya var olan bir depolama hesabını kullanmak için nasıl kullanılacağını gösterir.

```json
{
  "condition": "[equals(parameters('newOrExisting'),'new')]",
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2017-06-01",
  "name": "[variables('storageAccountName')]",
  "location": "[parameters('location')]",
  "sku": {
    "name": "[variables('storageAccountType')]"
  },
  "kind": "Storage",
  "properties": {}
}
```

Parametre `newOrExisting` **Yeni** olarak ayarlandığında, koşul true olarak değerlendirilir. Depolama hesabı dağıtılır. Ancak, `newOrExisting` **var** olarak ayarlandığında, koşul yanlış olarak değerlendirilir ve depolama hesabı dağıtılmaz.

Öğesini kullanan bir örnek şablon için `condition` , bkz. [Yeni veya var olan sanal ağ, depolama ve genel IP ile VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="allow-condition"></a>Koşula izin ver

Bir koşula izin verilip verilmeyeceğini gösteren bir parametre değeri geçirebilirsiniz. Aşağıdaki örnek bir SQL Server dağıtır ve isteğe bağlı olarak Azure IP 'lerine izin verir.

```json
{
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2015-05-01-preview",
  "name": "[parameters('serverName')]",
  "location": "[parameters('location')]",
  "properties": {
    "administratorLogin": "[parameters('administratorLogin')]",
    "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
    "version": "12.0"
  },
  "resources": [
    {
      "condition": "[parameters('allowAzureIPs')]",
      "type": "firewallRules",
      "apiVersion": "2015-05-01-preview",
      "name": "AllowAllWindowsAzureIps",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
      ],
      "properties": {
        "endIpAddress": "0.0.0.0",
        "startIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Tüm şablon için bkz. [Azure SQL mantıksal sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

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
