---
title: Şablonlarda Kullanıcı tanımlı işlevler
description: Bir Azure Resource Manager şablonunda Kullanıcı tanımlı işlevlerin nasıl tanımlanacağını ve kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: 58b9ba7b162736329cf775e2be5a47bfcae0a4ca
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122483"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Azure Resource Manager şablondaki Kullanıcı tanımlı işlevler

Şablonunuz içinde kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler, şablonunuzda kullanıma sunulmuştur. Kullanıcı tanımlı işlevler, şablonunuzda otomatik olarak kullanılabilen [Standart şablon işlevlerinden](template-functions.md) ayrıdır. Şablonunuzda tekrar tekrar kullanılan karmaşık deyimleriniz varsa kendi işlevlerinizi oluşturun.

Bu makalede, Azure Resource Manager şablonunuzda Kullanıcı tanımlı işlevlerin nasıl ekleneceği açıklanmaktadır.

## <a name="define-the-function"></a>İşlevi tanımlayın

İşlevleriniz, şablon işlevleriyle adlandırma çakışmalarını önlemek için bir ad alanı değeri gerektirir. Aşağıdaki örnek, bir depolama hesabı adı döndüren bir işlevi göstermektedir:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>İşlevini kullanma

Aşağıdaki örnek, işlevinizin nasıl çağrılacağını gösterir.

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "South Central US",
    "tags": {},
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

## <a name="limitations"></a>Sınırlamalar

Bir Kullanıcı işlevi tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlevi yalnızca işlevinde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içinde [Parameters](template-functions-deployment.md#parameters) işlevini kullandığınızda, bu işlevin parametreleriyle sınırlandırılırsınız.
* İşlev, Kullanıcı tanımlı diğer işlevleri çağıramaz.
* İşlev, [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden hiçbirini kullanamaz.
* İşlevin parametreleri varsayılan değerlere sahip olamaz.


## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcı tanımlı işlevlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Kullanılabilir şablon işlevlerinin bir listesi için, bkz. [Azure Resource Manager şablon işlevleri](template-functions.md).
