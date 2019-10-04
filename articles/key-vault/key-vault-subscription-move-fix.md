---
title: Abonelik taşıma-Azure Key Vault sonrasında Anahtar Kasası kiracı KIMLIĞINI değiştirme | Microsoft Docs
description: Abonelik farklı bir kiracıya taşındıktan sonra Anahtar Kasası için kiracı KIMLIĞINI nasıl değiştireceğinizi öğrenin
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 87025767725142cc2f861ff8b390d6ea916f8e38
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71947723"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Abonelik taşıdıktan sonra Anahtar Kasası kiracı KIMLIĞINI değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>S: aboneliğim A kiracısından B kiracısına taşındı. Nasıl yaparım? var olan anahtar kasam için kiracı KIMLIĞINI değiştirin ve B kiracısındaki sorumlular için doğru ACL 'Leri ayarlayın mi?

Bir abonelikte yeni bir Anahtar Kasası oluşturduğunuzda, bu abonelik için otomatik olarak varsayılan Azure Active Directory kiracı KIMLIĞINE bağlanır. Tüm erişim ilkesi girdileri de bu kiracı KIMLIĞINE bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez. Bu sorunu onarmak için şunları yapmanız gerekir:

* Bu abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı KIMLIĞINI B kiracısı ile değiştirin.
* Var olan tüm erişim ilkesi girdilerini kaldırın.
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleyin.

Örneğin, A kiracısından B kiracısına taşınan bir abonelikte ' mykasa' Anahtar Kasası varsa, bu Anahtar Kasası için kiracı KIMLIĞINI değiştirme ve eski erişim ilkelerini kaldırma işlemlerinin nasıl yapılacağı aşağıda verilmiştir.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Bu kasa taşımadan önce kiracıda olduğundan, $vault özgün değeri **. Properties. Tenantıd** kiracı A, while **(Get-azcontext). Tenant. Tenantıd** , B kiracısıdır.

Artık kasanız doğru kiracı KIMLIĞIYLE ilişkilendirildiğinden ve eski erişim ilkesi girdileri kaldırıldığına göre, [set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy)ile yeni erişim ilkesi girdileri ayarlayın.

MSI kullanıyorsanız, eski kimlik artık doğru AAD kiracısında yer kalmadığında MSI kimliğini de güncelleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Key Vault hakkında sorularınız varsa [Azure Key Vault forumlarını](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)ziyaret edin.
