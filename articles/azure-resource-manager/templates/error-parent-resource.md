---
title: Üst kaynak hataları
description: Azure Resource Manager şablonunda bir üst kaynakla çalışırken hataların nasıl çözümleneceğini açıklar.
ms.topic: troubleshooting
ms.date: 08/01/2018
ms.openlocfilehash: 474cb85d16382136e24e5502b87ba8a1a65488ef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84673669"
---
# <a name="resolve-errors-for-parent-resources"></a>Üst kaynaklar için hataları çözümleme

Bu makalede, bir üst kaynağa bağımlı olan bir kaynağı dağıttığınızda karşılaşabileceğiniz hatalar açıklanır.

## <a name="symptom"></a>Belirti

Bir alt öğe olan bir kaynağı başka bir kaynağa dağıttığınızda, şu hatayı alabilirsiniz:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Nedeni

Bir kaynak başka bir kaynağın alt öğesi olduğunda, alt kaynak oluşturulmadan önce üst kaynağın mevcut olması gerekir. Alt kaynağın adı üst kaynakla bağlantıyı tanımlar. Alt kaynağın adı biçimindedir `<parent-resource-name>/<child-resource-name>` . Örneğin, bir SQL veritabanı şöyle tanımlanabilir:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Hem sunucu hem de veritabanını aynı şablonda dağıtırsanız ancak sunucuda bir bağımlılık belirtmezseniz, sunucu dağıtılmadan önce veritabanı dağıtımı başlayabilir.

Üst kaynak zaten varsa ve aynı şablonda dağıtılmamışsa, Kaynak Yöneticisi alt kaynağı üst ile ilişkilendiremediğinde bu hatayı alırsınız. Alt kaynak doğru biçimde olmadığında veya alt kaynak, üst kaynak için kaynak grubundan farklı bir kaynak grubuna dağıtıldığında bu hata oluşabilir.

## <a name="solution"></a>Çözüm

Üst ve alt kaynaklar aynı şablonda dağıtıldığında bu hatayı çözmek için bir bağımlılık ekleyin.

```json
"dependsOn": [
  "[variables('databaseServerName')]"
]
```

Üst kaynak daha önce farklı bir şablonda dağıtıldığında bu hatayı çözmek için bir bağımlılık ayarlayamazsınız. Bunun yerine, alt öğeyi aynı kaynak grubuna dağıtın ve üst kaynağın adını sağlayın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "sqlServerName": {
      "type": "string"
    },
    "databaseName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Sql/servers/databases",
      "apiVersion": "2014-04-01",
      "name": "[concat(parameters('sqlServerName'), '/', parameters('databaseName'))]",
      "location": "[resourceGroup().location]",
      "properties": {
        "collation": "SQL_Latin1_General_CP1_CI_AS",
        "edition": "Basic"
      }
    }
  ],
  "outputs": {}
}
```

Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarda kaynak dağıtma sırasını tanımlama](define-resource-dependency.md).