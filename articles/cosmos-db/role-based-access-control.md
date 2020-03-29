---
title: Azure Cosmos DB'de rol tabanlı erişim denetimi
description: Azure Cosmos DB'nin Etkin dizin tümleştirmesi (RBAC) ile veritabanı koruması sağladığını öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 0c7332a42751b35b6ad8ec3f88afb7bc78cc85e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445095"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Azure Cosmos DB'de rol tabanlı erişim denetimi

Azure Cosmos DB, Azure Cosmos DB'deki yaygın yönetim senaryoları için yerleşik rol tabanlı erişim denetimi (RBAC) sağlar. Azure Active Directory'de profili olan bir kişi, Azure Cosmos DB kaynaklarındaki kaynaklara ve işlemlere erişim sağlamak veya reddetmek için bu RBAC rollerini kullanıcılara, gruplara, hizmet ilkelerine veya yönetilen kimliklere atayabilir. Rol atamaları, Azure Cosmos hesaplarına, veritabanlarına, kapsayıcılara ve tekliflere (iş ortası) erişimi içeren yalnızca denetim düzlemi erişimine göre kapsamlıdır.

## <a name="built-in-roles"></a>Yerleşik roller

Azure Cosmos DB tarafından desteklenen yerleşik roller şunlardır:

|**Yerleşik rol**  |**Açıklama**  |
|---------|---------|
|[DocumentDB Hesap Katılımcısı](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Azure Cosmos DB hesaplarını yönetebilirsiniz.|
|[Cosmos DB Hesap Okuyucu](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Azure Cosmos DB hesap verilerini okuyabilir.|
|[Cosmos Yedekleme Operatörü](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)|Azure Cosmos veritabanı veya kapsayıcı için geri yükleme isteği gönderebilir.|
|[Cosmos DB Operatörü](../role-based-access-control/built-in-roles.md#cosmos-db-operator)|Azure Cosmos hesaplarını, veritabanlarını ve kapsayıcıları sağlayabilir, ancak verilere erişmek için gereken anahtarlara erişemez.|

> [!IMPORTANT]
> Azure Cosmos DB'deki RBAC desteği yalnızca düzlem işlemlerini denetlemek için geçerlidir. Veri düzlemi işlemleri ana anahtarlar veya kaynak belirteçleri kullanılarak güvenlihale alınır. Daha fazla bilgi için Azure [Cosmos DB'deki verilere güvenli erişim](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Kimlik ve erişim yönetimi (IAM)

Azure portalındaki **Access denetimi (IAM)** bölmesi, Azure Cosmos kaynaklarında rol tabanlı erişim denetimini yapılandırmak için kullanılır. Roller, Etkin Dizini'ndeki kullanıcılara, gruplara, hizmet ilkelerine ve yönetilen kimliklere uygulanır. Kişiler ve gruplar için yerleşik rolleri veya özel rolleri kullanabilirsiniz. Aşağıdaki ekran görüntüsü, Azure portalında erişim denetimi (IAM) kullanarak Active Directory tümleştirmesini (RBAC) gösterir:

![Azure portalında erişim denetimi (IAM) - veritabanı güvenliğini gösterme](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Özel roller

Yerleşik rollere ek olarak, kullanıcılar Azure'da [özel roller](../role-based-access-control/custom-roles.md) oluşturabilir ve bu rolleri Active Directory kiracıları içindeki tüm aboneliklerde hizmet ilkelerine uygulayabilir. Özel roller, kullanıcılara özel bir kaynak sağlayıcı işlemleri kümesiyle RBAC rol tanımları oluşturmanın bir yolunu sağlar. Azure Cosmos DB için özel roller oluşturmak için hangi işlemlerin kullanılabildiği hakkında bilgi edinmek için [Azure Cosmos DB kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="preventing-changes-from-cosmos-sdk"></a>Cosmos SDK'dan değişikliklerin önlenmesi

Cosmos kaynak sağlayıcısı, hesap anahtarları üzerinden bağlanan herhangi bir istemciden (yani Cosmos SDK üzerinden bağlanan uygulamalar) cosmos hesabı, veritabanları, kaplar ve iş miktarı dahil olmak üzere kaynaklarda herhangi bir değişiklik olmasını önlemek için kilitlenebilir. Ayarlandığında, herhangi bir kaynakdeğişiklikleri uygun RBAC rolü ve kimlik bilgilerine sahip bir kullanıcıdan olmalıdır. Bu özellik, `disableKeyBasedMetadataWriteAccess` Cosmos kaynak sağlayıcısında özellik değeri ile ayarlanır. Bu özellik ayarı ile bir Azure Kaynak Yöneticisi şablonu örneği aşağıdadır.

```json
{
    {
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "name": "[variables('accountName')]",
      "apiVersion": "2019-08-01",
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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
- [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)
- [Azure Cosmos DB kaynak sağlayıcı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
