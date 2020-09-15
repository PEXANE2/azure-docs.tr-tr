---
title: Yayımcı doğrulamasına genel bakış-Microsoft Identity platform | Mavisi
description: Microsoft Identity platformu için yayımcı doğrulama programına genel bir bakış sağlar. Avantajları, program gereksinimlerini ve sık sorulan soruları listeler. Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcının doğrulama işlemini tamamlamış bir Microsoft İş Ortağı Ağı hesabı kullanarak kimliğini doğruladığını ve bu MPN hesabını uygulama kaydıyla ilişkilendirdiğini anlamına gelir.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/19/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: 0c30b5305fb5496db86a20be7a37f36b894ecc8b
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90089760"
---
# <a name="publisher-verification"></a>Yayımcı doğrulaması

Yayımcı doğrulaması, yöneticilerin ve son kullanıcıların Microsoft Identity platformu ile tümleştirerek uygulama geliştiricilerinin orijinalliğini anlamalarına yardımcı olur. Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcının [doğrulama](/partner-center/verification-responses) işlemini tamamlamış bir [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership) hesabı kullanarak kimliğini DOĞRULADıĞıNı ve bu MPN hesabını uygulama kaydıyla ilişkilendirdiğini anlamına gelir. 

Azure AD onay istemi ve diğer ekranlarda mavi "doğrulanan" bir rozet görünür: ![ onay istemi](./media/publisher-verification-overview/consent-prompt.png)

