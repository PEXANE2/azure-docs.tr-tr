---
title: Azure Cosmos DB'de rol tabanlı erişim denetimi
description: Azure Cosmos DB Active Directory Tümleştirmesi (RBAC) ile nasıl veritabanı koruması sağladığını öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 6edf5de852ea836de8be02636dd8a971ccebb86d
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87530580"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Azure Cosmos DB'de rol tabanlı erişim denetimi

Azure Cosmos DB, Azure Cosmos DB ortak yönetim senaryoları için yerleşik rol tabanlı erişim denetimi (RBAC) sağlar. Azure Active Directory bir profili olan bir kişi, Azure Cosmos DB kaynaklardaki kaynaklara ve işlemlere erişim vermek veya erişimi reddetmek için bu Azure rollerini kullanıcılara, gruplara, hizmet sorumlularına veya yönetilen kimliklere atayabilir. Rol atamaları yalnızca, Azure Cosmos hesaplarına, veritabanlarına, kapsayıcılarına ve tekliflere (verimlilik) erişimi de içeren denetim düzlemi erişimini kapsar.

## <a name="built-in-roles"></a>Yerleşik roller

Azure Cosmos DB tarafından desteklenen yerleşik roller aşağıda verilmiştir:

|**Yerleşik rol**  |**Açıklama**  |
|---------|---------|
|[DocumentDB hesabı Katılımcısı](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|, Azure Cosmos DB hesaplarını yönetebilir.|
|[Cosmos DB hesabı okuyucusu](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Azure Cosmos DB hesabı verilerini okuyabilir.|
|[Cosmos yedekleme Işletmeni](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)|, Bir Azure Cosmos veritabanı veya kapsayıcısı için geri yükleme isteği gönderebilir.|
|[Cosmos DB Işleci](../role-based-access-control/built-in-roles.md#cosmos-db-operator)|Azure Cosmos hesaplarını, veritabanlarını ve kapsayıcıları sağlayabilir, ancak verilere erişmek için gereken anahtarlara erişemez.|

> [!IMPORTANT]
> Azure Cosmos DB ' de RBAC desteği yalnızca denetim düzlemi işlemleri için geçerlidir. Veri düzlemi işlemleri, ana anahtarlar veya kaynak belirteçleri kullanılarak güvenli hale getirilir. Daha fazla bilgi için bkz. [Azure Cosmos DB verilere güvenli erişim](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Kimlik ve erişim yönetimi (ıAM)

Azure portal **erişim denetimi (IAM)** bölmesi Azure Cosmos kaynaklarında rol tabanlı erişim denetimini yapılandırmak için kullanılır. Roller, kullanıcılar, gruplar, hizmet sorumluları ve Active Directory içindeki yönetilen kimliklere uygulanır. Bireyler ve gruplar için yerleşik roller veya özel roller kullanabilirsiniz. Aşağıdaki ekran görüntüsünde, Azure portal erişim denetimi (ıAM) kullanan Active Directory tümleştirme (RBAC) gösterilmektedir:

:::image type="content" source="./media/role-based-access-control/database-security-identity-access-management-rbac.png" alt-text="Azure portal gösteren veritabanı güvenliğine erişim denetimi (ıAM)":::

## <a name="custom-roles"></a>Özel roller

Kullanıcılar, yerleşik rollere ek olarak Azure 'da [özel roller](../role-based-access-control/custom-roles.md) de oluşturabilir ve bu rolleri, Active Directory kiracısındaki tüm aboneliklerde hizmet sorumlularına uygulayabilir. Özel roller, kullanıcılara özel bir kaynak sağlayıcısı işlemleri kümesiyle Azure rol tanımları oluşturmak için bir yol sağlar. Azure Cosmos DB için özel roller oluşturmaya yönelik hangi işlemlerin kullanılabildiğini öğrenmek için bkz. [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="preventing-changes-from-the-azure-cosmos-db-sdks"></a><a id="prevent-sdk-changes"></a>Azure Cosmos DB SDK 'lardan değişiklikler engelleniyor

Azure Cosmos DB kaynak sağlayıcısı, hesap anahtarlarını (Azure Cosmos SDK aracılığıyla bağlanan uygulamalar) kullanarak bağlanan bir istemciden kaynaklardaki değişikliklerin oluşmasını engellemek için kilitlenebilir. Bu da Azure portal yapılan değişiklikleri içerir. Bu özellik, üretim ortamları için daha yüksek düzeyde denetim ve idare isteyen kullanıcılar için istenebilir. SDK 'daki değişikliklerin önlenmesi, denetim düzlemi işlemlerine yönelik kaynak kilitleri ve tanılama günlükleri gibi özellikleri de devre da sunar. Azure Cosmos DB SDK 'dan bağlanan istemcilerin Azure Cosmos hesapları, veritabanları, kapsayıcılar ve verimlilik için herhangi bir özelliği değiştirmesi engellenir. Verileri, Cosmos kapsayıcılarına okumayı ve yazmayı ilgilendiren işlemler etkilenmez.

Bu özellik etkinleştirildiğinde, herhangi bir kaynakta yapılan değişiklikler yalnızca, doğru Azure rolüne sahip bir kullanıcıdan ve yönetilen hizmet kimlikleri dahil Azure Active Directory kimlik bilgileriyle yapılabilir.

> [!WARNING]
> Bu özelliğin etkinleştirilmesi, uygulamanız üzerinde etkiye sahip olabilir. Etkinleştirilmeden önce etkisini anladığınızdan emin olun.

### <a name="check-list-before-enabling"></a>Etkinleştirmeden önce onay listesi

Bu ayar herhangi bir Cosmos kaynağında herhangi bir Cosmos DB SDK, hesap anahtarı aracılığıyla bağlanan herhangi bir araç ve Azure portal gibi hesap anahtarlarını kullanarak bağlanan herhangi bir istemciden herhangi bir değişiklik yapılmasını engeller. Bu özelliği etkinleştirdikten sonra uygulamalardan oluşan sorunları veya hataları engellemek için, bu özelliği etkinleştirmeden önce uygulamaların veya Azure portal kullanıcıların aşağıdaki eylemlerden herhangi birini gerçekleştirmesini denetleyin:

- Tüm özellikler dahil olmak üzere Cosmos hesabında yapılan herhangi bir değişiklik veya bölge ekleme veya kaldırma.

- Veritabanları ve kapsayıcılar gibi alt kaynakları oluşturma, silme. Bu, Cassandra, MongoDB, Gremlin ve tablo kaynakları gibi diğer API 'Lerin kaynaklarını içerir.

- Veritabanı veya kapsayıcı düzeyindeki kaynaklarda üretilen iş güncelleştiriliyor.

- Dizin ilkesi, TTL ve benzersiz anahtarlar dahil kapsayıcı özelliklerini değiştirme.

- Saklı yordamları, Tetikleyicileri veya Kullanıcı tanımlı işlevleri değiştirme.

Uygulamalarınız (veya Azure portal aracılığıyla) bu eylemlerden herhangi birini gerçekleştirdiyse, bu eylemlerin [ARM şablonları](manage-sql-with-resource-manager.md), [POWERSHELL](manage-with-powershell.md), [Azure CLI](manage-with-cli.md), REST veya [Azure Yönetim Kitaplığı](https://github.com/Azure-Samples/cosmos-management-net)aracılığıyla yürütülmesi için geçirilmesi gerekir. Azure yönetiminin [birden çok dilde](https://docs.microsoft.com/azure/?product=featured#languages-and-tools)kullanılabilir olduğunu unutmayın.

### <a name="set-via-arm-template"></a>ARM şablonu aracılığıyla ayarla

Bir ARM şablonu kullanarak bu özelliği ayarlamak için, mevcut şablonunuzu güncelleştirin veya geçerli dağıtımınız için yeni bir şablon dışarı aktarın ve ardından `"disableKeyBasedMetadataWriteAccess": true` kaynakların özelliklerine ekleyin `databaseAccounts` . Aşağıda, bu özellik ayarıyla Azure Resource Manager şablonun temel bir örneği verilmiştir.

```json
{
    {
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "name": "[variables('accountName')]",
      "apiVersion": "2020-04-01",
      "location": "[parameters('location')]",
      "kind": "GlobalDocumentDB",
      "properties": {
        "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
        "locations": "[variables('locations')]",
        "databaseAccountOfferType": "Standard",
        "disableKeyBasedMetadataWriteAccess": true
        }
    }
}
```

> [!IMPORTANT]
> Bu özellikle redploying olduğunda, hesabınız ve alt kaynaklarınızın diğer özelliklerini eklediğinizden emin olun. Bu şablonu olduğu gibi dağıtmayın veya tüm hesap özelliklerinizi sıfırlayacaktır.

### <a name="set-via-azure-cli"></a>Azure CLı aracılığıyla ayarlama

Azure CLı 'yı kullanmayı etkinleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az cosmosdb update  --name [CosmosDBAccountName] --resource-group [ResourceGroupName]  --disable-key-based-metadata-write-access true

```

### <a name="set-via-powershell"></a>PowerShell ile ayarlama

Azure PowerShell kullanmayı etkinleştirmek için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Update-AzCosmosDBAccount -ResourceGroupName [ResourceGroupName] -Name [CosmosDBAccountName] -DisableKeyBasedMetadataWriteAccess true
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../role-based-access-control/overview.md)
- [Özel Azure rolleri](../role-based-access-control/custom-roles.md)
- [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
