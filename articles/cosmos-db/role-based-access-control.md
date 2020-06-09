---
title: Azure Cosmos DB'de rol tabanlı erişim denetimi
description: Azure Cosmos DB Active Directory Tümleştirmesi (RBAC) ile nasıl veritabanı koruması sağladığını öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 0782d5f091671a235df1ab85a8b9706c7efe9170
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509041"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Azure Cosmos DB'de rol tabanlı erişim denetimi

Azure Cosmos DB, Azure Cosmos DB ortak yönetim senaryoları için yerleşik rol tabanlı erişim denetimi (RBAC) sağlar. Azure Active Directory bir profili olan bir kişi, Azure Cosmos DB kaynaklardaki kaynaklara ve işlemlere erişim vermek veya erişimi reddetmek için bu RBAC rollerini kullanıcılara, gruplara, hizmet sorumlularına veya yönetilen kimliklere atayabilir. Rol atamaları yalnızca, Azure Cosmos hesaplarına, veritabanlarına, kapsayıcılarına ve tekliflere (verimlilik) erişimi de içeren denetim düzlemi erişimini kapsar.

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

![Azure portal gösteren veritabanı güvenliğine erişim denetimi (ıAM)](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Özel roller

Kullanıcılar, yerleşik rollere ek olarak Azure 'da [özel roller](../role-based-access-control/custom-roles.md) de oluşturabilir ve bu rolleri, Active Directory kiracısındaki tüm aboneliklerde hizmet sorumlularına uygulayabilir. Özel roller, kullanıcılara özel bir kaynak sağlayıcısı işlemleri kümesiyle RBAC rol tanımları oluşturmak için bir yol sağlar. Azure Cosmos DB için özel roller oluşturmaya yönelik hangi işlemlerin kullanılabildiğini öğrenmek için bkz. [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="preventing-changes-from-cosmos-sdk"></a>Cosmos SDK 'dan değişiklikler engelleniyor

> [!WARNING]
> Bu özelliğin etkinleştirilmesi, uygulamanız üzerinde tehlikeli bir etkiye sahip olabilir. Bu özelliği etkinleştirmeden önce lütfen iyice okuyun.

Azure Cosmos DB kaynak sağlayıcısı, hesap anahtarlarını kullanarak bağlanan herhangi bir istemciden (Cosmos SDK aracılığıyla bağlanan uygulamalar) yapılan herhangi bir değişikliği engellemek için kilitlenebilir. Bu, Azure portal yapılan değişikliği de içerir. Bu, üretim ortamları için daha yüksek düzeyde denetim ve idare isteyen ve kaynak kilitleri gibi özellikleri etkinleştiren ve ayrıca denetim düzlemi işlemleri için tanılama günlüklerini etkinleştiren kullanıcılar için istenebilir. Cosmos DB SDK aracılığıyla bağlanan istemciler, Cosmos hesapları, veritabanları, kapsayıcılar ve verimlilik için herhangi bir özelliği değiştirmeyecektir. Verileri, Cosmos kapsayıcılarına okumayı ve yazmayı kapsayan işlemler etkilenmez.

Ayarlandığında, herhangi bir kaynakta yapılan değişiklikler yalnızca uygun RBAC rolüne ve yönetilen hizmet kimlikleri dahil Azure Active Directory kimlik bilgilerine sahip bir kullanıcıdan yapılabilir.

### <a name="check-list-before-enabling"></a>Etkinleştirmeden önce onay listesi

Bu ayar herhangi bir Cosmos kaynağında herhangi bir Cosmos DB SDK, hesap anahtarı aracılığıyla bağlanan herhangi bir araç ve Azure portal gibi hesap anahtarlarını kullanarak bağlanan herhangi bir istemciden herhangi bir değişiklik yapılmasını engeller. Bu özelliği etkinleştirdikten sonra uygulamalardan oluşan sorunları veya hataları engellemek için, bu özelliği etkinleştirmeden önce uygulamaların veya Azure portal kullanıcıların aşağıdaki eylemlerden herhangi birini gerçekleştirmesini denetleyin:

- Tüm özellikler dahil olmak üzere Cosmos hesabında yapılan herhangi bir değişiklik veya bölge ekleme veya kaldırma.

- Veritabanları ve kapsayıcılar gibi alt kaynakları oluşturma, silme. Bu, Cassandra, MongoDB, Gremlin ve tablo kaynakları gibi diğer API 'leri için kaynaklar içerir.

- Veritabanı veya kapsayıcı düzeyindeki kaynaklarda üretilen iş güncelleştiriliyor.

- Dizin ilkesi, TTL ve benzersiz anahtarlar dahil kapsayıcı özelliklerini değiştirme.

- Saklı yordamları, Tetikleyicileri veya Kullanıcı tanımlı işlevleri değiştirme.

Uygulamalarınız (veya Azure portal aracılığıyla kullanıcılar) bu eylemlerden herhangi birini gerçekleştirdiyse, bu eylemlerin [ARM şablonları](manage-sql-with-resource-manager.md), [POWERSHELL](manage-with-powershell.md), [Azure CLI](manage-with-cli.md), [rest](/rest/api/cosmos-db-resource-provider/) veya [Azure Yönetim Kitaplığı](https://github.com/Azure-Samples/cosmos-management-net)aracılığıyla yürütülmesi için geçirilmesi gerekir. Azure yönetiminin [birden çok dilde](https://docs.microsoft.com/azure/?product=featured#languages-and-tools)kullanılabilir olduğunu unutmayın.

### <a name="set-via-arm-template"></a>ARM şablonu aracılığıyla ayarla

Bu özelliği bir ARM şablonu kullanarak ayarlamak için, mevcut şablonunuzu güncelleştirin veya geçerli dağıtımınız için yeni bir şablon verin ve ardından `"disableKeyBasedMetadataWriteAccess": true` databaseAccounts kaynakları için özellikleri ekleyin. Aşağıda, bu özellik ayarıyla Azure Resource Manager şablonun temel bir örneği verilmiştir.

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

Azure CLı kullanarak etkinleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az cosmosdb update  --name [CosmosDBAccountName] --resource-group [ResourceGroupName]  --disable-key-based-metadata-write-access true

```

### <a name="set-via-powershell"></a>PowerShell ile ayarlama

Azure PowerShell kullanmayı etkinleştirmek için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Update-AzCosmosDBAccount -ResourceGroupName [ResourceGroupName] -Name [CosmosDBAccountName] -DisableKeyBasedMetadataWriteAccess true
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
- [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)
- [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
