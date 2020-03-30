---
title: Kaynaklar için dağıtım sırasını ayarlama
description: Kaynakların doğru sırada dağıtılmasını sağlamak için dağıtım sırasında bir kaynağın başka bir kaynağa bağımlı olarak nasıl ayarlanılabildiğini açıklar.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: f11f79df875492a568a76f494dfffb4a163f64cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153293"
---
# <a name="define-the-order-for-deploying-resources-in-arm-templates"></a>ARM şablonlarında kaynak dağıtma sırasını tanımlama

Bir kaynağı dağıtırken, dağıtılmadan önce diğer kaynakların var olduğundan emin olmanız gerekebilir. Örneğin, bir SQL veritabanı dağıtmadan önce bir SQL sunucusu gerekir. Bu ilişkiyi, bir kaynağı diğer kaynağa bağımlı olarak işaretleyerek tanımlarsınız. **Bağımlı** öğesi ile bir bağımlılık tanımlarsınız veya **başvuru** işlevini kullanırsınız.

Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Yalnızca aynı şablonda dağıtılan kaynaklar için bağımlılıkları tanımlamanız gerekir.

## <a name="dependson"></a>dependsOn

Şablonunuzda, DependsOn öğesi bir kaynağı bir veya daha fazla kaynağa bağımlı olarak tanımlamanızı sağlar. Değeri, kaynak adlarının virgülle ayrılmış bir listesidir. Liste, [koşullu olarak dağıtılan](conditional-resource-deployment.md)kaynakları içerebilir. Koşullu bir kaynak dağıtılmadığında, Azure Kaynak Yöneticisi kaynağı otomatik olarak gerekli bağımlılıklardan kaldırır.

Aşağıdaki örnekte, yük bakiyesi, sanal ağ ve birden çok depolama hesabı oluşturan bir döngüye bağlı sanal makine ölçeği kümesi gösterilmektedir. Bu diğer kaynaklar aşağıdaki örnekte gösterilmez, ancak şablonun başka bir yerinde bulunmaları gerekir.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "apiVersion": "2016-03-30",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

