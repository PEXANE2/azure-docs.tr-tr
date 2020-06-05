---
title: Azure Cosmos DB kaynaklarına yönelik idare ve denetimleri uygulamak için Azure Ilkesini kullanma
description: Azure Cosmos DB kaynaklarına yönelik idare ve denetimleri uygulamak için Azure Ilkesi 'ni nasıl kullanacağınızı öğrenin.
author: plzm
ms.author: paelaz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: a1b1c01f7cf720690decd9c7aac5fb14b92121ec
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84431998"
---
# <a name="use-azure-policy-to-implement-governance-and-controls-for-azure-cosmos-db-resources"></a>Azure Cosmos DB kaynaklarına yönelik idare ve denetimleri uygulamak için Azure Ilkesini kullanma

[Azure ilkesi](../governance/policy/overview.md) , kurumsal idare standartlarına zorlamaya, kaynak uyumluluğunu değerlendirmenize ve otomatik düzeltme uygulamanıza yardımcı olur. Yaygın kullanım örnekleri arasında güvenlik, maliyet yönetimi ve yapılandırma tutarlılığı vardır.

Azure Ilkesi yerleşik ilke tanımları sağlar. Yerleşik ilke tanımları tarafından açıklanmayan senaryolar için özel ilke tanımları oluşturabilirsiniz. Daha fazla bilgi için bkz. [Azure ilkesi belgeleri](../governance/policy/overview.md) .

## <a name="assign-a-built-in-policy-definition"></a>Yerleşik bir ilke tanımı atama

İlke tanımları, kaynak uyumluluk koşullarını ve bir koşul karşılanırsa gerçekleştirilecek etkiyi tanımlar. İlke _atamaları_ ilke _tanımlarından_oluşturulur. Azure Cosmos DB kaynaklarınız için yerleşik veya özel ilke tanımlarını kullanabilirsiniz. İlke atamaları, bir Azure yönetim grubu, bir Azure aboneliği veya bir kaynak grubu kapsamlandırılır ve seçilen kapsam içindeki kaynaklara uygulanır. İsteğe bağlı olarak, belirli kaynakları kapsamdan dışlayabilirsiniz.

[Azure Portal](../governance/policy/assign-policy-portal.md), [Azure POWERSHELL](../governance/policy/assign-policy-powershell.md), [Azure CLI](../governance/policy/assign-policy-azurecli.md)veya [ARM şablonuyla](../governance/policy/assign-policy-template.md)ilke atamaları oluşturabilirsiniz.

Azure Cosmos DB için yerleşik ilke tanımından bir ilke ataması oluşturmak için, [Azure Portal makalesinde ilke ataması oluşturma](../governance/policy/assign-policy-portal.md) bölümündeki adımları kullanın.

İlke tanımı seçme adımında, `Cosmos DB` kullanılabilir yerleşik ilke tanımlarının listesini filtrelemek Için arama alanına girin. Kullanılabilir yerleşik ilke tanımlarından birini seçin ve ardından ilke atamasını oluşturmaya devam etmek için **Seç** ' i seçin.

> [!TIP]
> İlke atamaları oluşturmak için Azure PowerShell, Azure CLı veya ARM şablonlarıyla **kullanılabilir tanımlar** bölmesinde gösterilen yerleşik ilke tanımı adlarını da kullanabilirsiniz.

:::image type="content" source="./media/policy/available-definitions.png" alt-text="Azure Cosmos DB yerleşik ilke tanımlarını arayın":::

## <a name="create-a-custom-policy-definition"></a>Özel ilke tanımı oluşturma

Yerleşik ilkeler tarafından açıklanmayan belirli senaryolar için [özel bir ilke tanımı](../governance/policy/tutorials/create-custom-policy-definition.md)oluşturabilirsiniz. Daha sonra özel ilke _tanımınızdan_bir ilke _ataması_ oluşturursunuz.

### <a name="property-types-and-property-aliases-in-policy-rules"></a>İlke kurallarında Özellik türleri ve özellik diğer adları

İlke kuralları oluşturmak için gereken kaynak özelliklerini ve özellik diğer adlarını belirlemek için [özel ilke tanımı adımlarını](../governance/policy/tutorials/create-custom-policy-definition.md) kullanın.

Belirli özellik diğer adlarını Azure Cosmos DB belirlemek için, `Microsoft.DocumentDB` özel ilke tanımı adımları makalesinde gösterilen yöntemlerden biriyle ad alanını kullanın.

