---
title: Azure Ilkesi kullanılarak uyumluluk
description: Azure Container kayıt defterlerinden uyumluluğu denetlemek için Azure Ilkesinde yerleşik ilkeler atama
ms.topic: article
ms.date: 02/26/2020
ms.openlocfilehash: 331fcfaf72b1ad2022aa3edeefefa597e5bcfe17
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77925676"
---
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme

[Azure ilkesi](../governance/policy/overview.md) , Azure 'da ilke oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar.

Bu makalede Azure Container Registry için yerleşik ilkeler (Önizleme) tanıtılmıştır. Yeni ve mevcut kayıt defterlerinin uyumluluğunu denetlemek için bu ilkeleri kullanın.

Azure Ilkesi kullanımı için herhangi bir ücret alınmaz.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="built-in-policy-definitions"></a>Yerleşik ilke tanımları

Aşağıdaki yerleşik ilke tanımları Azure Container Registry özgüdür:

[!INCLUDE [azure-policy-samples-policies-container-registry](../../includes/azure-policy-samples-policies-container-registry.md)]

Ayrıca bkz. yerleşik ağ ilkesi tanımı: [[Önizleme] Container Registry sanal ağ hizmeti uç noktası kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4857be7-912a-4c75-87e6-e30292bcdf78).

## <a name="assign-policies"></a>İlke atama

* [Azure Portal](../governance/policy/assign-policy-portal.md), [azure CLI](../governance/policy/assign-policy-azurecli.md), [Kaynak Yöneticisi şablonu](../governance/policy/assign-policy-template.md)veya Azure ilkesi SDK 'larını kullanarak ilke atayın.
* Bir kaynak grubuna, aboneliğe veya bir [Azure yönetim grubuna](../governance/management-groups/overview.md)ilke atamasını kapsam. Kapsayıcı kayıt defteri ilke atamaları, kapsamdaki mevcut ve yeni kapsayıcı kayıt defterleri için geçerlidir.
* [İlke zorlamayı](../governance/policy/concepts/assignment-structure.md#enforcement-mode) dilediğiniz zaman etkinleştirin veya devre dışı bırakın.

> [!NOTE]
> Bir ilkeyi atadıktan veya güncelleştirdikten sonra, bu, atamanın tanımlanan kapsamdaki kaynaklara uygulanması biraz zaman alır. [İlke değerlendirme Tetikleyicileri](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers)hakkında bilgi için bkz.

## <a name="review-policy-compliance"></a>İlke uyumluluğunu gözden geçirme

İlke atamalarınız tarafından oluşturulan uyumluluk bilgilerine Azure portal, Azure komut satırı araçlarını veya Azure Policy SDK 'larını kullanarak erişin. Ayrıntılar için bkz. [Azure kaynaklarının uyumluluk verilerini edinme](../governance/policy/how-to/get-compliance-data.md).

Bir kaynak uyumsuz olduğunda birçok olası neden vardır. Nedenini öğrenmek veya sorumluyu bulmak için bkz. [uyumsuzluk belirleme](../governance/policy/how-to/determine-non-compliance.md).

### <a name="policy-compliance-in-the-portal"></a>Portalda ilke uyumluluğu:

1. **Tüm hizmetler**' i seçin ve **ilke**araması yapın.
1. **Uyumluluk**' i seçin.
1. Uyumluluk durumlarını sınırlamak veya Portal](./media/container-registry-azure-policy/azure-policy-compliance.png)Ilke uyumluluğunu ![ilkeleri aramak için filtreleri kullanın.
1. Toplu uyumluluk ayrıntılarını ve olaylarını gözden geçirmek için bir ilke seçin. İsterseniz, kaynak uyumluluğu için belirli bir kayıt defteri seçin.

### <a name="policy-compliance-in-the-azure-cli"></a>Azure CLı 'de ilke uyumluluğu

Uyumluluk verilerini almak için Azure CLı 'yi de kullanabilirsiniz. Örneğin, uygulanan Azure Container Registry ilkelerinin ilke kimliklerini almak için CLı 'daki [az Policy atama listesi](/cli/azure/policy/assignment#az-policy-assignment-list) komutunu kullanın:

```azurecli
az policy assignment list --query "[?contains(displayName,'Container Registries')].{name:displayName, ID:id}" --output table
```

Örnek çıktı:

```
Name                                                                                   ID
-------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------------------------------------------
[Preview]: Container Registries should not allow unrestricted network access           /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/b4faf132dc344b84ba68a441
[Preview]: Container Registries should be encrypted with a Customer-Managed Key (CMK)  /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/cce1ed4f38a147ad994ab60a
```

Ardından, belirli bir ilke KIMLIĞI altındaki tüm kaynaklar için JSON biçimli uyumluluk durumunu döndürmek üzere [az Policy State List](/cli/azure/policy/state#az-policy-assignment-list) öğesini çalıştırın:

```azurecli
az policy state list \
  --resource <policyID>
```

Veya, *myregistry*gibi belirli bir kayıt DEFTERI kaynağının JSON biçimli uyumluluk durumunu döndürmek için [az Policy State List](/cli/azure/policy/state#az-policy-assignment-list) ' i çalıştırın:

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Ilke [tanımları](../governance/policy/concepts/definition-structure.md) ve [etkileri](../governance/policy/concepts/effects.md) hakkında daha fazla bilgi edinin

* [Özel ilke tanımı](../governance/policy/tutorials/create-custom-policy-definition.md) oluşturma

* Azure 'da [idare özellikleri](../governance/index.yml) hakkında daha fazla bilgi edinin


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/