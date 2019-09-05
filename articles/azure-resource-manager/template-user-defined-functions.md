---
title: Azure Resource Manager şablondaki Kullanıcı tanımlı işlevler
description: Bir Azure Resource Manager şablonunda Kullanıcı tanımlı işlevlerin nasıl tanımlanacağını ve kullanılacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 71c166b9c596c3c8628f943ae5c7dbebd9c2d51c
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384144"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Azure Resource Manager şablondaki Kullanıcı tanımlı işlevler

Şablonunuz içinde kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler, şablonunuzda kullanıma sunulmuştur. Kullanıcı tanımlı işlevler, şablonunuzda otomatik olarak kullanılabilen [Standart şablon işlevlerinden](resource-group-template-functions.md) ayrıdır. Şablonunuzda tekrar tekrar kullanılan karmaşık deyimleriniz varsa kendi işlevlerinizi oluşturun.

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
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="limitations"></a>Sınırlamalar

Bir Kullanıcı işlevi tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlevi yalnızca işlevinde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içinde [Parameters](resource-group-template-functions-deployment.md#parameters) işlevini kullandığınızda, bu işlevin parametreleriyle sınırlandırılırsınız.
* İşlev, Kullanıcı tanımlı diğer işlevleri çağıramaz.
* İşlev, [başvuru](resource-group-template-functions-resource.md#reference) veya [liste](resource-group-template-functions-resource.md#list) işlevlerini kullanamaz.
* İşlevin parametreleri varsayılan değerlere sahip olamaz.


## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcı tanımlı işlevlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
* Kullanılabilir şablon işlevlerinin bir listesi için, bkz. [Azure Resource Manager şablon işlevleri](resource-group-template-functions.md).
