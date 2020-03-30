---
title: Şablonlarla koşullu dağıtım
description: Azure Kaynak Yöneticisi şablonundaki kaynağın koşullu olarak nasıl dağıtılancaya kadar dağıtılabildiğini açıklar.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: f170710118c0e3de6f3643b6216ed55b83b5c7df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153429"
---
# <a name="conditional-deployment-in-arm-templates"></a>ARM şablonlarında koşullu dağıtım

Bazen bir kaynağı azure kaynak yöneticisi (ARM) şablonuna isteğe bağlı olarak dağıtmanız gerekir. Kaynağın `condition` dağıtılıp dağıtılmadığını belirtmek için öğeyi kullanın. Bu öğenin değeri doğru veya yanlış olarak çözülür. Değer doğru olduğunda, kaynak oluşturulur. Değer yanlış olduğunda, kaynak oluşturulmaz. Değer yalnızca tüm kaynağa uygulanabilir.

## <a name="new-or-existing-resource"></a>Yeni veya varolan kaynak

Yeni bir kaynak oluşturmak veya varolan bir kaynak kullanmak için koşullu dağıtımı kullanabilirsiniz. Aşağıdaki örnek, yeni bir depolama hesabı dağıtmak veya varolan bir depolama hesabını kullanmak için koşulun nasıl kullanılacağını gösterir.

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

Parametre **newOrExisting** **yeni**olarak ayarlandığında, koşul doğru olarak değerlendirilir. Depolama hesabı dağıtıldı. Ancak, **newOrExisting** **varolacak**şekilde ayarlandığında, koşul false olarak değerlendirilir ve depolama hesabı dağıtılmayız.

Öğeyi `condition` kullanan tam bir örnek şablon için, [vm'ye yeni veya varolan Sanal Ağ, Depolama ve Genel IP'ye](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions)bakın.

## <a name="allow-condition"></a>Koşula izin ver

Bir koşula izin verilip verilmediğini gösteren bir parametre değeri geçirebilirsiniz. Aşağıdaki örnekte bir SQL sunucusu dağıtılır ve isteğe bağlı olarak Azure IP'lerine izin verir.

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

Şablonun tamamı için [Azure SQL mantıksal sunucusuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server)bakın.

## <a name="runtime-functions"></a>Çalışma zamanı işlevleri

Koşullu olarak dağıtılan bir kaynakla bir [başvuru](template-functions-resource.md#reference) veya [liste](template-functions-resource.md#list) işlevi kullanırsanız, kaynak dağıtılsa bile işlev değerlendirilir. İşlev var olmayan bir kaynağa başvuruyorsa bir hata alırsınız.

İşlevin yalnızca kaynak dağıtıldığındaki koşullar için değerlendirildiğinden emin olmak için [if](template-functions-logical.md#if) işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla if ve başvuru kullanan örnek şabloniçin [if işlevini](template-functions-logical.md#if) görün.

Bir kaynağı koşullu kaynağa tam olarak diğer kaynaklarda olduğu gibi [bağımlı olarak](define-resource-dependency.md) ayarlarsınız. Koşullu bir kaynak dağıtılmadığında, Azure Kaynak Yöneticisi kaynağı otomatik olarak gerekli bağımlılıklardan kaldırır.

## <a name="condition-with-complete-mode"></a>Tam modlu koşul

[Tam modlu](deployment-modes.md) bir şablon dağıtırsanız ve koşul yanlış olarak değerlendirildiği için bir kaynak dağıtılmazsa, sonuç şablonu dağıtmak için hangi REST API sürümünü kullandığınıza bağlıdır. Bir sürümü 2019-05-10'dan önce kullanıyorsanız, kaynak **silinmez.** 2019-05-10 veya sonrası ile kaynak **silinir.** Azure PowerShell ve Azure CLI'nin en son sürümleri, koşul yanlış olduğunda kaynağı siler.

## <a name="next-steps"></a>Sonraki adımlar

* Şablon oluşturma yla ilgili öneriler için [ARM şablonu en iyi uygulamaları'na](template-best-practices.md)bakın.
* Bir kaynağın birden çok örneğini oluşturmak için [ARM şablonlarında Kaynak yinelemesine](copy-resources.md)bakın.