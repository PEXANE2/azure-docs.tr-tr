---
title: Azure Anahtar Kasası 'na erişmek için birçok uygulamaya izin verme-Azure Key Vault | Microsoft Docs
description: Birçok uygulamaya bir anahtar kasasına erişmek için izin verme hakkında bilgi edinin
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 07ee544057ffeb0a5859cc771b124523ec79c9c0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976395"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Birçok uygulamaya bir Anahtar Kasası erişimi verme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Erişim denetimi ilkesi, çeşitli uygulamalara bir Anahtar Kasası erişimi vermek için kullanılabilir. Bir erişim denetim ilkesi en fazla 1024 uygulamayı destekleyebilir ve aşağıdaki şekilde yapılandırılır:

1. Azure Active Directory bir güvenlik grubu oluşturun. 
2. Tüm uygulamaların ilişkili hizmet sorumlularını güvenlik grubuna ekleyin.
3. Key Vault güvenlik grubuna erişim izni verin.

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar şunlardır:
* [Azure PowerShell'i yükleme](/powershell/azure/overview).
* [Azure Active Directory v2 PowerShell modülünü yükler](https://www.powershellgallery.com/packages/AzureAD).
* Azure Active Directory kiracısında Grup oluşturma/düzenleme izinleri. İzinleriniz yoksa Azure Active Directory yöneticinize başvurmanız gerekebilir. Key Vault erişim ilkesi izinleri hakkında ayrıntılar için bkz. [Azure Key Vault anahtarlar, gizlilikler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md) .

## <a name="granting-key-vault-access-to-applications"></a>Uygulamalara Key Vault erişim verme

PowerShell 'de aşağıdaki komutları çalıştırın:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Bir uygulama grubuna farklı bir izin kümesi vermeniz gerekiyorsa, bu gibi uygulamalar için ayrı bir Azure Active Directory güvenlik grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

[Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)hakkında daha fazla bilgi edinin.
