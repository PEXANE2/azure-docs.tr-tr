---
title: Şablonlarda Kullanıcı tanımlı işlevler
description: Bir Azure Resource Manager şablonunda Kullanıcı tanımlı işlevlerin nasıl tanımlanacağını ve kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 69f4e98d389cc8dbe5cd3f4b628189676c501106
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84672944"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Azure Resource Manager şablondaki Kullanıcı tanımlı işlevler

Şablonunuz içinde kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler, şablonunuzda kullanıma sunulmuştur. Kullanıcı tanımlı işlevler, şablonunuzda otomatik olarak kullanılabilen [Standart şablon işlevlerinden](template-functions.md) ayrıdır. Şablonunuzda tekrar tekrar kullanılan karmaşık deyimleriniz varsa kendi işlevlerinizi oluşturun.

Bu makalede, Azure Resource Manager şablonunuzda Kullanıcı tanımlı işlevlerin nasıl ekleneceği açıklanmaktadır.

## <a name="define-the-function"></a>İşlevi tanımlayın

İşlevleriniz, şablon işlevleriyle adlandırma çakışmalarını önlemek için bir ad alanı değeri gerektirir. Aşağıdaki örnek, benzersiz bir ad döndüren bir işlevi göstermektedir:

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

Aşağıdaki örnek, Kullanıcı tanımlı bir işlev içeren bir şablonu gösterir. Bir depolama hesabı için benzersiz bir ad almak üzere bu işlevi kullanır. Şablon, işleve parametre olarak geçen **storageNamePrefix** adlı bir parametreye sahiptir.

```json
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Bir Kullanıcı işlevi tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlevi yalnızca işlevinde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içinde [Parameters](template-functions-deployment.md#parameters) işlevini kullandığınızda, bu işlevin parametreleriyle sınırlandırılırsınız.
* İşlev, Kullanıcı tanımlı diğer işlevleri çağıramaz.
* İşlev, [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden hiçbirini kullanamaz.
* İşlevin parametreleri varsayılan değerlere sahip olamaz.


## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcı tanımlı işlevlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Kullanılabilir şablon işlevlerinin bir listesi için, bkz. [Azure Resource Manager şablon işlevleri](template-functions.md).
