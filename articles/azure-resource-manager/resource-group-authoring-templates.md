---
title: Azure Resource Manager şablon yapısı ve sözdizimi | Microsoft Docs
description: Bildirim temelli JSON sözdizimi kullanan Azure Resource Manager şablonlarının yapısını ve özelliklerini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: tomfitz
ms.openlocfilehash: 9858e8a52888304edd48893db02faa992b356b3b
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774911"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın

Bu makalede bir Azure Resource Manager şablonunun yapısı açıklanır. Bir şablonun farklı bölümlerini ve bu bölümlerde kullanılabilen özellikleri gösterir. Şablon, dağıtımınız için değer oluşturmak üzere kullanabileceğiniz JSON ve ifadelerden oluşur.

Bu makale, Kaynak Yöneticisi şablonları hakkında bazı benzerlik sahibi olan kullanıcılara yöneliktir. Şablonun yapısı ve sözdizimi hakkında ayrıntılı bilgi sağlar. Şablon oluşturmaya giriş istiyorsanız, bkz. [ilk Azure Resource Manager şablonunuzu oluşturma](resource-manager-create-first-template.md).

## <a name="template-format"></a>Şablon biçimi

En basit yapısında, bir şablon aşağıdaki öğelere sahiptir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| $schema |Evet |Şablon dilinin sürümünü açıklayan JSON Şema dosyasının konumu.<br><br> Kaynak grubu dağıtımları için şunu kullanın:`https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>Abonelik dağıtımları için şunu kullanın:`https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#` |
| contentVersion |Evet |Şablonun sürümü (1.0.0.0 gibi). Bu öğe için herhangi bir değer sağlayabilirsiniz. Şablonunuzda önemli değişiklikleri belgelemek için bu değeri kullanın. Şablonu kullanarak kaynakları dağıttığınızda, bu değer doğru şablonun kullanıldığından emin olmak için kullanılabilir. |
| apiProfile |Hayır | Kaynak türleri için API sürümleri koleksiyonu görevi gören bir API sürümü. Şablondaki her kaynak için API sürümlerini belirtmek zorunda kalmamak için bu değeri kullanın. Bir API profili sürümü belirttiğinizde ve kaynak türü için bir API sürümü belirtmezseniz Kaynak Yöneticisi, profilde tanımlanan bu kaynak türü için API sürümünü kullanır.<br><br>API profili özelliği, bir şablonu Azure Stack ve küresel Azure gibi farklı ortamlara dağıtmada özellikle faydalıdır. Şablonunuzun her iki ortamda da desteklenen sürümleri otomatik olarak kullandığından emin olmak için API profili sürümünü kullanın. Geçerli API profili sürümlerinin ve profilde tanımlanan kaynak API sürümlerinin bir listesi için bkz. [API profili](https://github.com/Azure/azure-rest-api-specs/tree/master/profile).<br><br>Daha fazla bilgi için bkz. [API profillerini kullanarak sürümleri izleme](templates-cloud-consistency.md#track-versions-using-api-profiles). |
| [parameters](#parameters) |Hayır |Kaynak dağıtımını özelleştirmek için dağıtım yürütüldüğünde belirtilen değerler. |
| [variables](#variables) |Hayır |Şablon dil ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerler. |
| [functions](#functions) |Hayır |Şablon içinde kullanılabilen Kullanıcı tanımlı işlevler. |
| [resources](#resources) |Evet |Bir kaynak grubunda veya abonelikte dağıtılan veya güncellenen kaynak türleri. |
| [çıkışı](#outputs) |Hayır |Dağıtımdan sonra döndürülen değerler. |

Her öğenin ayarlayabileceğiniz özellikleri vardır. Bu makalede, şablonun bölümleri daha ayrıntılı olarak açıklanmaktadır.

## <a name="syntax"></a>Sözdizimi

Şablonun temel sözdizimi JSON ' dır. Ancak, şablon içinde kullanılabilir olan JSON değerlerini genişletmek için ifadeleri kullanabilirsiniz.  İfadeler sırasıyla Köşeli parantezlerle `[` `]`başlar ve biter. İfade değeri, şablon dağıtıldığında değerlendirilir. Bir ifade dize, tamsayı, Boolean, dizi veya nesne döndürebilir. Aşağıdaki örnek, bir parametresinin varsayılan değerindeki bir ifadeyi gösterir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade içinde sözdizimi `resourceGroup()` , Kaynak Yöneticisi bir şablon içinde kullanmak için sağladığı işlevlerden birini çağırır. JavaScript içinde olduğu gibi, işlev çağrıları olarak `functionName(arg1,arg2,arg3)`biçimlendirilir. Söz dizimi `.location` , bu işlev tarafından döndürülen nesneden bir özelliği alır.

Şablon işlevleri ve parametreleri büyük/küçük harfe duyarlıdır. Örneğin, Kaynak Yöneticisi **değişkenleri (' var1 ')** ve **DEĞIŞKENLERI (' var1 ')** aynı şekilde çözümler. Değerlendirildiğinde, işlev açıkça büyük/küçük harf (toUpper veya toLower gibi) değiştirmediği sürece işlev, büyük/küçük harf durumunu korur. İşlevlerin nasıl değerlendirildiğinden bağımsız olarak belirli kaynak türlerinde durum gereksinimleri olabilir.

Değişmez bir dizenin bir sol köşeli ayraç `[` ile başlaması ve sağ parantez `]`ile bitmesi, ancak bir ifade olarak yorumlanmaması için, dizeyi ile `[[`başlatmak için fazladan bir köşeli ayraç ekleyin. Örneğin, değişkeni:

```json
"demoVar1": "[[test value]"
```

Olarak `[test value]`çözümlenir.

Ancak, değişmez dize bir köşeli ayraç ile bitmezse ilk köşeli ayracı atmayın. Örneğin, değişkeni:

```json
"demoVar2": "[test] value"
```

Olarak `[test] value`çözümlenir.

Bir parametreye parametre olarak bir dize değeri geçirmek için tek tırnak kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Şablonda JSON nesnesi ekleme gibi bir ifadede çift tırnak işaretleri için ters eğik çizgi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

Şablon ifadesi 24.576 karakterden uzun olamaz.

Şablon işlevlerinin tam listesi için bkz. [Azure Resource Manager şablon işlevleri](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametreler

Şablonun parametreler bölümünde, kaynakları dağıttığınızda hangi değerleri gir, istediğinizi belirtirsiniz. Bu parametre değerleri, belirli bir ortam (geliştirme, test ve üretim gibi) için uyarlanmış değerler sağlayarak dağıtımı özelleştirmenizi sağlar. Şablonunuzda parametreler sağlamanız gerekmez, ancak parametre olmadan şablonunuz her zaman aynı adları, konumları ve özellikleri olan kaynakları dağıtır.

Bir şablonda 256 parametre ile sınırlı olursunuz. Bu makalede gösterildiği gibi, birden çok özellik içeren nesneleri kullanarak parametre sayısını azaltabilirsiniz.

### <a name="available-properties"></a>Kullanılabilir özellikler

Bir parametre için kullanılabilen özellikler şunlardır:

```json
"parameters": {
  "<parameter-name>" : {
    "type" : "<type-of-parameter-value>",
    "defaultValue": "<default-value-of-parameter>",
    "allowedValues": [ "<array-of-allowed-values>" ],
    "minValue": <minimum-value-for-int>,
    "maxValue": <maximum-value-for-int>,
    "minLength": <minimum-length-for-string-or-array>,
    "maxLength": <maximum-length-for-string-or-array-parameters>,
    "metadata": {
      "description": "<description-of-the parameter>" 
    }
  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| parameterName |Evet |Parametrenin adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| type |Evet |Parametre değerinin türü. İzin verilen türler ve değerler **dize**, **SecureString**, **int**, **bool**, **nesne**, **secureobject**ve **dizidir**. |
| Değerinin |Hayır |Parametresi için değer sağlanmazsa, parametre için varsayılan değer. |
| allowedValues |Hayır |Doğru değerin sağlandığından emin olmak için parametresi için izin verilen değerler dizisi. |
| minValue |Hayır |İnt tür parametrelerinin minimum değeri, bu değer dahil değildir. |
| Değerini |Hayır |İnt tür parametrelerinin en büyük değeri, bu değer dahil değildir. |
| minLength |Hayır |Dize, güvenli dize ve dizi türü parametrelerinin minimum uzunluğu, bu değer dahil değildir. |
| 'In |Hayır |Dize, güvenli dize ve dizi türü parametrelerinin uzunluk üst sınırı, bu değer dahil değildir. |
| description |Hayır |Portalda kullanıcılara görüntülenen parametrenin açıklaması. Daha fazla bilgi için bkz. [şablonlarda açıklamalar](#comments). |

### <a name="define-and-use-a-parameter"></a>Bir parametre tanımlama ve kullanma

Aşağıdaki örnek bir basit parametre tanımını gösterir. Parametrenin adını tanımlar ve bir dize değeri aldığını belirtir. Parametresi yalnızca amaçlanan kullanımı için anlamlı olan değerleri kabul eder. Dağıtım sırasında hiçbir değer sağlanmamışsa varsayılan bir değer belirtir. Son olarak, parametresi kullanım açıklamasını içerir.

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

Şablonunda, aşağıdaki söz dizimi ile parametre için değere başvurun:

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

### <a name="template-functions-with-parameters"></a>Parametrelere sahip şablon işlevleri

Bir parametre için varsayılan değeri belirtirken, çoğu şablon işlevini kullanabilirsiniz. Varsayılan bir değer oluşturmak için başka bir parametre değeri de kullanabilirsiniz. Aşağıdaki şablon, varsayılan değer içindeki işlevlerin kullanımını gösterir:

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

Parametreler bölümünde `reference` işlevini kullanamazsınız. Parametreler dağıtımdan önce değerlendirilir, `reference` bu nedenle işlev bir kaynağın çalışma zamanı durumunu alamaz. 

### <a name="objects-as-parameters"></a>Parametre olarak nesneler

İlgili değerleri bir nesne olarak geçirerek düzenlemek daha kolay olabilir. Bu yaklaşım ayrıca şablondaki parametre sayısını azaltır.

Şablonunuzda parametreyi tanımlayın ve dağıtım sırasında tek bir değer yerine bir JSON nesnesi belirtin. 

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

Sonra, nokta işlecini kullanarak parametresinin alt özelliklerine başvurun.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
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

### <a name="parameter-example-templates"></a>Parametre örnek şablonları

Bu örnek şablonlar, parametreleri kullanmaya yönelik bazı senaryolar gösterir. Parametrelerin farklı senaryolarda nasıl işlendiğini test etmek için bunları dağıtın.

|Şablon  |Açıklama  |
|---------|---------|
|[Varsayılan değerler için işlevlere sahip parametreler](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametreler için varsayılan değerleri tanımlarken şablon işlevlerinin nasıl kullanılacağını gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parameter nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir parametre için bir nesne kullanmayı gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |

## <a name="variables"></a>Değişkenler

Değişkenler bölümünde, şablonunuzun tamamında kullanılabilecek değerler oluşturursunuz. Değişken tanımlamanız gerekmez, ancak genellikle karmaşık ifadeleri azaltarak şablonunuzu basitleştirir.

### <a name="available-definitions"></a>Kullanılabilir tanımlar

Aşağıdaki örnek, bir değişken tanımlamaya yönelik kullanılabilir seçenekleri gösterir:

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": { 
    <variable-complex-type-value> 
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

Bir değişken için birkaç `copy` değer oluşturmak üzere kullanma hakkında bilgi için bkz. [değişken yineleme](resource-group-create-multiple.md#variable-iteration).

### <a name="define-and-use-a-variable"></a>Değişken tanımlama ve kullanma

Aşağıdaki örnekte bir değişken tanımı gösterilmektedir. Depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak için çeşitli şablon işlevleri kullanır ve onu benzersiz bir dizeye birleştirir.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Kaynağı tanımlarken değişkeni kullanırsınız.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

### <a name="configuration-variables"></a>Yapılandırma değişkenleri

Bir ortam için ilgili değerleri tanımlamak üzere karmaşık JSON türlerini kullanabilirsiniz.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

Parametreler ' de, hangi yapılandırma değerlerinin kullanılacağını belirten bir değer oluşturursunuz.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Şu andaki ayarları alırsınız:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

### <a name="variable-example-templates"></a>Değişken örnek şablonları

Bu örnek şablonlar, değişken kullanımı için bazı senaryolar gösterir. Değişkenlerin farklı senaryolarda nasıl işlendiğini test etmek için bunları dağıtın. 

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı değişken türlerini gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [Yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişkenin kullanımını gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubuna güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |


## <a name="functions"></a>İşlevler

Şablonunuz içinde kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler, şablonunuzda kullanıma sunulmuştur. Genellikle, şablonunuzun tamamında yinelemek istemediğiniz karmaşık ifadeleri tanımlarsınız. Şablonlarda desteklenen ifadelerden ve [işlevlerden](resource-group-template-functions.md) Kullanıcı tanımlı işlevler oluşturursunuz.

Bir Kullanıcı işlevi tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlevi yalnızca işlevinde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içinde [Parameters işlevini](resource-group-template-functions-deployment.md#parameters) kullandığınızda, bu işlevin parametreleriyle sınırlandırılırsınız.
* İşlev, Kullanıcı tanımlı diğer işlevleri çağıramaz.
* İşlev, [başvuru işlevini](resource-group-template-functions-resource.md#reference)kullanamaz.
* İşlevin parametreleri varsayılan değerlere sahip olamaz.

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

İşlevini ile çağırabilirsiniz:

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

## <a name="resources"></a>Kaynaklar
Kaynaklar bölümünde, dağıtılan veya güncellenen kaynakları tanımlarsınız.

### <a name="available-properties"></a>Kullanılabilir özellikler

Kaynakları aşağıdaki yapıyla tanımlarsınız:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| condition | Hayır | Bu dağıtım sırasında kaynağın sağlanıp sağlanmayacağını belirten Boole değeri. Ne `true`zaman, kaynak dağıtım sırasında oluşturulur. Ne `false`zaman, bu dağıtım için kaynak atlanır. [Koşula](#condition)bakın. |
| apiVersion |Evet |Kaynağı oluşturmak için kullanılacak REST API sürümü. Kullanılabilir değerleri anlamak için bkz. [şablon başvurusu](/azure/templates/). |
| türü |Evet |Kaynağın türü. Bu değer, kaynak sağlayıcının ve kaynak türünün ( **Microsoft. Storage/storageAccounts**gibi) ad alanının bir birleşimidir. Kullanılabilir değerleri anlamak için bkz. [şablon başvurusu](/azure/templates/). Bir alt kaynak için, türün biçimi üst kaynak içinde iç içe veya üst kaynak dışında tanımlanmış olmasına bağlıdır. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md). |
| name |Evet |Kaynağın adı. Ad, RFC3986 içinde tanımlanan URI bileşen kısıtlamalarına uymalıdır. Ayrıca, kaynak adını dış tarafların kullanımına sunan Azure Hizmetleri, başka bir kimliği sızma girişimi olmadığından emin olmak için adı doğrular. Bir alt kaynak için, adın biçimi üst kaynak içinde iç içe veya üst kaynak dışında tanımlanmış olmasına bağlıdır. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md). |
| location |Varies |Belirtilen kaynağın desteklenen coğrafi konumları. Kullanılabilir konumlardan herhangi birini seçebilirsiniz, ancak genellikle kullanıcılarınıza yakın olan bir seçim yapmak mantıklı olur. Genellikle, aynı bölgedeki birbirleriyle etkileşim kuran kaynakları yerleştirmek de anlamlı hale gelir. Çoğu kaynak türü bir konum gerektirir, ancak bazı türler (örneğin, rol ataması) bir konum gerektirmez. |
| tags |Hayır |Kaynakla ilişkili Etiketler. Aboneliğiniz genelinde kaynakları mantıksal olarak düzenlemek için etiketleri uygulayın. |
| açıklamaları |Hayır |Şablonunuzda kaynakları belgelemek için notlarınız. Daha fazla bilgi için bkz. [şablonlarda açıklamalar](resource-group-authoring-templates.md#comments). |
| kopyala |Hayır |Birden fazla örnek gerekliyse, oluşturulacak kaynak sayısı. Varsayılan mod paraleldir. Tüm veya kaynakların aynı anda dağıtılmasını istemiyorsanız seri modunu belirtin. Daha fazla bilgi için bkz. [Azure Resource Manager çeşitli kaynak örnekleri oluşturma](resource-group-create-multiple.md). |
| dependsOn |Hayır |Bu kaynak dağıtılmadan önce dağıtılması gereken kaynaklar. Kaynak Yöneticisi, kaynaklar arasındaki bağımlılıkları değerlendirir ve doğru sırayla dağıtır. Kaynaklar birbirine bağımlı olmadığında, paralel olarak dağıtılır. Değer, kaynak adlarının veya kaynak benzersiz tanımlayıcılarının virgülle ayrılmış bir listesi olabilir. Yalnızca bu şablonda dağıtılan kaynakları listeleyin. Bu şablonda tanımlı olmayan kaynaklar zaten var olmalıdır. Dağıtımınızı yavaşlatabilir ve dairesel bağımlılıklar oluşturduklarında gereksiz bağımlılıkları eklemekten kaçının. Bağımlılıkları ayarlama hakkında yönergeler için bkz. [Azure Resource Manager şablonlarda bağımlılıkları tanımlama](resource-group-define-dependencies.md). |
| properties |Hayır |Kaynağa özgü yapılandırma ayarları. Özelliklerin değerleri, kaynağı oluşturmak için REST API işlem (PUT yöntemi) için istek gövdesinde sağladığınız değerlerle aynıdır. Ayrıca, bir özelliğin birkaç örneğini oluşturmak için bir kopya dizisi belirtebilirsiniz. Kullanılabilir değerleri anlamak için bkz. [şablon başvurusu](/azure/templates/). |
| sku | Hayır | Bazı kaynaklar, dağıtılacak SKU 'YU tanımlayan değerlere izin verir. Örneğin, bir depolama hesabı için artıklık türünü belirtebilirsiniz. |
| denetlenmesi | Hayır | Bazı kaynaklar, dağıttığınız kaynak türünü tanımlayan bir değere izin verir. Örneğin, oluşturulacak Cosmos DB türünü belirtebilirsiniz. |
| planınızın | Hayır | Bazı kaynaklar, dağıtılacak planı tanımlayan değerlere izin verir. Örneğin, bir sanal makine için Market görüntüsünü belirtebilirsiniz. | 
| kaynaklar |Hayır |Tanımlanmakta olan kaynağa bağlı olan alt kaynaklar. Yalnızca üst kaynağın şemasına izin verilen kaynak türlerini sağlayın. Üst kaynağın bağımlılığı örtük değildir. Bu bağımlılığı açıkça tanımlamanız gerekir. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md). |

### <a name="condition"></a>Koşul

Dağıtım sırasında bir kaynak oluşturulup oluşturulmayacağını cağınıza karar vermeniz gerektiğinde, `condition` öğesini kullanın. Bu öğenin değeri true veya false olarak çözümlenmektedir. Değer true olduğunda kaynak oluşturulur. Değer false olduğunda kaynak oluşturulmaz. Değer yalnızca kaynağın tamamına uygulanabilir.

Genellikle, bu değeri, yeni bir kaynak oluşturmak veya var olan bir kaynağı kullanmak istediğinizde kullanırsınız. Örneğin, yeni bir depolama hesabının dağıtılıp dağıtılmadığını veya var olan bir depolama hesabının kullanıldığını belirtmek için şunu kullanın:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

`condition` Öğesini kullanan bir örnek şablon için, bkz. [Yeni veya var olan sanal ağ, depolama ve genel IP ile VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

Koşullu olarak dağıtılan bir kaynakla [başvuru](resource-group-template-functions-resource.md#reference) veya [liste](resource-group-template-functions-resource.md#list) işlevi kullanıyorsanız, işlev, kaynak dağıtılmasa bile değerlendirilir. İşlev mevcut olmayan bir kaynağa başvuruyorsa bir hata alırsınız. İşlevin yalnızca kaynak dağıtıldığında koşullara göre değerlendirildiğinden emin olmak için [IF](resource-group-template-functions-logical.md#if) işlevini kullanın. Koşullu olarak dağıtılan bir kaynakla IF ve Reference kullanan bir örnek şablon için [IF işlevine](resource-group-template-functions-logical.md#if) bakın.

### <a name="resource-names"></a>Kaynak adları

Genellikle Kaynak Yöneticisi içinde üç tür kaynak adı ile çalışırsınız:

* Benzersiz olması gereken kaynak adları.
* Benzersiz olması gerekmeyen kaynak adları, ancak kaynağı tanımanıza yardımcı olabilecek bir ad sağlamayı tercih edersiniz.
* Genel olabilecek kaynak adları.

Veri erişim uç noktası olan herhangi bir kaynak türü için **benzersiz bir kaynak adı** sağlayın. Benzersiz bir ad gerektiren bazı ortak kaynak türleri şunlardır:

* Azure depolama<sup>1</sup> 
* Azure Uygulama Hizmeti’nin Web Apps özelliği
* SQL Server
* Azure Key Vault
* Redis için Azure Önbelleği
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> depolama hesabı adları da küçük, 24 karakter veya daha az olmalı ve herhangi bir tire içermemelidir.

Adı ayarlarken, bir ad oluşturmak için el ile benzersiz bir ad oluşturabilir veya [Uniquestring ()](resource-group-template-functions-string.md#uniquestring) işlevini kullanabilirsiniz. Ayrıca, **Uniquestring** sonucuna bir ön ek veya sonek eklemek isteyebilirsiniz. Benzersiz adı değiştirmek, kaynak türünü adından daha kolay bir şekilde tanımanıza yardımcı olabilir. Örneğin, aşağıdaki değişkeni kullanarak bir depolama hesabı için benzersiz bir ad oluşturabilirsiniz:

```json
"variables": {
  "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

Bazı kaynak türlerinde, **tanımlayıcı için bir ad**sağlamak isteyebilirsiniz, ancak adın benzersiz olması gerekmez. Bu kaynak türleri için, kullanımı veya özellikleri açıklayan bir ad sağlayın.

```json
"parameters": {
  "vmName": { 
    "type": "string",
    "defaultValue": "demoLinuxVM",
    "metadata": {
      "description": "The name of the VM to create."
    }
  }
}
```

Farklı bir kaynak üzerinden çok daha önce erişebileceğiniz kaynak türleri için, şablonda sabit kodlanmış **genel bir ad** kullanabilirsiniz. Örneğin, bir SQL Server 'da güvenlik duvarı kuralları için standart, genel bir ad belirleyebilirsiniz:

```json
{
  "type": "firewallrules",
  "name": "AllowAllWindowsAzureIps",
  ...
}
```

### <a name="resource-location"></a>Kaynak konumu

Bir şablonu dağıttığınızda, her kaynak için bir konum sağlamanız gerekir. Farklı konumlarda farklı kaynak türleri desteklenir. Kaynak türü için desteklenen konumları almak üzere bkz. [Azure kaynak sağlayıcıları ve türleri](resource-manager-supported-services.md).

Kaynak konumunu belirtmek için bir parametre kullanın ve varsayılan değeri olarak `resourceGroup().location`ayarlayın.

Aşağıdaki örnekte, parametresi olarak belirtilen bir konuma dağıtılan bir depolama hesabı gösterilmektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="outputs"></a>outputs

Çıkış bölümünde dağıtımdan döndürülen değerlerini belirtin. Genellikle, dağıtılan kaynaklardan değerleri döndürürler.

### <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki örnek, bir çıkış tanımı yapısını gösterir:

```json
"outputs": {
  "<outputName>" : {
    "condition": "<boolean-value-whether-to-output-value>",
    "type" : "<type-of-output-value>",
    "value": "<output-value-expression>"
  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| outputName |Evet |Çıkış değeri adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| condition |Hayır | Bu çıkış değerinin döndürülüp döndürülmeyeceğini belirten Boolean değeri. Ne `true`zaman, bu değer dağıtımın çıktısına dahil edilir. Ne `false`zaman, bu dağıtım için çıkış değeri atlanır. Belirtilmediğinde, varsayılan değer `true`. |
| türü |Evet |Çıkış değeri türü. Çıkış değerleri şablon giriş parametreleri aynı türlerini destekler. Çıktı türü için **SecureString** belirtirseniz, değer dağıtım geçmişinde gösterilmez ve başka bir şablondan alınamaz. Birden fazla şablonda gizli bir değer kullanmak için, gizli anahtarı bir Key Vault depolayın ve parametre dosyasındaki gizli dizi başvurusu yapın. Daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](resource-manager-keyvault-parameter.md). |
| value |Evet |Değerlendirilen ve çıkış değeri döndürülen şablon dili ifadesi. |

### <a name="define-and-use-output-values"></a>Tanımlama ve çıkış değerlerini kullanma

Aşağıdaki örnek, kaynak kimliği için bir genel IP adresi döndürülecek gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Sonraki örnekte, bir genel IP adresi için kaynak KIMLIĞININ, yeni bir birinin dağıtılıp dağıtıldığına göre nasıl koşullu olarak döndürdüğü gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Koşullu çıkışın basit bir örneği için bkz. [koşullu çıkış şablonu](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

Dağıtımdan sonra değeri betiğiyle alabilirsiniz. PowerShell için şunu kullanın:

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Azure CLI için şunu kullanın:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Çıkış değeri kullanarak bağlantılı bir şablondan alabilirsiniz [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Bağlantılı bir şablondan bir çıkış değeri almak için özellik değeri gibi bir söz dizimi ile Al: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bir çıkış özelliği bağlı şablonundan alınırken, özellik adı bir tire içeremez.

Aşağıdaki örnek, bir yük dengeleyicide, bağlantılı şablondan bir değer alarak IP adresinin nasıl ayarlanacağını gösterir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Kullanamazsınız `reference` çıktılar bölümünü işlevinde bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Dönüş değerleri dağıtılan kaynağın içinde iç içe geçmiş bir şablon için iç içe geçmiş şablon bağlantılı şablona dönüştürebilirsiniz.

### <a name="output-example-templates"></a>Çıkış örneği şablonları

|Şablon  |Açıklama  |
|---------|---------|
|[Değişkenleri kopyalayın](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri çıkarır. Tüm kaynakları dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Genel bir IP adresi oluşturur ve kaynak kimliği çıkarır |
|[Yük Dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Yukarıdaki şablonu bağlar. Kaynak Kimliği, yük dengeleyici oluştururken çıktısında kullanır. |


<a id="comments" />

## <a name="comments-and-metadata"></a>Açıklamalar ve meta veriler

Şablonunuza açıklama ve meta veri eklemek için birkaç seçeneğiniz vardır.

Şablonunuzda neredeyse her yerde `metadata` bir nesne ekleyebilirsiniz. Kaynak Yöneticisi nesneyi yoksayar, ancak JSON düzenleyiciniz özelliğin geçerli olmadığını uyarabilir. Nesnesinde, ihtiyacınız olan özellikleri tanımlayın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

**Parametreler**için, `metadata` `description` özelliği olan bir nesne ekleyin.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

Şablonu Portal üzerinden dağıttığınızda, açıklamada sağladığınız metin, bu parametre için bir ipucu olarak otomatik olarak kullanılır.

![Parametre ipucunu göster](./media/resource-group-authoring-templates/show-parameter-tip.png)

**Kaynaklar**için bir `comments` öğe veya meta veri nesnesi ekleyin. Aşağıdaki örnek hem bir Comments öğesini hem de bir meta veri nesnesini gösterir.

```json
"resources": [
  {
    "comments": "Storage account used to store VM disks",
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

**Çıktılar**için, çıkış değerine bir meta veri nesnesi ekleyin.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

Kullanıcı tanımlı işlevlere meta veri nesnesi ekleyemezsiniz.

Satır içi açıklamalarda, `//` bu söz dizimi tüm araçlarla birlikte kullanılamaz. Şablonu satır içi açıklamalarla dağıtmak için Azure CLı 'yi kullanamazsınız. Ve, satır içi açıklamalarla şablonlar üzerinde çalışmak için Portal Şablon düzenleyicisini kullanamazsınız. Bu açıklama stilini eklerseniz, kullandığınız araçların satır içi JSON açıklamalarını desteklemesini sağlayın.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  "dependsOn": [ // storage account and network interface must be deployed first
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

VS Code, dil modunu, yorumlarla birlikte JSON olarak ayarlayabilirsiniz. Satır içi açıklamalar artık geçersiz olarak işaretlenmemiştir. Modu değiştirmek için:

1. Dil modu seçimini aç (CTRL + K M)

1. **Yorumlarla JSON**seçin.

   ![Dil modunu seçin](./media/resource-group-authoring-templates/select-json-comments.png)

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Dağıtım sırasında birkaç şablonu birleştirmek için bkz. [Azure Resource Manager ile bağlantılı şablonları kullanma](resource-group-linked-templates.md).
* Şablon oluşturma hakkında öneriler için bkz. [Azure Resource Manager şablonu en iyi yöntemleri](template-best-practices.md).
* Tüm Azure ortamlarında kullanabileceğiniz Kaynak Yöneticisi şablonları oluşturmaya yönelik öneriler ve Azure Stack için bkz. [bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](templates-cloud-consistency.md).