Bu özellik, birincil olarak, OAuth 2,0 'den yararlanan çok kiracılı uygulamalar oluşturan geliştiriciler ve [Microsoft Identity platformu](v2-overview.md)Ile [OpenID Connect 'e](active-directory-v2-protocols.md) yöneliktir. Bu uygulamalar, OpenID Connect kullanarak kullanıcıları imzalayabilirler veya [Microsoft Graph](https://developer.microsoft.com/graph/)gibi API 'leri kullanarak verilere erişim Istemek için OAuth 2,0 kullanabilirler.

## <a name="benefits"></a>Yararları
Yayımcı doğrulaması aşağıdaki avantajları sağlar:
- **Müşteriler Için daha fazla saydamlık ve risk azaltma**-bu özellik, müşterilerin kuruluşlarında kullanılan uygulamaları öğrendikleri geliştiriciler tarafından yayımlandığını anlamalarına yardımcı olur. 

- **Geliştirilmiş marka**-Azure AD [onay Istemi](application-consent-experience.md), kurumsal uygulamalar sayfasında ve son kullanıcılar ve YÖNETICILER tarafından kullanılan ek UX yüzeyleri üzerinde "doğrulanan" bir rozet görüntülenir. 

- **Daha yumuşak bir kurumsal benimseme**-Yöneticiler, [Kullanıcı onay ilkelerini](../manage-apps/configure-user-consent.md), birincil ilke ölçütlerinden biri olarak yayımcı doğrulama durumuyla yapılandırabilir. 

- **İyileştirilmiş risk değerlendirmesi**-Microsoft 'un "riskli" izin istekleri için algılamaları, yayımcı doğrulamasını bir sinyal olarak içerecektir. 

## <a name="requirements"></a>Gereksinimler
Yayımcı doğrulaması için birkaç önkoşul vardır; bazıları zaten birçok Microsoft iş ortağı tarafından tamamlanmış olacaktır. Bunlar: 

-  [Doğrulama](/partner-center/verification-responses) işlemini tamamlamış geçerli bir [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership) HESABı için MPN kimliği. Bu MPN hesabının kuruluşunuz için [ortak genel hesabı (PGA)](/partner-center/account-structure#the-top-level-is-the-partner-global-account-pga) olması gerekir. 

-  Bir [Yayımcı etki alanı](howto-configure-publisher-domain.md) yapılandırılmış BIR Azure AD kiracısında kayıtlı bir uygulama.

-  MPN hesap doğrulaması sırasında kullanılan e-posta adresinin etki alanı, uygulamada yapılandırılan yayımcı etki alanı veya Azure AD kiracısına eklenen DNS tarafından doğrulanan [özel etki](../fundamentals/add-custom-domain.md) alanı ile eşleşmelidir. 

-  Azure AD 'de uygulama kaydında ve Iş Ortağı Merkezi 'nde MPN hesabında değişiklik yapmak için doğrulamayı gerçekleştiren kullanıcı yetkilendirilmelidir. 

    -  Azure AD 'de bu kullanıcı şu [rollerden](../users-groups-roles/directory-assign-admin-roles.md)birine üye olmalıdır: uygulama Yöneticisi, bulut uygulaması Yöneticisi veya genel yönetici. 

    -  Iş Ortağı Merkezi 'nde bu kullanıcının şu [rollere](/partner-center/permissions-overview)sahip olması gerekir: MPN Yöneticisi, hesaplar Yöneticisi veya genel yönetici (Bu, Azure AD 'de ana kopyalı bir paylaşılan roldür).
    
-  Doğrulama gerçekleştiren kullanıcının [Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)kullanarak oturum açması gerekir.

-  Yayımcı, [geliştiricilerin kullanım koşulları Için Microsoft Identity platformunu](/legal/microsoft-identity-platform/terms-of-use)kabul eder.

Bu önkoşulların zaten karşıladığı geliştiriciler, birkaç dakika içinde doğrulanır. Gereksinimler karşılanmazsa, kurulum alma ücretsizdir. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
Yayımcı doğrulama programıyla ilgili sık sorulan bazı sorular aşağıda verilmiştir. Gereksinimleriyle ve işlemle ilgili SSS için bkz. [uygulamayı yayımcı doğrulandı olarak işaretleme](mark-app-as-publisher-verified.md).

- **Yayımcı __doğrulaması hangi bilgileri sağlamaz?__**  Bir uygulama tarafından doğrulanmış olarak işaretlendiğinde, bu, uygulamanın veya yayımcısının belirli bir sertifikaya sahip olup olmadığını, sektör standartlarına uygunluğunu, en iyi yöntemlere bağlı olduğunu veya vb. olduğunu göstermez. Diğer Microsoft programları, [Microsoft 365 uygulama sertifikası](/microsoft-365-app-certification/overview)da dahil olmak üzere bu bilgileri sağlar.

- **Bu maliyet ne kadar sürer? Lisans gerektiriyor mu?** Microsoft, geliştiricilere yayımcı doğrulaması için ücret alınmaz ve belirli bir lisans gerektirmez. 

- **Bu, yayımcı kanıtlama Microsoft 365 nasıl ilgilidir? Microsoft 365 App Certification ne olacak?** Bunlar, geliştiricilerin müşteriler tarafından güvenle benimsenen güvenilir uygulamalar oluşturmak için kullanabilecekleri tamamlayıcı programlardır. Yayımcı doğrulaması bu işlemin ilk adımıdır ve yukarıdaki ölçütlere uyan uygulamalar oluşturan tüm geliştiriciler tarafından tamamlanmalıdır. 

  Microsoft 365 ile Tümleştirdiğiniz geliştiriciler de bu programlardan ek avantajlar alabilir. Daha fazla bilgi için [Yayımcı kanıtlama Microsoft 365](/microsoft-365-app-certification/docs/attestation) ve [uygulama sertifikası Microsoft 365](/microsoft-365-app-certification/docs/certification)' na bakın. 

- **Bu, Azure AD uygulama galerisiyle aynı şeydir mi?** Yayımcı doğrulaması yok, [Azure Active Directory Uygulama galerisinde](../azuread-dev/howto-app-gallery-listing.md)tamamlayıcı ancak ayrı bir programdır. Yukarıdaki ölçütlere uyan geliştiriciler, yayımcı doğrulama sürecini bu programa katılımdan bağımsız olarak tamamlamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulamayı yayımcı doğrulanmış olarak nasıl işaretleyeceğinizi](mark-app-as-publisher-verified.md)öğrenin.
* Yayımcı doğrulamasında [sorun giderin](troubleshoot-publisher-verification.md) .
