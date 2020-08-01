---
title: Azure Active Directory bir acil durumda Kullanıcı erişimini iptal etme | Microsoft Docs
description: Azure Active Directory 'de Azure AD Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleme
services: active-directory
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.topic: how-to
author: curtand
ms.author: curtand
manager: daveba
ms.reviewer: krbain
ms.date: 07/15/2020
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbb2ad8e6d37190d0473f3f9f4af7738edd3b27f
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475218"
---
# <a name="revoke-user-access-in-azure-active-directory"></a>Azure Active Directory Kullanıcı erişimini iptal etme

Bir yöneticinin bir kullanıcıya yönelik tüm erişimi iptal etmesine gerek duyduğu senaryolar arasında güvenliği aşılmış hesaplar, çalışan sonlandırması ve diğer Insider tehditleri vardır. Ortamın karmaşıklığına bağlı olarak Yöneticiler, erişimin iptal edildiğinden emin olmak için birkaç adım sürebilir. Bazı senaryolarda, erişim iptali başlatma ile erişim etkin bir şekilde iptal edildiğinde bir süre olabilir.

Riskleri azaltmak için belirteçlerin nasıl çalıştığını anlamanız gerekir. Aşağıdaki bölümlerde bahsedilen desenlerden birine giren birçok belirteç türü vardır.

## <a name="access-tokens-and-refresh-tokens"></a>Belirteçleri ve yenileme belirteçlerini erişim

Erişim belirteçleri ve yenileme belirteçleri, genellikle kalın istemci uygulamalarıyla kullanılır ve aynı zamanda tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalarda kullanılır.

- Kullanıcılar Azure AD 'de kimlik doğrulaması yaparken, kullanıcının belirli bir kaynağa erişim izni verilip verilmediğini belirlemede yetkilendirme ilkeleri değerlendirilir.  

- Yetkilendirilirse, Azure AD kaynak için bir erişim belirteci ve yenileme belirteci yayınlar.  

- Azure AD tarafından varsayılan olarak 1 saat boyunca yayınlanan erişim belirteçleri. Kimlik doğrulama protokolü izin veriyorsa, uygulama, erişim belirtecinin süresi dolduğu zaman yenileme belirtecini Azure AD 'ye geçirerek kullanıcıyı sessizce yeniden kimlik doğrulaması yapabilir.

Azure AD daha sonra yetkilendirme ilkelerini yeniden değerlendirin. Kullanıcı hala yetkilendirilirse, Azure AD yeni bir erişim belirteci yayınlar ve belirteci yeniler.

Erişim belirteçleri, genellikle bir saat içinde olan belirtecin yaşam süresinden daha kısa bir süre içinde iptal edilmeli bir güvenlik sorunu olabilir. Bu nedenle, Microsoft, Office 365 uygulamalarına [sürekli erişim değerlendirmesi](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-continuous-access-evaluation) getirmek için etkin bir şekilde çalışır ve bu da neredeyse gerçek zamanlı olarak erişim belirteçlerinin doğrulanması sağlanmasına yardımcı olur.  

## <a name="session-tokens-cookies"></a>Oturum belirteçleri (tanımlama bilgileri)

Tarayıcı tabanlı çoğu uygulama, erişim ve yenileme belirteçleri yerine oturum belirteçlerini kullanır.  

- Bir Kullanıcı bir tarayıcı açtığında ve Azure AD aracılığıyla bir uygulamanın kimliğini doğruladığında, Kullanıcı iki oturum belirteci alır. Bunlardan biri Azure AD 'den ve diğeri uygulamadan.  

- Bir uygulama kendi oturum belirtecini yaptıktan sonra, uygulamaya erişim uygulamanın oturumuna tabidir. Bu noktada, Kullanıcı yalnızca uygulamanın farkında olduğu yetkilendirme ilkelerinden etkilenir.

- Azure AD yetkilendirme ilkeleri, uygulamanın kullanıcıyı Azure AD 'ye geri göndermesi halinde yeniden değerlendirilir. Sıklık genellikle sessizce gerçekleşir, ancak sıklık uygulamanın nasıl yapılandırıldığına bağlıdır. Oturum belirteci geçerli olduğu sürece, uygulama hiçbir zaman kullanıcıyı Azure AD 'ye geri gönderemeyebilir.

- Bir oturum belirtecinin iptal edilmesi için, uygulamanın kendi yetkilendirme ilkelerine göre erişimi iptal etmeniz gerekir. Azure AD, bir uygulama tarafından verilen bir oturum belirtecini doğrudan iptal edemiyor.  

## <a name="revoke-access-for-a-user-in-the-hybrid-environment"></a>Karma ortamdaki bir kullanıcı için erişimi iptal etme

