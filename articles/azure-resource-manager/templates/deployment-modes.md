---
title: Dağıtım modları
description: Azure Kaynak Yöneticisi ile tam veya artımlı dağıtım modunun kullanılıp kullanılmayacağının nasıl belirtilen açıklanır.
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 1077d92f076797fb03c4fe750b353e2306f9b6de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460254"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Kaynak Yöneticisi dağıtım modları

Kaynaklarınızı dağıtırken, dağıtımın artımlı bir güncelleştirme veya tam bir güncelleştirme olduğunu belirtirsiniz. Bu iki mod arasındaki fark, Kaynak Yöneticisi'nin kaynak grubunda şablonda olmayan varolan kaynakları nasıl işlediğidir.

Her iki mod için de Kaynak Yöneticisi şablonda belirtilen tüm kaynakları oluşturmaya çalışır. Kaynak grubunda zaten varsa ve ayarları değişmemişse, bu kaynak için işlem yapılmaz. Bir kaynağın özellik değerlerini değiştirirseniz, kaynak bu yeni değerlerle güncelleştirilir. Varolan bir kaynağın konumunu veya türünü güncelleştirmeye çalışırsanız, dağıtım bir hatayla başarısız olur. Bunun yerine, gereksinim duyduğunuz konumu veya türü içeren yeni bir kaynak dağıtın.

Varsayılan mod artımlıdır.

## <a name="complete-mode"></a>Tam mod

Tam modda, Kaynak Yöneticisi kaynak grubunda bulunan ancak şablonda belirtilmeyen kaynakları **siler.**

Şablonunuz, [koşul](conditional-resource-deployment.md) yanlış olarak değerlendirildiği için dağıtılmamada bir kaynak içeriyorsa, sonuç şablonu dağıtmak için hangi REST API sürümünü kullandığınıza bağlıdır. Bir sürümü 2019-05-10'dan önce kullanıyorsanız, kaynak **silinmez.** 2019-05-10 veya sonrası ile kaynak **silinir.** Azure PowerShell ve Azure CLI'nin en son sürümleri kaynağı siler.

[Kopyalama döngüleri](copy-resources.md)ile tam modu kullanarak dikkatli olun. Kopyalama döngüsü çözüldükten sonra şablonda belirtilmeyen tüm kaynaklar silinir.

[Bir şablonda birden fazla kaynak grubuna](cross-resource-group-deployment.md)dağıtırsanız, dağıtım işleminde belirtilen kaynak grubundaki kaynaklar silinebilir. İkincil kaynak gruplarındaki kaynaklar silinmez.

Kaynak türlerinin tam mod silme işlemlerini nasıl işlediği konusunda bazı farklılıklar vardır. Üst kaynaklar, tam modda dağıtılan bir şablonda olmadığında otomatik olarak silinir. Şablonda olmadığında bazı alt kaynaklar otomatik olarak silinmez. Ancak, üst kaynak silinirse bu alt kaynaklar silinir.

Örneğin, kaynak grubunuz bir DNS bölgesi (Microsoft.Network/dnsZones kaynak türü) ve bir CNAME kaydı (Microsoft.Network/dnsZones/CNAME kaynak türü) içeriyorsa, DNS bölgesi CNAME kaydının ana kaynağıdır. Tam modla dağıtırsanız ve şablonunuzda DNS bölgesini eklemezseniz, DNS bölgesi ve CNAME kaydı silinir. Şablonunuza DNS bölgesini ekler, ancak CNAME kaydını içermezseniz, CNAME silinmez.

Kaynak türlerinin silme işlemini nasıl ele alabildiğini öğrenmek [için, tam mod dağıtımları için Azure kaynaklarının silinmesine](complete-mode-deletion.md)bakın.

Kaynak grubu [kilitliyse,](../management/lock-resources.md)tam mod kaynakları silmez.

> [!NOTE]
> Yalnızca kök düzeyinde şablonlar dağıtım ın tamamını destekler. [Bağlantılı veya iç içe olan şablonlar](linked-templates.md)için artımlı mod kullanmanız gerekir.
>
> [Abonelik düzeyi dağıtımları](deploy-to-subscription.md) tam modu desteklemez.
>
> Şu anda, portal tam modu desteklemiyor.
>

## <a name="incremental-mode"></a>Artımlı mod

Artımlı modda, Kaynak Yöneticisi kaynak grubunda bulunan ancak şablonda belirtilmeyen değişmemiş kaynaklar **bırakır.** Şablondaki kaynaklar kaynak grubuna **eklenir.**

> [!NOTE]
> Varolan bir kaynağı artımlı modda yeniden dağıtırken, tüm özellikler yeniden uygulanır. **Özellikler artımlı olarak eklenmez.** Ortak bir yanlış anlama, şablonda belirtilmeyen özelliklerin değişmeden bırakıldığını düşünmektir. Belirli özellikleri belirtmezseniz, Kaynak Yöneticisi dağıtımı bu değerlerin üzerine yazmak olarak yorumlar. Şablona dahil olmayan özellikler varsayılan değerlere sıfırlanır. Kaynak için varsayılan olmayan tüm değerleri belirtin, sadece güncellediğiniz değerleri değil. Şablondaki kaynak tanımı her zaman kaynağın son durumunu içerir. Varolan bir kaynağa kısmi bir güncelleştirmeyi temsil edemez.

## <a name="example-result"></a>Örnek sonuç

Artımlı ve tam modlar arasındaki farkı göstermek için aşağıdaki senaryoyu göz önünde bulundurun.

**Kaynak Grubu** şunları içerir:

* Kaynak A
* Kaynak B
* Kaynak C

**Şablon** şunları içerir:

* Kaynak A
* Kaynak B
* Kaynak D

**Artımlı** modda dağıtıldığında, kaynak grubu vardır:

* Kaynak A
* Kaynak B
* Kaynak C
* Kaynak D

**Tam** modda dağıtıldığında, Kaynak C silinir. Kaynak grubu:

* Kaynak A
* Kaynak B
* Kaynak D

## <a name="set-deployment-mode"></a>Dağıtım modunu ayarlama

PowerShell ile dağıtım yaparken dağıtım modunu `Mode` ayarlamak için parametreyi kullanın.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Azure CLI ile dağıtım yaparken dağıtım modunu `mode` ayarlamak için parametreyi kullanın.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Aşağıdaki örnekte, artımlı dağıtım moduna ayarlanmış bağlantılı bir şablon gösterilmektedir:

```json
"resources": [
  {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için [bkz.](template-syntax.md)
* Kaynakları dağıtma hakkında bilgi edinmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.
* Bir kaynak sağlayıcısının işlemlerini görüntülemek için [Azure REST API'sine](/rest/api/)bakın.
