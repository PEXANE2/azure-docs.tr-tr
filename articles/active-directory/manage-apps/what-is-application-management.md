---
title: Azure Active Directory ile Uygulamaları Yönetme | Microsoft Docs
description: Bulut ve şirket içi uygulamalarınız için kimlik ve erişim yönetimi (ıAM) sistemi olarak Azure Active Directory (AD) kullanılmasına genel bakış.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28b04828bc778a2b9076c9ea1970ce80f231fbe3
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386019"
---
# <a name="what-is-application-management"></a>Uygulama yönetimi nedir?

Azure AD, bir kimlik ve erişim yönetimi (ıAM) sistemidir. Bu, dijital kimlikler hakkında bilgi depolamak için tek bir yer sağlar. Yazılım uygulamalarınızı, Kullanıcı bilgilerinin depolandığı yer olarak Azure AD 'yi kullanacak şekilde yapılandırabilirsiniz. 

Azure AD bir uygulamayla tümleştirilecek şekilde yapılandırılmalıdır. Diğer bir deyişle, bir kimlik sistemi olarak hangi uygulamaların kullandığını bilmeleri gerekir. Azure AD 'nin bu uygulamalardan haberdar olduğu ve bunların nasıl işlenmesi gerektiği, uygulama yönetimi olarak bilinen bir işlemdir.

Uygulamaları, Azure Active Directory portalının Yönet bölümünde yer alan **Kurumsal uygulamalar** dikey penceresinde yönetirsiniz.

![Azure AD portalının Yönet bölümünün altındaki kurumsal uygulamalar seçeneği.](media/what-is-application-management/enterprise-applications-in-nav.png)

## <a name="what-is-an-identity-and-access-management-iam-system"></a>Kimlik ve erişim yönetimi (ıAM) sistemi nedir?
Uygulama, bir amaç için kullanılan bir yazılım parçasıdır. Çoğu uygulama, uygulamanın söz konusu Kullanıcı için özel bir deneyim sağlayabilmesi için kullanıcıların oturum açmasını gerektirir. Diğer bir deyişle uygulamanın, uygulamayı kullanarak kullanıcının kimliğini bilmeleri gerekir. Bu, Kullanıcı için teklif edilecek veya kaldırılacak işlevselliği bildiği için.

Her uygulama, kullanıcıları ayrı ayrı izleirse sonuç, her uygulama için farklı Kullanıcı adları ve oturum açma işlemleri için bir silmeydir. Bir uygulama diğer uygulamalardaki kullanıcılarla ilgili hiçbir şeyi bilmez.

Merkezi bir kimlik sistemi, daha sonra tüm uygulamalar tarafından kullanılabilecek Kullanıcı bilgilerini depolamak için tek bir yer sağlayarak bu sorunu çözer. Bu sistemler, kimlik ve erişim yönetimi (ıAM) sistemleri olarak bilinmelidir. Azure Active AD, Microsoft bulutu için ıAM sistemidir.

>[!TIP]
>Bir ıAM sistemi, Kullanıcı kimliklerini izlemek için tek bir yer sağlar. Azure AD, Microsoft bulutu için ıAM sistemidir.


## <a name="why-manage-applications-with-a-cloud-solution"></a>Neden bir bulut çözümüyle uygulamalar yönetilmelidir?

Kuruluşların çoğu zaman kullanıcıların işlerini yapmak için bağımlı oldukları yüzlerce uygulaması vardır. Kullanıcılar birçok cihaz ve konumdan bu uygulamalara erişir. Her gün yeni uygulamalar eklenir ve geliştirilir. Bu kadar çok uygulama ve erişim noktası sayesinde, tüm uygulamalara Kullanıcı erişimini yönetmek için bulut tabanlı bir çözüm kullanmak her zamankinden daha önemlidir.

>[!TIP]
>Azure AD Uygulama Galerisi, Azure AD ile bir kimlik sağlayıcısı olarak çalışmak üzere önceden yapılandırılmış çok sayıda popüler uygulama içerir.

