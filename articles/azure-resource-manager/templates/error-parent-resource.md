---
title: Üst kaynak hataları
description: Azure Resource Manager şablonunda bir üst kaynakla çalışırken hataların nasıl çözümleneceğini açıklar.
ms.topic: troubleshooting
ms.date: 08/01/2018
ms.openlocfilehash: 9fcf12db7375e6d19ef9e77ea4dcaf13130175b5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75484538"
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

Bir kaynak başka bir kaynağın alt öğesi olduğunda, alt kaynak oluşturulmadan önce üst kaynağın mevcut olması gerekir. Alt kaynağın adı üst kaynakla bağlantıyı tanımlar. Alt kaynağın adı `<parent-resource-name>/<child-resource-name>`biçimindedir. Örneğin, bir SQL veritabanı şöyle tanımlanabilir:

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
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
            "apiVersion": "2014-04-01",
            "type": "Microsoft.Sql/servers/databases",
            "location": "[resourceGroup().location]",
            "name": "[concat(parameters('sqlServerName'), '/', parameters('databaseName'))]",
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