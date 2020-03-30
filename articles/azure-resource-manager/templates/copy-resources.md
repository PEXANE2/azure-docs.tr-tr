---
title: Birden çok kaynak örneğini dağıtma
description: Kaynak türünü birçok kez dağıtmak için Azure Kaynak Yöneticisi şablonundaki kopyalama işlemini ve dizileri kullanın.
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: e65ab93c21daffa0053e53d953fe95fa9f28e2a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153327"
---
# <a name="resource-iteration-in-arm-templates"></a>ARM şablonlarında kaynak yinelemesi

Bu makalede, Azure Kaynak Yöneticisi (ARM) şablonunuzda birden fazla kaynak örneğinin nasıl oluşturulabileceğiniz gösterilmektedir. **Şablonunuzun** kaynaklar bölümüne kopyalama öğesini ekleyerek, dağıtılacak kaynak sayısını dinamik olarak ayarlayabilirsiniz. Ayrıca şablon sözdizimini yinelemek zorunda kalmaktan da kaçının.

Ayrıca [özellikleri,](copy-properties.md) [değişkenler](copy-variables.md) ve [çıkışları](copy-outputs.md)ile kopya kullanabilirsiniz.

Bir kaynağın dağıtılıp dağıtılmadığını belirtmeniz gerekiyorsa, [koşul öğesine](conditional-resource-deployment.md)bakın.

## <a name="resource-iteration"></a>Kaynak yinelemesi

Kopyalama öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": {
  "name": "<name-of-loop>",
  "count": <number-of-iterations>,
  "mode": "serial" <or> "parallel",
  "batchSize": <number-to-deploy-serially>
}
```

**Ad** özelliği döngütanımlayan herhangi bir değerdir. **Sayım** özelliği, kaynak türü için istediğiniz yineleme sayısını belirtir.

Kaynakların paralel veya sırayla dağıtılmak üzere dağıtılmak üzere **modu** ve **toplu Boyut** özelliklerini kullanın. Bu özellikler [Seri veya Paralel](#serial-or-parallel)olarak açıklanmıştır.

Aşağıdaki örnek, **depolamaSayalanı** parametresinde belirtilen depolama hesabı sayısını oluşturur.

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

Her kaynağın adının döngüdeki `copyIndex()` geçerli yinelemeyi döndüren işlevi içerdiğine dikkat edin. `copyIndex()` sıfır tabanlıdır. Yani, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex())]",
```

Şu adları oluşturur:

* depolama0
* depolama1
* depolama2.

Dizin değerini kaydırmak için copyIndex () işlevine bir değer geçirebilirsiniz. Yineleme sayısı yine kopya öğesinde belirtilir, ancak copyIndex değeri belirtilen değerle dengelenir. Yani, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Şu adları oluşturur:

* depolama1
* depolama2
* depolama3

Dizideki her öğeyi yineleyebildiğiniziçin dizilerle çalışırken kopyalama işlemi yararlıdır. Yinelemesayısı `length` belirtmek ve `copyIndex` dizideki geçerli diziyi almak için dizideki işlevi kullanın.

Aşağıdaki örnek, parametrede sağlanan her ad için bir depolama hesabı oluşturur.

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

Dağıtılan kaynaklardan değerleri döndürmek istiyorsanız, [çıktılar bölümünde kopyayı](copy-outputs.md)kullanabilirsiniz.

## <a name="serial-or-parallel"></a>Seri veya Paralel

Varsayılan olarak, Kaynak Yöneticisi kaynakları paralel olarak oluşturur. Şablondaki toplam 800 kaynak sınırı dışında paralel olarak dağıtılan kaynak sayısına sınır uygulamaz. Oluşturuldukları sıra garanti değildir.

Ancak, kaynakların sırayla dağıtılmış olduğunu belirtmek isteyebilirsiniz. Örneğin, bir üretim ortamını güncelleştirirken, güncelleştirmeleri yalnızca belirli bir sayının herhangi bir anda güncelleştirilebilmesini sağlamak isteyebilirsiniz. Bir kaynağın birden fazla örneğini seri `mode` olarak `batchSize` dağıtmak için, **seri** olarak ayarlayın ve bir seferde dağıtılacak örnek sayısına ayarlayın. Seri modu ile Kaynak Yöneticisi döngüdeki önceki örneklere bağımlılık oluşturur, böylece önceki toplu iş tamamlanana kadar bir toplu iş başlatmaz.

Örneğin, depolama hesaplarını aynı anda ikişer seri olarak dağıtmak için şunları kullanın:

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

Mod özelliği, varsayılan değer olan **paraleli**de kabul eder.

