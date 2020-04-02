---
title: Azure Veri Gezgini kümesi için yönetilen kimlikler nasıl yapılandırılabilen
description: Azure Veri Gezgini kümesi için yönetilen kimlikleri nasıl yapılandırıştırılamayı öğrenin.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 900bf815917a4b7c9841860d663a2183b1ab71b3
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529668"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırma

[Azure Active Directory'nin yönetilen kimliği,](/azure/active-directory/managed-identities-azure-resources/overview) kümenizin Azure Anahtar Kasası gibi AAD korumalı diğer kaynaklara kolayca erişmesine olanak tanır. Kimlik Azure platformu tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. Bu makalede, Azure Veri Gezgini kümeleri için yönetilen bir kimlik nasıl oluşturulacaksınız gösterilmektedir. Yönetilen kimlik yapılandırması şu anda yalnızca [kümeniz için müşteri tarafından yönetilen anahtarları etkinleştirmek için](/azure/data-explorer/security#customer-managed-keys-with-azure-key-vault)desteklenir.

> [!Note]
> Azure Veri Gezgini kümeniz abonelikler veya kiracılar arasında taşınırsa, Azure Veri Gezgini için yönetilen kimlikler beklendiği gibi çalışmaz. Uygulamanın, özelliği [devre dışı bırakarak](#disable-a-system-assigned-identity) ve yeniden etkinleştirerek yapılabilecek yeni bir kimlik [edinmesi](#add-a-system-assigned-identity) gerekir. Yeni kimliği kullanmak için alt akış kaynaklarının erişim ilkelerinin de güncelleştirilmesi gerekir.

## <a name="add-a-system-assigned-identity"></a>Sisteme atanmış kimlik ekleme
                                                                                                    
Kümenize bağlı ve kümeniz silinirse silinen sistem tarafından atanmış bir kimlik atayın. Bir kümeyalnızca bir sistem atanmış kimliğe sahip olabilir. Sistem tarafından atanan bir kimliğe sahip bir küme oluşturmak, kümeüzerinde ayarlanacak ek bir özellik gerektirir. Sisteme atanan kimlik, aşağıda açıklandığı gibi C#, ARM şablonları veya Azure portalı kullanılarak eklenir.

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

### <a name="add-a-system-assigned-identity-using-the-azure-portal"></a>Azure portalını kullanarak sisteme atanmış bir kimlik ekleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.

#### <a name="new-azure-data-explorer-cluster"></a>Yeni Azure Veri Gezgini kümesi

1. [Azure Veri Gezgini kümesi oluşturma](/azure/data-explorer/create-cluster-database-portal#create-a-cluster) 
1. **Güvenlik** sekmesinde > **Sistemi atanan kimlik**, **On**seçin . Atanan sistemi kaldırmak için **Kapalı'yı**seçin.
2. **Sonraki:Etiketler>** veya **Gözden Geçir +** kümeoluşturmak için oluşturun'u seçin.

    ![Yeni kümeye atanan sistem ekleme](media/managed-identities/system-assigned-identity-new-cluster.png)

#### <a name="existing-azure-data-explorer-cluster"></a>Varolan Azure Veri Gezgini kümesi

1. Varolan bir Azure Veri Gezgini kümesini açın.
1. Portalın sol bölmesinde **Ayarlar** > **Kimliği'ni** seçin.
1. **Kimlik** bölmesinde > **Sistemi atanan** sekme:
   1. **Durum** kaydırıcısını **A'ya**taşıyın.
   1. **Kaydet**’i seçin
   1. Açılan pencerede **Evet'i** seçin

    ![Sistem atanmış kimlik ekleme](media/managed-identities/turn-system-assigned-identity-on.png)

1. Birkaç dakika sonra, ekran gösterir: 
  * **Object ID** - müşteri yönetilen anahtarlar için kullanılır 
  * **Rol atamaları** - ilgili rolleri atamak için bağlantıyı tıklatın

    ![Sistem üzerinde kimlik atanan](media/managed-identities/system-assigned-identity-on.png)

# <a name="c"></a>[C #](#tab/c-sharp)

### <a name="add-a-system-assigned-identity-using-c"></a>C kullanarak sisteme atanmış kimlik ekleme #

#### <a name="prerequisites"></a>Ön koşullar

Azure Veri Gezgini C# istemcisini kullanarak yönetilen bir kimlik oluşturmak için:

* Azure [Veri Gezgini (Kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükleyin.
* Kimlik doğrulaması için [Microsoft.IdentityModel.Clients.ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükleyin.
* Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi [oluşturun.](/azure/active-directory/develop/howto-create-service-principal-portal) Abonelik kapsamında rol ataması ekler ve `Directory (tenant) ID` `Application ID`gerekli `Client Secret`, , ve .

#### <a name="create-or-update-your-cluster"></a>Kümenizi oluşturma veya güncelleme

1. Özelliği kullanarak kümenizi `Identity` oluşturun veya güncelleştirin:

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
    
2. Kümenizin başarılı bir şekilde oluşturulıp oluşturulmamasını veya bir kimlikle güncelleştirilip güncelleştirilemediğinizi denetlemek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Sonuç değeri `ProvisioningState` içeriyorsa, `Succeeded` küme oluşturuldu veya güncelleştirildi ve aşağıdaki özelliklere sahip olmalıdır:

    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

`PrincipalId`ve `TenantId` GUIDs ile değiştirilir. Özellik, `TenantId` kimliğin ait olduğu AAD kiracısını tanımlar. Kümenin `PrincipalId` yeni kimliği için benzersiz bir tanımlayıcıdır. AAD'de, hizmet sorumlusu, Uygulama Hizmetinize veya Azure İşlevlerinize vermiş olduğunuz ada sahiptir.

# <a name="arm-template"></a>[ARM şablonu](#tab/arm)

### <a name="add-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak sisteme atanmış kimlik ekleme

Azure Kaynak Yöneticisi şablonu, Azure kaynaklarınızın dağıtımını otomatikleştirmek için kullanılabilir. Azure Veri Gezgini'ne dağıtım hakkında daha fazla bilgi edinmek için azure [kaynak yöneticisi şablonu kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturma 'na](create-cluster-database-resource-manager.md)bakın.

Sistem tarafından atanan türü eklemek, Azure'a kümenizin kimliğini oluşturmasını ve yönetmesini söyler. Herhangi bir `Microsoft.Kusto/clusters` tür kaynak kaynak tanımına aşağıdaki özelliği ekleyerek bir kimlik ile oluşturulabilir: 

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

`<TENANTID>`ve `<PRINCIPALID>` GUIDs ile değiştirilir. Özellik, `TenantId` kimliğin ait olduğu AAD kiracısını tanımlar. Kümenin `PrincipalId` yeni kimliği için benzersiz bir tanımlayıcıdır. AAD'de, hizmet sorumlusu, Uygulama Hizmetinize veya Azure İşlevlerinize vermiş olduğunuz ada sahiptir.

---

## <a name="disable-a-system-assigned-identity"></a>Sistemtarafından atanan kimliği devre dışı

Sistem tarafından atanan bir kimliğin kaldırılması da aad onu siler. Küme kaynağı silindiğinde, sisteme atanan kimlikler de Otomatik olarak AAD'den kaldırılır. Sistem tarafından atanan kimlik özelliği devre dışı bırakılarak kaldırılabilir.  Sisteme atanan kimlik, aşağıda açıklandığı gibi C#, ARM şablonları veya Azure portalı kullanılarak kaldırılır.

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

### <a name="disable-a-system-assigned-identity-using-the-azure-portal"></a>Azure portalını kullanarak sisteme atanmış bir kimliği devre dışı

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Portalın sol bölmesinde **Ayarlar** > **Kimliği'ni** seçin.
1. **Kimlik** bölmesinde > **Sistemi atanan** sekme:
    1. **Durum** kaydırıcısını **Kapat'a**taşıyın.
    1. **Kaydet**’i seçin
    1. Açılan pencerede, sistem tarafından atanan kimliği devre dışı kısımak için **Evet'i** seçin. **Kimlik** bölmesi, sistem tarafından atanan kimliğin eklenmesinden önceki yle aynı duruma geri döner.

    ![Sistem atanan kimlik kapalı](media/managed-identities/system-assigned-identity.png)

# <a name="c"></a>[C #](#tab/c-sharp)

### <a name="remove-a-system-assigned-identity-using-c"></a>C kullanarak sisteme atanmış kimliği kaldırma #

Sistem tarafından atanan kimliği kaldırmak için aşağıdakileri çalıştırın:

```csharp
var identity = new Identity(IdentityType.None);
var cluster = new Cluster(location, sku, identity: identity);
await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
```

# <a name="arm-template"></a>[ARM şablonu](#tab/arm)

### <a name="remove-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak sisteme atanmış kimliği kaldırma

Sistem tarafından atanan kimliği kaldırmak için aşağıdakileri çalıştırın:

```json
"identity": {
    "type": "None"
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da Güvenli Azure Veri Gezgini kümeleri](security.md)
* Şifrelemeyi hazır layarak [Azure Veri Gezgini - Azure portalında kümenizi güvenli](manage-cluster-security.md) hale verin.
 * [C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #](customer-managed-keys-csharp.md)
 * [Azure Kaynak Yöneticisi şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın](customer-managed-keys-resource-manager.md)
