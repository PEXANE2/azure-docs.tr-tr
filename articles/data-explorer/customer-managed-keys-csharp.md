---
title: 'C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #'
description: Bu makalede, Azure Veri Gezgini'nde verilerinizde müşteri tarafından yönetilen anahtar şifrelemesi nasıl yapılandırılabildiğini açıklanmaktadır.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: a00b0876c4a188b932032129ed5a394e94198930
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297955"
---
# <a name="configure-customer-managed-keys-using-c"></a>C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #

> [!div class="op_single_selector"]
> * [Portal](customer-managed-keys-portal.md)
> * [C #](customer-managed-keys-csharp.md)
> * [Azure Resource Manager şablonu](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

[!INCLUDE [data-explorer-configure-customer-managed-keys part 2](../../includes/data-explorer-configure-customer-managed-keys-b.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Bu bölümde, Azure Veri Gezgini C# istemcisini kullanarak müşteri tarafından yönetilen anahtarşifrelemeyi nasıl yapılandırabileceğiniz gösterilmektedir. 

### <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

### <a name="install-c-nuget"></a>C# NuGet yükle

* Azure [Veri Gezgini (Kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükleyin.

* Kimlik doğrulaması için [Microsoft.IdentityModel.Clients.ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükleyin.

### <a name="authentication"></a>Kimlik doğrulaması

Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi [oluşturun.](/azure/active-directory/develop/howto-create-service-principal-portal) Abonelik kapsamında rol ataması ekleyebilir ve `Directory (tenant) ID` `Application ID`gerekli `Client Secret`, , ve .

### <a name="configure-cluster"></a>Kümeyi yapılandırma

Varsayılan olarak, Azure Veri Gezgini şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Azure Veri Gezgini kümenizi müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve kümeyle ilişkilendirecek anahtarı belirtin.

1. Aşağıdaki kodu kullanarak kümenizi güncelleştirin:

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
    var keyName = "myKey";
    var keyVersion = "5b52b20e8d8a42e6bd7527211ae32654";
    var keyVaultUri = "https://mykeyvault.vault.azure.net/";
    var keyVaultProperties = new KeyVaultProperties (keyName, keyVersion, keyVaultUri);
    var clusterUpdate = new ClusterUpdate(keyVaultProperties: keyVaultProperties);
    await kustoManagementClient.Clusters.UpdateAsync(resourceGroupName, clusterName, clusterUpdate);
    ```

1. Kümenizin başarıyla güncelleştirilip güncelleştirilemeip güncellenmedisini kontrol etmek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Sonuç değeri `ProvisioningState` içeriyorsa, `Succeeded` kümeniz başarıyla güncelleştirildi.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için kümeyi güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için arayın. `Get-AzKeyVaultKey` Daha [sonra, Yapılandırma kümesinde](#configure-cluster)gösterildiği gibi anahtarın yeni sürümünü kullanmak için kümenin anahtar kasa özelliklerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da Güvenli Azure Veri Gezgini kümeleri](security.md)
* [Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırma](managed-identities.md)
* Şifrelemeyi hazır layarak [Azure Veri Gezgini - Azure portalında kümenizi güvenli](manage-cluster-security.md) hale verin.
* [Azure Kaynak Yöneticisi şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın](customer-managed-keys-resource-manager.md)


