---
title: Kaynaklar için dağıtım sırasını ayarla
description: Kaynakların doğru sırada dağıtılmasını sağlamak için dağıtım sırasında bir kaynağı başka bir kaynağa bağımlı olarak nasıl ayarlayabileceğinizi açıklar.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: 44cf793859d2817695a58bd1159e2f4465c1f9c2
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76121973"
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarda kaynak dağıtma sırasını tanımlayın

Bir kaynağı dağıtırken, dağıtılmadan önce diğer kaynakların mevcut olduğundan emin olmanız gerekebilir. Örneğin, bir SQL veritabanı dağıtılmadan önce bir SQL Server gerekir. Bu ilişkiyi, bir kaynağı diğer kaynağa bağımlı olarak işaretleyerek tanımlarsınız. Dependency **DSON** öğesiyle bir bağımlılık tanımlar veya **başvuru** işlevini kullanarak.

Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Yalnızca aynı şablonda dağıtılan kaynaklar için bağımlılıklar tanımlamanız gerekir.

## <a name="dependson"></a>dependsOn

Şablonunuzda, Bağımlıdson öğesi bir kaynağı bir veya daha fazla kaynağa bağımlı olarak tanımlamanızı sağlar. Değeri, kaynak adlarının virgülle ayrılmış listesidir. Bu liste, [koşullu olarak dağıtılan](conditional-resource-deployment.md)kaynakları içerebilir. Koşullu bir kaynak dağıtıldığında Azure Resource Manager, gerekli bağımlılıklardan otomatik olarak kaldırır.

Aşağıdaki örnek, bir yük dengeleyiciye, sanal ağa ve birden çok depolama hesabı oluşturan bir döngüye bağlı olan bir sanal makine ölçek kümesini gösterir. Bu diğer kaynaklar aşağıdaki örnekte gösterilmez, ancak şablonda başka bir yerde bulunması gerekir.

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

Yukarıdaki örnekte, **Storageloop**adlı bir kopya döngüsüyle oluşturulan kaynaklara bir bağımlılık dahildir. Bir örnek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](create-multiple-instances.md).

