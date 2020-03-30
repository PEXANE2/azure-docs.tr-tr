---
title: Çok kiracılı uygulamalar için otomatik kullanıcı sağlama yı etkinleştirme - Azure AD
description: Otomatik sağlama sağlamak için bağımsız yazılım satıcıları için bir rehber
services: active-directory
documentationcenter: azure
author: BarbaraSelden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: baselden
ms.reviewer: zhchia
ms.collection: active-directory
ms.openlocfilehash: 93e1d879f69a95fe7472ce530e0e9f38f3480f39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522402"
---
# <a name="enable-automatic-user-provisioning-for-your-multi-tenant-application"></a>Çok kiracılı uygulamanız için otomatik kullanıcı sağlamayı etkinleştirme

Otomatik kullanıcı sağlama, hizmet olarak yazılım uygulamalarınız gibi hedef sistemlerde kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatikleştirmek işlemidir.

## <a name="why-enable-automatic-user-provisioning"></a>Neden otomatik kullanıcı sağlama etkinleştirin?

Kullanıcının ilk oturum açma dan önce uygulamada kullanıcı kaydının bulunmasını gerektiren uygulamalar, kullanıcı sağlamayı gerektirir. Bir hizmet sağlayıcısı olarak sizin için avantajlar ve müşterileriniz için de avantajlar vardır.

### <a name="benefits-to-you-as-the-service-provider"></a>Servis sağlayıcı olarak size avantajlar

* Microsoft kimlik platformunu kullanarak uygulamanızın güvenliğini artırın.

* Uygulamanızı benimsemek için gerçek ve algılanan müşteri çabasını azaltın.

* System for Domain Identity Management (SCIM) tabanlı sağlama sistemini kullanarak otomatik kullanıcı sağlama için birden çok kimlik sağlayıcısıyla (IDP) tümleştirme maliyetlerinizi azaltın.

* Müşterilerin kullanıcı sağlama sorunlarını gidermelerine yardımcı olmak için zengin günlükler sağlayarak destek maliyetlerini azaltın.

* [Azure AD uygulama galerisinde](https://azuremarketplace.microsoft.com/marketplace/apps)uygulamanızın görünürlüğünü artırın.

* Uygulama Eğitimleri sayfasında öncelikli bir giriş alın.

### <a name="benefits-to-your-customers"></a>Müşterilerinize avantajlar

* Rol değiştiren veya kuruluşu uygulamanıza bırakan kullanıcılar için uygulamanıza erişimi otomatik olarak kaldırarak güvenliği artırın.

* İnsan hatalarından ve el ile sağlamayla ilişkili yinelenen çalışmalardan kaçınarak uygulamanız için kullanıcı yönetimini basitleştirin.

* Özel olarak geliştirilmiş tedarik çözümleri barındırma ve sürdürme maliyetlerini azaltın.

## <a name="choose-a-provisioning-method"></a>Sağlama yöntemini seçin

Azure AD, uygulamanız için otomatik kullanıcı sağlamayı etkinleştirmek için çeşitli tümleştirme yolları sağlar.

* [Azure AD Sağlama Hizmeti,](../app-provisioning/user-provisioning.md) kullanıcıların Azure AD'den uygulamanıza (giden sağlama) ve uygulamanızdan Azure AD'ye (gelen sağlama) sağlanmasını ve bunların yok olmasını yönetir. Hizmet, uygulamanız tarafından sağlanan Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) kullanıcı yönetimi API uç noktalarına bağlanır.

