---
title: Azure Active Directory Domain Services yönetilen etki alanınızı güvenli hale getirin | Microsoft Docs
description: Yönetilen etki alanınızın güvenliğini sağlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: 923ecae9dc649b8f5cdcfd447b78fdec0805927a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879160"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanınızı güvenli hale getirin
Bu makale, yönetilen etki alanınızı güvenli hale getirmeye yardımcı olur. Zayıf şifre paketlerinin kullanımını kapatabilir ve NTLM kimlik bilgisi karma eşitlemesini devre dışı bırakabilirsiniz.

## <a name="install-the-required-powershell-modules"></a>Gerekli PowerShell modüllerini yükler

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell 'i yükleyip yapılandırma
[Azure AD PowerShell modülünü yüklemek ve Azure AD 'ye bağlanmak](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json)için makalesindeki yönergeleri izleyin.

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
[Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json)için makalesindeki yönergeleri izleyin.


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Zayıf şifre paketlerini ve NTLM kimlik bilgisi karma eşitlemesini devre dışı bırak
İçin aşağıdaki PowerShell betiğini kullanın:

1. Yönetilen etki alanında NTLM v1 desteğini devre dışı bırakın.
2. Şirket içi AD 'nizden NTLM parola karmalarının eşitlemesini devre dışı bırakın.
3. Yönetilen etki alanında TLS v1’i devre dışı bırakın.

*Microsoft. AAD/DomainServices* kaynağının `Get-AzResource` mevcut olmadığı komutla ilgili bir hata alırsanız, [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişiminizi](../role-based-access-control/elevate-access-global-admin.md)yükseltin.

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// 1. Disable NTLM v1 support on the managed domain.
// 2. Disable the synchronization of NTLM password hashes from
//    on-premises AD to Azure AD and Azure AD Domain Services
// 3. Disable TLS v1 on the managed domain.
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}

// Apply the settings to the managed domain.
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

> [!IMPORTANT]
> Azure AD Domain Services örneğiniz üzerinde NTLM parola karması eşitlemesini devre dışı bırakmış kullanıcılar (ve hizmet hesapları) LDAP basit bağlamalar gerçekleştiremez.  NTLM parola karma eşitlemesini devre dışı bırakma hakkında daha fazla bilgi için [Azure AD etki alanı Hizmetleri tarafından yönetilen etki alanınızı](secure-your-domain.md)okuyun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Domain Services eşitlemeyi anlama](synchronization.md)
