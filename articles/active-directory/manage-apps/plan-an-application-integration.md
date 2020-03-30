---
title: Azure AD'yi uygulamalarla tümleştirmeye başlayın | Microsoft Dokümanlar
description: Bu makale, Azure Active Directory'yi (AD) şirket içi uygulamalarve bulut uygulamalarıyla tümleştirmek için başlangıç kılavuzudur.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89b16a8479f8975d101b8a4e26dcb1885d9730bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063389"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory'yi başlangıç kılavuzuyla tümleştirme

Bu konu, uygulamaları Azure Etkin Dizini (AD) ile tümleştirme işlemini özetler. Aşağıdaki bölümlerin her biri daha ayrıntılı bir konunun kısa bir özetini içerir, böylece bu başlangıç kılavuzunun hangi bölümlerinin sizinle alakalı olduğunu belirleyebilirsiniz.

Ayrıntılı dağıtım planlarını indirmek için [Sonraki adımlara](#next-steps)bakın.

## <a name="take-inventory"></a>Envanter için
Uygulamaları Azure AD ile tümleştirmeden önce nerede olduğunuzu ve nereye gitmek istediğinizi bilmeniz önemlidir.  Aşağıdaki sorular, Azure AD uygulama tümleştirme projeniz hakkında düşünmenize yardımcı olmak için tasarlanmıştır.

### <a name="application-inventory"></a>Uygulama envanteri
* Tüm başvurularınız nerede? Onlara kim sahip?
* Uygulamalarınız ne tür bir kimlik doğrulaması gerektirir?
* Hangi uygulamalara kimlerin erişmesi gerekir?
* Yeni bir uygulama dağıtmak istiyor musunuz?
  * Şirket içinde inşa edip Azure işlem örneğinde dağıtacak mısın?
  * Azure Uygulama Galerisi'nde bulunan bir tane kullanacak sınız mı?

### <a name="user-and-group-inventory"></a>Kullanıcı ve grup envanteri
* Kullanıcı hesaplarınız nerede ikamet ediyor?
  * Şirket içi Active Directory
  * Azure AD
  * Sahip olduğunuz ayrı bir uygulama veritabanı içinde
  * Onaylanmamış uygulamalarda
  * Yukarıdakilerin tümü
* Tek tek kullanıcıların şu anda sahip olduğu izinler ve rol atamaları nelerdir? Erişimlerini gözden geçirmeniz mi gerekiyor yoksa kullanıcı erişiminizin ve rol atamalarınızın artık uygun olduğundan emin misiniz?
* Şirket içi Active Directory'nizde gruplar zaten kuruldu mu?
  * Gruplarınız nasıl organize ediliyor?
  * Grup üyeleri kimlerdir?
  * Grupların şu anda sahip olduğu izinler/rol atamaları nelerdir?
* Tümleştirmeden önce kullanıcı/grup veritabanlarını temizlemeniz gerekecek mi?  (Bu oldukça önemli bir sorudur. Çöp içeri, çöp dışarı.)

### <a name="access-management-inventory"></a>Erişim yönetimi envanteri
* Şu anda uygulamalara kullanıcı erişimini nasıl yönetebiliyorsun? Bunun değişmesi gerekiyor mu?  Örneğin [RBAC'da](../../role-based-access-control/role-assignments-portal.md) olduğu gibi erişimi yönetmenin başka yollarını düşündünmü mü?
* Kimin neye ihtiyacı var ki?

Belki tüm bu soruların cevaplarını önceden bilmiyorsunama sorun değil.  Bu kılavuz, bu soruların bazılarını yanıtlamanıza ve bazı bilinçli kararlar vermenize yardımcı olabilir.

### <a name="find-unsanctioned-cloud-applications-with-cloud-discovery"></a>Cloud Discovery ile onaylanmamış bulut uygulamalarını bulma

Yukarıda belirtildiği gibi, kuruluşunuz tarafından şimdiye kadar yönetilmeyen uygulamalar olabilir.  Envanter sürecinin bir parçası olarak, onaylanmamış bulut uygulamalarını bulmak mümkündür. Bkz. [Bulut Bulma'yı ayarlama](/cloud-app-security/set-up-cloud-discovery).

## <a name="integrating-applications-with-azure-ad"></a>Uygulamaları Azure AD ile tümleştirme
Aşağıdaki makaleler, uygulamaların Azure AD ile nasıl tümleştirilen farklı yolları tartışır ve bazı kılavuzlar sağlar.

* [Hangi Etkin Dizinin Kullanılacağını Belirleme](../fundamentals/active-directory-administer.md)
* [Azure uygulama galerisindeki uygulamaları kullanma](what-is-single-sign-on.md)
* [SaaS uygulamaları öğreticiler listesini entegre etme](../active-directory-saas-tutorial-list.md)

### <a name="authentication-types"></a>Kimlik Doğrulama Türleri
Uygulamalarınızın her birinin farklı kimlik doğrulama gereksinimleri olabilir. Azure AD ile imzalama sertifikaları SAML 2.0, WS-Federation veya OpenID Connect Protokolleri'nin yanı sıra Parola Tek Oturum Açma kullanan uygulamalarda kullanılabilir. Azure AD ile kullanılmak üzere uygulama kimlik doğrulama türleri hakkında daha fazla bilgi için Azure Active Directory ve [Password tabanlı tek işarette](what-is-single-sign-on.md)Federe Tek Oturum Açma [için Sertifikaları Yönetme'ye](manage-certificates-for-federated-single-sign-on.md) bakın.

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure AD App Proxy ile SSO'ya olanak sağlama
Microsoft Azure AD Application Proxy ile, özel ağınızda bulunan uygulamalara güvenli bir şekilde, her yerden ve her cihazdan erişebilirsiniz. Ortamınıza bir uygulama proxy bağlayıcısı yükledikten sonra, azure AD ile kolayca yapılandırılabilir.

### <a name="integrating-custom-applications"></a>Özel uygulamaları tümleştirme
Yeni bir uygulama yazıyorsanız ve Azure AD'nin gücünden yararlanmada geliştiricilere yardımcı olmak istiyorsanız, [bkz.](../active-directory-applications-guiding-developers-for-lob-applications.md)

Azure Uygulama Galerisi'ne özel uygulamanızı eklemek istiyorsanız, [Azure AD Self Servis SAML yapılandırmasıyla "Kendi uygulamanızı getirin"](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/)başlıklı bakın.

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
Aşağıdaki makaleler, Azure AD Bağlayıcıları ve Azure AD'yi kullanarak Azure AD ile entegre edildikten sonra uygulamalara erişimi yönetmenin yollarını açıklar.

* [Azure AD'yi kullanarak uygulamalara erişimi yönetme](what-is-access-management.md)
* [Azure AD Bağlayıcıları ile otomatikleme](../app-provisioning/user-provisioning.md)
* [Uygulamaya kullanıcı atama](../active-directory-applications-guiding-developers-assigning-users.md)
* [Uygulamaya grup atama](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Hesapları paylaşma](../active-directory-sharing-accounts.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi için, Azure Active Directory dağıtım planlarını [GitHub'dan](https://aka.ms/deploymentplans)indirebilirsiniz. Galeri uygulamaları için, tek oturum açma, Koşullu Erişim ve Kullanıcı sağlama için dağıtım planlarını [Azure portalı](https://portal.azure.com)üzerinden indirebilirsiniz. 

Azure portalından dağıtım planı indirmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kurumsal Uygulamaları** | Seçin Uygulama | **Dağıtım Planı****seçin.**

Lütfen [Dağıtım planı anketini](https://aka.ms/DeploymentPlanFeedback)alarak dağıtım planları hakkında geri bildirim sağlayın.