* [Microsoft Graph'ı](https://docs.microsoft.com/graph/)kullanırken, uygulamanız Microsoft Graph API'sini sorgulayarak Azure AD'den uygulamanıza kullanıcıların ve grupların gelen ve giden sağlamalarını yönetir.

* Uygulamanız federasyon için SAML kullanıyorsa, Güvenlik İddiası Biçimlendirme Dili (SAML JIT) kullanıcı sağlama özelliği etkinleştirilebilir. Saml belirtecinde kullanıcılara gönderilen talep bilgilerini kullanır.

Uygulamanız için hangi tümleştirme seçeneğini kullanacağınızı belirlemeye yardımcı olmak için üst düzey karşılaştırma tablosuna bakın ve ardından her seçenekle ilgili daha ayrıntılı bilgilere bakın.

| Otomatik Sağlama ile etkinleştirilen veya geliştirilmiş yetenekler| Azure REKLAM Sağlama Hizmeti (SCIM 2.0)| Microsoft Grafik API (OData v4.0)| SAML JIT |
|---|---|---|---|
| Azure AD'de kullanıcı ve grup yönetimi| √| √| Yalnızca kullanıcı |
| Şirket içi Active Directory'den senkronize edilen kullanıcıları ve grupları yönetme| √*| √*| Yalnızca kullanıcı* |
| O365 verilerine erişim sağlama sırasında kullanıcıların ve grupların ötesindeki verilere erişin (Takımlar, SharePoint, E-posta, Takvim, Belgeler, vb.)| X+| √| X |
| İş kurallarına göre kullanıcıları oluşturma, okuma ve güncelleme| √| √| √ |
| İş kurallarına göre kullanıcıları silme| √| √| X |
| Azure portalındaki tüm uygulamalar için otomatik kullanıcı sağlamayı yönetme| √| X| √ |
| Birden çok kimlik sağlayıcısını destekleme| √| X| √ |
| Destek konuk hesapları (B2B)| √| √| √ |
| Kurumsal olmayan hesapları destekle (B2C)| X| √| √ |

<sup>*</sup>– Kullanıcıları AD'den Azure AD'ye senkronize etmek için Azure AD Connect kurulumu gereklidir.  
<sup>+</sup >– SCIM'i sağlama için kullanmak, uygulamanızı başka amaçlarla MIcrosoft Graph ile entegre etmenizi engellemez.

## <a name="azure-ad-provisioning-service-scim"></a>Azure AD Sağlama Hizmeti (SCIM)

Azure AD sağlama hizmetleri, birçok kimlik sağlayıcısı (IDP) tarafından desteklenen sağlama nın yanı sıra uygulamaları (örn. Slack, G Suite, Dropbox) sağlayan bir endüstri standardı olan [SCIM'i](https://aka.ms/SCIMOverview)kullanır. SCIM uyumlu herhangi bir IdP SCIM bitiş noktanıza bağlanabileceğinden, Azure AD'ye ek olarak IDP'leri desteklemek istiyorsanız Azure AD sağlama hizmetini kullanmanızı öneririz. Basit bir /Kullanıcı bitiş noktası oluşturarak, kendi eşitleme altyapınızı korumak zorunda kalmadan sağlamayı etkinleştirebilirsiniz. 

Azure AD Sağlama Hizmeti kullanıcılarının SCIM'i hakkında daha fazla bilgi için bkz: 

* [SCIM standardı hakkında daha fazla bilgi edinin](https://aka.ms/SCIMOverview)

* [Azure Active Directory'den uygulamalara kullanıcıları ve grupları otomatik olarak sağlamak için Etki Alanı Arası Kimlik Yönetimi Sistemi'ni (SCIM) kullanma](../app-provisioning/use-scim-to-provision-users-and-groups.md)

* [Azure AD SCIM uygulamasını anlama](../app-provisioning/use-scim-to-provision-users-and-groups.md)

## <a name="microsoft-graph-for-provisioning"></a>Sağlama için Microsoft Grafiği

Microsoft Graph'ı sağlama için kullandığınızda, Graph'ta bulunan tüm zengin kullanıcı verilerine erişebilirsiniz. Kullanıcıların ve grupların ayrıntılarına ek olarak, kullanıcının rolleri, yönetici ve doğrudan raporlar, sahip olunan ve kayıtlı aygıtlar ve [Microsoft Graph'ta](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)bulunan yüzlerce diğer veri parçası gibi ek bilgileri de getirebilirsiniz. 

15 milyondan fazla kuruluş ve fortune 500 şirketlerinin %90'ı, Office 365, Microsoft Azure, Enterprise Mobility Suite veya Microsoft 365 gibi Microsoft bulut hizmetlerine abone olurken Azure AD kullanır. Uygulamanızı çalışan onboarding (ve sonlandırma), profil bakımı ve daha fazlası gibi yönetimiş akışlarıyla tümleştirmek için Microsoft Graph'ı kullanabilirsiniz. 

Sağlama için Microsoft Graph'ı kullanma hakkında daha fazla bilgi edinin:

* [Microsoft Graph Giriş sayfası](https://developer.microsoft.com/graph)

* [Microsoft Graph’a genel bakış](https://docs.microsoft.com/graph/overview)

* [Microsoft Graph Auth Genel Bakış](https://docs.microsoft.com/graph/auth/)

* [Microsoft Graph ile başlarken](https://developer.microsoft.com/graph/get-started)

## <a name="using-saml-jit-for-provisioning"></a>Sağlama için SAML JIT'yi kullanma

Kullanıcıları yalnızca uygulamanızda ilk oturum açabilirsiniz ve kullanıcıları otomatik olarak deprovision'ı devre açmanız gerekmiyorsa, SAML JIT bir seçenektir. Uygulamanız SAML JIT kullanmak için bir federasyon protokolü olarak SAML 2.0 desteklemesi gerekir.

SAML JIT, uygulamadaki kullanıcı bilgilerini oluşturmak ve güncelleştirmek için SAML belirtecindeki talep bilgilerini kullanır. Müşteriler bu gerekli talepleri Azure AD uygulamasında gerektiği gibi yapılandırabilir. Bazen müşterinin bu özelliği kullanabilmesi için JIT sağlamanın uygulama tarafından etkinleştirilmesi gerekir. SAML JIT, kullanıcıları oluşturmak ve güncelleştirmek için yararlıdır, ancak uygulamadaki kullanıcıları silemez veya devre dışı bırakamaz.

## <a name="next-steps"></a>Sonraki Adımlar

* [Uygulamanız için Tek Oturum Açma'yı etkinleştirin](../manage-apps/isv-sso-content.md)

* Microsoft'un sitesinde belgeler oluşturmak için [başvuru kaydınızı](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx) ve iş ortağınızı Microsoft ile gönderin.

* [Microsoft İş Ortağı Ağı'na katılın (ücretsiz) ve pazar agit planınızı oluşturun.](https://partner.microsoft.com/en-us/explore/commercial)
