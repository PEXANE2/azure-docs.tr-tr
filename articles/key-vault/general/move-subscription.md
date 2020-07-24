---
title: Kasayı farklı bir aboneliğe taşımak Azure Key Vault | Microsoft Docs
description: Anahtar kasasını farklı bir aboneliğe taşıma Kılavuzu.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another subscription.
ms.openlocfilehash: b95cecdc5470e50830371b7a6556f9526dae7591
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100534"
---
# <a name="moving-an-azure-key-vault-to-another-subscription"></a>Azure Key Vault başka bir aboneliğe taşıma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> **Anahtar kasasının başka bir aboneliğe taşınması ortamınızda bir değişikliğe neden olur.**
> Bu değişikliğin etkisini anladığınızdan emin olun ve anahtar kasasını yeni bir aboneliğe taşımaya karar vermeden önce bu makaledeki yönergeleri dikkatle izleyin.
> Yönetilen hizmet kimlikleri (MSI) kullanıyorsanız lütfen belgenin sonundaki taşıma sonrası yönergeleri okuyun. 

Bir Anahtar Kasası oluşturduğunuzda, otomatik olarak oluşturulduğu aboneliğin varsayılan Azure Active Directory kiracı KIMLIĞINE bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki hizmet sorumluları (kullanıcılar ve uygulamalar) tarafından erişilemez olur. Bu sorunu onarmak için şunları yapmanız gerekir:

* Abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı KIMLIĞINI B kiracısı ile değiştirin.
* Mevcut tüm erişim ilkesi girdilerini kaldırın.
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleme.

## <a name="limitations"></a>Sınırlamalar

Bazı hizmet sorumluları (kullanıcılar ve uygulamalar) belirli bir kiracıya bağlanır. Anahtar kasanızı başka bir Kiracıdaki bir aboneliğe taşırsanız, belirli bir hizmet sorumlusuna erişimi geri yükleyemeyeceksiniz. Anahtar kasanızı taşıdığınız kiracıda tüm gerekli hizmet sorumlularının mevcut olduğundan emin olun.

## <a name="design-considerations"></a>Tasarım konusunda dikkat edilmesi gerekenler

Kuruluşunuz, abonelik düzeyinde uygulama veya dışlamaları olan Azure Ilkesi uygulamış olabilir. Abonelikte, anahtar kasanızın Şu anda bulunduğu ve anahtar kasanızı taşıdığınız aboneliğin farklı bir ilke atamaları kümesi olabilir. İlke gereksinimlerinde bir çakışma, uygulamalarınızı bozma potansiyelini içerir.

### <a name="example"></a>Örnek

İki yıl geçerli olan sertifikaları oluşturan anahtar kasasına bağlı bir uygulamanız var. Anahtar kasanızı taşımaya çalıştığınız abonelik, bir yıldan daha uzun bir süre geçerli olan sertifikaların oluşturulmasını engelleyen bir ilke atamasına sahip. Anahtar kasanızı yeni aboneliğe taşıdıktan sonra, iki yıl için geçerli olan bir sertifika oluşturma işlemi bir Azure ilke ataması tarafından engellenir.

### <a name="solution"></a>Çözüm

Azure portal Azure Ilke sayfasına gitdiğinizden emin olun ve geçerli aboneliğiniz için ilke atamalarından ve taşıdığınız aboneliğin yanı sıra uyuşmazlıkların bulunmadığından emin olun.

## <a name="prerequisites"></a>Önkoşullar

* Katılımcı düzeyinde, anahtar kasanızın bulunduğu geçerli aboneliğe erişimi veya daha yükseği.
* Katılımcı düzeyi, anahtar kasanızı taşımak istediğiniz aboneliğe erişim veya daha yüksek.
* Yeni abonelikte bir kaynak grubu.

## <a name="procedure"></a>Yordam

### <a name="initial-steps-moving-key-vault"></a>İlk adımlar (Key Vault taşınıyor)

1. Azure portalında oturum açma
2. Anahtar kasanıza gidin
3. "Genel bakış" sekmesine tıklayın
4. "Taşı" düğmesini seçin
5. Açılan menüden "başka aboneliğe geçme" seçeneğini belirleyin
6. Anahtar kasanızı taşımak istediğiniz kaynak grubunu seçin
7. Kaynakları taşıma hakkında uyarı bildirimi
8. "Tamam" ı seçin

### <a name="additional-steps-post-move"></a>Ek adımlar (taşıma sonrası)

Anahtar kasanızı yeni aboneliğe taşıdığınıza göre, kiracı KIMLIĞINI güncelleştirmeniz ve eski erişim ilkelerini kaldırmanız gerekir. PowerShell ve Azure CLı 'deki bu adımlarla ilgili öğreticiler aşağıda verilmiştir.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.

Clear-AzContext                                                            #Clear the context from PowerShell
Connect-AzAccount                                                          #Log in again to confirm you have the correct tenant id
````

```azurecli
az account set -s <your-subscriptionId>                                    # Select your Azure Subscription
tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Artık kasanız doğru kiracı KIMLIĞIYLE ilişkilendirildiğinden ve eski erişim ilkesi girdileri kaldırıldığına göre, Azure PowerShell [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) cmdlet 'ı veya Azure CLI [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu kullanarak yeni erişim ilkesi girdileri ayarlayın.

Azure kaynakları için yönetilen bir kimlik kullanıyorsanız, bunu yeni Azure AD kiracısıyla de güncelleştirmeniz gerekir. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](managed-identity.md).

MSI kullanıyorsanız, eski kimlik artık doğru AAD kiracısında yer kalmadığında MSI kimliğini de güncelleştirmeniz gerekir. Bu sorunu gidermeye yardımcı olması için aşağıdaki belgelere bakın. 

* [MSI güncelleştiriliyor](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories)
* [Aboneliği yeni dizine aktar](https://docs.microsoft.com/azure/role-based-access-control/transfer-subscription)


