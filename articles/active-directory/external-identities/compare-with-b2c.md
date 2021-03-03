---
title: Dış kimlikleri Karşılaştır-Azure Active Directory | Microsoft Docs
description: Azure AD dış kimlikleri, kuruluşunuzun dışındaki kişilerin kendi kimliklerini kullanarak uygulamalarınıza ve kaynaklarınıza erişmesine izin verir. Azure Active Directory B2B işbirliği ve Azure AD B2C dahil olmak üzere dış kimliklerin çözümlerini karşılaştırın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 03/02/2021
ms.custom: project-no-code
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 78fd91e2ba7badb1936b8603c8ed089130b29ffb
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644074"
---
# <a name="what-are-external-identities-in-azure-active-directory"></a>Azure Active Directory dış kimlikler nelerdir?

Azure AD 'de dış kimliklerle, kuruluşunuzun dışındaki kişilerin uygulamalarınıza ve kaynaklarınıza erişmesine izin vererek tercih ettikleri kimliği kullanarak oturum açmasını sağlayabilirsiniz. İş ortaklarınıza, dağıtımlarınız, tedarikçilerleriniz, satıcılarınız ve diğer konuk kullanıcılarınız "kendi kimliklerini getirebilir". Şirket veya devlet tarafından verilen bir dijital kimliğe ya da Google veya Facebook gibi yönetilmeyen bir sosyal kimliğe sahip olup olmadıklarında, oturum açmak için kendi kimlik bilgilerini kullanabilirler. Dış kullanıcının kimlik sağlayıcısı kimliklerini yönetir ve kaynaklarınızın korunmasını sağlamak için Azure AD ile uygulamalarınıza erişimi yönetirsiniz.

## <a name="external-identities-scenarios"></a>Dış kimlikler senaryoları

Azure AD dış kimlikleri, kullanıcının kuruluşunuzla olan ilişkisine ve Kullanıcı uygulamalarınızda ve kaynaklarda oturum açmak istediğinde daha az odaklanır. Bu çerçevede, Azure AD, işletmeden işletmeye (B2B) işbirliğiyle tüketici/müşteri veya vatandaşlık 'e yönelik uygulamalar (işletmeler arası veya B2C) için erişim yönetimine kadar çeşitli senaryolar destekler.

