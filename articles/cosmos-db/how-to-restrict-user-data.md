---
title: Yalnızca Azure Cosmos DB ile kullanıcı erişimini veri işlemlerine kısıtlama
description: Yalnızca Azure Cosmos DB ile veri işlemlerine erişimi nasıl kısıtlayın öğrenin
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/9/2019
ms.author: tvoellm
ms.openlocfilehash: 03cad9e4c3752b5f35be785a6280bf18aaa14860
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74980380"
---
# <a name="restrict-user-access-to-data-operations-only"></a>Kullanıcı erişimini yalnızca veri işlemleriyle sınırlama

Azure Cosmos DB'de, veritabanı hizmetiyle etkileşimlerinizi doğrulamanın iki yolu vardır:
- Azure portalı ile etkileşimde çalışırken Azure Etkin Dizin kimliğinizi kullanarak,
- API'lerden ve SDK'lardan arama lar çıkarırken Azure Cosmos DB [anahtarlarını](secure-access-to-data.md#master-keys) veya [kaynak belirteçlerini](secure-access-to-data.md#resource-tokens) kullanma.

Her kimlik doğrulama yöntemi, bazı çakışmalarla farklı ![işlem kümelerine erişim sağlar: Kimlik doğrulama türüne göre işlemlerin bölünmesi](./media/how-to-restrict-user-data/operations.png)

Bazı senaryolarda, kuruluşunuzun bazı kullanıcılarını yalnızca veri işlemleri (CRUD istekleri ve sorguları) gerçekleştirmeleri için kısıtlamak isteyebilirsiniz. Bu genellikle, kaynak oluşturması veya silmesi veya üzerinde çalıştıkları kapsayıcıların sağlanan iş kısmını değiştirmesi gerekmeyen geliştiriciler için geçerlidir.

Aşağıdaki adımları uygulayarak erişimi kısıtlayabilirsiniz:
1. Erişimi kısıtlamak istediğiniz kullanıcılar için özel bir Azure Etkin Dizin rolü oluşturma. Özel Active Directory rolü, Azure Cosmos DB'nin [parçalı eylemlerini](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)kullanarak işlemlere ince taneli erişim düzeyine sahip olmalıdır.
1. Anahtarlarla veri dışı işlemlerin yürütülmesine izin vermeme. Bu işlemleri yalnızca Azure Kaynak Yöneticisi çağrılarıyla sınırlandırarak bunu başarabilirsiniz.

Bu makalenin sonraki bölümleri, bu adımların nasıl gerçekleştirileceğimi gösterir.

> [!NOTE]
> Sonraki bölümlerdeki komutları yürütmek için Azure PowerShell Modülü 3.0.0 veya sonraki bölümlerinin yanı sıra değiştirmeye çalıştığınız abonelikte [Azure Sahibi Rolünü](../role-based-access-control/built-in-roles.md#owner) yüklemeniz gerekir.

Sonraki bölümlerdeki PowerShell komut dosyalarında, aşağıdaki yer tutucuları ortamınıza özel değerlerle değiştirin:
- `$MySubscriptionId`- İzinleri sınırlamak istediğiniz Azure Cosmos hesabını içeren abonelik kimliği. Örneğin: `e5c8766a-eeb0-40e8-af56-0eb142ebf78e`.
- `$MyResourceGroupName`- Azure Cosmos hesabını içeren kaynak grubu. Örneğin: `myresourcegroup`.
- `$MyAzureCosmosDBAccountName`- Azure Cosmos hesabınızın adı. Örneğin: `mycosmosdbsaccount`.
- `$MyUserName`- Erişimi sınırlamakusername@domainistediğiniz kullanıcının girişi . Örneğin: `cosmosdbuser@contoso.com`.

## <a name="select-your-azure-subscription"></a>Azure aboneliğinizi seçin

Azure PowerShell komutları, komutları çalıştırmak için oturum açmanızı ve aboneliği seçmenizi gerektirir:

```azurepowershell
Login-AzAccount
Select-AzSubscription $MySubscriptionId
```

## <a name="create-the-custom-azure-active-directory-role"></a>Özel Azure Etkin Dizin rolünü oluşturma

Aşağıdaki komut dosyası, Azure Cosmos hesapları için "Yalnızca Anahtar" erişimine sahip bir Azure Etkin Dizin rol ataması oluşturur. Bu rol, [Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md) ve Azure [Cosmos DB için parçalı eylemleri](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)temel alır. Bu roller ve eylemler `Microsoft.DocumentDB` Azure Etkin Dizin ad alanının bir parçasıdır.

1. İlk olarak, aşağıdaki içeriği `AzureCosmosKeyOnlyAccess.json` içeren bir JSON belgesi oluşturun:

    ```
    {
        "Name": "Azure Cosmos DB Key Only Access Custom Role",
        "Id": "00000000-0000-0000-0000-0000000000",
        "IsCustom": true,
        "Description": "This role restricts the user to read the account keys only.",
        "Actions":
        [
            "Microsoft.DocumentDB/databaseAccounts/listKeys/action"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "AssignableScopes":
        [
            "/subscriptions/$MySubscriptionId"
        ]
    }
    ```

1. Rol atamasını oluşturmak ve kullanıcıya atamak için aşağıdaki komutları çalıştırın:

    ```azurepowershell
    New-AzRoleDefinition -InputFile "AzureCosmosKeyOnlyAccess.json"
    New-AzRoleAssignment -SignInName $MyUserName -RoleDefinitionName "Azure Cosmos DB Key Only Access Custom Role" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName -ResourceType "Microsoft.DocumentDb/databaseAccounts"
    ```

## <a name="disallow-the-execution-of-non-data-operations"></a>Veri dışı işlemlerin yürütülmesine izin vermeme

Aşağıdaki komutlar, aşağıdaki leri kullanma yeteneğini kaldırır:
- kaynakları oluşturma, değiştirme veya silme
- kapsayıcı ayarlarını güncelleştirin (dizin oluşturma ilkeleri, iş sonu vb. dahil).

```azurepowershell
$cdba = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName
$cdba.Properties.disableKeyBasedMetadataWriteAccess="True"
$cdba | Set-AzResource -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- [Cosmos DB'nin rol tabanlı erişim kontrolü](role-based-access-control.md) hakkında daha fazla bilgi edinin
- [Cosmos DB'deki verilere güvenli erişime](secure-access-to-data.md) genel bir bakış alın