Bağımlılıkları tanımlarken, belirsizlik olmaması için kaynak sağlayıcısı ad alanını ve kaynak türünü dahil edebilirsiniz. Örneğin, diğer kaynaklarla aynı ada sahip olabilecek bir yük dengeleyiciyi ve sanal ağı netleştirmek için aşağıdaki biçimi kullanın:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
```

Kaynaklarınız arasındaki ilişkileri eşlemek için Bağımlılıson ' u kullanmaya da ihtiyacınız olsa da, bunu neden yaptığınızı anlamak önemlidir. Örneğin, kaynakların nasıl birbirine bağlı olduğunu belgelemek için, Bağımlıdson doğru yaklaşım değildir. Dağıtımdan sonra Bağımlıdson öğesinde tanımlanan kaynakları sorgulayamaz. Bağımlılıkların kullanıldığı paralel iki kaynak olarak Kaynak Yöneticisi dağıtmadığından, bağımlılar kullanarak dağıtım süresini etkilersiniz.

## <a name="child-resources"></a>Alt kaynaklar

Resources özelliği, tanımlanmakta olan kaynakla ilgili alt kaynakları belirtmenize olanak tanır. Alt kaynaklar yalnızca beş düzey derinlikli tanımlanmış olabilir. Bir alt kaynak ve üst kaynak arasında örtük bir dağıtım bağımlılığının oluşturulmadığını göz önünde bulundurulmamak önemlidir. Alt kaynağın ana kaynaktan sonra dağıtılması gerekiyorsa, bu bağımlılığı Bağımlıdson özelliği ile açıkça sağlamalısınız.

Her üst kaynak, alt kaynaklar olarak yalnızca belirli kaynak türlerini kabul eder. Kabul edilen kaynak türleri, üst kaynağın [şablon şemasında](https://github.com/Azure/azure-resource-manager-schemas) belirtilir. Alt kaynak türünün adı, **Microsoft. Web**/ **Sites/config** ve **Microsoft. Web/Sites/Extensions** gibi üst kaynak türünün adını içerir.

Aşağıdaki örnekte bir SQL Server ve SQL veritabanı gösterilmektedir. Veritabanı sunucusunun bir alt öğesi olsa bile SQL veritabanı ile SQL Server arasında açık bir bağımlılık tanımlandığına dikkat edin.

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

## <a name="reference-and-list-functions"></a>Başvuru ve liste işlevleri

[Başvuru işlevi](template-functions-resource.md#reference) , bir ifadenin DEĞERINI diğer JSON adından ve değer çiftlerinden veya çalışma zamanı kaynaklarından türemesini sağlar. [Liste * işlevleri](template-functions-resource.md#list) bir kaynak için bir liste işleminden değerler döndürür.  Başvuru ve liste ifadeleri, başvurulan kaynak aynı şablonda dağıtıldığında ve adı (kaynak KIMLIĞI değil) tarafından başvurulduğu zaman, bir kaynağın başka bir kaynağa bağlı olduğunu dolaylı olarak bildirir. Kaynak KIMLIĞINI başvuru veya liste işlevlerine geçirirseniz, örtük bir başvuru oluşturulmaz.

Başvuru işlevinin genel biçimi:

```json
reference('resourceName').propertyPath
```

ListKeys işlevinin genel biçimi:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

Aşağıdaki örnekte, bir CDN uç noktası açıkça CDN profiline bağımlıdır ve örtülü olarak bir Web uygulamasına bağlıdır.

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

Bağımlılıkları belirtmek için bu öğeyi ya da Bağımlıdson öğesini kullanabilirsiniz, ancak aynı bağımlı kaynak için her ikisini de kullanmanız gerekmez. Mümkün olduğunda, gereksiz bir bağımlılık eklemekten kaçınmak için örtük bir başvuru kullanın.

Daha fazla bilgi için bkz. [başvuru işlevi](template-functions-resource.md#reference).

## <a name="circular-dependencies"></a>Döngüsel bağımlılıklar

Kaynak Yöneticisi, şablon doğrulaması sırasında dairesel bağımlılıkları belirler. Döngüsel bağımlılığın olduğunu belirten bir hata alırsanız, herhangi bir bağımlılığın gerekli olup olmadığını ve kaldırılamadığını görmek için şablonunuzu değerlendirin. Bağımlılıkları kaldırmak işe yaramazsa, bazı dağıtım işlemlerini, döngüsel bağımlılığı olan kaynaklardan sonra dağıtılan alt kaynaklara taşıyarak dairesel bağımlılıklardan kaçınabilirsiniz. Örneğin, iki sanal makine dağıttığını, ancak birbirlerine başvuran her bir üzerinde Özellikler ayarlamanız gerektiğini varsayalım. Bunları aşağıdaki sırayla dağıtabilirsiniz:

1. vm1
2. vm2
3. VM1 üzerindeki uzantı VM1 ve VM2 'ye bağımlıdır. Uzantı, VM1 'teki VM2 'dan aldığı değerleri ayarlar.
4. VM2 üzerindeki uzantı VM1 ve VM2 'ye bağımlıdır. Uzantı, VM2 'teki VM1 'dan aldığı değerleri ayarlar.

Dağıtım sırasını değerlendirme ve bağımlılık hatalarını çözme hakkında bilgi için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarıyla Ilgili sorunları giderme](common-deployment-errors.md).

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](template-tutorial-create-templates-with-dependent-resources.md).
* Bağımlılıkları ayarlarken öneriler için bkz. [Azure Resource Manager şablonu en iyi yöntemleri](template-best-practices.md).
* Dağıtım sırasında bağımlılıklar sorunlarını giderme hakkında bilgi edinmek için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarıyla Ilgili sorunları giderme](common-deployment-errors.md).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için bkz. [yazma şablonları](template-syntax.md).
* Bir şablondaki kullanılabilir işlevlerin listesi için bkz. [Şablon işlevleri](template-functions.md).