Şirket içi Active Directory Azure Active Directory ile eşitlenmiş bir karma ortamda, Microsoft, BT yöneticilerinin aşağıdaki işlemleri gerçekleştirmesini öneriyor.  

### <a name="on-premises-active-directory-environment"></a>Şirket içi Active Directory ortamı

Active Directory yönetici olarak şirket içi ağınıza bağlanın, PowerShell 'i açın ve aşağıdaki işlemleri gerçekleştirin:

1. Active Directory kullanıcıyı devre dışı bırakın. [Disable-ADAccount](https://docs.microsoft.com/powershell/module/addsadministration/disable-adaccount?view=win10-ps)öğesine bakın.

    ```PowerShell
    Disable-ADAccount -Identity johndoe  
    ```

1. Active Directory kullanıcının parolasını iki kez sıfırlayın. [Set-ADAccountPassword](https://docs.microsoft.com/powershell/module/addsadministration/set-adaccountpassword?view=win10-ps)bölümüne bakın.

    > [!NOTE]
    > Bir kullanıcının parolasını iki kez değiştirme nedeni, özellikle de şirket içi parola çoğaltmasında gecikme olması durumunda karma geçişi riskini hafifletmektir. Bu hesabı güvenli bir şekilde kabul ediyorsanız, parolayı yalnızca bir kez sıfırlayabilirsiniz.

    > [!IMPORTANT] 
    > Aşağıdaki cmdlet 'lerde örnek parolalar kullanmayın. Parolaların rastgele bir dize olarak değiştirilmesini unutmayın.

    ```PowerShell
    Set-ADAccountPassword -Identity johndoe -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd1" -Force)
    Set-ADAccountPassword -Identity johndoe -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd2" -Force)
    ```

### <a name="azure-active-directory-environment"></a>Azure Active Directory ortamı

Azure Active Directory yönetici olarak PowerShell ' i açın, çalıştırın ``Connect-AzureAD`` ve aşağıdaki işlemleri gerçekleştirin:

1. Azure AD 'de kullanıcıyı devre dışı bırakın. [Set-AzureADUser](https://docs.microsoft.com/powershell/module/azuread/Set-AzureADUser?view=azureadps-2.0)öğesine bakın.

    ```PowerShell
    Set-AzureADUser -ObjectId johndoe@contoso.com -AccountEnabled $false
    ```
1. Kullanıcının Azure AD yenileme belirteçlerini iptal edin. [Revoke-AzureADUserAllRefreshToken](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0)öğesine başvurun.

    ```PowerShell
    Revoke-AzureADUserAllRefreshToken -ObjectId johndoe@contoso.com
    ```

1. Kullanıcının cihazlarını devre dışı bırakın. [Get-AzureADUserRegisteredDevice](https://docs.microsoft.com/powershell/module/azuread/get-azureaduserregistereddevice?view=azureadps-2.0)öğesine başvurun.

    ```PowerShell
    Get-AzureADUserRegisteredDevice -ObjectId johndoe@contoso.com | Set-AzureADDevice -AccountEnabled $false
    ```

## <a name="optional-steps"></a>İsteğe bağlı adımlar

- [Intune tarafından yönetilen uygulamalardan kurumsal verileri silme](https://docs.microsoft.com/mem/intune/apps/apps-selective-wipe).

- [Şirkete ait cihazları Sil cihazı fabrika varsayılan ayarlarına sıfırlıyor](https://docs.microsoft.com/mem/intune/remote-actions/devices-wipe).

> [!NOTE]
> Bir silme işleminden sonra cihazdaki veriler kurtarılamaz.

## <a name="when-access-is-revoked"></a>Erişim iptal edildiğinde

Yöneticiler yukarıdaki adımları gerçekleştirdikten sonra, Azure Active Directory bağlı herhangi bir uygulama için Kullanıcı yeni belirteçler elde edebilir. İptal ve Kullanıcı erişimini kaybetme arasındaki geçen süre, uygulamanın erişim verme yöntemine bağlıdır:

- Erişim **belirteçleri kullanan uygulamalar**için, erişim belirtecinin süresi dolarsa Kullanıcı erişimi kaybeder.

- **Oturum belirteçleri kullanan uygulamalar**için, mevcut oturumların belirtecin süresi dolduktan hemen sonra sona erer. Kullanıcının devre dışı durumu uygulamayla eşitlenirse, uygulama bunu yapmak üzere yapılandırılmışsa, kullanıcının mevcut oturumlarını otomatik olarak iptal edebilir.  Bu süre, uygulama ile Azure AD arasındaki eşitlemenin sıklığına bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yöneticileri için güvenli erişim uygulamaları](directory-admin-roles-secure.md)
- [Kullanıcı profili bilgilerini ekleme veya güncelleştirme](../fundamentals/active-directory-users-profile-azure-portal.md)