## <a name="how-does-azure-ad-work-with-applications"></a>Azure AD, uygulamalarla nasıl çalışır?

Azure AD, bulut ve şirket içi uygulamalarınız için tek bir kimlik sistemi sağlayarak uygulamalarınızı yönetmenin yolunu basitleştirir. Hizmet olarak yazılım (SaaS) uygulamaları, şirket içi uygulamalar ve iş kolu (LOB) uygulamalarını Azure AD 'ye ekleyebilirsiniz. Ardından kullanıcılar bir kez oturum açarak Office 365 ve Microsoft 'un diğer iş uygulamalarıyla birlikte bu uygulamalara güvenli ve sorunsuz bir şekilde erişin. [Kullanıcı sağlamayı otomatikleştirerek](../app-provisioning/user-provisioning.md)yönetim maliyetlerini azaltabilirsiniz. Ayrıca, güvenli uygulama erişimi sağlamak için Multi-Factor Authentication ve koşullu erişim ilkelerini de kullanabilirsiniz.

![Azure AD aracılığıyla federe olan uygulamaları gösteren diyagram](media/what-is-application-management/app-management-overview.png)

## <a name="what-types-of-applications-can-i-integrate-with-azure-ad"></a>Azure AD ile tümleştirilebilen uygulama türleri nelerdir?

**Kurumsal uygulamalarınıza** ekleyebileceğiniz ve Azure AD ile yönetebileceğiniz dört ana uygulama türü vardır:

