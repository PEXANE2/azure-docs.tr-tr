---
title: Azure AD 'yi uygulamalarla tümleştirmeyle çalışmaya başlama
description: Bu makale, Azure Active Directory (AD) Şirket içi uygulamalarla ve bulut uygulamalarıyla tümleştirmek için bir başlangıç kılavuzudur.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: kenwith
ms.reviewer: asteen
ms.openlocfilehash: 8b321acb00e6e9b4b6cca117afba8bf0c9432719
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99258475"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory uygulamalar ile tümleştirme kılavuzunu kullanmaya başlama

Bu konu, uygulamaları Azure Active Directory (AD) ile tümleştirme işlemini özetler. Aşağıdaki bölümde, bu başlangıç kılavuzunun hangi bölümlerinin sizin için uygun olduğunu belirleyebilmeniz için daha ayrıntılı bir konunun kısa bir özeti yer almaktadır.

Derinlemesine dağıtım planlarını indirmek için bkz. [sonraki adımlar](#next-steps).

## <a name="take-inventory"></a>Sayım yapın
Uygulamaları Azure AD ile tümleştirmadan önce nerede olduğunuzu ve nereye gitmek istediğinizi bilmeniz önemlidir.  Aşağıdaki sorular, Azure AD uygulama tümleştirme projeniz hakkında düşünmenize yardımcı olmaya yöneliktir.

### <a name="application-inventory"></a>Uygulama envanteri
* Tüm uygulamalarınız nerede? Kimin sahibi kim?
* Uygulamalarınız için ne tür bir kimlik doğrulaması gerekir?
* Hangi uygulamalara erişmesi gerekiyor?
* Yeni bir uygulama dağıtmak istiyor musunuz?
  * Şirket içinde oluşturacak ve bir Azure işlem örneğinde dağıtacaksınız musunuz?
  * Azure Uygulama galerisinde kullanılabilir olan birini kullanacaksınız misiniz?

### <a name="user-and-group-inventory"></a>Kullanıcı ve grup envanteri
* Kullanıcı hesaplarınız nerede bulunur?
  * Şirket içi Active Directory
  * Azure AD
  * Sahip olduğunuz ayrı bir uygulama veritabanı içinde
  * Tasdikli olmayan uygulamalarda
  * Yukarıdakilerin tümü
* Şu anda bireysel kullanıcı olan izinler ve rol atamaları nelerdir? Erişimini gözden geçirmeniz veya Kullanıcı erişiminizin ve rol atamalarınızın şimdi uygun olduğundan emin misiniz?
* Gruplar şirket içi Active Directory zaten kurulu mı?
  * Gruplarınız nasıl düzenlenir?
  * Grup üyeleri kim?
  * Şu anda grupları olan izinler/rol atamaları var mı?
* Tümleştirmadan önce Kullanıcı/Grup veritabanlarını temizlemeniz gerekecek mi?  (Bu oldukça önemli bir sorudır. Çöp kutusu, atık çıkış.)

### <a name="access-management-inventory"></a>Erişim yönetimi envanteri
* Şu anda uygulamalara Kullanıcı erişimini nasıl yönetirsiniz? Bunun değiştirilmesi gerekiyor mu?  Örneğin [Azure RBAC](../../role-based-access-control/role-assignments-portal.md) gibi farklı erişimi yönetmenin diğer yollarını kabul etmeniz mi gerekiyor?
* Neye ne erişmesi gerekiyor?

Belki de tüm bu sorulara yanıt verme, ancak bu sorun.  Bu kılavuz, bu sorulardan bazılarını cevaplamanıza ve bazı bilinçli kararlar almanıza yardımcı olabilir.

### <a name="find-unsanctioned-cloud-applications-with-cloud-discovery"></a>Cloud Discovery ile tasdiksiz bulut uygulamalarını bulma

Yukarıda belirtildiği gibi, kuruluşunuz tarafından yönetilene kadar yönetilmeyen uygulamalar olabilir.  Envanter sürecinin bir parçası olarak, tasdik edilmemiş bulut uygulamalarını bulmak mümkündür. Bkz. [set up Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="integrating-applications-with-azure-ad"></a>Uygulamaları Azure AD ile tümleştirme
Aşağıdaki makalelerde, uygulamaların Azure AD ile tümleştirilmesine yönelik farklı yollar ele alınmaktadır ve bazı kılavuzluk sağlanmaktadır.

* [Hangi Active Directory kullanacağınızı belirleme](../fundamentals/active-directory-whatis.md)
* [Azure Uygulama galerisinde uygulama kullanma](what-is-single-sign-on.md)
* [SaaS uygulamalarını tümleştirme öğreticiler listesi](../saas-apps/tutorial-list.md)

### <a name="authentication-types"></a>Kimlik doğrulama türleri
Uygulamalarınızın her biri farklı kimlik doğrulama gereksinimlerine sahip olabilir. Azure AD ile imzalama sertifikaları, SAML 2,0, WS-Federation veya OpenID Connect protokollerinin yanı sıra parola çoklu oturum açma 'yı kullanan uygulamalarla kullanılabilir. Azure AD ile kullanım için uygulama kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. Azure Active Directory ve [parola tabanlı çoklu oturum açma](what-is-single-sign-on.md) [içindeki federe tek Sign-On için sertifikaları yönetme](manage-certificates-for-federated-single-sign-on.md) .

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure AD Uygulaması Proxy ile SSO 'yu etkinleştirme
Microsoft Azure AD uygulama proxy 'Si ile, özel ağınızın içinde bulunan uygulamalara, her yerden ve herhangi bir cihazdan erişim sağlayabilirsiniz. Ortamınızda bir uygulama proxy Bağlayıcısı yükledikten sonra, Azure AD ile kolayca yapılandırılabilir.

### <a name="integrating-custom-applications"></a>Özel uygulamaları tümleştirme
Özel uygulamanızı Azure Uygulama galerisine eklemek istiyorsanız bkz. [uygulamanızı Azure AD uygulama galerisine yayımlama](../develop/v2-howto-app-gallery-listing.md).

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
Aşağıdaki makalelerde, Azure AD bağlayıcıları ve Azure AD 'yi kullanarak Azure AD ile tümleştirildiğinde uygulamalara erişimi yönetme yolları açıklanır.

* [Azure AD kullanarak uygulamalara erişimi yönetme](what-is-access-management.md)
* [Azure AD bağlayıcıları ile otomatikleştirme](../app-provisioning/user-provisioning.md)
* [Uygulamaya kullanıcı atama](./assign-user-or-group-access-portal.md)
* [Uygulamaya grup atama](./assign-user-or-group-access-portal.md)
* [Hesapları paylaşma](../enterprise-users/users-sharing-accounts.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi için, [GitHub](../fundamentals/active-directory-deployment-plans.md)'dan Azure Active Directory dağıtım planlarını indirebilirsiniz. Galeri uygulamaları için, [Azure Portal](https://portal.azure.com)aracılığıyla çoklu oturum açma, koşullu erişim ve Kullanıcı sağlama için dağıtım planlarını indirebilirsiniz. 

Azure portal bir dağıtım planı indirmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Kurumsal uygulamaları** seçin  |  **bir uygulama**  |  **dağıtım planı** seçin.

Lütfen [dağıtım planı anketini](https://aka.ms/DeploymentPlanFeedback)alarak dağıtım planlarında geri bildirim sağlayın.