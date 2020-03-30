---
title: Azure Uygulama Yapılandırması ile yönetilen kimlikleri yapılandırma
description: Azure Uygulama Yapılandırmasında yönetilen kimliklerin nasıl çalıştığını ve yönetilen bir kimliği nasıl yapılandırılabildiğini öğrenin
author: jpconnock
ms.topic: article
ms.date: 02/25/2020
ms.author: jeconnoc
ms.reviewer: lcozzens
ms.service: azure-app-configuration
ms.openlocfilehash: fe66466395a100221e6a3cdebdef870bdf195afc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623039"
---
# <a name="how-to-use-managed-identities-for-azure-app-configuration"></a>Azure Uygulama Yapılandırması için yönetilen kimlikler nasıl kullanılır?

Bu konu, Azure Uygulama Yapılandırması için yönetilen bir kimliğin nasıl oluşturulabildiğini gösterir. Azure Active Directory 'nin (AAD) yönetilen kimliği, Azure Uygulama Yapılandırması'nın Azure Key Vault gibi AAD korumalı diğer kaynaklara kolayca erişmesine olanak tanır. Kimlik, Azure platformu tarafından yönetilir. Herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. AAD'de yönetilen kimlikler hakkında daha fazla şey için [Azure kaynakları için Yönetilen kimlikler'e](../active-directory/managed-identities-azure-resources/overview.md)bakın.

Başvurunuza iki tür kimlik verilebilir:

- **Sistem tarafından atanan kimlik,** yapılandırma mağazanıza bağlıdır. Yapılandırma mağazanız silinirse silinir. Bir yapılandırma deposunda yalnızca bir sistem atanmış kimlik olabilir.
- **Kullanıcı tarafından atanan kimlik,** yapılandırma mağazanıza atanabilecek bağımsız bir Azure kaynağıdır. Yapılandırma deposunda birden çok kullanıcı tarafından atanmış kimlik olabilir.

## <a name="adding-a-system-assigned-identity"></a>Sisteme atanmış kimlik ekleme

Sistem tarafından atanmış bir kimliğe sahip bir Uygulama Yapılandırma deposu oluşturmak için mağazada ayarlanacak ek bir özellik gerekir.

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI'yi kullanarak yönetilen bir kimlik oluşturmak için, varolan bir yapılandırma deposuna karşı [az appconfig identity atama] komutunu kullanın. Bu bölümdeki örnekleri çalıştırmak için üç seçeneğiniz var:

- Azure portalından [Azure Bulut Kabuğu'nu](../cloud-shell/overview.md) kullanın.
- Aşağıdaki her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
- Yerel bir CLI konsolu kullanmak isterseniz [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.1 veya sonraki) en son sürümünü yükleyin.

Aşağıdaki adımlar, bir Uygulama Yapılandırma sıcağı oluşturmave CLI'yi kullanarak bir kimlik atamanız için size yol açacaktır:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login] kullanarak Azure'da oturum açın. Azure aboneliğinizle ilişkili bir hesap kullanın:

    ```azurecli-interactive
    az login
    ```

1. CLI'yi kullanarak bir Uygulama Yapılandırma sı oluşturun. Azure Uygulama Yapılandırması ile CLI'nin nasıl kullanılacağına daha fazla örnek için [Bkz. Uygulama Yapılandırması CLI örnekleri:](scripts/cli-create-service.md)

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    az appconfig create --name myTestAppConfigStore --location eastus --resource-group myResourceGroup --sku Free
    ```

1. Bu yapılandırma deposu için sistem tarafından atanan kimlik oluşturmak için [az appconfig identity atama] komutunu çalıştırın:

    ```azurecli-interactive
    az appconfig identity assign --name myTestAppConfigStore --resource-group myResourceGroup
    ```

## <a name="adding-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik ekleme

Kullanıcı tarafından atanan bir kimliğe sahip bir Uygulama Yapılandırma sıcağı oluşturmak, kimliği oluşturmanızı ve ardından kaynak tanımlayıcısını mağazanıza atamanızı gerektirir.

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI'yi kullanarak yönetilen bir kimlik oluşturmak için, varolan bir yapılandırma deposuna karşı [az appconfig identity atama] komutunu kullanın. Bu bölümdeki örnekleri çalıştırmak için üç seçeneğiniz var:

- Azure portalından [Azure Bulut Kabuğu'nu](../cloud-shell/overview.md) kullanın.
- Aşağıdaki her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
- Yerel bir CLI konsolu kullanmak isterseniz [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 veya sonraki) en son sürümünü yükleyin.

Aşağıdaki adımlar, kullanıcı tarafından atanan bir kimlik ve Bir Uygulama Yapılandırma sıcağı oluşturmada ve ardından cli'yi kullanarak mağazaya kimlik atamanız için size yol açacaktır:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login] kullanarak Azure'da oturum açın. Azure aboneliğinizle ilişkili bir hesap kullanın:

    ```azurecli-interactive
    az login
    ```

1. CLI'yi kullanarak bir Uygulama Yapılandırma sı oluşturun. Azure Uygulama Yapılandırması ile CLI'nin nasıl kullanılacağına daha fazla örnek için [Bkz. Uygulama Yapılandırması CLI örnekleri:](scripts/cli-create-service.md)

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    az appconfig create --name myTestAppConfigStore --location eastus --resource-group myResourceGroup --sku Free
    ```

1. CLI kullanarak çağrılan `myUserAssignedIdentity` kullanıcı tarafından atanmış bir kimlik oluşturun.

    ```azurecli-interactive
    az identity create -resource-group myResourceGroup --name myUserAssignedIdentity
    ```

    Bu komutun çıktısında, özelliğin `id` değerini not edin.

1. Bu yapılandırma deposuna yeni kullanıcı tarafından atanan kimlik atamak için [az appconfig identity atama] komutunu çalıştırın. Önceki adımda `id` belirttiğiniz özelliğin değerini kullanın.

    ```azurecli-interactive
    az appconfig identity assign --name myTestAppConfigStore --resource-group myResourceGroup --identities /subscriptions/[subscription id]/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myUserAssignedIdentity
    ```

## <a name="removing-an-identity"></a>Kimliği kaldırma

Sistem tarafından atanan kimlik, Azure CLI'deki az [appconfig identity remove](/cli/azure/appconfig/identity?view=azure-cli-latest#az-appconfig-identity-remove) komutunu kullanarak özelliği devre dışı bırakarak kaldırılabilir. Kullanıcı tarafından atanan kimlikler tek tek kaldırılabilir. Bu şekilde sistemle atanan bir kimliğin kaldırılması da aad onu siler. Sistem tarafından atanan kimlikler, uygulama kaynağı silindiğinde aad'den otomatik olarak kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Uygulama Yapılandırması ile ASP.NET Core uygulaması oluşturun](quickstart-aspnet-core-app.md)

[az appconfig kimlik atamak]: /cli/azure/appconfig/identity?view=azure-cli-latest#az-appconfig-identity-assign
[az login]: /cli/azure/reference-index#az-login
