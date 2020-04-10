---
title: Şablon dağıtımı ne-if (Önizleme)
description: Azure Kaynak Yöneticisi şablonunu dağıtmadan önce kaynaklarınızda hangi değişikliklerin gerçekleşeceğini belirleyin.
author: mumian
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: jgao
ms.openlocfilehash: b8e94d0b4f364e2873dfc21792a67f11c33483bf
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010198"
---
# <a name="arm-template-deployment-what-if-operation-preview"></a>ARM şablon dağıtımı ne-if işlemi (Önizleme)

Azure Kaynak Yöneticisi (ARM) şablonu dağıtmadan önce, meydana gelecek değişiklikleri önizlemek isteyebilirsiniz. Azure Kaynak Yöneticisi, şablonu dağıttınızca kaynakların nasıl değişeceğini görmenizi sağlamak için "durum" işlemini sağlar. What-if işlemi varolan kaynaklarda herhangi bir değişiklik yapmaz. Bunun yerine, belirtilen şablon dağıtılırsa değişiklikleri tahmin eder.

> [!NOTE]
> What-if işlemi şu anda önizlemede. Önizleme sürümü olarak, sonuçlar bazen bir kaynağın gerçekte bir değişiklik olmadığında değişeceğini gösterebilir. Bu sorunları azaltmak için çalışıyoruz ama yardımınıza ihtiyacımız var. Lütfen bu sorunları [https://aka.ms/whatifissues](https://aka.ms/whatifissues).

PowerShell komutları veya REST API işlemleri ile what-if işlemini kullanabilirsiniz.

## <a name="install-powershell-module"></a>PowerShell modüllerini yükleyin

PowerShell'de what-if'i kullanmak için PowerShell galerisinden Az.Resources modülünün önizleme sürümünü yükleyin.

### <a name="install-preview-version"></a>Önizleme sürümünü yükleme

Önizleme modüllerini yüklemek için şunları kullanın:

```powershell
Install-Module Az.Resources -RequiredVersion 1.12.1-preview -AllowPrerelease
```

### <a name="uninstall-alpha-version"></a>Alfa sürümünü kaldırma

Daha önce if-if modülünün alfa sürümünü yüklediyseniz, bu modülü kaldırın. Alfa sürümü yalnızca erken bir önizleme için kaydolan kullanıcılar tarafından kullanılabilir. Bu önizlemeyi yüklemediyseniz, bu bölümü atlayabilirsiniz.

1. PowerShell’i yönetici olarak çalıştırın
1. Az.Resources modülünün yüklü sürümlerini kontrol edin.

   ```powershell
   Get-InstalledModule -Name Az.Resources -AllVersions | select Name,Version
   ```

1. **2.x.x-alpha**biçiminde sürüm numarası olan yüklü bir sürümünüz varsa, bu sürümü kaldırın.

   ```powershell
   Uninstall-Module Az.Resources -RequiredVersion 2.0.1-alpha5 -AllowPrerelease
   ```

1. Önizlemeyi yüklemek için kullandığınız olasılık deposunun kaydını kaldırın.

   ```powershell
   Unregister-PSRepository -Name WhatIfRepository
   ```

Eğer olursa onu kullanmaya hazırsın.

## <a name="see-results"></a>Sonuçlara bakın

PowerShell'de çıktı, farklı değişiklik türlerini görmenize yardımcı olan renk kodlu sonuçlar içerir.

![Kaynak Yöneticisi şablon dağıtımı ne-if işlemi tam kaynak payload ve değişiklik türleri](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

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

* `$results = Get-AzResourceGroupDeploymentWhatIfResult`kaynak grubu dağıtımları için
* `$results = Get-AzSubscriptionDeploymentWhatIfResult`veya `$results = Get-AzDeploymentWhatIfResult` abonelik düzeyi dağıtımları için

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

Çıktının alt kısmında, Sahibi etiketinin silindiği gösterir. Adres öneki 10.0.0.0/16'dan 10.0.0.0/15'e değiştirildi. Subnet001 adlı alt ağ silindi. Bu değişikliklerin aslında dağıtılmadığını unutmayın. Şablonu dağıtsanız meydana gelecek değişikliklerin önizlemesini görürsünüz.

Silinmiş olarak listelenen özelliklerden bazıları gerçekte değişmez. Özellikler şablonda yokken yanlış olarak silinmiş olarak bildirilebilir, ancak dağıtım sırasında varsayılan değerler olarak otomatik olarak ayarlanır. Bu sonuç, if yanıtında "gürültü" olarak kabul edilir. Dağıtılan son kaynak, özellikler için ayarlanan değerlere sahip olur. What-if işlemi olgunlaştıkça, bu özellikler sonuçtan filtrelenir.

## <a name="programmatically-evaluate-what-if-results"></a>Programlı olarak ne-if sonuçlarını değerlendirin

Şimdi, komutu bir değişkene ayarlayarak "ne olur" sonuçlarını programlı olarak değerlendirelim.

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIfResult `
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
