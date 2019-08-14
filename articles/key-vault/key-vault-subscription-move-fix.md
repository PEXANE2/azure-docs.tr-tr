---
title: Abonelik taşıma-Azure Key Vault sonrasında Anahtar Kasası kiracı KIMLIĞINI değiştirme | Microsoft Docs
description: Abonelik farklı bir kiracıya taşındıktan sonra anahtar kasasına ilişkin kiracı kimliğini nasıl değiştireceğinizi öğrenin
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990115"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>S: Aboneliğim A kiracısından B kiracısına taşındı. Mevcut anahtar kasama ilişkin kiracı kimliğini nasıl değiştirebilir ve B kiracısındaki sorumlular için doğru ACL'leri nasıl belirleyebilirim?

Abonelikte yeni bir anahtar kasası oluşturduğunuzda, kasa bu abonelik için varsayılan Azure Active Directory kiracı kimliğine otomatik olarak bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez hale gelir. Bu sorunu düzeltmek için şunları yapmanız gerekir:

* Bu abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı kimliklerini B kiracısı olarak değiştirin.
* Mevcut tüm erişim ilkesi girdilerini kaldırın.
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleyin.

Örneğin, bir abonelikte A kiracısından B kiracısına taşınan 'myvault' adlı bir anahtar kasanız varsa bu anahtar kasası için kiracı kimliğini nasıl değiştireceğiniz ve eski erişim ilkelerini nasıl kaldıracağınız aşağıda gösterilmiştir.

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

## <a name="next-steps"></a>Sonraki adımlar

Azure Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
