---
title: Sorun Giderme B2B işbirliği - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliğiile ilgili sık karşılaşılan sorunların çözüm yolları
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: troubleshooting
ms.date: 03/19/2020
tags: active-directory
ms.author: mimart
author: msmimart
ms.reviewer: mal
ms.custom:
- it-pro
- seo-update-azuread-jan"
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f8bafb04d0a5d9c6d25a7ed7e155888d492e9fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050777"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B işbirliğinde sorun giderme

Azure Etkin Dizin (Azure AD) B2B işbirliğiyle ilgili sık karşılaşılan sorunlara yönelik bazı çözümler aşağıda vereb.)

   > [!IMPORTANT]
   > **31 Mart 2021'den itibaren Microsoft,** B2B işbirliği senaryoları için yönetilmeyen Azure AD hesapları ve kiracılar oluşturarak artık davetlerin itfasını desteklemez. Hazırlık olarak, müşterilerin tek [seferlik parola kimlik doğrulaması e-postasını](one-time-passcode.md)seçmelerini öneririz. Bu genel önizleme özelliği hakkındaki görüşlerinizi memnuniyetle karşılıyoruz ve işbirliği yapmak için daha fazla yol oluşturmaktan heyecan duyuyoruz.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Harici bir kullanıcı ekledim ama bunları Global Adres Defterimde veya kişi seçicimde görmüyorum

Dış kullanıcıların listede doldurulmama durumlarda, nesnenin çoğaltılması birkaç dakika sürebilir.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Bir B2B konuk kullanıcı SharePoint Online/ OneDrive kişi seçici görünmüyor

SharePoint Online (SPO) kişi seçicide varolan konuk kullanıcıları arama olanağı, varsayılan olarak eski davranışla eşleşecek şekilde KAPALIdİr.

Bu özelliği, kiracı ve site koleksiyonu düzeyindeki 'ShowPeoplePickerSuggestionsForGuestUsers' ayarını kullanarak etkinleştirebilirsiniz. Bu özelliği, üyelerin dizindeki tüm mevcut konuk kullanıcıları aramasına olanak tanıyan Set-SPOTenant ve Set-SPOSite cmdlets'i kullanarak ayarlayabilirsiniz. Kiracı kapsamındaki değişiklikler zaten sağlanan SPO sitelerini etkilemez.

## <a name="invitations-have-been-disabled-for-directory"></a>Davetler dizin için devre dışı bırakıldı

Kullanıcıları davet etme izniniz olmadığı size bildirilirse, kullanıcı hesabınızın Dış kullanıcıları Azure Active Directory > Kullanıcı ayarları > Harici kullanıcılar > Dış işbirliği ayarlarını yönetme ye davet etme yetkisine sahip olduğunu doğrulayın:

![Harici Kullanıcılar ayarlarını gösteren ekran görüntüsü](media/troubleshoot/external-user-settings.png)

Bu ayarları yakın zamanda değiştirdiyseniz veya Konuk Davetli rolünü bir kullanıcıya atadıysanız, değişiklikler inşa atılmadan Ãnce 15- 60 dakika gecikme olabilir.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Davet ettiğim kullanıcı kullanım sırasında bir hata alıyor

Sık karşılaşılan hatalar şunlardır:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Davetlinin Yöneticisi E-posta Doğrulanmış Kullanıcıların kiracılarında oluşturulmasına izin vermedi

