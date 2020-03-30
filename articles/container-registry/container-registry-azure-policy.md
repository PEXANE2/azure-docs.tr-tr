---
title: Azure İlke'yi kullanarak uyumluluk
description: Azure konteyner kayıt defterlerinizin uyumluluğunu denetlemek için Azure İlkesi'nde yerleşik ilkeler atama
ms.topic: article
ms.date: 02/26/2020
ms.openlocfilehash: 012cd013de1c60fddcfb28e4bca96d761ada41ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330745"
---
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>Azure İlkesi'ni kullanarak Azure kapsayıcı kayıtlarının uyumluluğunu denetleme

[Azure İlkesi,](../governance/policy/overview.md) Azure'da ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar.

Bu makalede, Azure Kapsayıcı Kayıt Defteri için yerleşik ilkeler (önizleme) tanıtıştır. Uyumluluk için yeni ve varolan kayıtları denetlemek için bu ilkeleri kullanın.

Azure İlkesi'ni kullanmak için herhangi bir ücret yoktur.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="built-in-policy-definitions"></a>Yerleşik ilke tanımları

Aşağıdaki yerleşik ilke tanımları Azure Kapsayıcı Kayıt Defteri'ne özgüdir:

[!INCLUDE [azure-policy-samples-policies-container-registry](../../includes/azure-policy-samples-policies-container-registry.md)]

Ayrıca yerleşik ağ ilkesi tanımına bakın: [[Önizleme] Konteyner Kayıt Defteri sanal ağ hizmeti bitiş noktası kullanmalıdır.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4857be7-912a-4c75-87e6-e30292bcdf78)

## <a name="assign-policies"></a>İlke atama

* [Azure portalı, Azure](../governance/policy/assign-policy-portal.md) [CLI,](../governance/policy/assign-policy-azurecli.md) [Kaynak Yöneticisi şablonu](../governance/policy/assign-policy-template.md)veya Azure İlkesi SDK'larını kullanarak ilkeler atayın.
* Bir kaynak grubuna, aboneye veya [Azure yönetim grubuna](../governance/management-groups/overview.md)ilke atamasını kapsamını kapsamını. Kapsayıcı kayıt ilkesi atamaları kapsamdaki mevcut ve yeni konteyner kayıtları için geçerlidir.
* [İlke zorlamayı](../governance/policy/concepts/assignment-structure.md#enforcement-mode) herhangi bir zamanda etkinleştirin veya devre dışı edin.

> [!NOTE]
> Bir ilke yi atadıktan veya güncelledikten sonra, atamanın tanımlanan kapsamdaki kaynaklara uygulanması biraz zaman alır. [İlke değerlendirme tetikleyicileri](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers)hakkındaki bilgilere bakın.

## <a name="review-policy-compliance"></a>İlke uyumluluğunu gözden geçirme

Azure portalı, Azure komut satırı araçları veya Azure İlkesi SDK'larını kullanarak ilke atamalarınız tarafından oluşturulan uyumluluk bilgilerine erişin. Ayrıntılar için bkz: [Azure kaynaklarının uyumluluk verilerini al.](../governance/policy/how-to/get-compliance-data.md)

Bir kaynak uyumlu olmadığında, birçok olası nedeni vardır. Değişikliği niçin belirlemek veya değişikliği sorumlu bulmak için [bkz.](../governance/policy/how-to/determine-non-compliance.md)

### <a name="policy-compliance-in-the-portal"></a>Portalda ilke uyumluluğu:

1. **Tüm hizmetleri**seçin ve **İlke'yi**arayın.
1. **Uyumluluk'u**seçin.
1. Uyumluluk durumlarını sınırlamak veya portalda ![](./media/container-registry-azure-policy/azure-policy-compliance.png)ilke uyumluluğunu aramak için filtreleri kullanın.
1. Toplu uyumluluk ayrıntılarını ve olaylarını gözden geçirmek için bir ilke seçin. İstenirse, kaynak uyumluluğu için belirli bir kayıt defteri seçin.

### <a name="policy-compliance-in-the-azure-cli"></a>Azure CLI'de ilke uyumluluğu

Uyumluluk verilerini almak için Azure CLI'yi de kullanabilirsiniz. Örneğin, Azure Kapsayıcı Kayıt Defteri ilkelerinin uygulanan ilke sözcülerini almak için CLI'deki [az ilke atama listesi](/cli/azure/policy/assignment#az-policy-assignment-list) komutunu kullanın:

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

Ardından, belirli bir ilke kimliği altındaki tüm kaynaklar için JSON biçimlendirilmiş uyumluluk durumunu döndürmek için [az ilke durumu listesini](/cli/azure/policy/state#az-policy-state-list) çalıştırın:

```azurecli
az policy state list \
  --resource <policyID>
```

Veya myregistry gibi belirli bir kayıt defteri kaynağının JSON biçimlendirilmiş uyumluluk durumunu döndürmek için [az ilke durum listesini](/cli/azure/policy/state#az-policy-state-list) *çalıştırın:*

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure İlkesi [tanımları](../governance/policy/concepts/definition-structure.md) ve [efektleri](../governance/policy/concepts/effects.md) hakkında daha fazla bilgi edinin

* Özel [ilke tanımı](../governance/policy/tutorials/create-custom-policy-definition.md) oluşturma

* Azure'da [yönetim özellikleri](../governance/index.yml) hakkında daha fazla bilgi edinin


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/