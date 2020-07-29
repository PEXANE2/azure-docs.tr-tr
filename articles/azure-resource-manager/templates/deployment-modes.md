---
title: Dağıtım modları
description: Azure Resource Manager ile tamamlanmış veya artımlı dağıtım modunun kullanılıp kullanılmayacağını nasıl belirleyeceğiniz açıklanır.
ms.topic: conceptual
ms.date: 07/22/2020
ms.openlocfilehash: e584acd4af1dc6adb5f5d383acd5d16da0815f32
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371592"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager Dağıtım modları

Kaynaklarınızı dağıttığınızda, dağıtımın bir artımlı güncelleştirme ya da tamamen güncelleştirme olduğunu belirtirsiniz. Bu iki mod arasındaki fark, Kaynak Yöneticisi şablonda olmayan kaynak grubunda var olan kaynakları nasıl işleyeceğinden yapılır.

Her iki mod için Kaynak Yöneticisi şablonda belirtilen tüm kaynakları oluşturmaya çalışır. Kaynak, kaynak grubunda zaten varsa ve ayarları değişmezse, bu kaynak için hiçbir işlem yapılmaz. Bir kaynağın özellik değerlerini değiştirirseniz, kaynak bu yeni değerlerle güncelleştirilir. Mevcut bir kaynağın konumunu veya türünü güncelleştirmeye çalışırsanız, dağıtım bir hata ile başarısız olur. Bunun yerine, gereken konuma veya türe sahip yeni bir kaynak dağıtın.

Varsayılan mod artımlı ' dır.

## <a name="complete-mode"></a>Mod Tamam

Tüm modda Kaynak Yöneticisi kaynak grubunda var olan ancak şablonda belirtilmeyen kaynakları **siler** .

> [!NOTE]
> Bir şablonu tam modda dağıtmadan önce her zaman [ne yapılır işlemini](template-deploy-what-if.md) kullanın. Ne yapılır, hangi kaynakların oluşturulacağını, silineceğini veya değiştirildiğini gösterir. Kaynakları istenmeden silmeyi önlemek için ne yapılacağını kullanın.

Şablonunuz, [koşul](conditional-resource-deployment.md) false olarak değerlendirildiği için dağıtılan bir kaynağı içeriyorsa, sonuç, şablonu dağıtmak için kullandığınız REST API sürümüne bağlıdır. 2019-05-10 'den önceki bir sürümü kullanıyorsanız, kaynak **silinmez**. 2019-05-10 veya sonraki bir sürümü kullanarak kaynak **silinir**. Azure PowerShell ve Azure CLı 'nın en son sürümleri, kaynağı siler.

[Kopyalama döngülerine](copy-resources.md)sahip tüm modu kullanırken dikkatli olun. Kopyalama döngüsü çözümlendikten sonra şablonda belirtilmeyen kaynaklar silinir.

[Bir şablonda birden fazla kaynak grubuna](cross-scope-deployment.md)dağıtırsanız, dağıtım işleminde belirtilen kaynak grubundaki kaynaklar silinebilir. İkincil kaynak gruplarındaki kaynaklar silinmez.

Kaynak türlerinin tamamlanma modu silme işlemlerinin nasıl ele aldığı bazı farklılıklar vardır. Üst kaynaklar, tamamlanmış modda dağıtılan bir şablonda olmadığında otomatik olarak silinir. Bazı alt kaynaklar şablonda olmadığında otomatik olarak silinmez. Ancak, üst kaynak silinirse bu alt kaynaklar silinir.

Örneğin, kaynak grubunuz bir DNS bölgesi (Microsoft. Network/dnsZones kaynak türü) ve CNAME kaydı (Microsoft. Network/dnsZones/CNAME kaynak türü) içeriyorsa DNS bölgesi CNAME kaydı için üst kaynaktır. ' İ tüm moduyla dağıtıp DNS bölgesini şablonunuza eklemezseniz, DNS bölgesinin ve CNAME kaydının ikisi de silinir. Şablonunuzda DNS bölgesini dahil ederseniz ancak CNAME kaydını eklemezseniz CNAME silinmez.

