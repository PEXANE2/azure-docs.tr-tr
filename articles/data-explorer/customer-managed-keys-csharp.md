---
title: Müşteri tarafından yönetilen anahtarları kullanarak yapılandırmaC#
description: Bu makalede, Azure Veri Gezgini 'de verileriniz üzerinde müşteri tarafından yönetilen anahtarlar şifrelemesini yapılandırma açıklanmaktadır.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 16c108790dd696e98a1264691254c9e99dac6cd3
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76280620"
---
# <a name="configure-customer-managed-keys-using-c"></a>Müşteri tarafından yönetilen anahtarları kullanarak yapılandırmaC#

> [!div class="op_single_selector"]
> * [C#](customer-managed-keys-csharp.md)
> * [Azure Resource Manager şablonu](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Bu bölümde, Azure Veri Gezgini C# istemcisini kullanarak müşteri tarafından yönetilen anahtarlar şifrelemesini yapılandırma hakkında yönergeler verilmektedir. 

### <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilse, **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

### <a name="install-c-nuget"></a>NuGet C# 'i yükler

* [Azure Veri Gezgini (kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükler.

* Kimlik doğrulaması için [Microsoft. IdentityModel. clients. ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükler.

### <a name="authentication"></a>Kimlik Doğrulaması

Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen [bir Azure AD uygulaması](/azure/active-directory/develop/howto-create-service-principal-portal) ve hizmet sorumlusu oluşturun. Abonelik kapsamına rol ataması ekleyebilir ve gerekli `Directory (tenant) ID`, `Application ID`ve `Client Secret`alabilirsiniz.

### <a name="configure-cluster"></a>Küme yapılandırma

Azure Veri Gezgini şifrelemesi, varsayılan olarak Microsoft tarafından yönetilen anahtarları kullanır. Azure Veri Gezgini kümenizi, müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve kümeyle ilişkilendirilecek anahtarı belirtin.

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

1. Kümenizin başarıyla güncelleştirilip güncelleştirilmediğini denetlemek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    Sonuç `Succeeded` değerine sahip `ProvisioningState` içeriyorsa, kümeniz başarıyla güncelleştirildi.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için kümeyi güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için `Get-AzKeyVaultKey` çağırın. Ardından kümenin Anahtar Kasası özelliklerini, [küme yapılandırma](#configure-cluster)bölümünde gösterildiği gibi anahtarın yeni sürümünü kullanacak şekilde güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da Azure Veri Gezgini kümelerini güvenli hale getirme](security.md)
* [Azure Veri Gezgini kümeniz için Yönetilen kimlikler yapılandırma](managed-identities.md)
* Rest 'de şifrelemeyi etkinleştirerek [Azure Veri Gezgini Azure Portal kümenizin güvenliğini sağlayın](manage-cluster-security.md) .
* [Azure Resource Manager şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırma](customer-managed-keys-resource-manager.md)


