---
title: Güvenli Azure AD Etki Alanı Hizmetleri | Microsoft Dokümanlar
description: Azure Active Directory Domain Services yönetilen etki alanı için zayıf şifreleri, eski protokolleri ve NTLM şifre karma eşitlemeyi nasıl devre dışı kakabileceğinizi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 581963c94129c36acbd8761d93e369281797fa9f
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654714"
---
# <a name="disable-weak-ciphers-and-password-hash-synchronization-to-secure-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanını güvenli hale getirmek için zayıf şifrelemeleri ve parola karma eşitlemeyi devre dışı

Varsayılan olarak, Azure Active Directory Domain Services (Azure AD DS), NTLM v1 ve TLS v1 gibi şifrelerin kullanılmasını sağlar. Bu şifrelemeler bazı eski uygulamalar için gerekli olabilir, ancak zayıf olarak kabul edilir ve bunlara ihtiyacınız yoksa devre dışı bırakılabilir. Azure AD Connect'i kullanarak şirket içi karma bağlantınız varsa, NTLM parola karmalarının eşitlemesini de devre dışı kullanabilirsiniz.

Bu makalede, NTLM v1 ve TLS v1 şifrelerini nasıl devre dışı bırakıp NTLM şifre karma senkronizasyonunu nasıl devre dışı kabileceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]
* Azure PowerShell'i yükleyip yapılandırın.
    * Gerekirse, [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak için](/powershell/azure/install-az-ps)yönergeleri izleyin.
    * [Connect-AzAccount][Connect-AzAccount] cmdlet'i kullanarak Azure aboneliğinizde oturum açtığınızdan emin olun.
* Azure AD PowerShell'i yükleyin ve yapılandırın.
    * Gerekirse, Azure AD [PowerShell modülünü yüklemek ve Azure AD'ye bağlanmak için](/powershell/azure/active-directory/install-adv2)yönergeleri izleyin.
    * [Connect-AzureAD][Connect-AzureAD] cmdlet'ini kullanarak Azure AD kiracınızda oturum açtığınızdan emin olun.

## <a name="disable-weak-ciphers-and-ntlm-password-hash-sync"></a>Zayıf şifreleri ve NTLM şifre karma eşitlemeyi devre dışı

Zayıf şifreleme paketlerini ve NTLM kimlik bilgisi karma eşitlemasını devre dışı bırakmak için Azure hesabınızda oturum açın ve [ardından Get-AzResource][Get-AzResource] cmdlet'ini kullanarak Azure AD DS kaynağını alın:

> [!TIP]
> *Microsoft.AAD/DomainServices* kaynağının bulunmadığı [Get-AzResource][Get-AzResource] komutunu kullanarak bir hata alırsanız, [tüm Azure aboneliklerini ve yönetim gruplarını yönetmek için erişiminizi yükseltin.][global-admin]

```powershell
Login-AzAccount

$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"
```

Ardından, aşağıdaki güvenlik seçeneklerini yapılandırmak için *DomainSecuritySettings'i* tanımlayın:

1. NTLM v1 desteğini devre dışı kındırın.
2. Şirket içi AD'nizden NTLM parola işlerinin senkronizasyonunu devre dışı edin.
3. TLS v1'i devre dışı.

> [!IMPORTANT]
> Azure AD DS yönetilen etki alanında NTLM şifre karma eşitlemesini devre dışı biçerseniz, kullanıcılar ve hizmet hesapları LDAP basit bağlamalarını gerçekleştiremez. LDAP basit bağlamalarını gerçekleştirmeniz gerekiyorsa, aşağıdaki komutta *"SyncNtlmPasswords"="Devre dışı"* seçeneğini ayarlamayın;

```powershell
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}
```

Son olarak, Tanımlanan güvenlik ayarlarını [Set-AzResource][Set-AzResource] cmdlet'ini kullanarak Azure AD DS yönetilen etki alanına uygulayın. İlk adımdaki Azure AD DS kaynağını ve önceki adımdaki güvenlik ayarlarını belirtin.

```powershell
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

Güvenlik ayarlarının Azure AD DS yönetilen etki alanına uygulanması birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Eşitleme işlemi hakkında daha fazla bilgi edinmek için, [nesnelerin ve kimlik bilgilerinin Azure AD DS yönetilen etki alanında nasıl eşitlendirildiğini][synchronization]görün.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[global-admin]: ../role-based-access-control/elevate-access-global-admin.md
[synchronization]: synchronization.md

<!-- EXTERNAL LINKS -->
[Get-AzResource]: /powershell/module/az.resources/Get-AzResource
[Set-AzResource]: /powershell/module/Az.Resources/Set-AzResource
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD