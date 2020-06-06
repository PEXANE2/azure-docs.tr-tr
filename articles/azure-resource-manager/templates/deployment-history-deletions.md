---
title: Dağıtım geçmişi silme işlemleri
description: Azure Resource Manager dağıtım geçmişinden dağıtımları otomatik olarak silme işlemini açıklar. Geçmiş 800 sınırını aşmaya yakın olduğunda dağıtımlar silinir.
ms.topic: conceptual
ms.date: 06/05/2020
ms.openlocfilehash: 2d87cc1dcc0d3a41cb32bf7750ccdd832019f8bf
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84462745"
---
# <a name="automatic-deletions-from-deployment-history"></a>Dağıtım geçmişinden otomatik silme işlemleri

Bir şablonu dağıttığınız her seferinde dağıtım hakkındaki bilgiler dağıtım geçmişine yazılır. Her kaynak grubu, dağıtım geçmişinde 800 dağıtım ile sınırlıdır.

2020 Haziran 'dan itibaren, sınıra yaklaşmanıza kadar Azure Resource Manager otomatik olarak geçmişinizden dağıtımları siler. Otomatik silme, geçmiş davranıştaki bir değişiklikten fazla. Daha önce, bir hatadan kaçınmak için dağıtımları dağıtım geçmişinden el ile silmeniz gerekiyordu.

> [!NOTE]
> Bir dağıtımı geçmişten silmek, dağıtılan kaynakların hiçbirini etkilemez.

## <a name="when-deployments-are-deleted"></a>Dağıtımlar silindiğinde

Dağıtımlar yalnızca 800 sınırına yaklaşdığınızda dağıtım geçmişinizden silinir. Azure Resource Manager gelecekteki dağıtımlar için yer açmak üzere en eski dağıtımlar kümesini siler. Geçmişinizin çoğu değişmeden kalır. En eski dağıtımlar önce her zaman silinir.

:::image type="content" border="false" source="./media/deployment-history-deletions/deployment-history.svg" alt-text="Dağıtım geçmişinden silme işlemleri":::

Dağıtıma ek olarak, [durum işlemini](template-deploy-what-if.md) çalıştırdığınızda veya bir dağıtımı doğrulamak için de silmeleri tetiklersiniz.

Bir dağıtıma geçmişle aynı adı verdiğinizde, geçmişteki yerini sıfırladınız. Dağıtım, geçmişteki en son yere gider. Ayrıca bir hatadan sonra [Bu dağıtıma geri](rollback-on-error.md) döndüğünüzde bir dağıtımın yerini sıfırlayabilirsiniz.

> [!NOTE]
> Kaynak grubunuz zaten 800 sınırında ise, bir sonraki dağıtımınız hata vererek başarısız olur. Otomatik silme işlemi hemen başlatılır. Kısa bir bekleme sonrasında dağıtımınızı yeniden deneyebilirsiniz.

## <a name="opt-out-of-automatic-deletions"></a>Otomatik silme işlemleri devre dışı

Geçmişten otomatik silme işlemleri yapabilirsiniz. **Bu seçeneği yalnızca dağıtım geçmişini kendiniz yönetmek istediğinizde kullanın.** Geçmişteki 800 dağıtım limiti yine de zorlanır. 800 dağıtımlarını aşarsanız bir hata alırsınız ve dağıtımınız başarısız olur.

Otomatik silme işlemlerini devre dışı bırakmak için `Microsoft.Resources/DisableDeploymentGrooming` özellik bayrağını kaydedin. Özellik bayrağını kaydettiğinizde, tüm Azure aboneliği için otomatik silme işlemleri devre dışı. Yalnızca belirli bir kaynak grubunu devre dışı kaydedemezsiniz.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell için [register-AzProviderFeature](/powershell/module/az.resources/Register-AzProviderFeature)komutunu kullanın.

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

Aboneliğinizin geçerli durumunu görmek için şunu kullanın:

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı için [az Feature Register](/cli/azure/feature#az-feature-register)kullanın.

```azurecli-interactive
az feature register --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

Aboneliğinizin geçerli durumunu görmek için şunu kullanın:

```azurecli-interactive
az feature show --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

# <a name="rest"></a>[REST](#tab/rest)

REST API için, [Özellikler-kaydet](/rest/api/resources/features/register)' i kullanın.

```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

Aboneliğinizin geçerli durumunu görmek için şunu kullanın:

```rest
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

---

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım geçmişini görüntüleme hakkında bilgi edinmek için bkz. [Azure Resource Manager dağıtım geçmişini görüntüleme](deployment-history.md).