Kaynak türlerinin silinme işleminin nasıl yapıldığını gösteren bir liste için, bkz. [tüm mod dağıtımları Için Azure kaynaklarını silme](complete-mode-deletion.md).

Kaynak grubu [kilitliyse](../management/lock-resources.md), tamamlanmış mod kaynakları silmez.

> [!NOTE]
> Yalnızca kök düzeyindeki şablonlar, tam Dağıtım modunu destekler. [Bağlantılı veya iç içe şablonlar](linked-templates.md)için artımlı Mod kullanmanız gerekir.
>
> [Abonelik düzeyindeki dağıtımlar](deploy-to-subscription.md) , tamamlanma modunu desteklemez.
>
> Şu anda portal, tamamlanmış modunu desteklemez.
>

## <a name="incremental-mode"></a>Artımlı mod

Artımlı modda Kaynak Yöneticisi, kaynak grubunda var olan ancak şablonda belirtilmeyen, **değiştirilmemiş kaynakları bırakır** . Şablondaki kaynaklar kaynak grubuna **eklenir** .

> [!NOTE]
> Var olan bir kaynağı artımlı modda yeniden dağıttığınızda tüm özellikler yeniden uygulanır. **Özellikler artımlı olarak eklenmez**. Yaygın bir yanlış anlama, şablonda belirtilmeyen özellikleri düşünmek için kullanılır. Belirli özellikleri belirtmezseniz, Kaynak Yöneticisi bu değerlerin üzerine yazarak dağıtımı yorumlar. Şablonda bulunmayan özellikler varsayılan değerlere sıfırlanır. Yalnızca güncelleştirdiklerinizle değil, kaynağın varsayılan olmayan tüm değerlerini belirtin. Şablondaki kaynak tanımı her zaman kaynağın son durumunu içerir. Mevcut bir kaynağa yönelik kısmi bir güncelleştirmeyi temsil etmez.
>
> Nadir durumlarda, bir kaynak için belirttiğiniz Özellikler aslında bir alt kaynak olarak uygulanır. Örneğin, bir Web uygulaması için site yapılandırma değerlerini sağladığınızda, bu değerler alt kaynak türünde uygulanır `Microsoft.Web/sites/config` . Web uygulamasını yeniden dağıtın ve site yapılandırma değerleri için boş bir nesne belirtirseniz, alt kaynak güncellenmez. Ancak, yeni site yapılandırma değerleri sağlarsanız, alt kaynak türü güncelleştirilir.

## <a name="example-result"></a>Örnek sonuç

Artımlı ve tamamlanmış modlar arasındaki farkı göstermek için aşağıdaki senaryoyu göz önünde bulundurun.

**Kaynak grubu** şunları içerir:

* A kaynağı
* Kaynak B
* Kaynak C

**Şablon** şunları içerir:

* A kaynağı
* Kaynak B
* Kaynak D

**Artımlı** modda dağıtıldığında, kaynak grubu:

* A kaynağı
* Kaynak B
* Kaynak C
* Kaynak D

**Tüm** modda dağıtıldığında, kaynak C silinir. Kaynak grubu:

* A kaynağı
* Kaynak B
* Kaynak D

## <a name="set-deployment-mode"></a>Dağıtım modunu ayarla

PowerShell ile dağıtma sırasında dağıtım modunu ayarlamak için `Mode` parametresini kullanın.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Azure CLı ile dağıtma sırasında dağıtım modunu ayarlamak için `mode` parametresini kullanın.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Aşağıdaki örnek, artımlı dağıtım moduna ayarlanmış bir bağlı şablonu gösterir:

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

* Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için bkz. [yazma Azure Resource Manager şablonları](template-syntax.md).
* Kaynakları dağıtma hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](deploy-powershell.md).
* Bir kaynak sağlayıcısına yönelik işlemleri görüntülemek için bkz. [Azure REST API](/rest/api/).
