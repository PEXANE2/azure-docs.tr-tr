---
title: Azure Active Directory ile Uygulamaları Yönetme | Microsoft Docs
description: Bu makalede, Azure Active Directory'yi şirket içi, bulut ve SaaS uygulamalarınız ile tümleştirmenin yararları açıklanmaktadır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 06/05/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ca73fac06649f801461e53130a67aa9ec0ad0d0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77063331"
---
# <a name="application-management-with-azure-active-directory"></a>Azure Active Directory ile uygulama yönetimi

Azure Active Directory (Azure AD), bulut ve şirket içi uygulamalarınız için tek bir kimlik sistemi sağlayarak uygulamalarınızı yönetme şeklinizi kolaylaştırır. Yazılımınızı hizmet (SaaS) uygulamaları, şirket içi uygulamalar ve iş hattı (LOB) uygulamalarını Azure AD'ye ekleyebilirsiniz. Ardından kullanıcılar, Office 365 ve Microsoft'un diğer iş uygulamalarıyla birlikte bu uygulamalara güvenli ve sorunsuz bir şekilde erişmek için bir kez oturum açın. [Kullanıcı sağlamayı otomatikleştirerek](../app-provisioning/user-provisioning.md)yönetim maliyetlerini azaltabilirsiniz. Güvenli uygulama erişimi sağlamak için çok faktörlü kimlik doğrulama ve Koşullu Erişim ilkelerini de kullanabilirsiniz.

