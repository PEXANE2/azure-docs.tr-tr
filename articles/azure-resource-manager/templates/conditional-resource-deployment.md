---
title: Şablonlarla koşullu dağıtım
description: Azure Resource Manager şablonunda bir kaynağın koşullu olarak nasıl dağıtılacağını açıklar.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: b4ee733e81f3a923f268f33f5826387059923bcb
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122007"
---
# <a name="conditional-deployment-in-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarda koşullu dağıtım

Bazen bir şablonda kaynak dağıtmanız gerekebilir. Kaynağın dağıtılıp dağıtılmayacağını belirtmek için `condition` öğesini kullanın. Bu öğenin değeri true veya false olarak çözümlenmektedir. Değer true olduğunda kaynak oluşturulur. Değer false olduğunda kaynak oluşturulmaz. Değer yalnızca kaynağın tamamına uygulanabilir.

## <a name="new-or-existing-resource"></a>Yeni veya mevcut kaynak

Koşullu dağıtımı, yeni bir kaynak oluşturmak veya var olan bir kaynağı kullanmak için kullanabilirsiniz. Aşağıdaki örnek, yeni bir depolama hesabı dağıtmak veya var olan bir depolama hesabını kullanmak için koşulun nasıl kullanılacağını gösterir.

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

**Neworexıting** parametresi **New**olarak ayarlandığında, koşul true olarak değerlendirilir. Depolama hesabı dağıtılır. Ancak, **Neworexıting** özelliği **var**olarak ayarlandığında, koşul yanlış olarak değerlendirilir ve depolama hesabı dağıtılmaz.

`condition` öğesini kullanan tüm örnek bir şablon için, [Yeni veya var olan sanal ağ, depolama ve genel IP Ile VM 'ye](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions)bakın.

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

İşlevin yalnızca kaynak dağıtıldığında koşullara göre değerlendirildiğinden emin olmak için [IF](template-functions-logical.md#if) işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla IF ve Reference kullanan bir örnek şablon için [IF işlevine](template-functions-logical.md#if) bakın.

Bir kaynağı koşullu kaynağa [bağlı olarak](define-resource-dependency.md) , diğer tüm kaynaklar gibi ayarlarsınız. Koşullu bir kaynak dağıtıldığında Azure Resource Manager, gerekli bağımlılıklardan otomatik olarak kaldırır.

## <a name="condition-with-complete-mode"></a>Tamamlanmış mod ile koşul

Bir şablonu, [tamamlanma modu](deployment-modes.md) ile dağıtırsanız ve koşul false olarak değerlendirdiği için bir kaynak dağıtılmamışsa, sonuç, şablonu dağıtmak için kullandığınız REST API sürümüne bağlıdır. 2019-05-10 'den önceki bir sürümü kullanıyorsanız, kaynak **silinmez**. 2019-05-10 veya sonraki bir sürümü kullanarak kaynak **silinir**. Azure PowerShell ve Azure CLı 'nın en son sürümleri, koşul false olduğunda kaynağı siler.

## <a name="next-steps"></a>Sonraki adımlar

* Şablon oluşturma hakkında öneriler için bkz. [Azure Resource Manager şablonu en iyi yöntemleri](template-best-practices.md).
* Bir kaynağın birden çok örneğini oluşturmak için [Azure Resource Manager şablonlarda kaynak, özellik veya değişken yineleme](create-multiple-instances.md)bölümüne bakın.