- **Uygulamalarınızı ve kaynaklarınızı dış kullanıcılarla (B2B işbirliği) paylaşabilirsiniz**. Dış kullanıcıları kendi kiracınıza (yetkilendirme için), mevcut kimlik bilgilerini (kimlik doğrulama için) kullanmalarına izin vermek için izin atayabilmenizi sağlayan "Konuk" kullanıcılar olarak kendi kiracınıza davet edebilirsiniz. Kullanıcılar, iş, okul veya diğer e-posta hesapları ile basit bir davet ve kullanım süreci kullanarak paylaşılan kaynaklarda oturum açabilirler. Ayrıca, [dış kullanıcılara erişimi yöneten](../governance/entitlement-management-external-users.md#how-access-works-for-external-users)ilkeleri yapılandırmak IÇIN [Azure AD Yetkilendirme Yönetimi](../governance/entitlement-management-overview.md) de kullanabilirsiniz. Artık [self servis kaydolma Kullanıcı akışlarının](self-service-sign-up-overview.md)kullanılabilirliğiyle, dış kullanıcıların uygulamalara kaydolmalarına izin verebilirsiniz. Deneyim, bir iş, okul veya sosyal kimlik (Google veya Facebook gibi) ile kaydolma izni verecek şekilde özelleştirilebilir. Kaydolma işlemi sırasında kullanıcı hakkındaki bilgileri de toplayabilirsiniz. Daha fazla bilgi için bkz. [Azure AD B2B belgeleri](index.yml).

- **Tüketici ve müşterilere yönelik uygulamalar için bir beyaz etiketli kimlik yönetimi çözümüyle Kullanıcı yolculukları oluşturun (Azure AD B2C)**. Müşterilere yönelik uygulamalar oluşturan bir iş veya geliştiricisiyseniz Azure AD B2C kullanarak milyonlarca tüketiciye, müşteriye veya vatandaşları ölçeklendirebilirsiniz. Geliştiriciler, Azure AD 'yi uygulamalarına yönelik tam özellikli müşteri kimliği ve erişim yönetimi (CıHAR) sistemi olarak kullanabilir. Müşteriler, zaten oluşturmuş oldukları bir kimlikle (Facebook veya Gmail gibi) oturum açabilir. Azure AD B2C sayesinde, uygulamalarınızı kullanırken müşterilerin nasıl kaydolup oturum açdığını ve profillerini nasıl yönetebileceğinizi denetleyebilir. Daha fazla bilgi için [Azure AD B2C belgelerine](../../active-directory-b2c/index.yml)bakın.

## <a name="compare-external-identities-solutions"></a>Dış Kimlikler çözümlerini karşılaştırma

Aşağıdaki tabloda, Azure AD dış kimlikleriyle etkinleştirebileceğiniz senaryolara ilişkin ayrıntılı bir karşılaştırma sunulmaktadır.

|   | Dış Kullanıcı işbirliği (B2B) | Tüketici/müşteriye yönelik uygulamalara erişim (B2C)  |
| ---- | --- | --- |
| **Birincil senaryo** | Microsoft uygulamaları (Microsoft 365, takımlar, vb.) veya kendi uygulamalarınızı (SaaS uygulamaları, özel geliştirmiş uygulamalar vb.) kullanarak işbirliği yapın.  | Modern SaaS veya özel olarak geliştirilen uygulamalar için kimlik ve erişim yönetimi (birinci taraf Microsoft uygulamaları değil).   |
| **İçin tasarlanan**    | Sağlayıcılar, iş ortakları ve satıcılar gibi harici kuruluşların iş ortaklarıyla işbirliği yapma. Kullanıcılar, dizininizde Konuk Kullanıcı olarak görünürler. Bu kullanıcılar bu kullanıcı tarafından yönetilmeyebilir veya yönetilmeyebilir.  | Ürününüzün müşterileri. Bu kullanıcılar ayrı bir Azure AD dizininde yönetilir.  |
| **Desteklenen kimlik sağlayıcıları** | Dış kullanıcılar iş hesapları, okul hesapları, herhangi bir e-posta adresi, SAML ve WS-Fed tabanlı kimlik sağlayıcıları, Gmail ve Facebook kullanarak işbirliği yapabilirler.  | Yerel uygulama hesapları olan tüketici kullanıcıları (herhangi bir e-posta adresi veya Kullanıcı adı), desteklenen çeşitli sosyal kimlikler ve doğrudan Federasyon aracılığıyla şirket ve kamu tarafından verilen kimliklere sahip kullanıcılar.       |
| **Dış Kullanıcı Yönetimi**   | Dış kullanıcılar, çalışanlarla aynı dizinde yönetilir, ancak genellikle Konuk kullanıcılar olarak açıklanmalıdır. Konuk kullanıcılar, çalışanlarla aynı gruplara eklenen aynı şekilde yönetilebilir ve bu şekilde devam edebilir.    | Dış kullanıcılar Azure AD B2C dizininde yönetilir. Bunlar, kuruluşun çalışanı ve iş ortağı dizininden (varsa) ayrı olarak yönetilirler.  |
| **Çoklu oturum açma (SSO)**      | Tüm Azure AD ile bağlantılı uygulamalara yönelik SSO desteklenir. Örneğin, Microsoft 365 veya şirket içi uygulamalara ve Salesforce veya Workday gibi diğer SaaS uygulamalarına erişim sağlayabilirsiniz.    | Azure AD B2C kiracılarında müşterilere ait uygulamalarda SSO desteklenir. Microsoft 365 veya diğer Microsoft SaaS uygulamalarına yönelik SSO desteklenmez.    |
| **Güvenlik ilkesi ve uyumluluk**        | Konak/kuruluş tarafından yönetilir (örneğin, [koşullu erişim ilkeleriyle](conditional-access.md)). | Koşullu erişim ve kimlik koruması aracılığıyla kuruluş tarafından yönetilir.        |
| **Marka**  | Kuruluşun markasını barındırmak/davet etmek kullanılır.    | Uygulama veya kuruluş başına tamamen özelleştirilebilir marka.   |
| **Faturalama modeli** | Aylık etkin kullanıcıları (MAU) temel alan [dış kimlikler fiyatlandırması](https://azure.microsoft.com/en-us/pricing/details/active-directory/external-identities/) . <br>(Ayrıca bkz: [B2B kurulum ayrıntıları](external-identities-pricing.md)) | Aylık etkin kullanıcıları (MAU) temel alan [dış kimlikler fiyatlandırması](https://azure.microsoft.com/en-us/pricing/details/active-directory/external-identities/) . <br>(Ayrıca bkz: [B2C kurulum ayrıntıları](../../active-directory-b2c/billing.md)) |
| **Daha fazla bilgi** | [Blog gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Belgeler](what-is-b2b.md)                   | [Ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [Belgeler](../../active-directory-b2c/index.yml)       |

Azure AD dış kimlikleri ile kurumsal sınırlarınızın ötesinde müşterileri ve iş ortaklarını güvenli hale getirin ve yönetin.

## <a name="about-multitenant-applications"></a>Çok kiracılı uygulamalar hakkında

Bir hizmet olarak uygulama sağlıyorsanız ve müşterilerinizin Kullanıcı hesaplarını yönetmek istemiyorsanız, çok kiracılı bir uygulama muhtemelen sizin için doğru seçimdir. Diğer Azure AD kiracılarına yönelik uygulamalar geliştirirken, kullanıcıları tek bir kuruluştan (tek bir kiracı) veya bir Azure AD kiracısına (çok kiracılı uygulamalar) sahip olan herhangi bir kuruluştan hedefleyebilirsiniz. Azure AD 'deki Uygulama kayıtları varsayılan olarak tek kiracıya sahiptir, ancak kaydınızı çok kiracılı hale getirebilirsiniz. Bu çok kiracılı uygulama, kendi Azure AD 'niz tarafından bir kez kaydoldu. Ancak, herhangi bir kuruluştan herhangi bir Azure AD kullanıcısı, sizin bölüminizden ek çalışma gerektirmeden uygulamayı kullanabilir. Daha fazla bilgi için bkz. [çok kiracılı uygulamalarda kimlik yönetimi](/azure/architecture/multitenant-identity/), [nasıl yapılır Kılavuzu](../develop/howto-convert-app-to-be-multi-tenant.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure AD B2C hakkında](../../active-directory-b2c/overview.md)