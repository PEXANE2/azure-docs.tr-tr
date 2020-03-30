---
title: Şablonlarda kullanıcı tanımlı işlevler
description: Azure Kaynak Yöneticisi şablonunda kullanıcı tanımlı işlevlerin nasıl tanımlandığını ve kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 2c09572a460aa028b23987033d2b77e2aad8a0cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943213"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki kullanıcı tanımlı işlevler

Şablonunuzda kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler şablonunuzda kullanılabilir. Kullanıcı tanımlı işlevler, şablonunuzda otomatik olarak kullanılabilen [standart şablon işlevlerinden](template-functions.md) ayrıdır. Şablonunuzda art arda kullanılan karmaşık ifadeleriniz olduğunda kendi işlevlerinizi oluşturun.

Bu makalede, Azure Kaynak Yöneticisi şablonunuzda kullanıcı tanımlı işlevlerin nasıl ekleyeceğiniz açıklanmaktadır.

## <a name="define-the-function"></a>İşlevi tanımla

İşlevleriniz, şablon işlevleriyle çakışmaları adlandırmayı önlemek için bir ad alanı değeri gerektirir. Aşağıdaki örnekte benzersiz bir ad döndüren bir işlev gösterilmektedir:

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

## <a name="use-the-function"></a>İşlevi kullanma

Aşağıdaki örnekte, kullanıcı tanımlı bir işlev içeren bir şablon gösterilmektedir. Bir depolama hesabı için benzersiz bir ad almak için bu işlevi kullanır. Şablon, işlev için bir parametre olarak geçtiği **storageNamePrefix** adlı bir parametreye sahiptir.

```json
{
 "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
   "storageNamePrefix": {
     "type": "string",
     "maxLength": 11
   }
 },
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
 "resources": [
   {
     "type": "Microsoft.Storage/storageAccounts",
     "apiVersion": "2019-04-01",
     "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
     "location": "South Central US",
     "sku": {
       "name": "Standard_LRS"
     },
     "kind": "StorageV2",
     "properties": {
       "supportsHttpsTrafficOnly": true
     }
   }
 ]
}
```

## <a name="limitations"></a>Sınırlamalar

Bir kullanıcı işlevini tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlev yalnızca işlevde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içindeki [parametreler](template-functions-deployment.md#parameters) işlevini kullandığınızda, bu işlevin parametreleri ile sınırlısınız.
* İşlev, kullanıcı tarafından tanımlanan diğer işlevleri çağıramaz.
* İşlev [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamaz.
* İşlev için parametreler varsayılan değerlere sahip olamaz.


## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcı tanımlı işlevler için kullanılabilir özellikler hakkında bilgi edinmek için [bkz.](template-syntax.md)
* Kullanılabilir şablon işlevlerinin listesi için [Azure Kaynak Yöneticisi şablon işlevlerine](template-functions.md)bakın.
