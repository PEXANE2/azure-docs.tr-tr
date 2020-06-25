---
title: Dış kimlikleri Karşılaştır-Azure Active Directory | Microsoft Docs
description: Azure AD dış kimlikleri, kuruluşunuzun dışındaki kişilerin kendi kimliklerini kullanarak uygulamalarınıza ve kaynaklarınıza erişmesine izin verir. Azure Active Directory B2B işbirliği ve Azure AD B2C dahil olmak üzere dış kimliklerin çözümlerini karşılaştırın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c45aae35c7c1cf2fbde436303a2ac000dfdd5ec
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85339609"
---
# <a name="compare-solutions-for-external-identities-in-azure-active-directory"></a>Azure Active Directory dış kimliklerin çözümlerini karşılaştırın

Azure AD 'de dış kimliklerle, kuruluşunuzun dışındaki kişilerin uygulamalarınıza ve kaynaklarınıza erişmesine izin vererek tercih ettikleri kimliği kullanarak oturum açmasını sağlayabilirsiniz. İş ortaklarınıza, dağıtımlarınız, tedarikçilerleriniz, satıcılarınız ve diğer konuk kullanıcılarınız "kendi kimliklerini getirebilir". Azure AD veya başka bir BT tarafından yönetilen bir sistemin parçası olup olmadığı ya da Google veya Facebook gibi yönetilmeyen bir sosyal kimliğe sahip olup olmadıklarında, oturum açmak için kendi kimlik bilgilerini kullanabilirler. Kimlik sağlayıcısı, dış kullanıcının kimliğini yönetir ve kaynaklarınızın korunmasını sağlamak için Azure AD ile uygulamalarınıza erişimi yönetirsiniz. 

## <a name="external-identities-scenarios"></a>Dış kimlikler senaryoları

Azure AD dış kimlikleri, bir kullanıcının kuruluşunuzla olan ilişkisine ve bir bireyin uygulama ve kaynaklarınız üzerinde oturum açmak istediği şekilde daha az odaklanır. Bu çerçevede, Azure AD, işletmeler arası (B2B) işbirliğiyle müşterilere ve tüketicilere yönelik uygulama geliştirmeye (işletmeler arası veya B2C) çok çeşitli senaryoları destekler.

- **Uygulamaları dış kullanıcılarla (B2B işbirliği) paylaşabilirsiniz**. Dış kullanıcıları kendi kiracınıza, mevcut kimlik bilgilerini (kimlik doğrulama için) kullanmalarına izin verirken (yetkilendirme için) izin atayabilmenizi sağlayan "Konuk" kullanıcılar olarak kendi kiracınıza davet edebilirsiniz. Kullanıcılar, iş hesabı, okul hesabı veya herhangi bir e-posta hesabıyla basit bir davet ve kullanım süreci kullanarak paylaşılan kaynaklarda oturum açabilirler. Artık self servis kaydolma Kullanıcı akışlarının (Önizleme) kullanılabilirliğine göre, paylaşmak istediğiniz uygulama aracılığıyla dış kullanıcılarınız için de bir oturum açma deneyimi sağlayabilirsiniz. Kullanıcı akış ayarlarını, kullanıcının uygulama için nasıl oturum açdığına ve iş hesabını, okul hesabını ya da kullanmak istedikleri sosyal kimlik (Google veya Facebook gibi) kullanmasına izin veren şekilde yapılandırabilirsiniz.  Daha fazla bilgi için bkz. [Azure AD B2B belgeleri](index.yml).

- **Diğer Azure AD kiracılar (tek kiracılı veya çok kiracılı) için tasarlanan uygulamalar geliştirin**. Azure AD için uygulama geliştirirken, kullanıcıları tek bir kuruluştan (tek bir kiracı) veya bir Azure AD kiracısına sahip olan herhangi bir kuruluştan (çok kiracılı uygulamalar olarak adlandırılır) hedefleyebilirsiniz. Bu çok kiracılı uygulamalar kendi Azure AD 'niz tarafından bir kez kaydedilir, ancak bundan sonra herhangi bir kuruluştan herhangi bir Azure AD kullanıcısı tarafından herhangi bir ek iş olmadan herhangi bir kuruluştan kullanılabilir.

- **Tüketiciler ve müşteriler için beyaz etiketli uygulamalar geliştirin (Azure AD B2C)**. Müşterilere yönelik uygulamalar oluşturan bir iş veya geliştiricisiyseniz, Azure AD B2C kullanarak tüketicilere, müşterilere veya vatandaşları göre ölçeklendirebilirsiniz. Geliştiriciler, Azure AD 'yi uygulaması için tam özellikli kimlik sistemi olarak kullanabilir ve müşterilerin zaten oluşturmuş oldukları bir kimlikle (Facebook veya Gmail gibi) oturum açmalarına izin verir. Azure AD B2C sayesinde, uygulamalarınızı kullanırken müşterilerin nasıl kaydolup oturum açdığını ve profillerini nasıl yönetebileceğinizi denetleyebilir. Daha fazla bilgi için [Azure AD B2C belgelerine](https://docs.microsoft.com/azure/active-directory-b2c/)bakın.

