---
title: Azure Cosmos DB kaynaklarına yönelik idare ve denetimleri uygulamak için Azure Ilkesini kullanma
description: Azure Cosmos DB kaynaklarına yönelik idare ve denetimleri uygulamak için Azure Ilkesi 'ni nasıl kullanacağınızı öğrenin.
author: plzm
ms.author: paelaz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: 2249dbdebecc52a8f5d6decccb83d3b1fc0777f7
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747371"
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

Aşağıda, Azure Cosmos DB bir SQL veritabanının sağlanan aktarım hızı izin verilen maksimum 400 RU/sn sınırının daha büyük olup olmadığını denetleyen örnek bir ilke tanımı verilmiştir. Özel bir ilke tanımı iki kural içerir: bir tane, özellik diğer adının belirli türünü ve ikinci bir türün belirli bir özelliği için bir denetim. Her iki kural de diğer adları kullanır.

```json
"policyRule": {
  "if": {
    "allOf": [
      {
      "field": "type",
      "equals": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/throughputSettings"
      },
      {
      "field": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/throughputSettings/default.resource.throughput",
      "greater": 400
      }
    ]
  }
}
```

Özel ilke tanımları, yerleşik ilke tanımlarının kullanıldığı şekilde, ilke atamaları oluşturmak için kullanılabilir.

## <a name="policy-compliance"></a>İlke uyumluluğu

İlke atamaları oluşturulduktan sonra, Azure Ilkesi atamanın kapsamındaki kaynakları değerlendirir. Her kaynağın ilkeyle olan _uyumluluğu_ değerlendirilir. İlkede belirtilen _efekt_ daha sonra uyumlu olmayan kaynaklara uygulanır.

Uyumluluk sonuçlarını ve düzeltme ayrıntılarını [Azure Portal](../governance/policy/how-to/get-compliance-data.md#portal) veya [Azure CLI](../governance/policy/how-to/get-compliance-data.md#command-line) veya [Azure izleyici günlükleri](../governance/policy/how-to/get-compliance-data.md#azure-monitor-logs)aracılığıyla gözden geçirebilirsiniz.

Aşağıdaki ekran görüntüsünde iki örnek ilke ataması gösterilmektedir. Bir atama, Azure Cosmos DB kaynaklarının yalnızca izin verilen Azure bölgelerine dağıtıldığını denetleyen yerleşik bir ilke tanımına dayalıdır. Diğer atama, özel bir ilke tanımına dayalıdır. Bu atama, Azure Cosmos DB kaynaklarında sağlanan üretilen işin belirtilen bir üst sınırı aşmadığını denetler.

İlke atamaları dağıtıldıktan sonra, uyumluluk panosu değerlendirme sonuçlarını gösterir. Bu, ilke atamasını dağıttıktan sonra 30 dakika sürebileceğini unutmayın.

Ekran görüntüsünde aşağıdaki uyumluluk değerlendirmesi sonuçları gösterilmektedir:

- Belirtilen kapsamdaki bir Azure Cosmos DB hesapların hiçbiri, kaynakların izin verilen bölgelere dağıtılıp dağıtılmadığını denetlemek için ilke atamasıyla uyumludur.
- Belirtilen kapsamdaki iki Azure Cosmos DB veritabanı ya da koleksiyon kaynağı, belirtilen maksimum sınırı aşan sağlanan aktarım hızını denetlemek için ilke atamasıyla uyumludur.

:::image type="content" source="./media/policy/compliance.png" alt-text="Azure Cosmos DB yerleşik ilke tanımlarını arayın":::

Uyumlu olmayan kaynakları düzeltmek için bkz. [Azure ilkesi ile düzeltilen](../governance/policy/how-to/remediate-resources.md) makalesi.

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Cosmos DB için örnek özel ilke tanımlarını gözden geçirin](https://github.com/Azure/azure-policy/tree/master/samples/CosmosDB)
- [Azure portal ilke ataması oluşturma](../governance/policy/assign-policy-portal.md)
- [Azure Cosmos DB için Azure Ilkesi yerleşik ilke tanımlarını gözden geçirin](./policy-samples.md)
