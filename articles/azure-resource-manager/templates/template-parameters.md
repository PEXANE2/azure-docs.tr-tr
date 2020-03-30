---
title: Şablonlarda parametreler
description: Azure Kaynak Yöneticisi şablonundaki parametrelerin nasıl tanımlandığını açıklar.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: 89c6984c587e8dae59c1825a99d4f8da1c06dafb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122432"
---
# <a name="parameters-in-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarında parametreler

Bu makalede, Azure Kaynak Yöneticisi şablonunuzda parametrelerin nasıl tanımlanılı ve kullanılacağı açıklanmaktadır. Parametreler için farklı değerler sağlayarak, farklı ortamlar için bir şablonu yeniden kullanabilirsiniz.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce parametre değerlerini çözer. Şablonda parametre nin kullanıldığı her yerde, Kaynak Yöneticisi onu çözümlenen değerle değiştirir.

## <a name="define-parameter"></a>Parametre tanımlama

Aşağıdaki örnekbasit bir parametre tanımı gösterir. **DepolamaSKU**adlı bir parametre tanımlar. Parametre bir dize değeridir ve yalnızca kullanım amacı için geçerli olan değerleri kabul eder. Parametre, dağıtım sırasında değer sağlanmadığında varsayılan bir değer kullanır.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

## <a name="use-parameter"></a>Parametre kullanın

Şablonda, [parametre](template-functions-deployment.md#parameters) işlevini kullanarak parametrenin değerine başvurursunuz. Aşağıdaki örnekte, parametre değeri depolama hesabı için SKU ayarlamak için kullanılır.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="template-functions"></a>Şablon işlevleri

Bir parametre için varsayılan değeri belirtirken, çoğu şablon işlevlerini kullanabilirsiniz. Varsayılan değer oluşturmak için başka bir parametre değeri kullanabilirsiniz. Aşağıdaki şablon, işlevlerin varsayılan değerde kullanımını gösterir. Site için ad verilmediğinde, benzersiz bir dize değeri oluşturur ve **siteye**ekler. Ana bilgisayar planı için ad verilmediğinde, sitenin değerini alır ve ekler **-plan.**

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Parametreler bölümünde ki [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamazsınız. Bu işlevler kaynağın çalışma zamanı durumunu alır ve parametreler çözüldüğünde dağıtımdan önce yürütülemez.

## <a name="objects-as-parameters"></a>Parametreler olarak nesneler

İlgili değerleri nesne olarak geçirerek düzenlemek daha kolay olabilir. Bu yaklaşım, şablondaki parametre sayısını da azaltır.

Aşağıdaki örnekte bir nesne olan bir parametre gösterilmektedir. Varsayılan değer nesne için beklenen özellikleri gösterir.

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Nokta işleci kullanarak nesnenin özelliklerine başvurursunuz.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2015-06-15",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örneklerde parametreleri kullanmak için senaryolar gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
|[varsayılan değerler için fonksiyonlu parametreler](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametreler için varsayılan değerleri tanımlarken şablon işlevlerinin nasıl kullanılacağını gösterir. Şablon herhangi bir kaynak dağıtmaz. Parametre değerlerini inşa eder ve bu değerleri döndürür. |
|[parametre nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir parametre için bir nesne kullanarak gösterir. Şablon herhangi bir kaynak dağıtmaz. Parametre değerlerini inşa eder ve bu değerleri döndürür. |


## <a name="next-steps"></a>Sonraki adımlar

* Parametreler için kullanılabilir özellikler hakkında bilgi edinmek için [bkz.](template-syntax.md)
* Parametre değerlerini dosya olarak geçirme hakkında bilgi edinmek için kaynak [yöneticisi parametre dosyası oluştur'a](parameter-files.md)bakın.
* Parametreler oluşturma yla ilgili öneriler için en [iyi uygulamalar - parametrelere](template-best-practices.md#parameters)bakın.