Kuruluşu Azure Etkin Dizini kullanan, ancak belirli kullanıcının hesabının bulunmadığı (örneğin, kullanıcı Azure AD contoso.com' da bulunmaz) kullanıcıları davet ederken. contoso.com yöneticisinin, kullanıcıların oluşturulmasını engelleyen bir ilkesi olabilir. Kullanıcı, harici kullanıcılara izin verilip verilmediğini belirlemek için yöneticilerine danışmalıdır. Harici kullanıcıyöneticisinin etki alanlarında E-posta Doğrulanmış kullanıcılara izin vermesi gerekebilir (E-posta Doğrulanmış Kullanıcılara izin verme hakkındaki bu [makaleye](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) bakın).

![Kiracının e-posta yla doğrulanmış kullanıcılara izin vermediğini belirten hata](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Dış kullanıcı federe bir etki alanında zaten yok

Federasyon kimlik doğrulamasını kullanıyorsanız ve kullanıcı Azure Etkin Dizin'de zaten yoksa, kullanıcı davet edilemez.

Bu sorunu gidermek için harici kullanıcının yöneticisinin kullanıcının hesabını Azure Etkin Dizin ile eşitlemesi gerekir.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Normalde geçerli\#bir karakter olmayan '' ', Azure AD ile nasıl senkronize edilir?

"\#" " " Yutkan AD B2B işbirliği veya dış kullanıcılar user@contoso.com için UPNs ayrılmış@fabrikam.onmicrosoft.combir karakterdir, çünkü davet edilen hesap user_contoso.com#EXT# olur. Bu \# nedenle, şirket içi gelen UPN'lerde Azure portalında oturum açmaizni verilmez. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Senkronize bir gruba harici kullanıcı eklerken bir hata alıyorum

Dış kullanıcılar yalnızca "atanmış" veya "Güvenlik" gruplarına eklenebilir, şirket içinde ustalaşmayan gruplara eklenebilir.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Harici kullanıcım kullanmak için bir e-posta almadı

Davetli, aşağıdaki adrese izin verildiğinden emin olmak için ISS veya spam filtresine danışmalıdır:Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Özel iletinin zaman zaman davet iletilerine dahil olmadığını fark ettim

Gizlilik yasalarına uymak için API'lerimiz aşağıdaki durumlarda e-posta davetine özel iletiler içermez:

- Davet edenin davet eden kiracıda e-posta adresi yok
- Bir uygulama hizmeti müdürü daveti gönderdiğinde

Bu senaryo sizin için önemliyse, API davet e-postamızı bastırabilir ve seçtiğiniz e-posta mekanizması aracılığıyla gönderebilirsiniz. Bu şekilde gönderdiğiniz e-postaların gizlilik yasalarına da uygun olduğundan emin olmak için kuruluşunuzun yasal danışmanına danışın.

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>Bir Azure kaynağında oturum açmaya çalıştığınızda "AADSTS65005" hatası alırsınız

Konuk hesabı olan bir kullanıcı oturum açamıyor ve aşağıdaki hata iletisini alıyor:

    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.

Kullanıcının bir Azure kullanıcı hesabı vardır ve terk edilmiş veya yönetilmemiş viral bir kiracıdır. Ayrıca, kiracı hiçbir genel veya şirket yöneticileri vardır.

Bu sorunu çözmek için, terk edilmiş kiracı devralmanız gerekir. Azure [Etkin Dizin'de yönetici olarak yönetilmeyen bir dizinin devrin'ine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover)bakın. Ayrıca, ad alanını kontrol ettiğinize dair doğrudan kanıt sağlamak için söz konusu etki alanı soneki için internete bakan DNS'ye de erişmeniz gerekir. Kiracı yönetilen bir duruma döndürüldükten sonra, lütfen müşteriyle, kullanıcılardan ayrılmanın ve doğrulanmış etki alanı adının kuruluşları için en iyi seçenek olup olmadığını görüşün.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Tam zamanında veya "viral" kiracıya sahip bir konuk kullanıcı parolasını sıfırlayamıyor

Kimlik kiracısı tam zamanında (JIT) veya viral bir kiracıysa (yani ayrı, yönetilmeyen bir Azure kiracısıysa), parolasını yalnızca konuk kullanıcı sıyrabilir. Bazen bir kuruluş, çalışanlar hizmetlere kaydolmak için iş e-posta adreslerini kullandıklarında oluşturulan [viral kiracıların yönetimini devralır.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) Kuruluş viral bir kiracıyı devraldıktan sonra, yalnızca bu kuruluştaki bir yönetici kullanıcının parolasını sıfırladı veya SSPR'yi etkinleştirebilir. Gerekirse, davet eden kuruluş olarak konuk kullanıcı hesabını dizininizden kaldırabilir ve davetiyeyi yeniden gönderebilirsiniz.

## <a name="a-guest-user-is-unable-to-use-the-azuread-powershell-v1-module"></a>Konuk kullanıcı AzureAD PowerShell V1 modüllerini kullanamaz

18 Kasım 2019 itibariyle, dizininizdeki konuk kullanıcıların **(userType** özelliğinin **Konuk'a**eşit olduğu kullanıcı hesapları olarak tanımlanır) AzureAD PowerShell V1 modülünün kullanılması engellenir. İleriye dönük olarak, bir kullanıcının üye kullanıcı olması **(userType'ın** **Üye'ye**eşit olduğu durumlarda) veya AzureAD PowerShell V2 modüllerini kullanması gerekir.

## <a name="in-an-azure-us-government-tenant-i-cant-invite-a-b2b-collaboration-guest-user"></a>Bir Azure ABD Hükümeti kiracısında, B2B işbirliği konuk kullanıcılarını davet edemiyorum

Azure ABD Hükümeti bulutu içinde, B2B işbirliği şu anda yalnızca hem Azure ABD Hükümeti bulutu içinde olan hem de B2B işbirliğini destekleyen kiracılar arasında desteklenir. Bir kullanıcıyı Azure ABD Devlet bulutunun bir parçası olmayan veya henüz B2B işbirliğini desteklemeyen bir kiracıya davet ederseniz, bir hata alırsınız. Ayrıntılar ve sınırlamalar için [Azure Active Directory Premium P1 ve P2 Varyasyonları'na](https://docs.microsoft.com/azure/azure-government/documentation-government-services-securityandidentity#azure-active-directory-premium-p1-and-p2)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[B2B işbirliği için destek alın](get-support.md)
