---
title: Azure Anahtar Kasası'na erişmek için sistem tarafından atanmış yönetilen bir kimlik kullanma
description: Uygulama Hizmeti uygulamaları için yönetilen bir kimliği nasıl oluşturup Azure Anahtar Kasası'na erişmek için nasıl kullanacağınızı öğrenin
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: bb5288d043ab5638bb33c357cea55c64b03fcf1d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81432131"
---
# <a name="provide-key-vault-authentication-with-a-managed-identity"></a>Yönetilen bir kimlikle Anahtar Kasa kimlik doğrulaması sağlama

Azure Active Directory'nin yönetilen kimliği, uygulamanızın Azure AD korumalı diğer kaynaklara kolayca erişmesine olanak tanır. Kimlik Azure platformu tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. Daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md)bakın. 

Bu makalede, bir Uygulama Hizmeti uygulaması için yönetilen bir kimlik oluşturma ve Azure Anahtar Vault erişmek için kullanmak nasıl gösterir. Azure VM'lerinde barındırılan uygulamalar için bkz. [Azure Anahtar Kasası'na erişmek için Windows VM sistemine atanmış yönetilen kimlik kullanın.](../../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md)


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar 

Bu kılavuzu tamamlamak için aşağıdaki kaynaklara sahip olmalısınız. 

- Anahtar kasası. Varolan bir anahtar kasası kullanabilir veya aşağıdaki hızlı başlangıçlardan birinde aşağıdaki adımları izleyerek yeni bir tane oluşturabilirsiniz:
   - [Azure CLI ile önemli bir kasa oluşturun](../secrets/quick-create-cli.md)
   - [Azure PowerShell ile önemli bir kasa oluşturun](../secrets/quick-create-powershell.md)
   - [Azure portalı ile önemli bir kasa oluşturun.](../secrets/quick-create-portal.md)
- Anahtar kasaerişimi sağlamak için mevcut bir Uygulama Hizmeti uygulaması. [App Service belgelerindeki](../../app-service/overview.md)adımları izleyerek hızlı bir şekilde bir tane oluşturabilirsiniz.
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview). Alternatif olarak, Azure [portalını](https://portal.azure.com)kullanabilirsiniz.


## <a name="adding-a-system-assigned-identity"></a>Sisteme atanmış kimlik ekleme 

İlk olarak, bir uygulamaya sistem tarafından atanmış bir kimlik eklemeniz gerekir. 
 
### <a name="azure-portal"></a>Azure portal 

Portalda yönetilen bir kimlik ayarlamak için, önce normal bir uygulama oluşturacak ve ardından özelliği etkinleştireceksiniz. 

1. Bir işlev uygulaması kullanıyorsanız, **Platform özelliklerine**gidin. Diğer uygulama türleri için, sol navigasyondaki **Ayarlar** grubuna gidin. 

1. **Yönetilen kimliği**seçin. 

1. Atanan **Sistem** sekmesiiçinde, **Durum** Açık'a **On**geçin. **Kaydet**’e tıklayın. 

    ![](../media/managed-identity-system-assigned.png)

### <a name="azure-cli"></a>Azure CLI

Bu hızlı başlatma, Azure CLI sürümü 2.0.4 veya sonrası gerektirir. Geçerli sürümünüzü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest). 

Azure CLI ile oturum açmak için [az giriş](/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanın:

```azurecli-interactive
az login
```

Azure CLI ile giriş seçenekleri hakkında daha fazla bilgi için [Azure CLI ile Oturum Aç'a](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)bakın. 

Bu uygulamaiçin kimlik oluşturmak için Azure CLI [az webapp identity atsign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) komutunu veya [az işlevli kimlik atama](/cli/azure/functionapp/identity?view=azure-cli-latest#az-functionapp-identity-assign) komutunu kullanın:


```azurecli-interactive
az webapp identity assign --name myApp --resource-group myResourceGroup
```

```azurecli-interactive
az functionapp identity assign --name myApp --resource-group myResourceGroup
```

Bir sonraki bölümde `PrincipalId`gerekli olacak , bir not alın.

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```
## <a name="grant-your-app-access-to-key-vault"></a>Uygulamanızın Key Vault'a erişimini zedeleyin 

### <a name="azure-portal"></a>Azure portal

1.  Key Vault kaynağına gidin. 

1.  **Access ilkelerini** seçin ve **Erişim İlkesi Ekle'yi**tıklatın. 

1.  **Gizli izinlerde,** **Al, Liste'yi**seçin. 

1.  **Müdür Seç'i**seçin ve arama alanına uygulamanın adını girin.  Sonuç listesindeuygulamayı seçin ve **Seç'i**tıklatın. 

1.  Yeni erişim ilkesini eklemeyi bitirmek için **Ekle'yi** tıklatın.

    ![](../media/managed-identity-access-policy.png)

### <a name="azure-cli"></a>Azure CLI

Başvurunuza anahtar kasanıza erişim izni vermek için Azure CLI [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu kullanarak **ObjectId** parametresini yukarıda belirttiğiniz **principalId** ile birlikte sağlayabilir.

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <PrincipalId> --secret-permissions get list 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: Kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)
- [Erişim denetimi ilkesiyle Key Vault kimlik doğrulaması sağlayın](group-permissions-for-apps.md)
- [Anahtar kasanızı emniyete alanın).](secure-your-key-vault.md)
- [Azure Key Vault geliştirici kılavuzu](developers-guide.md)
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](best-practices.md)