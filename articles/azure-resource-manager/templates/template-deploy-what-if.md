---
title: Şablon dağıtımı ne-if (Önizleme)
description: Azure Kaynak Yöneticisi şablonunu dağıtmadan önce kaynaklarınızda hangi değişikliklerin gerçekleşeceğini belirleyin.
author: mumian
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: jgao
ms.openlocfilehash: bc42585204e5cc2c3ece5293a3934fd22fe8507b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156455"
---
# <a name="arm-template-deployment-what-if-operation-preview"></a>ARM şablon dağıtımı ne-if işlemi (Önizleme)

Azure Kaynak Yöneticisi (ARM) şablonu dağıtmadan önce, meydana gelecek değişiklikleri önizlemek isteyebilirsiniz. Azure Kaynak Yöneticisi, şablonu dağıttınızca kaynakların nasıl değişeceğini görmenizi sağlamak için "durum" işlemini sağlar. What-if işlemi varolan kaynaklarda herhangi bir değişiklik yapmaz. Bunun yerine, belirtilen şablon dağıtılırsa değişiklikleri tahmin eder.

> [!NOTE]
> What-if işlemi şu anda önizlemede. Kullanmak [için önizlemeye kaydolmalısınız.](https://aka.ms/armtemplatepreviews) Önizleme sürümü olarak, sonuçlar bazen bir kaynağın gerçekte bir değişiklik olmadığında değişeceğini gösterebilir. Bu sorunları azaltmak için çalışıyoruz ama yardımınıza ihtiyacımız var. Lütfen bu sorunları [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

PowerShell komutları veya REST API işlemleri ile what-if işlemini kullanabilirsiniz.

PowerShell'de çıktı, farklı değişiklik türlerini görmenize yardımcı olan renk kodlu sonuçlar içerir.

![Kaynak Yöneticisi şablon dağıtımı ne-if işlemi tam kaynak payload ve değişiklik türleri](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Metin ouptput:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

## <a name="what-if-commands"></a>What-if komutları

If işlemi için Azure PowerShell veya Azure REST API'sini kullanabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell

Şablondağıtmadan önce değişikliklerin önizlemesini görmek için `-Whatif` dağıtım komutuna geçiş parametresini ekleyin.

* `New-AzResourceGroupDeployment -Whatif`kaynak grubu dağıtımları için
* `New-AzSubscriptionDeployment -Whatif`ve `New-AzDeployment -Whatif` abonelik düzeyi dağıtımları için

Veya, değişiklikleri önizlemek `-Confirm` ve dağıtıma devam etmek için istenmek için geçiş parametresini kullanabilirsiniz.

* `New-AzResourceGroupDeployment -Confirm`kaynak grubu dağıtımları için
* `New-AzSubscriptionDeployment -Confirm`ve `New-AzDeployment -Confirm` abonelik düzeyi dağıtımları için

Önceki komutlar, el ile inceleyebileceğiniz bir metin özeti döndürer. Değişiklikler için programlı olarak inceleyebileceğiniz bir nesne almak için şunları kullanın:

* `$results = Get-AzResourceGroupDeploymentWhatIf`kaynak grubu dağıtımları için
* `$results = Get-AzSubscriptionDeploymentWhatIf`veya `$results = Get-AzDeploymentWhatIf` abonelik düzeyi dağıtımları için

> [!NOTE]
> Sürüm 2.0.1-alpha5 sürümünden önce komutu `New-AzDeploymentWhatIf` kullandınız. Bu komut `Get-AzDeploymentWhatIf`, , `Get-AzResourceGroupDeploymentWhatIf`ve `Get-AzSubscriptionDeploymentWhatIf` komutları ile değiştirildi. Önceki bir sürümü kullandıysanız, sözdizimini güncelleştirmeniz gerekir. Parametre `-ScopeType` kaldırıldı.

### <a name="azure-rest-api"></a>Azure REST API

REST API için şunları kullanın:

* [Dağıtımlar - Kaynak](/rest/api/resources/deployments/whatif) grubu dağıtımları için Ne Olur
* [Dağıtımlar - Abonelik](/rest/api/resources/deployments/whatifatsubscriptionscope) düzeyi dağıtımları için Abonelik Kapsamında Ne Olur

## <a name="change-types"></a>Türleri değiştir

What-if işlemi altı farklı değişiklik türünü listeler:

- **Create**: Kaynak şu anda yok, ancak şablonda tanımlanır. Kaynak oluşturulacak.

- **Sil**: Bu değişiklik türü yalnızca dağıtım için [tam mod](deployment-modes.md) kullanırken geçerlidir. Kaynak var, ancak şablonda tanımlanmamış. Tam modda kaynak silinir. Bu değişiklik türüne yalnızca [tam mod silmeyi destekleyen](complete-mode-deletion.md) kaynaklar dahildir.

- **Yoksay :** Kaynak var, ancak şablonda tanımlanmamış. Kaynak dağıtılamayacak veya değiştirilmez.

- **NoChange**: Kaynak var ve şablonda tanımlanır. Kaynak yeniden dağıtılır, ancak kaynağın özellikleri değişmez. Bu değişiklik [türü, Varsayılan](#result-format) değer `FullResourcePayloads`olan ResultFormat olarak ayarlandığında döndürülür.

- **Değiştir**: Kaynak var ve şablonda tanımlanır. Kaynak yeniden dağıtılır ve kaynağın özellikleri değişir. Bu değişiklik [türü, Varsayılan](#result-format) değer `FullResourcePayloads`olan ResultFormat olarak ayarlandığında döndürülür.

- **Deploy**: Kaynak var ve şablonda tanımlanır. Kaynak yeniden dağıtılır. Kaynağın özellikleri değişebilir veya değişmeyebilir. İşlem, herhangi bir özelliğin değişip değişmeyeceğini belirlemek için yeterli bilgiye sahip olmadığında bu değişiklik türünü döndürür. Bu koşulu yalnızca [ResultFormat](#result-format) `ResourceIdOnly`'a ayarlandığında görürsünüz.

## <a name="result-format"></a>Sonuç biçimi

Öngörülen değişikliklerle ilgili döndürülen ayrıntı düzeyini denetleyebilirsiniz. Dağıtım komutlarında (`New-Az*Deployment`), **-WhatIfResultFormat** parametresini kullanın. Programlı nesne komutlarında`Get-Az*DeploymentWhatIf`( ), **ResultFormat** parametresini kullanın.

Değişecek kaynakların listesini ve değişecek özellikler le ilgili ayrıntıları almak için biçim parametresini **FullResourcePayloads** olarak ayarlayın. Değişecek kaynakların listesini almak için biçim parametresini **ResourceIdOnly** olarak ayarlayın. Varsayılan değer **FullResourcePayloads'tir.**  

Aşağıdaki sonuçlar iki farklı çıktı biçimini gösterir:

- Tam kaynak yükleri

  ```powershell
  Resource and property changes are indicated with these symbols:
    - Delete
    + Create
    ~ Modify

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
      - tags.Owner: "Team A"
      ~ properties.addressSpace.addressPrefixes: [
        - 0: "10.0.0.0/16"
        + 0: "10.0.0.0/15"
        ]
      ~ properties.subnets: [
        - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

        ]

  Resource changes: 1 to modify.
  ```

- Yalnızca kaynak kimliği

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>"Nedir-if işlemini çalıştırın

### <a name="set-up-environment"></a>Ortam ayarlama

Ne işe yaradığını görmek için, bazı testler çalıştıralım. İlk olarak, [sanal ağ oluşturan bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json)dağıtın. Değişikliklerin ne olursa olup olmadığını test etmek için bu sanal ağı kullanırsınız.

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

### <a name="test-modification"></a>Test modifikasyonu

Dağıtım tamamlandıktan sonra, ne olur işlemini test etmeye hazırsınız. Bu kez sanal ağı değiştiren bir [şablon dağıtın.](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json) Orijinal etiketlerden biri eksik, bir alt ağ kaldırıldı ve adres öneki değişti.

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

What-if çıktısı aşağıdakilere benzer görünür:

![Kaynak Yöneticisi şablon dağıtımı ne-if işlem çıktısı](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Metin çıktısı:

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

        name:                     "subnet001"
        properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

Çıktının üst kısmında değişikliklerin türünü belirtmek için renklerin tanımlandığına dikkat edin.

Çıktının alt kısmında, Sahibi etiketinin silindiği gösterir. Adres öneki 10.0.0.0/16'dan 10.0.0.0/15'e değiştirildi. Subnet001 adlı alt ağ silindi. Bu değişikliklerin gerçekte dağıtılmadığını unutmayın. Şablonu dağıtsanız meydana gelecek değişikliklerin önizlemesini görürsünüz.

Silinmiş olarak listelenen özelliklerden bazıları gerçekte değişmez. Özellikler şablonda yokken yanlış olarak silinmiş olarak bildirilebilir, ancak dağıtım sırasında varsayılan değerler olarak otomatik olarak ayarlanır. Bu sonuç, if yanıtında "gürültü" olarak kabul edilir. Dağıtılan son kaynak, özellikler için ayarlanan değerlere sahip olur. What-if işlemi olgunlaştıkça, bu özellikler sonuçtan filtrelenir.

## <a name="programmatically-evaluate-what-if-results"></a>Programlı olarak ne-if sonuçlarını değerlendirin

Şimdi, komutu bir değişkene ayarlayarak "ne olur" sonuçlarını programlı olarak değerlendirelim.

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIf `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

Her değişikliğin bir özetini görebilirsiniz.

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

## <a name="confirm-deletion"></a>Silme işlemini onaylama

What-if işlemi [dağıtım modunu](deployment-modes.md)kullanarak destekler. Modu tamamlamak üzere ayarlandığında, şablonda olmayan kaynaklar silinir. Aşağıdaki örnek, tam modda [tanımlanmamış kaynakları olmayan](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) bir şablon dağıtır.

Şablondağıtmadan önce değişiklikleri önizlemek `-Confirm` için dağıtım komutuyla geçiş parametresini kullanın. Değişiklikler beklediğiniz gibiyse, dağıtımın tamamlanmasını istediğinizi onaylayın.

```azurepowershell
New-AzResourceGroupDeployment `
  -Confirm `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Şablonda hiçbir kaynak tanımlanmadığından ve dağıtım modu tamamlanmaya ayarlandığı için sanal ağ silinir.

![Kaynak Yöneticisi şablon dağıtımı nedir-if operasyon çıktısı dağıtım modu tamamlandı](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

Metin çıktısı:

```powershell
Resource and property changes are indicated with this symbol:
  - Delete

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  - Microsoft.Network/virtualNetworks/vnet-001

      id:
"/subscriptions/./resourceGroups/ExampleGroup/providers/Microsoft.Network/virtualNet
works/vnet-001"
      location:        "centralus"
      name:            "vnet-001"
      tags.CostCenter: "12345"
      tags.Owner:      "Team A"
      type:            "Microsoft.Network/virtualNetworks"

Resource changes: 1 to delete.

Are you sure you want to execute the deployment?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

Beklenen değişiklikleri görürsünüz ve dağıtımın çalışmasını istediğinizi onaylayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Eğer ne olursa filminin önizleme sürümünden kaynaklanan yanlış sonuçlar [https://aka.ms/whatifissues](https://aka.ms/whatifissues)fark ederseniz, lütfen sorunları .
- Azure PowerShell ile şablon dağıtmak için [BKZ.](deploy-powershell.md)
- REST ile şablondağıtmak için ARM [şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıt'a](deploy-rest.md)bakın.
