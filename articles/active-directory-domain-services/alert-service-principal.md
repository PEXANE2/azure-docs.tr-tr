---
title: Azure AD Etki Alanı Hizmetleri'ndeki hizmet temel uyarılarını çözümle | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri için hizmet temel yapılandırma uyarılarını nasıl gidereceklerini öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/20/2019
ms.author: iainfou
ms.openlocfilehash: 175bfe63176b78c5aeafc7147c46dd5ab1110325
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71257956"
---
# <a name="known-issues-service-principal-alerts-in-azure-active-directory-domain-services"></a>Bilinen sorunlar: Azure Active Directory Etki Alanı Hizmetlerinde hizmet temel uyarıları

[Hizmet ilkeleri,](../active-directory/develop/app-objects-and-service-principals.md) Azure platformunun Azure AD DS yönetilen bir etki alanını yönetmek, güncelleştirmek ve korumak için kullandığı uygulamalardır. Bir hizmet sorumlusu silinirse, Azure AD DS yönetilen etki alanındaişlevsellik etkilenir.

Bu makale, hizmet ilkesiyle ilgili yapılandırma uyarılarını sorun gidermenize ve çözümlemenize yardımcı olur.

## <a name="alert-aadds102-service-principal-not-found"></a>Uyarı AADDS102: Servis sorumlusu bulunamadı

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetlerinin düzgün çalışması için gereken bir Hizmet Yöneticisi, Azure REKLAM dizininizden silindi. Bu yapılandırma, Microsoft'un yönetilen etki alanınızı izleme, yönetme, düzeltme ve eşitleme yeteneğini etkiler.*

Gerekli bir hizmet sorumlusu silinirse, Azure platformu otomatik yönetim görevlerini gerçekleştiremez. Azure AD DS yönetilen etki alanı güncelleştirmeleri doğru şekilde uygulayamaz veya yedekleme alamaz.

### <a name="check-for-missing-service-principals"></a>Eksik servis ilkelerini denetleme

Hangi hizmet müdürünün eksik olduğunu ve yeniden oluşturulması gerektiğini denetlemek için aşağıdaki adımları tamamlayın:

1. Azure portalında, sol daki gezinme menüsünden **Azure Active Directory'yi** seçin.
1. **Kurumsal uygulamaları**seçin. Uygulama Türü açılır menüsünden *tüm uygulamaları* seçin ve ardından **Uygula'yı**seçin. **Application Type**
1. Uygulama adlarının her birini arayın. Varolan bir uygulama bulunamazsa, hizmet ilkesini oluşturmak veya ad alanını yeniden kaydetmek için *Çözüm* adımlarını izleyin.

    | Uygulama Kimliği | Çözüm |
    | :--- | :--- |
    | 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Eksik servis ilkesini yeniden oluşturma](#recreate-a-missing-service-principal) |
    | 443155a6-77f3-45e3-882b-22b3a8d431fb | [Microsoft.AAD ad alanını yeniden kaydedin](#re-register-the-microsoft-aad-namespace) |
    | abba844e-bc0e-44b0-947a-dc74e5d09022 | [Microsoft.AAD ad alanını yeniden kaydedin](#re-register-the-microsoft-aad-namespace) |
    | d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Microsoft.AAD ad alanını yeniden kaydedin](#re-register-the-microsoft-aad-namespace) |

### <a name="recreate-a-missing-service-principal"></a>Eksik Servis Müdürü'ni yeniden oluşturma

Uygulama kimliği *2565bd9d-da50-47d4-8b85-4c97f669dc36* Azure REKLAM dizininizde eksikse, aşağıdaki adımları tamamlamak için Azure AD PowerShell'i kullanın. Daha fazla bilgi için [Azure AD PowerShell'i yükleyin.](/powershell/azure/active-directory/install-adv2)

1. Azure AD PowerShell modüllerini yükleyin ve aşağıdaki gibi aktarın:

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

1. Şimdi [Yeni AzureAdServicePrincipal][New-AzureAdServicePrincipal] cmdlet kullanarak hizmet ilkesini yeniden oluşturun:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

### <a name="re-register-the-microsoft-aad-namespace"></a>Microsoft AAD ad alanını yeniden kaydetme

Eğer uygulama kimliği *443155a6-77f3-45e3-882b-22b3a8d431fb*, *abba844e-bc0e-44b0-947 a-dc74e5d09022*veya *d87dcbc6-a371-462e-88e3-28ad15ec4e64* Azure AD dizininizde eksik, *Microsoft.AAD* kaynak sağlayıcısını yeniden kaydetmek için aşağıdaki adımları tamamlayın:

1. Azure portalında **Abonelikleri**arayın ve seçin.
1. Azure AD DS yönetilen etki alanınızın ilişkili aboneliğini seçin.
1. Sol daki gezinmeden **Kaynak Sağlayıcıları'nı**seçin.
1. *Microsoft.AAD'yi*arayın, ardından **Yeniden kaydol'u**seçin.

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Uyarı AADDS105: Şifre senkronizasyon uygulaması güncel değil

### <a name="alert-message"></a>Uyarı iletisi

*"d87dcbc6-a371-462e-88e3-28ad15ec4e64" uygulama kimliği ile servis sorumlusu silinmiş ve yeniden oluşturulmuştur. Rekreasyon, yönetilen etki alanınıza hizmet vermek için gereken Azure AD Etki Alanı Hizmetleri kaynaklarında tutarsız izinler bırakır. Yönetilen etki alanınızdaki parolaların eşitlenmesi etkilenebilir.*

Azure AD DS, Azure AD'deki kullanıcı hesaplarını ve kimlik bilgilerini otomatik olarak eşitler. Bu işlem için kullanılan Azure AD uygulamasıyla ilgili bir sorun varsa, Azure AD DS ile Azure AD AD arasındaki kimlik bilgisi eşitlemesi başarısız olur.

### <a name="resolution"></a>Çözüm

Kimlik bilgisi eşitleme için kullanılan Azure AD uygulamasını yeniden oluşturmak için aşağıdaki adımları tamamlamak için Azure AD PowerShell'i kullanın. Daha fazla bilgi için [Azure AD PowerShell'i yükleyin.](/powershell/azure/active-directory/install-adv2)

1. Azure AD PowerShell modüllerini yükleyin ve aşağıdaki gibi aktarın:

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Şimdi aşağıdaki PowerShell cmdlets kullanarak eski uygulama ve nesne silin:

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```

Her iki uygulamayı da sildikten sonra, Azure platformu bunları otomatik olarak yeniden oluşturur ve parola eşitlemasını sürdürmeye çalışır. Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarınız hala varsa, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[New-AzureAdServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