#### <a name="use-the-azure-cli"></a>Azure CLı 'yı kullanın:
```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for namespace Microsoft.DocumentDB
az provider show --namespace Microsoft.DocumentDB --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

#### <a name="use-azure-powershell"></a>Azure PowerShell kullanın:
```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.DocumentDB namespace
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.DocumentDB').Aliases
```

Bu komutlar Azure Cosmos DB özelliği için özellik diğer adları listesini çıktı. Çıktının bir alıntısı aşağıda verilmiştir:

```json
[
  "Microsoft.DocumentDB/databaseAccounts/sku.name",
  "Microsoft.DocumentDB/databaseAccounts/virtualNetworkRules[*]",
  "Microsoft.DocumentDB/databaseAccounts/virtualNetworkRules[*].id",
  "Microsoft.DocumentDB/databaseAccounts/isVirtualNetworkFilterEnabled",
  "Microsoft.DocumentDB/databaseAccounts/consistencyPolicy.defaultConsistencyLevel",
  "Microsoft.DocumentDB/databaseAccounts/enableAutomaticFailover",
  "Microsoft.DocumentDB/databaseAccounts/Locations",
  "Microsoft.DocumentDB/databaseAccounts/Locations[*]",
  "Microsoft.DocumentDB/databaseAccounts/Locations[*].locationName",
  "..."
]
```

[Özel ilke tanımı kurallarında](../governance/policy/tutorials/create-custom-policy-definition.md#policy-rule)bu özellik diğer ad adlarından herhangi birini kullanabilirsiniz.

Aşağıda, birden fazla yazma konumu için Azure Cosmos DB hesabının yapılandırılıp yapılandırılmadığını denetleyen örnek bir ilke tanımı verilmiştir. Özel ilke tanımı iki kural içerir: bir tane, özellik diğer adının belirli bir türünü denetlemek için, ikincisi ise türün belirli bir özelliği için, bu durumda birden çok yazma konumu ayarını depolayan alan. Her iki kural de diğer adları kullanır.

```json
"policyRule": {
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.DocumentDB/databaseAccounts"
      },
      {
        "field": "Microsoft.DocumentDB/databaseAccounts/enableMultipleWriteLocations",
        "notEquals": true
      }
    ]
  },
  "then": {
    "effect": "Audit"
  }
}
```

Özel ilke tanımları, yerleşik ilke tanımlarının kullanıldığı şekilde, ilke atamaları oluşturmak için kullanılabilir.

## <a name="policy-compliance"></a>İlke uyumluluğu

İlke atamaları oluşturulduktan sonra, Azure Ilkesi atamanın kapsamındaki kaynakları değerlendirir. Her kaynağın ilkeyle olan _uyumluluğu_ değerlendirilir. İlkede belirtilen _efekt_ daha sonra uyumlu olmayan kaynaklara uygulanır.

Uyumluluk sonuçlarını ve düzeltme ayrıntılarını [Azure Portal](../governance/policy/how-to/get-compliance-data.md#portal) veya [Azure CLI](../governance/policy/how-to/get-compliance-data.md#command-line) veya [Azure izleyici günlükleri](../governance/policy/how-to/get-compliance-data.md#azure-monitor-logs)aracılığıyla gözden geçirebilirsiniz.

Aşağıdaki ekran görüntüsünde iki örnek ilke ataması gösterilmektedir.

Bir atama, Azure Cosmos DB kaynaklarının yalnızca izin verilen Azure bölgelerine dağıtıldığını denetleyen yerleşik bir ilke tanımına dayalıdır. Kaynak uyumluluğu, kapsam içi kaynaklar için ilke değerlendirme sonucunu (uyumlu veya uyumlu değil) gösterir.

Diğer atama, özel bir ilke tanımına dayalıdır. Bu atama Cosmos DB hesaplarının birden fazla yazma konumu için yapılandırıldığını denetler.

İlke atamaları dağıtıldıktan sonra, uyumluluk panosu değerlendirme sonuçlarını gösterir. Bu, ilke atamasını dağıttıktan sonra 30 dakika sürebileceğini unutmayın. Ayrıca, ilke [değerlendirme taramaları](../governance/policy/how-to/get-compliance-data.md#on-demand-evaluation-scan) , ilke atamaları oluşturulduktan hemen sonra isteğe bağlı olarak başlatılabilir.

Ekran görüntüsünde, kapsamdaki Azure Cosmos DB hesapları için aşağıdaki uyumluluk değerlendirmesi sonuçları gösterilmektedir:

- Sıfır iki hesap, sanal ağ (VNet) filtrelemesinin yapılandırılması gereken bir ilkeyle uyumludur.
- Hesabın birden fazla yazma konumu için yapılandırılmasını gerektiren bir ilkeyle uyumlu iki hesap yok
- İki hesabın sıfır olması, kaynakların izin verilen Azure bölgelerine dağıtıldığı ilkeyle uyumludur.

:::image type="content" source="./media/policy/compliance.png" alt-text="Listelenen Azure Ilke atamalarının uyumluluk sonuçları":::

Uyumlu olmayan kaynakları düzeltmek için bkz. [Azure ilkesi ile kaynakları](../governance/policy/how-to/remediate-resources.md)düzeltme.

## <a name="next-steps"></a>Sonraki adımlar

- Yukarıda gösterilen birden fazla yazma konumu ve VNet filtreleme ilkeleri için de dahil olmak üzere [Azure Cosmos DB için örnek özel ilke tanımlarını gözden geçirin](https://github.com/Azure/azure-policy/tree/master/samples/CosmosDB).
- [Azure portal ilke ataması oluşturma](../governance/policy/assign-policy-portal.md)
- [Azure Cosmos DB için Azure Ilkesi yerleşik ilke tanımlarını gözden geçirin](./policy-samples.md)
