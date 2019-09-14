---
title: B2B işbirliği sorunlarını giderme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B işbirliğiyle ilgili yaygın sorunlar için çözümler
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: v-miegge
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: f91ddee8668316df69c98ed14fbcabcb06b6da82
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983392"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>B2B işbirliği Azure Active Directory sorunlarını giderme

İşte Azure Active Directory (Azure AD) B2B işbirliğiyle ilgili yaygın sorunlar için bazı çözümler.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Dış Kullanıcı ekledim ancak bunları Genel Adres defterimde veya kişiler seçicisinde gördüm

Dış kullanıcıların listede doldurulmadığı durumlarda, nesnenin çoğaltılması birkaç dakika sürebilir.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>B2B Konuk Kullanıcı, SharePoint Online/OneDrive kişi seçicisinde gösterilmiyor

SharePoint Online (SPO) kişi seçicisindeki mevcut Konuk kullanıcıları arama özelliği, eski davranışa uyacak şekilde varsayılan olarak KAPALıDıR.

Kiracı ve site koleksiyonu düzeyinde ' ShowPeoplePickerSuggestionsForGuestUsers ' ayarını kullanarak bu özelliği etkinleştirebilirsiniz. Özelliği set-SPOTenant ve set-SPOSite cmdlet 'lerini kullanarak ayarlayabilirsiniz. Bu özellik, üyelerin dizindeki tüm mevcut Konuk kullanıcıları aramasını sağlar. Kiracı kapsamındaki değişiklikler zaten sağlanmış olan SPO sitelerini etkilemez.

## <a name="invitations-have-been-disabled-for-directory"></a>Dizin için davetler devre dışı bırakıldı

Kullanıcıları davet etme izniniz yoksa, Kullanıcı hesabınızın Azure Active Directory > Kullanıcı ayarlarından dış kullanıcıları davet etmek için > > dış işbirliği ayarlarını yönetme yetkisine sahip olduğunu doğrulayın:

![Dış Kullanıcı ayarlarını gösteren ekran görüntüsü](media/troubleshoot/external-user-settings.png)

Son zamanlarda bu ayarları değiştirdiyseniz veya konuk davet eden rolünü bir kullanıcıya atadıysanız, değişiklikler yürürlüğe girmeden önce 15-60 dakikalık bir gecikme olabilir.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Davet etdiğim Kullanıcı, kullanım sırasında bir hata alıyor

Sık karşılaşılan hatalar şunlardır:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Davetli kullanıcının yöneticisinin kiracısında, Emaildoğrulanan kullanıcıların oluşturulmasını izin vermemiş

Kuruluş, Azure Active Directory kullanan kullanıcıları davet ederken, ancak belirli kullanıcı hesabının bulunmadığı (örneğin, Kullanıcı Azure AD contoso.com 'de yok). Contoso.com yöneticisinin, kullanıcıların oluşturulmasını önlemek için bir ilke olabilir. Kullanıcının dış kullanıcılara izin verilip verilmeyeceğini öğrenmek için yönetici ile denetlemesi gerekir. Dış kullanıcının yöneticisinin etki alanında e-posta doğrulanmış kullanıcılara izin verilmesi gerekebilir (e-posta doğrulanan kullanıcılara izin verme konusunda bu [makaleye](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) bakın).

![Kiracıyı bildiren hata, e-posta tarafından doğrulanan kullanıcılara izin vermiyor](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Dış Kullanıcı zaten Federasyon etki alanında yok

Federasyon kimlik doğrulaması kullanıyorsanız ve Kullanıcı Azure Active Directory zaten mevcut değilse, Kullanıcı davet edilemez.

Bu sorunu çözmek için, dış kullanıcının Yöneticisi kullanıcının hesabını Azure Active Directory eşitlemelidir.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Normalde geçerli bir\#karakter olmayan ' ' ne olur? Azure AD ile eşitleme?

"\#", Azure AD B2B işbirliği veya dış kullanıcılar için UPN 'de ayrılmış bir karakter olduğundan, davet edilen hesap user@contoso.com user_contoso. com # ext #@fabrikam.onmicrosoft.comolur. Bu nedenle \# , Şirket içinden gelen UPN 'ler ' de Azure Portal oturum açmasına izin verilmez. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Dış kullanıcıları eşitlenmiş bir gruba eklerken bir hata alıyorum

Dış kullanıcılar, yalnızca "atanan" veya "güvenlik" gruplarına eklenebilir ve şirket içinde ana kopyalı gruplar için kullanılamaz.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Dış Kullanıcı kullanılacak e-posta almadı

Davetli kişinin, aşağıdaki adrese izin verildiğinden emin olmak için ISS veya e-posta filtresini denetlemesi gerekir:Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Özel iletinin, zaman içinde davet iletilerine dahil edilmediğini fark ediyorum

Gizlilik yasaları ile uyum sağlamak için, API 'lerimiz şu durumlarda e-posta davetine özel iletiler içermez:

- Davette, davet edilen kiracıda bir e-posta adresi yok
- Bir appservice sorumlusu daveti gönderdiğinde

Bu senaryo sizin için önemliyse, API davetiyesi e-postamızı engelleyebilir ve tercih ettiğiniz e-posta mekanizmasıyla gönderebilirsiniz. Bu şekilde göndereceğiniz tüm e-postaların gizlilik yasaları ile uyumlu olduğundan emin olmak için kuruluşunuzun yasal sayılığınıza başvurun.

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>Bir Azure kaynağında oturum açmaya çalıştığınızda bir "AADSTS65005" hatası alıyorsunuz

Konuk hesabına sahip olan bir Kullanıcı oturum açıp şu hata iletisini alıyor:

    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.

Kullanıcının bir Azure Kullanıcı hesabı vardır ve terk edilmiş ya da yönetilmeyen bir viral kiracısı vardır. Ayrıca, kiracıda genel veya Şirket Yöneticisi yoktur.

Bu sorunu çözmek için, bırakılan kiracıyı devralmalısınız. [Azure Active Directory yönetilmeyen bir dizinden yönetici olarak alma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover)bölümüne bakın. Ayrıca, ad alanını kontrol ettiğiniz doğrudan kanıt sağlamak için söz konusu etki alanı sonekine yönelik İnternet 'e yönelik DNS 'e erişmeniz gerekir. Kiracı yönetilen bir duruma dönmeden sonra, kullanıcılar ve doğrulanmış etki alanı adının kuruluş için en iyi seçenek olup olmadığını müşteriyle birlikte tartışın.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Tam zamanında veya "viral" kiracısına sahip bir Konuk Kullanıcı parolasını sıfırlayamaz

Kimlik kiracısı tam zamanında (JıT) veya viral kiracınız (yani ayrı, yönetilmeyen bir Azure kiracısı) ise, yalnızca Konuk Kullanıcı parolasını sıfırlayabilir. Bazen bir kuruluş, çalışanlar hizmetlere kaydolmak için iş e-posta adreslerini kullandıklarında oluşturulan [viral kiracılarının yönetimini ele geçirebilir](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) . Kuruluş bir viral kiracısı gerçekleştirdikten sonra, yalnızca o kuruluştaki bir yönetici kullanıcı parolasını sıfırlayabilir veya SSPR 'yi etkinleştirebilir. Gerekirse, kuruluş olarak, Konuk Kullanıcı hesabını dizininizden kaldırabilir ve bir daveti yeniden gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[B2B işbirliği için destek alın](get-support.md)
