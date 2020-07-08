---
title: Yayımcı doğrulamasına genel bakış-Microsoft Identity platform | Mavisi
description: Microsoft Identity platformu için yayımcı doğrulama programına (Önizleme) genel bir bakış sağlar. Avantajları, program gereksinimlerini ve sık sorulan soruları listeler. Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcının doğrulama işlemini tamamlamış bir Microsoft İş Ortağı Ağı hesabı kullanarak kimliğini doğruladığını ve bu MPN hesabını uygulama kaydıyla ilişkilendirdiğini anlamına gelir.
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
ms.openlocfilehash: 73a96f295d5dfa74130927e5096e9278a0e348e8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83682431"
---
# <a name="publisher-verification-preview"></a>Yayımcı doğrulaması (önizleme)

Yayımcı doğrulaması (Önizleme), yöneticilerin ve son kullanıcıların Microsoft Identity platformu ile tümleştirerek uygulama geliştiricilerinin orijinalliğini anlamalarına yardımcı olur. Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcının [doğrulama](/partner-center/verification-responses) işlemini tamamlamış bir [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership) hesabı kullanarak kimliğini DOĞRULADıĞıNı ve bu MPN hesabını uygulama kaydıyla ilişkilendirdiğini anlamına gelir. 

Azure AD onay istemi ve diğer ekranlarda mavi "doğrulanan" bir rozet görünür: ![ onay istemi](./media/publisher-verification-overview/consent-prompt.png)

