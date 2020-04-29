---
title: Birden çok kaynak örneğini dağıtma
description: Kaynak türünü birçok kez dağıtmak için bir Azure Resource Manager şablonunda kopyalama işlemini ve dizileri kullanın.
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: e65ab93c21daffa0053e53d953fe95fa9f28e2a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80153327"
---
# <a name="resource-iteration-in-arm-templates"></a>ARM şablonlarındaki kaynak yinelemesi

Bu makalede, Azure Resource Manager (ARM) şablonunuzda bir kaynağın birden fazla örneğini nasıl oluşturacağınız gösterilmektedir. Şablonunuzun kaynaklar bölümüne **Copy** öğesini ekleyerek, dağıtılacak kaynak sayısını dinamik olarak ayarlayabilirsiniz. Ayrıca, şablon söz dizimini yinelemek zorunda kalmaktan kaçının.

Ayrıca, [özellikleri](copy-properties.md), [değişkenleri](copy-variables.md) ve [çıkışları](copy-outputs.md)içeren Kopyala özelliğini de kullanabilirsiniz.

Bir kaynağın hiç dağıtılıp dağıtılmadığını belirtmeniz gerekiyorsa bkz. [koşul öğesi](conditional-resource-deployment.md).

## <a name="resource-iteration"></a>Kaynak yinelemesi

Copy öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": {
  "name": "<name-of-loop>",
  "count": <number-of-iterations>,
  "mode": "serial" <or> "parallel",
  "batchSize": <number-to-deploy-serially>
}
```

**Name** özelliği, döngüsünü tanımlayan herhangi bir değerdir. **Count** özelliği, kaynak türü için istediğiniz yineleme sayısını belirtir.

Kaynakların paralel veya sıralı olarak dağıtılıp dağıtılmadığını belirtmek için **Mode** ve **BatchSize** özelliklerini kullanın. Bu özellikler [seri veya paralel](#serial-or-parallel)olarak açıklanmaktadır.

Aşağıdaki örnek, **Storagecount** parametresinde belirtilen depolama hesabı sayısını oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

Her bir kaynağın adı, döngüsünde geçerli yinelemeyi döndüren `copyIndex()` işlevini içerir. `copyIndex()` sıfır tabanlıdır. Bu nedenle, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex())]",
```

Şu adları oluşturur:

* storage0
* storage1
* storage2.

Dizin değerini kaydırmak için copyIndex () işlevine bir değer geçirebilirsiniz. Yineleme sayısı kopyalama öğesinde hala belirtilir, ancak Copyındex değeri belirtilen değere göre denkleştirilir. Bu nedenle, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Şu adları oluşturur:

* storage1
* storage2
* storage3

Dizideki her öğe arasında yineleme yapmak için, diziler ile çalışırken kopyalama işlemi faydalıdır. Yineleme sayısını `length` belirtmek ve `copyIndex` dizideki geçerli dizini almak için dizideki işlevini kullanın.