## <a name="depend-on-resources-in-a-loop"></a>Döngüdeki kaynaklara bağlı

`dependsOn` Öğeyi kullanarak bir kaynağın başka bir kaynaktan sonra dağıtılan bir kaynak olduğunu belirtirsiniz. Bir döngüdeki kaynakların toplanmasına bağlı bir kaynak dağıtmak için, bağlı öğedeki kopya döngüsünün adını sağlayın. Aşağıdaki örnek, sanal makineyi dağıtmadan önce üç depolama hesabının nasıl dağıtılanabildiğini gösterir. Tam sanal makine tanımı gösterilmez. Kopyalama öğesinin ad olarak `storagecopy` ayarlanmasına ve sanal makine için bağlı `storagecopy`bağlı öğenin de .

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

## <a name="iteration-for-a-child-resource"></a>Bir alt kaynak için yineleme

Alt kaynak için kopyalama döngüsü kullanamazsınız. Genellikle başka bir kaynak içinde iç içe olarak tanımladığınız bir kaynağın birden fazla örneğini oluşturmak için, bu kaynağı üst düzey bir kaynak olarak oluşturmanız gerekir. Üst kaynakla olan ilişkiyi tür ve ad özellikleri yle tanımlarsınız.

Örneğin, bir veri kümesini genellikle bir veri fabrikasında alt kaynak olarak tanımladığınızı varsayalım.

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

Birden fazla veri kümesi oluşturmak için veri fabrikasının dışına taşıyın. Veri kümesi, veri fabrikasıyla aynı düzeyde olmalıdır, ancak yine de veri fabrikasının alt kaynağıdır. Tür ve ad özellikleri aracılığıyla veri kümesi ve veri fabrikası arasındaki ilişkiyi korursunuz. Tür artık şablondaki konumundan çıkarıladığından, biçimde tam nitelikli türü sağlamanız `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`gerekir: .

Veri fabrikasının bir örneğiyle üst/alt ilişkisi kurmak için, üst kaynak adını içeren veri kümesi için bir ad sağlayın. `{parent-resource-name}/{child-resource-name}` biçimini kullanın.

Aşağıdaki örnek, uygulamayı gösterir:

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

## <a name="copy-limits"></a>Kopyalama sınırları

Sayım 800'i geçemez.

Sayı negatif sayı olamaz. Azure PowerShell 2.6 veya sonraki sürümlerle, Azure CLI 2.0.74 veya sonraki sürümlerle veya REST API sürümü **2019-05-10** veya daha yeni bir şablon uyguluyorsanız, sayıyı sıfıra ayarlayabilirsiniz. PowerShell, CLI ve REST API'nin önceki sürümleri nde sayım için sıfır desteklenmez.

Kopya ile [tam mod dağıtımı](deployment-modes.md) kullanırken dikkatli olun. Tam modla bir kaynak grubuna yeniden dağıtırsanız, kopya döngüsü çözüldükten sonra şablonda belirtilmeyen tüm kaynaklar silinir.

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örnekler, bir kaynak veya özelliğin birden fazla örneğini oluşturmak için yaygın senaryoları gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Depolamayı kopyala](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Adında bir dizin numarası bulunan birden fazla depolama hesabı dağıtıyor. |
|[Seri kopya depolama](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Birden fazla depolama hesabını teker teker dağır. Ad dizin numarasını içerir. |
|[Depolamayı diziyle kopyalama](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Birkaç depolama hesabı dağır. Ad, bir diziden bir değer içerir. |
|[Değişken sayıda veri diski içeren VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Sanal bir makineyle birkaç veri diski dağıtıyor. |
|[Birden çok güvenlik kuralı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Bir ağ güvenlik grubuna çeşitli güvenlik kuralları dağıtLar. Güvenlik kurallarını bir parametreden inşa eder. Parametre için [birden çok NSG parametre dosyasına](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json)bakın. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiye geçmek için [Bkz. Öğretici: ARM şablonlarını kullanarak birden çok kaynak örneği oluşturun.](template-tutorial-create-multiple-instances.md)
* Kopyalama öğesinin diğer kullanımları için bkz:
  * [ARM şablonlarında özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
  * [ARM şablonlarında çıkış yinelemesi](copy-outputs.md)
* İç içe şablonlarla kopyalama yı kullanma hakkında bilgi [için](linked-templates.md#using-copy)bkz.
* Şablonun bölümleri hakkında bilgi edinmek istiyorsanız, [Bkz.](template-syntax.md)
* Şablonunuzu nasıl dağıtılayarak dağıtılayacaklarını öğrenmek için [bkz.](deploy-powershell.md)

