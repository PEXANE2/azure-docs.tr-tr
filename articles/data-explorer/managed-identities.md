---
title: Azure Veri Gezgini kümesi için Yönetilen kimlikler nasıl yapılandırılır
description: Azure Veri Gezgini kümesi için yönetilen kimliklerin nasıl yapılandırılacağını öğrenin.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: e76ae2e072bb780ac9788902e9157db871e4f09d
ms.sourcegitcommit: ef568f562fbb05b4bd023fe2454f9da931adf39a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2020
ms.locfileid: "77373382"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümeniz için Yönetilen kimlikler yapılandırma

[Azure Active Directory yönetilen bir kimlik](/azure/active-directory/managed-identities-azure-resources/overview) , kümenizin Azure Key Vault gıbı diğer AAD korumalı kaynaklara kolayca erişmesini sağlar. Kimlik, Azure platformu tarafından yönetilir ve herhangi bir gizli dizi sağlamanızı veya döndürmenizi gerektirmez. Bu makalede, Azure Veri Gezgini kümeleri için yönetilen kimlik oluşturma gösterilmektedir. Yönetilen kimlik yapılandırması şu anda yalnızca [kümeniz için müşteri tarafından yönetilen anahtarları etkinleştirmek](/azure/data-explorer/security#customer-managed-keys-with-azure-key-vault)üzere desteklenmektedir.

> [!Note]
> Azure Veri Gezgini için Yönetilen kimlikler, uygulamanız abonelikler veya kiracılar arasında geçirilirse beklendiği gibi davranır. Uygulamanın, [bir kimlik kaldır](#remove-an-identity)kullanılarak özelliği devre dışı bırakıp yeniden etkinleştirerek yapılabilen yeni bir kimlik alması gerekir. Aşağı akış kaynaklarının erişim ilkelerinin da yeni kimliği kullanması için güncelleştirilmeleri gerekir.

## <a name="add-a-system-assigned-identity"></a>Sistem tarafından atanan kimlik ekleme

Kümenize kümenize bağlı bir **sistem tarafından atanan kimlik** atanabilir ve kümeniz silinirse silinir. Bir küme yalnızca bir sistem tarafından atanmış kimliğe sahip olabilir. Sistem tarafından atanan kimlik ile küme oluşturmak, kümede ek bir özelliğin ayarlanmasını gerektirir.

### <a name="add-a-system-assigned-identity-using-c"></a>Kullanarak sistem tarafından atanan bir kimlik eklemeC#

Azure Veri Gezgini C# istemcisini kullanarak yönetilen bir kimlik ayarlamak için şunları yapın:

* [Azure Veri Gezgini (kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükler.
* Kimlik doğrulaması için [Microsoft. IdentityModel. clients. ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükler.
* Aşağıdaki örneği çalıştırmak için, kaynaklara erişebilen [bir Azure AD uygulaması](/azure/active-directory/develop/howto-create-service-principal-portal) ve hizmet sorumlusu oluşturun. Abonelik kapsamına rol ataması ekleyebilir ve gerekli `Directory (tenant) ID`, `Application ID`ve `Client Secret`alabilirsiniz.

#### <a name="create-or-update-your-cluster"></a>Kümenizi oluşturun veya güncelleştirin

1. `Identity` özelliğini kullanarak kümenizi oluşturun veya güncelleştirin:

    ```csharp
    var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
    var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
    var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
    var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
    var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
    var credential = new ClientCredential(clientId, clientSecret);
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);
    
    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);
    
    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = subscriptionId
    };
    
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "Central US";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var identity = new Identity(IdentityType.SystemAssigned);
    var cluster = new Cluster(location, sku, identity: identity);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```
    
2. Kümenizin başarıyla oluşturulduğunu veya bir kimlikle güncelleştirilip güncelleştirilmediğini denetlemek için şu komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Sonuç `Succeeded` değerine sahip `ProvisioningState` içeriyorsa, küme oluşturulmuştur veya güncellenir ve aşağıdaki özelliklere sahip olmalıdır:
   
    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

    `PrincipalId` ve `TenantId`, GUID 'lerle değiştirilmiştir. `TenantId` özelliği, kimliğin ait olduğu AAD kiracısını tanımlar. `PrincipalId`, kümenin yeni kimliği için benzersiz bir tanımlayıcıdır. AAD 'de, hizmet sorumlusu App Service veya Azure Işlevleri Örneğinizde verdiğiniz aynı ada sahiptir.

### <a name="add-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak sistem tarafından atanan kimlik ekleme

Azure Resource Manager şablonu, Azure kaynaklarınızın dağıtımını otomatikleştirmek için kullanılabilir. Azure Veri Gezgini dağıtma hakkında daha fazla bilgi için, bkz. [Azure Resource Manager şablonu kullanarak azure Veri Gezgini kümesi ve veritabanı oluşturma](create-cluster-database-resource-manager.md).

Sistem tarafından atanan tür eklendiğinde Azure, kümenizin kimliğini oluşturmak ve yönetmek üzere size bildirir. `Microsoft.Kusto/clusters` türündeki herhangi bir kaynak, kaynak tanımına aşağıdaki özelliği ekleyerek bir kimlikle oluşturulabilir: 

```json
"identity": {
    "type": "SystemAssigned"
}    
```

Örnek:

```json
{
    "apiVersion": "2019-09-07",
    "type": "Microsoft.Kusto/clusters",
    "name": "[variables('clusterName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "trustedExternalTenants": [],
        "virtualNetworkConfiguration": null,
        "optimizedAutoscale": null,
        "enableDiskEncryption": false,
        "enableStreamingIngest": false,
    }
}
```

Küme oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

`<TENANTID>` ve `<PRINCIPALID>`, GUID 'lerle değiştirilmiştir. `TenantId` özelliği, kimliğin ait olduğu AAD kiracısını tanımlar. `PrincipalId`, kümenin yeni kimliği için benzersiz bir tanımlayıcıdır. AAD 'de, hizmet sorumlusu App Service veya Azure Işlevleri Örneğinizde verdiğiniz aynı ada sahiptir.

## <a name="remove-an-identity"></a>Kimlik kaldırma

Sistem tarafından atanan bir kimliğin kaldırılması, AAD 'den de silinir. Küme kaynağı silindiğinde, sistem tarafından atanan kimlikler de AAD 'den otomatik olarak kaldırılır. Özelliği devre dışı bırakarak, sistem tarafından atanan bir kimlik kaldırılabilir:

```json
"identity": {
    "type": "None"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da Azure Veri Gezgini kümelerini güvenli hale getirme](security.md)
* Rest 'de şifrelemeyi etkinleştirerek [Azure Veri Gezgini Azure Portal kümenizin güvenliğini sağlayın](manage-cluster-security.md) .
 * [Müşteri tarafından yönetilen anahtarları kullanarak yapılandırmaC#](customer-managed-keys-csharp.md)
 * [Azure Resource Manager şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırma](customer-managed-keys-resource-manager.md)
