---
title: Şablon dağıtımı-if (Önizleme)
description: Azure Resource Manager şablonu dağıtılmadan önce kaynaklarınızda hangi değişikliklerin gerçekleşecektir belirleme.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/04/2020
ms.author: tomfitz
ms.openlocfilehash: 62f46d158bea9507246fda7f24750c3743a5e1f1
ms.sourcegitcommit: c052c99fd0ddd1171a08077388d221482026cd58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84424253"
---
# <a name="arm-template-deployment-what-if-operation-preview"></a>ARM şablonu dağıtımı ne-if işlemi (Önizleme)

Bir Azure Resource Manager (ARM) şablonu dağıtılmadan önce, gerçekleşen değişikliklerin önizlemesini yapabilirsiniz. Azure Resource Manager, şablonu dağıtırsanız kaynakların ne şekilde değişdiklerinizi görmenizi sağlamak için ne yapılır işlemini sağlar. Bu işlem, mevcut kaynaklarda hiçbir değişiklik yapmaz. Bunun yerine, belirtilen şablon dağıtılırsa değişiklikleri tahmin eder.

> [!NOTE]
> Bu işlem şu anda önizleme aşamasındadır. Önizleme sürümü olarak, sonuçlar bazen hiçbir değişiklik gerçekleşmediği zaman bir kaynağın değiştirileceği gösterebilir. Bu sorunları azaltmak için çalışıyoruz, ancak yardımımız için ihtiyacımız var. Lütfen bu sorunları konusunda bildirin [https://aka.ms/whatifissues](https://aka.ms/whatifissues) .

Azure PowerShell, Azure CLı veya REST API işlemleri ile durum işlemini kullanabilirsiniz. Kaynak grubu ve abonelik düzeyinde dağıtımlar için desteklenir.

## <a name="install-azure-powershell-module"></a>Azure PowerShell modülünü Install

PowerShell 'de ne olduğunu kullanmak için **az Module sürüm 4,2 veya sonraki bir**sürümünü kullanmanız gerekir.

Ancak, gerekli modülü yüklemeden önce PowerShell Core (6. x veya 7. x) olduğundan emin olun. PowerShell 5. x veya daha önceki [bir sürümünü kullanıyorsanız, PowerShell sürümünüzü güncelleştirin](/powershell/scripting/install/installing-powershell). Gerekli modülü PowerShell 5. x veya önceki bir sürümüne yükleyemezsiniz.

### <a name="install-latest-version"></a>En son sürümü yükler

Modülünü yüklemek için şunu kullanın:

```powershell
Install-Module -Name Az -Force
```

Modül yükleme hakkında daha fazla bilgi için bkz. [yükleme Azure PowerShell](/powershell/azure/install-az-ps).

### <a name="uninstall-alpha-version"></a>Alfa sürümünü kaldır

Daha önce bir durum modülünün Alpha sürümünü yüklediyseniz, bu modülü kaldırın. Alfa sürümü yalnızca erken önizlemeye kaydolan kullanıcılar tarafından kullanılabilir. Bu önizlemeyi yüklemediyseniz, bu bölümü atlayabilirsiniz.

1. PowerShell’i yönetici olarak çalıştırın
1. Az. resources modülünün yüklü sürümlerini denetleyin.

   ```powershell
   Get-InstalledModule -Name Az.Resources -AllVersions | select Name,Version
   ```

1. Sürüm numarası **2. x. x-Alpha**biçiminde yüklü bir sürümünüz varsa, bu sürümü kaldırın.

   ```powershell
   Uninstall-Module Az.Resources -RequiredVersion 2.0.1-alpha5 -AllowPrerelease
   ```

1. Önizlemeyi yüklerken kullandığınız durum deposunun kaydını silin.

   ```powershell
   Unregister-PSRepository -Name WhatIfRepository
   ```

Ne yapılacağını kullanmaya hazırsınız.

## <a name="install-azure-cli-module"></a>Azure CLı modülünü yükler

Azure CLı 'de ne olduğunu kullanmak için Azure CLı 2.5.0 veya sonraki bir sürüme sahip olmanız gerekir. Gerekirse, [en son Azure CLI sürümünü yüklemelisiniz](/cli/azure/install-azure-cli).

## <a name="see-results"></a>Sonuçlara bakın

PowerShell veya Azure CLı 'de ne olursa kullanın, çıktı, farklı değişiklik türlerini görmenizi sağlayan renk kodlu sonuçlar içerir.

![Kaynak Yöneticisi şablonu dağıtımı ne yapılır işlemi fullresourcepayload ve değişiklik türleri](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

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

## <a name="what-if-commands"></a>Durum komutları

### <a name="azure-powershell"></a>Azure PowerShell

Bir şablonu dağıtımdan önce değişiklikleri önizlemek için [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) veya [New-azsubscriptiondeployment](/powershell/module/az.resources/new-azdeployment)komutunu kullanın. `-Whatif`Dağıtım komutuna anahtar parametresini ekleyin.

* `New-AzResourceGroupDeployment -Whatif`kaynak grubu dağıtımları için
* `New-AzSubscriptionDeployment -Whatif``New-AzDeployment -Whatif`abonelik düzeyinde dağıtımlar için

`-Confirm`Değişiklikleri önizlemek ve dağıtıma devam etmek isteyip istemediğiniz sorulduğunda anahtar parametresini kullanabilirsiniz.

* `New-AzResourceGroupDeployment -Confirm`kaynak grubu dağıtımları için
* `New-AzSubscriptionDeployment -Confirm``New-AzDeployment -Confirm`abonelik düzeyinde dağıtımlar için

Yukarıdaki komutlar el ile inceleyebilmeniz için bir metin Özeti döndürür. Program aracılığıyla değişiklikler için inceleyebileceğiniz bir nesne almak için [Get-Azresourcegroupdeploymentwhatıresult](/powershell/module/az.resources/get-azresourcegroupdeploymentwhatifresult) veya [Get-azsubscriptiondeploymentwhatıresult](/powershell/module/az.resources/get-azdeploymentwhatifresult)' ı kullanın.

* `$results = Get-AzResourceGroupDeploymentWhatIfResult`kaynak grubu dağıtımları için
* `$results = Get-AzSubscriptionDeploymentWhatIfResult``$results = Get-AzDeploymentWhatIfResult`abonelik düzeyi dağıtımlar için veya

### <a name="azure-cli"></a>Azure CLI

Bir şablonu dağıtımdan önce değişiklikleri önizlemek için [az Deployment Group ne-if](/cli/azure/deployment/group#az-deployment-group-what-if) veya [az Deployment Sub ne-if](/cli/azure/deployment/sub#az-deployment-sub-what-if)' ı kullanın.

* `az deployment group what-if`kaynak grubu dağıtımları için
* `az deployment sub what-if`abonelik düzeyinde dağıtımlar için

`--confirm-with-what-if` `-c` Değişiklikleri önizlemek ve dağıtıma devam etmek isteyip istemediğiniz sorulduğunda anahtarı (veya kısa biçimini) kullanabilirsiniz. Bu anahtarı [az Deployment Group Create](/cli/azure/deployment/group#az-deployment-group-create) veya [az Deployment Sub Create](/cli/azure/deployment/sub#az-deployment-sub-create)öğesine ekleyin.

* `az deployment group create --confirm-with-what-if`veya `-c` kaynak grubu dağıtımları için
* `az deployment sub create --confirm-with-what-if``-c`abonelik düzeyi dağıtımlar için veya

Yukarıdaki komutlar el ile inceleyebilmeniz için bir metin Özeti döndürür. Değişiklikler için programlı olarak inceleyebileceğiniz bir JSON nesnesi almak için şunu kullanın:

* `az deployment group what-if --no-pretty-print`kaynak grubu dağıtımları için
* `az deployment sub what-if --no-pretty-print`abonelik düzeyinde dağıtımlar için

### <a name="azure-rest-api"></a>Azure REST API

REST API için şunu kullanın:

* [Dağıtımlar-](/rest/api/resources/deployments/whatif) kaynak grubu dağıtımları için What If
* Dağıtımlar-abonelik düzeyindeki dağıtımlar için [abonelik kapsamında What If](/rest/api/resources/deployments/whatifatsubscriptionscope)

## <a name="change-types"></a>Değişiklik türleri

Ne yapılır işlemi altı farklı değişiklik türünü listeler:

- **Oluştur**: kaynak şu anda mevcut değil, ancak şablonda tanımlandı. Kaynak oluşturulacaktır.

- **Sil**: Bu değişiklik türü yalnızca dağıtım için [tamamlanmış mod](deployment-modes.md) kullanılırken geçerlidir. Kaynak var, ancak şablonda tanımlı değil. Bu, tüm modda kaynak silinir. Yalnızca [tamamlanmış mod silme işlemini destekleyen](complete-mode-deletion.md) kaynaklar bu değişiklik türüne dahil edilir.

- **Yoksay**: kaynak var, ancak şablonda tanımlı değil. Kaynak dağıtılmaz veya değiştirilmez.

- **NOCHANGE**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak, ancak kaynağın özellikleri değişmeyecek. Bu değişiklik türü, varsayılan değer olan [RESULTFORMAT](#result-format) ayarlandığında döndürülür `FullResourcePayloads` .

- **Değiştir**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak ve kaynağın özellikleri değişecektir. Bu değişiklik türü, varsayılan değer olan [RESULTFORMAT](#result-format) ayarlandığında döndürülür `FullResourcePayloads` .

- **Dağıtım**: kaynak vardır ve şablonda tanımlanmıştır. Kaynak yeniden dağıtılacak. Kaynağın özellikleri değişebilir veya değişmeyebilir. İşlem, herhangi bir özellik değişmeyeceği için yeterli bilgi olmadığında bu değişiklik türünü döndürür. Bu durumu yalnızca [RESULTFORMAT](#result-format) olarak ayarlandığında görürsünüz `ResourceIdOnly` .

## <a name="result-format"></a>Sonuç biçimi

Tahmin edilen değişiklikler hakkında döndürülen ayrıntı düzeyini kontrol edersiniz. İki seçeneğiniz vardır:

* **Fullresourceyükleri** -değiştirilecek kaynakların bir listesini ve değiştirilecek özelliklerle ilgili ayrıntıları döndürür
* **Resourceıdonly** -değiştirilecek kaynak listesini döndürür

Varsayılan değer **Fullresourceyükleri**' dir.

PowerShell dağıtım komutları için `-WhatIfResultFormat` parametresini kullanın. Programlı nesne komutlarında `ResultFormat` parametresini kullanın.

Azure CLı için `--result-format` parametresini kullanın.
 
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

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group create \
  --name ExampleGroup \
  --location "Central US"
az deployment group create \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

---

### <a name="test-modification"></a>Test değişikliği

Dağıtım tamamlandıktan sonra, durum işlemini test etmeye hazırsınız demektir. Bu sefer [, sanal ağı değiştiren bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json)dağıtırsınız. Özgün etiketlerden biri eksik, bir alt ağ kaldırıldı ve adres ön eki değişti.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group what-if \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

---

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

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIfResult `
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

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
results=$(az deployment group what-if --resource-group ExampleGroup --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json" --no-pretty-print)
```

---

## <a name="confirm-deletion"></a>Silme işlemini onaylama

[Dağıtım modunun](deployment-modes.md)kullanımını destekleyen durum işlemi. Tamamlanmış moda ayarlandığında şablonda olmayan kaynaklar silinir. Aşağıdaki örnek, tamamlanmış bir [kaynağı olmayan bir şablon dağıtmıştır](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) .

Bir şablonu dağıtımdan önce değişiklikleri önizlemek için dağıtım komutuyla Onayla anahtarını onaylama parametresini kullanın. Değişiklikler beklediğiniz gibi olduğunda, dağıtımın tamamlanmasını istediğinizi kabul edin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -Mode Complete `
  -Confirm `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --resource-group ExampleGroup \
  --mode Complete \
  --confirm-with-what-if \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json"
```

---

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

## <a name="sdks"></a>SDK’lar

Azure SDK 'Ları aracılığıyla ne yapılır işlemini kullanabilirsiniz.

* Python için, [ne yapılacağını](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.deploymentsoperations?view=azure-python#what-if-resource-group-name--deployment-name--properties--location-none--custom-headers-none--raw-false--polling-true----operation-config-)kullanın.

* Java için [Deploymentwhatif sınıfını](/java/api/com.microsoft.azure.management.resources.deploymentwhatif?view=azure-java-stable)kullanın.

* .NET için [Deploymentwhatif sınıfını](/dotnet/api/microsoft.azure.management.resourcemanager.models.deploymentwhatif?view=azure-dotnet)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Önizleme sürümünden yanlış sonuçlar olduğunu fark ederseniz, lütfen sorunları bildirin [https://aka.ms/whatifissues](https://aka.ms/whatifissues) .
- Şablonları Azure PowerShell dağıtmak için bkz. [ARM şablonlarıyla kaynak dağıtma ve Azure PowerShell](deploy-powershell.md).
- Şablonları Azure CLı ile dağıtmak için bkz. [ARM şablonları ve Azure CLI ile kaynak dağıtma](deploy-cli.md).
- Şablonları REST ile dağıtmak için bkz. [ARM şablonlarıyla kaynak dağıtma ve Kaynak Yöneticisi REST API](deploy-rest.md).
