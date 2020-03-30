---
title: Kaynak grubunu ve kaynakları silme
description: Kaynak gruplarının ve kaynaklarının nasıl silinir olduğunu açıklar. Azure Kaynak Yöneticisi'nin kaynak grubunu silerken kaynakların silinmesini nasıl sipariş ettiğini açıklar. Yanıt kodlarını ve Kaynak Yöneticisi'nin silme işleminin başarılı olup olmadığını belirlemek için bunları nasıl işleyeceğini açıklar.
ms.topic: conceptual
ms.date: 09/03/2019
ms.custom: seodec18
ms.openlocfilehash: db56cf0897cd90f1e6e51199032d0d9712530f1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274027"
---
# <a name="azure-resource-manager-resource-group-and-resource-deletion"></a>Azure Kaynak Yöneticisi kaynak grubu ve kaynak silme

Bu makalede, kaynak grupları ve kaynakları nasıl silinir gösterir. Bir kaynak grubunu sildiğinizde Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl sipariş ettiği açıklanır.

## <a name="how-order-of-deletion-is-determined"></a>Silme sırası nasıl belirlenir?

Bir kaynak grubunu sildiğinizde, Kaynak Yöneticisi kaynakları silme sırasını belirler. Aşağıdaki sırayı kullanır:

1. Tüm alt (iç içe) kaynaklar silinir.

2. Diğer kaynakları yöneten kaynaklar daha sonra silinir. Bir kaynak, `managedBy` farklı bir kaynağın onu yönettiğini belirtmek için özellik ayarlayabilir. Bu özellik ayarlandığında, diğer kaynağı yöneten kaynak diğer kaynaklardan önce silinir.

3. Kalan kaynaklar önceki iki kategoriden sonra silinir.

Sipariş belirlendikten sonra, Kaynak Yöneticisi her kaynak için bir DELETE işlemi verir. Devam etmeden önce herhangi bir bağımlılıkların tamamlanmasını bekler.

Senkron işlemler için beklenen başarılı yanıt kodları şunlardır:

* 200
* 204
* 404

Eşkron operasyonlar için beklenen başarılı yanıt 202'dir. Kaynak Yöneticisi, eşzamanlı silme işleminin durumunu belirlemek için konum üstbilgisini veya azure-async işlem üstbilgisini izler.
  
### <a name="deletion-errors"></a>Silme işlemi hataları

Bir silme işlemi bir hata döndürdüğünde, Kaynak Yöneticisi DELETE çağrısını yeniden dener. Yeniden denemeler 5xx, 429 ve 408 durum kodları için olur. Varsayılan olarak, yeniden deneme süresi 15 dakikadır.

## <a name="after-deletion"></a>Silme işleminden sonra

Kaynak Yöneticisi, silmeye çalıştığı her kaynak için GET çağrısı verir. Bu GET çağrısının cevabının 404 olması bekleniyor. Kaynak Yöneticisi 404 aldığında, silmeişleminin başarıyla tamamlandığını düşünür. Kaynak Yöneticisi kaynağı önbelleğinden kaldırır.

Ancak, kaynaktaki GET çağrısı 200 veya 201 döndürürse, Kaynak Yöneticisi kaynağı yeniden oluşturur.

GET işlemi bir hata döndürürse, Kaynak Yöneticisi GET'i aşağıdaki hata kodu için yeniden dener:

* 100'den az
* 408
* 429
* 500'den büyük

Diğer hata kodları için Kaynak Yöneticisi kaynağın silinmesini başarısız olur.

## <a name="delete-resource-group"></a>Kaynak grubunu silme

Kaynak grubunu silmek için aşağıdaki yöntemlerden birini kullanın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResourceGroup -Name ExampleResourceGroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group delete --name ExampleResourceGroup
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Portalda,](https://portal.azure.com)silmek istediğiniz kaynak grubunu seçin.

1. **Kaynak grubunu sil**'i seçin.

   ![Kaynak grubunu silme](./media/delete-resource-group/delete-group.png)

1. Silme işlemini onaylamak için kaynak grubunun adını yazın

---

## <a name="delete-resource"></a>Kaynağı silme

Kaynağı silmek için aşağıdaki yöntemlerden birini kullanın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResource `
  -ResourceGroupName ExampleResourceGroup `
  -ResourceName ExampleVM `
  -ResourceType Microsoft.Compute/virtualMachines
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az resource delete \
  --resource-group ExampleResourceGroup \
  --name ExampleVM \
  --resource-type "Microsoft.Compute/virtualMachines"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Portalda,](https://portal.azure.com)silmek istediğiniz kaynağı seçin.

1. **Sil**’i seçin. Aşağıdaki ekran görüntüsü, sanal bir makinenin yönetim seçeneklerini gösterir.

   ![Kaynağı silme](./media/delete-resource-group/delete-resource.png)

1. Sorulduğunda silme işlemini onaylayın.

---


## <a name="next-steps"></a>Sonraki adımlar

* Kaynak Yöneticisi kavramlarını anlamak için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
* Silme komutları için [PowerShell](/powershell/module/az.resources/Remove-AzResourceGroup), [Azure CLI](/cli/azure/group?view=azure-cli-latest#az-group-delete)ve [REST API'ye](/rest/api/resources/resourcegroups/delete)bakın.
