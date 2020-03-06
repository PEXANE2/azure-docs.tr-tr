---
title: Şablon dağıtımı-if (Önizleme)
description: Azure Resource Manager şablonu dağıtılmadan önce kaynaklarınızda hangi değişikliklerin gerçekleşecektir belirleme.
author: mumian
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: jgao
ms.openlocfilehash: b9d4150779842614a5dc284a2b3a489593fabfe1
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388542"
---
# <a name="resource-manager-template-deployment-what-if-operation-preview"></a>Kaynak Yöneticisi şablonu dağıtımı durum işlemi (Önizleme)

Bir şablonu dağıtımdan önce, gerçekleşecek değişiklikleri önizlemek isteyebilirsiniz. Azure Resource Manager, şablonu dağıtırsanız kaynakların ne şekilde değişdiklerinizi görmenizi sağlamak için ne yapılır işlemini sağlar. Bu işlem, mevcut kaynaklarda hiçbir değişiklik yapmaz. Bunun yerine, belirtilen şablon dağıtılırsa değişiklikleri tahmin eder.

> [!NOTE]
> Bu işlem şu anda önizleme aşamasındadır. Bunu kullanmak için, [Önizleme için kaydolmanız](https://aka.ms/armtemplatepreviews)gerekir. Önizleme sürümü olarak, sonuçlar bazen hiçbir değişiklik gerçekleşmediği zaman bir kaynağın değiştirileceği gösterebilir. Bu sorunları azaltmak için çalışıyoruz, ancak yardımımız için ihtiyacımız var. Lütfen bu sorunları [https://aka.ms/whatifissues](https://aka.ms/whatifissues)bildirin.

PowerShell komutlarıyla veya REST API işlemlerinde ne yapılır işlemini kullanabilirsiniz.

PowerShell 'de, çıktı, farklı değişiklik türlerini görmenizi sağlayan renk kodlu sonuçları içerir.

![Kaynak Yöneticisi şablonu dağıtımı ne yapılır işlemi fullresourcepayload ve değişiklik türleri](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

Ouptput metni:

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

## <a name="what-if-commands"></a>Durum komutları

Durum işlemi için Azure PowerShell veya Azure REST API kullanabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell

Bir şablonu dağıtımdan önce değişikliklerin önizlemesini görmek için dağıtım komutuna `-Whatif` Switch parametresini ekleyin.

* kaynak grubu dağıtımları için `New-AzResourceGroupDeployment -Whatif`
* abonelik düzeyindeki dağıtımlar için `New-AzSubscriptionDeployment -Whatif` ve `New-AzDeployment -Whatif`

Ya da değişiklikleri önizlemek ve dağıtıma devam etmek isteyip istemediğiniz sorulduğunda `-Confirm` Switch parametresini kullanabilirsiniz.

* kaynak grubu dağıtımları için `New-AzResourceGroupDeployment -Confirm`
* abonelik düzeyindeki dağıtımlar için `New-AzSubscriptionDeployment -Confirm` ve `New-AzDeployment -Confirm`

Yukarıdaki komutlar el ile inceleyebilmeniz için bir metin Özeti döndürür. Değişiklikler için programlı olarak inceleyebileceğiniz bir nesne almak için şunu kullanın:

* kaynak grubu dağıtımları için `$results = Get-AzResourceGroupDeploymentWhatIf`
* abonelik düzeyindeki dağıtımlar için `$results = Get-AzSubscriptionDeploymentWhatIf` veya `$results = Get-AzDeploymentWhatIf`

> [!NOTE]
> Sürüm 2.0.1-alpha5 sürümü öncesinde `New-AzDeploymentWhatIf` komutunu kullandınız. Bu komut, `Get-AzDeploymentWhatIf`, `Get-AzResourceGroupDeploymentWhatIf`ve `Get-AzSubscriptionDeploymentWhatIf` komutlarıyla değiştirilmiştir. Önceki bir sürümü kullandıysanız söz dizimini güncelleştirmeniz gerekir. `-ScopeType` parametresi kaldırıldı.

### <a name="azure-rest-api"></a>Azure REST API

REST API için şunu kullanın:

* [Dağıtımlar-](/rest/api/resources/deployments/whatif) kaynak grubu dağıtımları için What If
* Dağıtımlar-abonelik düzeyindeki dağıtımlar için [abonelik kapsamında What If](/rest/api/resources/deployments/whatifatsubscriptionscope)

## <a name="change-types"></a>Değişiklik türleri

Ne yapılır işlemi altı farklı değişiklik türünü listeler:

- **Oluştur**: kaynak şu anda mevcut değil, ancak şablonda tanımlandı. Kaynak oluşturulacaktır.

- **Sil**: Bu değişiklik türü yalnızca dağıtım için [tamamlanmış mod](deployment-modes.md) kullanılırken geçerlidir. Kaynak var, ancak şablonda tanımlı değil. Bu, tüm modda kaynak silinir. Yalnızca [tamamlanmış mod silme işlemini destekleyen](complete-mode-deletion.md) kaynaklar bu değişiklik türüne dahil edilir.

- **Yoksay**: kaynak var, ancak şablonda tanımlı değil. Kaynak dağıtılmaz veya değiştirilmez.

- **NOCHANGE**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak, ancak kaynağın özellikleri değişmeyecek. Bu değişiklik türü, [RESULTFORMAT](#result-format) `FullResourcePayloads`, varsayılan değer olan olarak ayarlandığında döndürülür.

- **Değiştir**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak ve kaynağın özellikleri değişecektir. Bu değişiklik türü, [RESULTFORMAT](#result-format) `FullResourcePayloads`, varsayılan değer olan olarak ayarlandığında döndürülür.

- **Dağıtım**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak. Kaynağın özellikleri değişebilir veya değişmeyebilir. İşlem, herhangi bir özellik değişmeyeceği için yeterli bilgi olmadığında bu değişiklik türünü döndürür. Bu durumu yalnızca [RESULTFORMAT](#result-format) `ResourceIdOnly`olarak ayarlandığında görürsünüz.

## <a name="result-format"></a>Sonuç biçimi

Tahmin edilen değişiklikler hakkında döndürülen ayrıntı düzeyini kontrol edebilirsiniz. Dağıtım komutları 'nda (`New-Az*Deployment`) **-WhatIfResultFormat** parametresini kullanın. Programlı nesne komutları 'nda (`Get-Az*DeploymentWhatIf`) **RESULTFORMAT** parametresini kullanın.

Değiştirilecek kaynakların bir listesini ve değiştirilecek özelliklerle ilgili ayrıntıları almak için, biçim parametresini **Fullresourceyükleri** olarak ayarlayın. Format parametresini yalnızca, değiştirilecek kaynakların listesini almak için **Resourceıdonly** olarak ayarlayın. Varsayılan değer **Fullresourceyükleri**' dir.  

Aşağıdaki sonuçlar iki farklı çıkış biçimini göstermektedir:

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

- Yalnızca kaynak KIMLIĞI

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>Durum işlemini çalıştır

### <a name="set-up-environment"></a>Ortamı ayarlama

Nasıl çalıştığını görmek için bazı testler çalıştıralım. İlk olarak, [sanal ağ oluşturan bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json)dağıtın. Bu sanal ağı, değişikliklerin ne yapılır-if tarafından raporlanacağı test etmek için kullanacaksınız.

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

### <a name="test-modification"></a>Test değişikliği

Dağıtım tamamlandıktan sonra, durum işlemini test etmeye hazırsınız demektir. Bu süre [, sanal ağı değiştiren bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json)dağıtır. Özgün etiketlerden biri eksik, bir alt ağ kaldırıldı ve adres ön eki değişti.

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

Şuna benzer bir durum ortaya çıkar:

![Kaynak Yöneticisi şablonu dağıtımı ne yapılır işlemi çıkışı](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

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

Çıktının en üstünde, değişikliklerin türünü belirtmek için renklerin tanımlandığını görürsünüz.

Çıktının en altında, etiketin sahibinin silindiğini gösterir. Adres ön eki 10.0.0.0/16 olarak 10.0.0.0/15 olarak değiştirildi. Subnet001 adlı alt ağ silindi. Bu değişikliklerin gerçekten dağıtılmadığını unutmayın. Şablonu dağıtırsanız gerçekleştirilecek değişikliklerin önizlemesini görürsünüz.

Silinmiş olarak listelenen özelliklerden bazıları aslında değişmeyecektir. Özellikler, şablonda olmadıkları sırada silinmiş olarak yanlış bildirilebilir, ancak dağıtım sırasında varsayılan değer olarak otomatik olarak ayarlanır. Bu sonuç, durum yanıtı içinde "gürültü" olarak değerlendirilir. Son dağıtılan kaynak, özellikler için ayarlanmış değerlere sahip olacaktır. Ne gibi işlemler söz konusu olduğunda, bu özellikler sonuçtan filtrelenmez.

## <a name="programmatically-evaluate-what-if-results"></a>Programlı sonuçları değerlendir

Şimdi, komutu bir değişkene ayarlayarak, bu sonuçları program aracılığıyla değerlendirelim.

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIf `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

Her bir değişikliğin özetini görebilirsiniz.

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

## <a name="confirm-deletion"></a>Silme işlemini onaylama

[Dağıtım modunun](deployment-modes.md)kullanımını destekleyen durum işlemi. Tamamlanmış moda ayarlandığında şablonda olmayan kaynaklar silinir. Aşağıdaki örnek, tamamlanmış bir [kaynağı olmayan bir şablon dağıtmıştır](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) .

Bir şablonu dağıtımdan önce değişiklikleri önizlemek için dağıtım komutuyla `-Confirm` Switch parametresini kullanın. Değişiklikler beklediğiniz gibi olduğunda, dağıtımın tamamlanmasını istediğinizi onaylayın.

```azurepowershell
New-AzResourceGroupDeployment `
  -Confirm `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

Şablonda kaynak tanımlanmadığı ve dağıtım modu Tamam olarak ayarlandığından, sanal ağ silinir.

![Kaynak Yöneticisi şablonu dağıtımı ne yapılır işlemi çıkışı dağıtım modu tamamlanmıştır](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

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

Beklenen değişiklikleri görürsünüz ve dağıtımın çalıştırılmasını istediğinizi doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Önizleme sürümünden yanlış sonuçlar olduğunu fark ederseniz, lütfen [https://aka.ms/whatifissues](https://aka.ms/whatifissues)sorunları bildirin.
- Şablonları Azure PowerShell dağıtmak için bkz. [Kaynak Yöneticisi şablonları ve Azure PowerShell ile kaynak dağıtma](deploy-powershell.md).
- Şablonları REST ile dağıtmak için bkz. [Kaynak Yöneticisi şablonlarıyla kaynak dağıtma ve REST API Kaynak Yöneticisi](deploy-rest.md).