Aşağıdaki örnek, parametresinde belirtilen her ad için bir depolama hesabı oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageNames": {
          "type": "array",
          "defaultValue": [
            "contoso",
            "fabrikam",
            "coho"
          ]
      }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(parameters('storageNames')[copyIndex()], uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": "[length(parameters('storageNames'))]"
      }
    }
  ],
  "outputs": {}
}
```

Dağıtılan kaynaklardan değer döndürmek istiyorsanız, [çıktılar bölümünde Kopyala](copy-outputs.md)' yı kullanabilirsiniz.

## <a name="serial-or-parallel"></a>Seri veya paralel

Varsayılan olarak Kaynak Yöneticisi, kaynakları paralel olarak oluşturur. Şablondaki 800 kaynağın toplam sınırının dışında, paralel olarak dağıtılan kaynak sayısına sınır uygulanmaz. Bunların oluşturulma sırası garanti edilmez.

Ancak, kaynakların sırayla dağıtılmasını belirtmek isteyebilirsiniz. Örneğin, bir üretim ortamını güncelleştirirken, yalnızca belirli bir sayının herhangi bir zamanda güncelleştirilmesini sağlamak isteyebilirsiniz. Bir kaynağın birden fazla örneğini seri olarak dağıtmak için, **seri** olarak `batchSize` ve `mode` aynı anda dağıtılacak örnek sayısına ayarlayın. Seri modda Kaynak Yöneticisi, döngüdeki önceki örneklerde bir bağımlılık oluşturur, bu nedenle, önceki toplu işlem tamamlanana kadar bir toplu işlem başlatmaz.

Örneğin, depolama hesaplarını tek seferde bir kez dağıtmak için şunu kullanın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Mode özelliği, varsayılan değer olan **Parallel**öğesini de kabul eder.

## <a name="depend-on-resources-in-a-loop"></a>Bir döngüdeki kaynaklara bağlıdır

Bir kaynağın, `dependsOn` öğesini kullanarak başka bir kaynaktan sonra dağıtıldığını belirtirsiniz. Bir döngüde kaynak koleksiyonuna bağlı olan bir kaynağı dağıtmak için, Bağımlıdson öğesinde kopyalama döngüsünün adını sağlayın. Aşağıdaki örnek, sanal makineyi dağıtmadan önce üç depolama hesabının nasıl dağıtılacağını göstermektedir. Tam sanal makine tanımı gösterilmez. Kopyalama öğesinin adı olarak `storagecopy` ayarlanmış olduğuna ve sanal makine Için Bağımlıdson öğesinin de olarak `storagecopy`ayarlandığından emin olun.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-06-15",
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

## <a name="iteration-for-a-child-resource"></a>Alt kaynak için yineleme

Alt kaynak için bir kopyalama döngüsü kullanamazsınız. Genellikle başka bir kaynak içinde iç içe olarak tanımladığınız bir kaynağın birden fazla örneğini oluşturmak için, bunun yerine o kaynağı en üst düzey kaynak olarak oluşturmanız gerekir. Üst kaynakla ilişkiyi tür ve ad özellikleri aracılığıyla tanımlarsınız.

Örneğin, genellikle bir veri kümesini bir veri fabrikası içinde alt kaynak olarak tanımladığınızı varsayalım.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

Birden fazla veri kümesi oluşturmak için veri fabrikası dışına taşıyın. Veri kümesi Data Factory ile aynı düzeyde olmalıdır, ancak yine de Data Factory 'nin bir alt kaynağıdır. Veri kümesi ve Veri Fabrikası arasındaki ilişkiyi tür ve ad özellikleriyle koruyabilirsiniz. Tür artık şablondaki konumundan çıkarsanamıyor, tam türü şu biçimde sağlamanız gerekir: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Veri fabrikasının bir örneğiyle üst/alt ilişkisi kurmak için, üst kaynak adını içeren veri kümesi için bir ad sağlayın. `{parent-resource-name}/{child-resource-name}` biçimini kullanın.

Aşağıdaki örnek, uygulamayı göstermektedir:

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="copy-limits"></a>Sınırları Kopyala

Sayım 800 ' i aşamaz.

Sayı negatif bir sayı olamaz. Azure PowerShell 2,6 veya üzeri, Azure CLı 2.0.74 veya üzeri ya da REST API sürüm **2019-05-10** veya üzeri bir şablon dağıtırsanız, sayıyı sıfıra ayarlayabilirsiniz. PowerShell, CLı ve REST API 'nin önceki sürümleri Count için sıfırı desteklemez.

Kopya ile [tamamlanmış mod dağıtımını](deployment-modes.md) kullanırken dikkatli olun. Tüm modu bir kaynak grubuna yeniden dağıtıyorsanız, kopyalama döngüsü çözümlendikten sonra şablonda belirtilmeyen tüm kaynaklar silinir.

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde bir kaynak veya özelliğin birden fazla örneğini oluşturmak için yaygın senaryolar gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
|[Depolama alanını Kopyala](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Adında Dizin numarası olan birden fazla depolama hesabı dağıtır. |
|[Seri kopyalama depolaması](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Birden çok depolama hesabını zamanında dağıtır. Ad, Dizin numarasını içerir. |
|[Depolamayı dizi ile kopyalama](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Birkaç depolama hesabı dağıtır. Ad, diziden bir değer içerir. |
|[Değişken sayıda veri diskine sahip VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Bir sanal makine ile birden fazla veri diski dağıtır. |
|[Çoklu güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Bir ağ güvenlik grubuna birkaç güvenlik kuralı dağıtır. Bir parametreden güvenlik kuralları oluşturur. Parametresi için bkz. [birden çok NSG parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: ARM şablonları kullanarak birden çok kaynak örneği oluşturma](template-tutorial-create-multiple-instances.md).
* Copy öğesinin diğer kullanımları için bkz.:
  * [ARM şablonlarındaki Özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
  * [ARM şablonlarındaki çıkış yinelemesi](copy-outputs.md)
* İç içe şablonlar ile kopyalama kullanma hakkında daha fazla bilgi için, bkz. [kopyalamayı kullanma](linked-templates.md#using-copy).
* Bir şablonun bölümleri hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [ARM şablonları yazma](template-syntax.md).
* Şablonunuzu dağıtmayı öğrenmek için bkz. [ARM şablonuyla uygulama dağıtma](deploy-powershell.md).