Aşağıdaki tabloda, Azure AD dış kimlikleriyle etkinleştirebileceğiniz çeşitli senaryolara ilişkin ayrıntılı bir karşılaştırma sağlanır.

| Çok kiracılı uygulamalar  | Dış Kullanıcı işbirliği (B2B) | Tüketiciler veya müşteriler için uygulamalar (B2C)  |
| ---- | --- | --- |
| Birincil senaryo: kurumsal hizmet olarak yazılım (SaaS) | Birincil senaryo: Microsoft uygulamalarını (Microsoft 365, takımları,...) veya kendi işbirliği yazılımınızı kullanarak Işbirliği yapın.  | Birincil senaryo: özel geliştirilmiş uygulamalar kullanan Işlem uygulamaları.   |
| Hedeflenen: birçok kurumsal müşteriye yazılım sağlamak isteyen kuruluşlar.    | Hedeflenen: Kimlik sağlayıcısına bakılmaksızın bir üst kuruluştaki kullanıcıların kimliğini doğrulayabilmek isteyen kuruluşlar.    | İçin tasarlanan: kişiler, kurum veya kurumsal müşterilerin kendi kuruluşunuzun dizininden ayrı bir Azure AD dizinine sahip olup olmadığı mobil ve Web uygulamalarınızın müşterilerini davet edin. |
| Desteklenen kimlikler: Azure AD hesapları olan çalışanlar. | Desteklenen kimlikler: İş veya okul hesapları olan çalışanlar, iş ya da okul hesapları veya herhangi bir e-posta adresi olan iş ortakları. Yakında doğrudan federasyon desteklenecektir.      | Desteklenen kimlikler: Yerel uygulama hesapları (herhangi bir e-posta adresi veya kullanıcı adı) ya da doğrudan federasyon özellikli herhangi bir desteklenen sosyal kimliği olan tüketici kullanıcılar.       |
| Dış kullanıcılar kendi dizininde yönetilir ve uygulamanın kaydedildiği dizinden yalıtılır.    | Dış kullanıcılar, çalışanlarla aynı dizinde yönetilir, ancak özellikle açıklama eklenir. Bunlar, çalışanlarla aynı şekilde yönetilebilir, aynı gruplara eklenebilir ve bu şekilde devam edebilir.    | Dış kullanıcılar, uygulama dizininde yönetilir. Bunlar, kuruluşun çalışanı ve iş ortağı dizininden (varsa) ayrı olarak yönetilirler.  |
| Çoklu oturum açma: Azure AD ile bağlantılı tüm uygulamalara yönelik SSO desteklenir.          | Çoklu oturum açma: Azure AD ile bağlantılı tüm uygulamalara yönelik SSO desteklenir. Örneğin, Microsoft 365 veya şirket içi uygulamalara ve Salesforce veya Workday gibi diğer SaaS uygulamalarına erişim sağlayabilirsiniz.    | Çoklu oturum açma: Azure AD B2C kiracılar içindeki müşterinin sahip olduğu uygulamalara yönelik SSO desteklenir. Microsoft 365 veya diğer Microsoft SaaS uygulamalarına yönelik SSO desteklenmez.    |
| Müşteri yaşam döngüsü: kullanıcının ana organizasyonu tarafından yönetiliyor.      | İş ortağı yaşam döngüsü: Ana bilgisayar/davet eden kuruluş tarafından yönetilir.    | Müşteri yaşam döngüsü: Self servistir veya uygulama tarafından yönetilir.      |
| Güvenlik ilkesi ve uyumluluğu: konak/kuruluş tarafından yönetilir (örneğin, [koşullu erişim ilkeleriyle](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).           | Güvenlik ilkesi ve uyumluluğu: konak/kuruluş tarafından yönetilir (örneğin, [koşullu erişim ilkeleriyle](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)). | Güvenlik ilkesi ve uyumluluk: Uygulama tarafından yönetilir.        |
| Marka: kuruluşun markasını barındırmak/davet etmek kullanılır.   | Marka: kuruluşun markasını barındırmak/davet etmek kullanılır.    | Marka: Uygulama tarafından yönetilir. Genellikle kuruluşun arka planda soluk olduğu şekilde ürün markalı olma eğilimindedir.   |
| Daha fazla bilgi: [çok kiracılı uygulamalardaki kimlik yönetimi](https://docs.microsoft.com/azure/architecture/multitenant-identity/), [nasıl yapılır Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/howto-convert-app-to-be-multi-tenant) | Daha fazla bilgi: [Blog gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Belgeler](what-is-b2b.md)                   | Daha fazla bilgi: [Ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [Belgeler](https://docs.microsoft.com/azure/active-directory-b2c/)       |

Azure AD dış kimlikleri ile kurumsal sınırlarınızın ötesinde müşterileri ve iş ortaklarını güvenli hale getirin ve yönetin.

### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure AD B2C hakkında](https://docs.microsoft.com/azure/active-directory-b2c/overview)