![Azure AD üzerinden federe uygulamaları gösteren diyagram](media/what-is-application-management/app-management-overview.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>Neden bir bulut çözümüyle uygulamalar yönetilmelidir?

Kuruluşların çoğu zaman kullanıcıların işlerini yapmak için bağımlı oldukları yüzlerce uygulaması vardır. Kullanıcılar birçok cihaz ve konumdan bu uygulamalara erişir. Her gün yeni uygulamalar eklenir ve geliştirilir. Bu kadar çok uygulama ve erişim noktası yla, tüm uygulamalara kullanıcı erişimini yönetmek için bulut tabanlı bir çözüm kullanmak her zamankinden daha önemlidir.

## <a name="what-types-of-applications-can-i-integrate-with-azure-ad"></a>Azure AD ile ne tür uygulamalar entegre edebilirim?

**Kurumsal uygulamalarınıza** ekleyebileceğiniz ve Azure AD ile yönetebileceğiniz dört ana uygulama türü vardır:

- **Azure AD Galerisi uygulamaları** – Azure AD uygulamaları, Azure AD ile tek oturum açma için önceden tümleştirilmiş binlerce uygulama içeren bir galeriye sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. [Uygulama tümleştirmenizi planlama hakkında bilgi edinin](plan-an-application-integration.md)veya [SaaS uygulama öğreticilerinde](https://docs.microsoft.com/azure/active-directory/saas-apps/)tek tek uygulamalar için ayrıntılı entegrasyon adımları alın.

- **Uygulama Proxy ile şirket içi uygulamalar** – Azure AD Application Proxy ile şirket içi web uygulamalarınızı Azure AD ile tek bir oturum açmayı desteklemek için entegre edebilirsiniz. Böylece son kullanıcılar şirket içi web uygulamalarınıza Office 365 ve diğer SaaS uygulamalarına erişdikleri şekilde erişebilir. [Uygulama Proxy'sini neden kullanacağınızı ve nasıl çalıştığını öğrenin.](what-is-application-proxy.md)

- **Özel olarak geliştirilmiş uygulamalar** – Kendi iş hattı uygulamalarınızı geliştirirken, tek oturum açmayı desteklemek için bunları Azure AD ile tümleştirebilirsiniz. Uygulamanızı Azure AD'ye kaydederek, uygulamanın kimlik doğrulama ilkesi üzerinde denetime sahip olursunuz. Daha fazla bilgi [için geliştiriciler için kılavuza](developer-guidance-for-integrating-applications.md)bakın.

- **Galeri dışı uygulamalar** – Kendi uygulamalarınızı getirin! Azure AD'ye ekleyerek diğer uygulamalar için tek oturum açma'yı destekleyin. İstediğiniz herhangi bir web bağlantısını veya kullanıcı adı ve parola alanı oluşturan, SAML veya OpenID Connect protokollerini destekleyen veya SCIM'i destekleyen herhangi bir uygulamayı tümleştirebilirsiniz. Daha fazla bilgi için galeri dışı uygulamalar için tek oturum açma [yapılandırılması'na](configure-single-sign-on-non-gallery-applications.md)bakın.

## <a name="manage-risk-with-conditional-access-policies"></a>Koşullu Erişim ilkeleriyle riski yönetme

Azure AD tek oturum açma (SSO) [ile Koşullu Erişim'i](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) bağlama, uygulamalara erişmek için yüksek düzeyde güvenlik sağlar. Güvenlik yetenekleri bulut ölçekli kimlik koruması, risk tabanlı erişim denetimi, yerel çok faktörlü kimlik doğrulaması ve Koşullu Erişim ilkelerini içerir. Bu özellikler, uygulamalara veya yüksek düzeyde güvenlik gereken gruplara göre ayrıntılı denetim ilkelerine olanak sağlar.

## <a name="improve-productivity-with-single-sign-on"></a>Çoklu oturum açma ile üretkenliği artırma

Uygulamalar ve Office 365 arasında çoklu oturum açma (SSO) etkinleştirildiğinde, oturum açma istemleri azaltılarak veya ortadan kaldırılarak mevcut kullanıcılar için üstün bir oturum açma deneyimi sağlanır. Birden çok istem veya birden çok parolayı yönetme gereksinimi olmadan kullanıcının ortamı çok daha bütünlüklü ve daha az rahatsız edici olur. İş grubu, self servis ve dinamik üyelik aracılığıyla erişimi yönetebilir ve onaylayabilir. İşletmede doğru kişilerin bir uygulamaya erişmesine izin verilmesi, kimlik sisteminin güvenliğini artırır.

SSO, güvenliği artırır. *Çoklu oturum açma olmadan* yöneticilerin her bir bireysel uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir; bu da zaman alır. Ayrıca kullanıcıların uygulamalarına erişmek için birden çok kimlik bilgisini izlemesi gerekir. Sonuç olarak kullanıcılar, parolalarını yazma veya başka parola yönetim çözümlerini kullanma eğilimindedir; bu da veri güvenliği riskleri oluşturur. [Tek oturum açma hakkında daha fazla bilgi edinin.](what-is-single-sign-on.md)

## <a name="address-governance-and-compliance"></a>İdare ve uyumu ele alma

Azure AD ile, Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarından yararlanan raporlar aracılığıyla uygulama oturum açma işlemlerini izleyebilirsiniz. Portaldan veya API’lerden raporlara erişebilirsiniz. Uygulamalarınıza kimlerin erişiminin olduğunu programlama yoluyla denetleyin ve erişim gözden geçirmeleri yoluyla etkin olmayan kullanıcılara erişimi kaldırın.

## <a name="manage-costs"></a>Maliyetleri yönetme

Azure AD’ye geçirerek maliyet tasarrufu yapabilir ve şirket içi altyapınızı yönetme zahmetini ortadan kaldırabilirsiniz. Azure AD, uygulamalara self servis erişimi de sağlar; böylece hem yöneticiler hem de kullanıcılar için zaman tasarrufu sağlanır. Çoklu oturum açma, uygulamaya özgü parolaları ortadan kaldırır. Bir kez oturum açma, uygulamalar için parola sıfırlama ve parolalar alınırken oluşan üretkenlik kaybı ile ilgili maliyetlerden tasarruf sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusu nedir?](what-is-application-proxy.md)
- [Hızlı başlatma: Azure AD kiracınıza galeri uygulaması ekleme](add-application-portal.md)
