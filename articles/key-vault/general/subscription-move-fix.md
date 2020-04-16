---
title: Abonelik hareketinden sonra anahtar kasa kiracı kimliğini değiştirme - Azure Key Vault | Microsoft Dokümanlar
description: Abonelik farklı bir kiracıya taşındıktan sonra anahtar kasasına ilişkin kiracı kimliğini nasıl değiştireceğinizi öğrenin
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 94bcba80e5768d57e3dc97bed1a74a8369ac60b9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422896"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


Abonelikte yeni bir anahtar kasası oluşturduğunuzda, kasa bu abonelik için varsayılan Azure Active Directory kiracı kimliğine otomatik olarak bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. 

Azure aboneliğinizi kiracı A'dan kiracı B'ye taşırsanız, varolan anahtar kasalarınıza kiracı B'deki müdürler (kullanıcılar ve uygulamalar) tarafından erişilemez. Bu sorunu gidermek için şunları yapmanız gerekir:

* Abonelikteki tüm anahtar kasalarıyla ilişkili kiracı kimliğini kiracı B ile değiştirin.
* Mevcut tüm erişim ilkesi girdilerini kaldırın.
* Kiracı B ile ilişkili yeni erişim ilkesi girişleri ekleyin.

Örneğin, kiracı A'dan kiracı B'ye taşınmış bir abonelikte 'myvault' anahtar kasanız varsa, kiracı kimliğini değiştirmek ve eski erişim ilkelerini kaldırmak için Azure PowerShell'i kullanabilirsiniz.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.
````

Veya Azure CLI'yi kullanabilirsiniz.

```azurecli
az account set -s <your-subscriptionId>                                    # Select your Azure Subscription
tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Kasanız doğru kiracı kimliğiyle ilişkilendirildiğine ve eski erişim ilkesi girdileri kaldırıldığına gününden, Azure PowerShell [Set- AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) cmdlet veya Azure CLI [az keyvault set- ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuyla yeni erişim ilkesi girdileri ayarlayın.

Azure kaynakları için yönetilen bir kimlik kullanıyorsanız, bu kimliği yeni Azure AD kiracısına da güncelleştirmeniz gerekir. Yönetilen kimlikler hakkında daha fazla bilgi için [bkz.](managed-identity.md)


MSI kullanıyorsanız, eski kimlik artık doğru AAD kiracısında olmayacağından, MSI kimliğini de güncelleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