Bu özellik, birincil olarak, OAuth 2,0 'den yararlanan çok kiracılı uygulamalar oluşturan geliştiriciler ve [Microsoft Identity platformu](v2-overview.md)Ile [OpenID Connect 'e](active-directory-v2-protocols.md) yöneliktir. Bu uygulamalar, OpenID Connect kullanarak kullanıcıları imzalayabilirler veya [Microsoft Graph](https://developer.microsoft.com/graph/)gibi API 'leri kullanarak verilere erişim Istemek için OAuth 2,0 kullanabilirler.

## <a name="benefits"></a>Yararları
Yayımcı doğrulaması aşağıdaki avantajları sağlar:
- **Müşteriler Için daha fazla saydamlık ve risk azaltma**-bu özellik, müşterilerin kuruluşlarında kullanılan uygulamaları öğrendikleri geliştiriciler tarafından yayımlandığını anlamalarına yardımcı olur. 

- **Geliştirilmiş marka**-Azure AD [onay Istemi](application-consent-experience.md), kurumsal uygulamalar sayfasında ve son kullanıcılar ve YÖNETICILER tarafından kullanılan ek UX yüzeyleri üzerinde "doğrulanan" bir rozet görüntülenir. 

- **Daha yumuşak bir kurumsal benimseme**-Yöneticiler yeni Kullanıcı onay ilkeleri yapılandırabilir ve yayımcı doğrulama durumu, birincil ilke ölçütlerinden biri olacaktır. 

- **İyileştirilmiş risk değerlendirmesi**-Microsoft 'un "riskli" izin istekleri için algılamaları, yayımcı doğrulamasını bir sinyal olarak içerecektir. 

## <a name="requirements"></a>Gereksinimler
Yayımcı doğrulaması için birkaç önkoşul vardır; bazıları zaten birçok Microsoft iş ortağı tarafından tamamlanmış olacaktır. Bunlar: 

-  [Doğrulama](/partner-center/verification-responses) işlemini tamamlamış geçerli bir [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership) HESABı için MPN kimliği. Bu MPN hesabının kuruluşunuz için [ortak genel hesabı (PGA)](/partner-center/account-structure#the-top-level-is-the-partner-global-account-pga) olması gerekir. 

-  DNS tarafından doğrulanan [özel etki alanı](/azure/active-directory/fundamentals/add-custom-domain)olan BIR Azure AD kiracısı. Özel etki alanı, önceki adımda doğrulama sırasında kullanılan e-posta adresi etki alanıyla aynı olmalıdır. 

-  Daha önce kullanılan etki alanı kullanılarak yapılandırılmış bir [Yayımcı etki alanı](howto-configure-publisher-domain.md) Ile BIR Azure AD kiracısında kayıtlı bir uygulama. 

-  Azure AD 'de uygulama kaydında ve Iş Ortağı Merkezi 'nde MPN hesabında değişiklik yapmak için doğrulamayı gerçekleştiren kullanıcı yetkilendirilmelidir. 

    -  Azure AD 'de bu kullanıcı uygulamanın sahibi olmalıdır veya şu [rollerden](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)birine sahip olmalıdır: uygulama Yöneticisi, bulut uygulaması Yöneticisi, genel yönetici. 

    -  Iş Ortağı Merkezi 'nde bu kullanıcının şu [rollere](/partner-center/permissions-overview)sahip olması gerekir: MPN Yöneticisi, hesaplar Yöneticisi veya genel yönetici (Bu, Azure AD 'de ana kopyalı bir paylaşılan roldür).
    
-  Yayımcı, [geliştiricilerin kullanım koşulları Için Microsoft Identity platformunu](/legal/microsoft-identity-platform/terms-of-use)kabul eder.

Bu önkoşulların zaten karşıladığı geliştiriciler, birkaç dakika içinde doğrulanır. Gereksinimler karşılanmazsa, kurulum alma ücretsizdir. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
Yayımcı doğrulama programıyla ilgili sık sorulan bazı sorular aşağıda verilmiştir. Gereksinimleriyle ve işlemle ilgili SSS için bkz. [uygulamayı yayımcı doğrulandı olarak işaretleme](mark-app-as-publisher-verified.md).

- **Yayımcı __doğrulaması hangi bilgileri sağlamaz?__**  Bir uygulama tarafından doğrulanmış olarak işaretlendiğinde, bu, uygulamanın veya yayımcısının belirli bir sertifikaya sahip olup olmadığını, sektör standartlarına uygunluğunu, en iyi yöntemlere bağlı olduğunu veya vb. olduğunu göstermez. Diğer Microsoft programları, [Microsoft 365 uygulama sertifikası](/microsoft-365-app-certification/overview)da dahil olmak üzere bu bilgileri sağlar.

- **Bu maliyet ne kadar sürer? Lisans gerektiriyor mu?** Microsoft, geliştiricilere yayımcı doğrulaması için ücret alınmaz ve belirli bir lisans gerektirmez. 

- **Bu, yayımcı kanıtlama Microsoft 365 nasıl ilgilidir? Microsoft 365 App Certification ne olacak?** Bunlar, geliştiricilerin müşteriler tarafından güvenle benimsenen güvenilir uygulamalar oluşturmak için kullanabilecekleri tamamlayıcı programlardır. Yayımcı doğrulaması bu işlemin ilk adımıdır ve yukarıdaki ölçütlere uyan uygulamalar oluşturan tüm geliştiriciler tarafından tamamlanmalıdır. 

  Microsoft 365 ile Tümleştirdiğiniz geliştiriciler de bu programlardan ek avantajlar alabilir. Daha fazla bilgi için [Yayımcı kanıtlama Microsoft 365](/microsoft-365-app-certification/docs/attestation) ve [uygulama sertifikası Microsoft 365](/microsoft-365-app-certification/docs/certification)' na bakın. 

- **Bu, Azure AD uygulama galerisiyle aynı şeydir mi?** Yayımcı doğrulaması yok, [Azure Active Directory Uygulama galerisinde](/azure/active-directory/azuread-dev/howto-app-gallery-listing)tamamlayıcı ancak ayrı bir programdır. Yukarıdaki ölçütlere uyan geliştiriciler, yayımcı doğrulama sürecini bu programa katılımdan bağımsız olarak tamamlamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulamayı yayımcı doğrulanmış olarak nasıl işaretleyeceğinizi](mark-app-as-publisher-verified.md)öğrenin.
* Yayımcı doğrulamasında [sorun giderin](troubleshoot-publisher-verification.md) .