- Azure AD **Galeri uygulamaları** – Azure AD, Azure AD ile çoklu oturum açma için önceden tümleştirilmiş binlerce uygulamayı içeren bir galeriye sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. [Uygulama tümleştirmenizi planlama](plan-an-application-integration.md)veya [SaaS uygulama öğreticilerinde](https://docs.microsoft.com/azure/active-directory/saas-apps/)tek tek uygulamalar için ayrıntılı tümleştirme adımları alma hakkında bilgi edinin.

- **Uygulama proxy 'si ile şirket içi uygulamalar** – Azure AD uygulama ara sunucusu ile şirket içi Web UYGULAMALARıNıZı Azure AD ile tümleştirebilir ve çoklu oturum açma desteği sağlayabilirsiniz. Ardından, son kullanıcılar şirket içi Web uygulamalarınıza Office 365 ve diğer SaaS uygulamalarına erişen şekilde erişebilir, bkz. [Azure AD 'Nin uygulama proxy 'si aracılığıyla şirket içi uygulamalara uzaktan erişim sağlama](application-proxy.md).

- **Özel geliştirmiş uygulamalar** – kendi iş kolu uygulamalarınızı oluştururken, çoklu oturum açmayı desteklemek Için BUNLARı Azure AD ile tümleştirebilirsiniz. Uygulamanızı Azure AD 'ye kaydederek, uygulamanın kimlik doğrulama ilkesi üzerinde denetiminiz vardır. Daha fazla bilgi için bkz. [geliştiricilere yönelik kılavuz](developer-guidance-for-integrating-applications.md).

- **Galeri olmayan uygulamalar** – kendi uygulamalarınızı getirin! Azure AD 'ye ekleyerek diğer uygulamalar için çoklu oturum açmayı destekler. Bir uygulamayı tümleştirmenin birden çok yolu vardır ve bunlardan bazıları aşağıda listelenmiştir. Daha fazla bilgi için bkz. [SAML çoklu oturum açmayı yapılandırma](configure-saml-single-sign-on.md).

>[!TIP]
>Azure AD 'yi önceden yapılandırılmış olmasa ve uygulama galerisinde bile bir uygulamayla tümleştirebilirsiniz. **Azure AD 'yi** aşağıdakilerden biriyle tümleştirebilirsiniz
> - Bir **Kullanıcı adı ve parola alanı**işleyen herhangi bir Web bağlantısı veya uygulama.
> - **SAML veya OpenID Connect protokollerini**destekleyen herhangi bir uygulama.
> - **Etki alanları arası kimlik yönetimi (SCıM) standardı Için sistemi** destekleyen herhangi bir uygulama.

## <a name="manage-risk-with-conditional-access-policies"></a>Koşullu erişim ilkeleriyle riskleri yönetme

[Koşullu erişim](../conditional-access/concept-conditional-access-cloud-apps.md) Ile Azure AD çoklu oturum açma (SSO), uygulamalara erişmek için yüksek düzeyde güvenlik sağlar. Güvenlik özellikleri, bulut ölçeğinde kimlik koruması, risk tabanlı erişim denetimi, yerel Multi-Factor Authentication ve koşullu erişim ilkelerini içerir. Bu özellikler, uygulamalara veya yüksek düzeyde güvenlik gereken gruplara göre ayrıntılı denetim ilkelerine olanak sağlar.

## <a name="improve-productivity-with-single-sign-on"></a>Çoklu oturum açma ile üretkenliği artırma

Uygulamalar ve Office 365 arasında çoklu oturum açma (SSO) etkinleştirildiğinde, oturum açma istemleri azaltılarak veya ortadan kaldırılarak mevcut kullanıcılar için üstün bir oturum açma deneyimi sağlanır. Birden çok istem veya birden çok parolayı yönetme gereksinimi olmadan kullanıcının ortamı çok daha bütünlüklü ve daha az rahatsız edici olur. İş grubu, self servis ve dinamik üyelik aracılığıyla erişimi yönetebilir ve onaylayabilir. İşletmede doğru kişilerin bir uygulamaya erişmesine izin verilmesi, kimlik sisteminin güvenliğini artırır.

SSO, güvenliği artırır. *Çoklu oturum açma olmadan* yöneticilerin her bir bireysel uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir; bu da zaman alır. Ayrıca kullanıcıların uygulamalarına erişmek için birden çok kimlik bilgisini izlemesi gerekir. Sonuç olarak kullanıcılar, parolalarını yazma veya başka parola yönetim çözümlerini kullanma eğilimindedir; bu da veri güvenliği riskleri oluşturur. [Çoklu oturum açma hakkında daha fazla bilgi edinin](what-is-single-sign-on.md).

## <a name="address-governance-and-compliance"></a>İdare ve uyumu ele alma

Azure AD ile, Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarından yararlanan raporlar aracılığıyla uygulama oturum açma işlemlerini izleyebilirsiniz. Portaldan veya API’lerden raporlara erişebilirsiniz. Uygulamalarınıza kimlerin erişiminin olduğunu programlama yoluyla denetleyin ve erişim gözden geçirmeleri yoluyla etkin olmayan kullanıcılara erişimi kaldırın.

## <a name="manage-costs"></a>Maliyetleri yönetme

Azure AD’ye geçirerek maliyet tasarrufu yapabilir ve şirket içi altyapınızı yönetme zahmetini ortadan kaldırabilirsiniz. Azure AD, uygulamalara self servis erişimi de sağlar; böylece hem yöneticiler hem de kullanıcılar için zaman tasarrufu sağlanır. Çoklu oturum açma, uygulamaya özgü parolaları ortadan kaldırır. Bir kez oturum açma, uygulamalar için parola sıfırlama ve parolalar alınırken oluşan üretkenlik kaybı ile ilgili maliyetlerden tasarruf sağlar.

Insan kaynakları odaklı uygulamalar veya büyük bir kullanıcı kümesine sahip diğer uygulamalar için, uygulama sağlama özelliğinden yararlanarak kullanıcıları hazırlama ve sağlama sürecini otomatik hale getirebilirsiniz, bkz. [uygulama sağlama nedir?](../app-provisioning/user-provisioning.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD 'de uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md)
- [Uygulama tümleştirmesini kullanmaya başlayın](plan-an-application-integration.md)
- [Sağlamayı otomatikleştirmeyi öğrenin](../app-provisioning/user-provisioning.md)
