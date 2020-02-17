---
title: Azure Active Directory B2B en iyi uygulamaları ve önerileri
description: Azure Active Directory 'de işletmeler arası (B2B) Konuk Kullanıcı erişimi için en iyi yöntemleri ve önerileri öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e17e0f1f01e836a7a240100c1c0e1f015da5f00
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368157"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Azure Active Directory B2B en iyi uygulamaları
Bu makale, Azure Active Directory (Azure AD) ile işletmeler arası (B2B) işbirliği için öneriler ve en iyi uygulamalar içerir.

## <a name="b2b-recommendations"></a>B2B önerileri
| Öneri | Yorumlar |
| --- | --- |
| En iyi oturum açma deneyimi için, kimlik sağlayıcılarıyla federasyona ayırın | Mümkün olduğunda, davet edilen kullanıcıların Microsoft hesapları (MSAs) veya Azure AD hesapları oluşturmaya gerek kalmadan paylaşılan uygulamalarınızda ve kaynaklarda oturum açmalarına izin vermek için doğrudan kimlik sağlayıcılarıyla federasyona ayırın. B2B Konuk kullanıcılarının Google hesaplarıyla oturum açmasını sağlamak için [Google Federation özelliğini](google-federation.md) kullanabilirsiniz. Ya da [doğrudan Federasyon (Önizleme) özelliğini](direct-federation.md) kullanarak, kimlik sağlayıcısı (ıDP) SAML 2,0 veya WS-beslenir protokolünü destekleyen herhangi bir kuruluşla doğrudan Federasyonu ayarlayabilirsiniz. |
| Diğer yollarla kimlik doğrulaması yapamayan B2B konukları için e-posta bir kerelik geçiş kodu (Önizleme) özelliğini kullanın | [E-posta bir kerelik geçiş kodu (Önizleme)](one-time-passcode.md) özelliği, Azure AD, MICROSOFT HESABı (MSA) veya Google Federasyonu gibi diğer yollarla kimlik doğrulamasından GEÇIYORLARSA B2B Konuk kullanıcılarının kimliğini doğrular. Konuk Kullanıcı bir davetiyeyi bir davet edebilir veya paylaşılan bir kaynağa eriştiğinde, kendi e-posta adreslerine gönderilen geçici bir kod isteyebilir. Sonra oturum açmaya devam etmek için bu kodu girer. |
| Oturum açma sayfanıza şirket markası ekleme | Çoklu oturum açma sayfanızı, B2B Konuk kullanıcılarınız için daha sezgisel olacak şekilde özelleştirebilirsiniz. Bkz. [oturum açma ve erişim paneli sayfalarına Şirket markası ekleme](../fundamentals/customize-branding.md). |
| B2B Konuk Kullanıcı kullanım deneyimine Gizlilik Bildirimimizi ekleyin | Davet edilen bir kullanıcının devam etmek için Gizlilik koşullarınızı onaylaması gerekir diye, kuruluşunuzun gizlilik bildiriminin URL 'sini ilk kez davet kullanım sürecine ekleyebilirsiniz. Bkz. [nasıl yapılır: kuruluşunuzun gizlilik bilgilerini Azure Active Directory ekleme](https://aka.ms/adprivacystatement). |
| Birden çok B2B Konuk kullanıcıyı aynı anda davet etmek için toplu davet (Önizleme) özelliğini kullanın | Azure portal toplu davet önizleme özelliğini kullanarak birden çok Konuk kullanıcıyı kuruluşunuza aynı anda davet edin. Bu özellik, B2B Konuk kullanıcıları oluşturmak ve davetleri toplu olarak göndermek için bir CSV dosyasını karşıya yüklemenize olanak sağlar. [B2B kullanıcılarını toplu olarak davet etmek Için öğreticiye](tutorial-bulk-invite.md)bakın. |
| Multi-Factor Authentication için koşullu erişim ilkelerini zorlama (MFA) | İş ortağı B2B kullanıcılarıyla paylaşmak istediğiniz uygulamalarda MFA ilkelerini zorunlu olarak yapmanızı öneririz. Bu şekilde, iş ortağı kuruluşun MFA kullanıp kullanmadığını fark etmeksizin, MFA, kiracınızdaki uygulamalarda sürekli olarak zorlanır. [B2B işbirliği kullanıcıları Için koşullu erişim](conditional-access.md)konusuna bakın. |
| Cihaz tabanlı koşullu erişim ilkeleri zorlarken, B2B kullanıcılarına erişime izin vermek için dışlama listeleri kullanın | Kuruluşunuzda cihaz tabanlı koşullu erişim ilkeleri etkinse, B2B Konuk Kullanıcı cihazları kuruluşunuz tarafından yönetilmediği için engellenir. Cihaz tabanlı koşullu erişim ilkesinden hariç tutmak üzere belirli iş ortağı kullanıcılarını içeren dışlama listeleri oluşturabilirsiniz. [B2B işbirliği kullanıcıları Için koşullu erişim](conditional-access.md)konusuna bakın. |
| B2B Konuk kullanıcılarınıza doğrudan bağlantılar sağlarken kiracıya özgü bir URL kullanın | Davet e-postasına alternatif olarak, bir konuğa uygulamanız veya portalınızın doğrudan bağlantısını sağlayabilirsiniz. Bu doğrudan bağlantı kiracıya özgü olmalıdır, yani bir kiracı KIMLIĞI veya doğrulanmış etki alanı içermesi gerekir, böylelikle Konuk, paylaşılan uygulamanın bulunduğu kiracınızda kimlik doğrulaması yapılabilir. [Konuk Kullanıcı için kullanım deneyimini](redemption-experience.md)inceleyin. |
| Bir uygulama geliştirirken, Konuk Kullanıcı deneyimini öğrenmek için UserType kullanın  | Bir uygulama geliştiriyorsanız ve kiracı kullanıcıları ve Konuk kullanıcılar için farklı deneyimler sağlamak istiyorsanız UserType özelliğini kullanın. UserType talebi şu anda belirtece dahil değildir. Uygulamalar, kullanıcının UserType değerlerini alması için dizini sorgulamak üzere Microsoft Graph API 'sini kullanmalıdır. |
| UserType özelliğini *yalnızca* kullanıcının kuruluşla olan ilişkisi değişirse değiştirin | Bir kullanıcının UserType özelliğini bir üyenin üyesine konuğa (ve tam tersi) dönüştürmek için PowerShell 'i kullanmak mümkün olsa da, bu özelliği yalnızca kullanıcının kuruluşunuzla olan ilişkisi değişirse değiştirmelisiniz. [B2B Konuk kullanıcısının özelliklerine](user-properties.md)bakın.|

## <a name="next-steps"></a>Sonraki adımlar

[B2B paylaşımını yönetme](delegate-invitations.md)