Önceki örnekte, **storageLoop**adlı bir kopyalama döngüsü aracılığıyla oluşturulan kaynaklara bir bağımlılık dahildir. Örneğin, Azure [Kaynak Yöneticisi'nde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.

Bağımlılıkları tanımlarken, belirsizliği önlemek için kaynak sağlayıcısı ad alanı ve kaynak türünü ekleyebilirsiniz. Örneğin, diğer kaynaklarla aynı adlara sahip olabilecek bir yük dengeleyicisini ve sanal ağı netleştirmek için aşağıdaki biçimi kullanın:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
```

Kaynaklarınız arasındaki ilişkileri haritalamak için bağlı olarak kullanmaya meyilli olsanız da, bunu neden yaptığınızı anlamak önemlidir. Örneğin, kaynakların nasıl birbiriyle bağlantılı olduğunu belgelemek için, Bağlı olmak doğru bir yaklaşım değildir. Dağıtımdan sonra bağlı öğe öğesinde hangi kaynakların tanımlandığını sorgulayamadığınız. Kaynak Yöneticisi bağımlılık olan paralel iki kaynakta dağıtmadığından, bağlı olarak kullanma süresini etkileebilirsiniz.

## <a name="child-resources"></a>Alt kaynaklar

Kaynaklar özelliği, tanımlanan kaynakla ilgili alt kaynakları belirtmenize olanak tanır. Alt kaynaklar yalnızca beş seviye derinliğinde tanımlanabilir. Alt kaynak ve üst kaynak arasında örtük dağıtım bağımlılığı oluşturulmadığını unutmayın. Alt kaynağın üst kaynaktan sonra dağıtılması gerekiyorsa, bağımlı özelliğine olan bağımlılığı açıkça belirtmeniz gerekir.

Her üst kaynak, yalnızca belirli kaynak türlerini alt kaynak olarak kabul eder. Kabul edilen kaynak türleri, ana kaynağın [şablon şemasında](https://github.com/Azure/azure-resource-manager-schemas) belirtilir. Alt kaynak türünün adı, **Microsoft.Web/sites/config** ve **Microsoft.Web/sites/extensions** gibi üst kaynak türünün adını içerir ve **microsoft.Web/sites/extensions microsoft.Web/sites'in**her ikisi de alt kaynaktır.

Aşağıdaki örnekte bir SQL sunucusu ve SQL veritabanı gösterilmektedir. Veritabanı sunucunun bir alt olmasına rağmen, SQL veritabanı ve SQL sunucusu arasında açık bir bağımlılık tanımlandığına dikkat edin.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "apiVersion": "2014-04-01-preview",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "apiVersion": "2014-04-01-preview",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "tags": {
          "displayName": "Database"
        },
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-and-list-functions"></a>referans ve liste fonksiyonları

[Başvuru işlevi,](template-functions-resource.md#reference) bir ifadenin değerini diğer JSON adı ve değer çiftlerinden veya çalışma zamanı kaynaklarından türetmesini sağlar. [Liste* işlevleri](template-functions-resource.md#list) bir liste işleminden bir kaynak için döndürme değerleri.  Başvuru ve liste ifadeleri, başvurulan kaynak aynı şablonda dağıtıldığında ve adıyla (kaynak kimliği ne de kaynak kimliği) ile anıldığında, bir kaynağın diğerine bağlı olduğunu dolaylı olarak bildirir. Kaynak kimliğini başvuru veya liste işlevlerine geçirirseniz, örtülü bir başvuru oluşturulmaz.

Başvuru işlevinin genel biçimi:

```json
reference('resourceName').propertyPath
```

listKeys işlevinin genel biçimi:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

Aşağıdaki örnekte, CDN bitiş noktası açıkça CDN profiline bağlıdır ve dolaylı olarak bir web uygulamasına bağlıdır.

```json
{
    "name": "[variables('endpointName')]",
    "apiVersion": "2016-04-02",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "dependsOn": [
      "[variables('profileName')]"
    ],
    "properties": {
      "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
      ...
    }
```

Bağımlılıkları belirtmek için bu öğeyi veya bağımlı öğeyi kullanabilirsiniz, ancak her ikisini de aynı bağımlı kaynak için kullanmanız gerekmez. Mümkün olduğunda, gereksiz bir bağımlılık eklemeyi önlemek için örtülü bir başvuru kullanın.

Daha fazla bilgi için [başvuru işlevine](template-functions-resource.md#reference)bakın.

## <a name="circular-dependencies"></a>Dairesel bağımlılıklar

Kaynak Yöneticisi şablon doğrulama sırasında dairesel bağımlılıkları tanımlar. Dairesel bir bağımlılık olduğunu belirten bir hata alırsanız, şablonunuzu herhangi bir bağımlılık gerekip gerekmediğini ve kaldırılıp kaldırılamadığını görmek için değerlendirin. Bağımlılıkları kaldırma işe yaramazsa, bazı dağıtım işlemlerini dairesel bağımlılığa sahip kaynaklardan sonra dağıtılan alt kaynaklara taşıyarak dairesel bağımlılıkları önleyebilirsiniz. Örneğin, iki sanal makine dağıtıyorsanız, ancak her birinin diğerine atıfta bulunan özellikleri ayarlamanız gerektiğini varsayalım. Bunları aşağıdaki sırada dağıtabilirsiniz:

1. vm1
2. vm2
3. VM1'in uzatılması vm1 ve vm2'ye bağlıdır. Uzantı vm2'den aldığı değerleri vm1 olarak ayarlar.
4. VM2'nin uzatılması vm1 ve vm2'ye bağlıdır. Uzantı vm1'den aldığı değerleri vm2'de ayarlar.

Dağıtım sırasını değerlendirme ve bağımlılık hatalarını çözme hakkında bilgi için Azure [Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)sorununa bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiyi gözden geçirmek için [Bkz. Öğretici: Bağımlı kaynaklara sahip Azure Kaynak Yöneticisi şablonları oluşturun.](template-tutorial-create-templates-with-dependent-resources.md)
* Bağımlılıkları ayarlarken öneriler için azure [kaynak yöneticisi şablonu en iyi uygulamaları](template-best-practices.md)görün.
* Dağıtım sırasında sorun giderme bağımlılıkları hakkında bilgi edinmek için Azure [Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)sorununa bakın.
* Azure Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için [bkz.](template-syntax.md)
* Şablondaki kullanılabilir işlevlerin listesi için [Şablon işlevlerine](template-functions.md)bakın.

