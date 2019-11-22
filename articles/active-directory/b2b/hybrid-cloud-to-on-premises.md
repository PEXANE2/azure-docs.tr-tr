---
title: B2B kullanıcılarına şirket içi uygulamalarınıza erişim verme-Azure AD
description: Bulut B2B kullanıcılarına Azure AD B2B işbirliğiyle şirket içi uygulamalar için erişim verme şeklini gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 098f464b6af5f10866403e1cd1549d571d883ac1
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74272792"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Şirket içi uygulamalarınıza Azure AD erişimi için B2B kullanıcıları verme

Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş, artık bu B2B kullanıcıları şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları, Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

## <a name="access-to-saml-apps"></a>SAML uygulamalarına erişim

Şirket içi uygulamanız SAML tabanlı kimlik doğrulaması kullanıyorsa, bu uygulamaları Azure portal aracılığıyla Azure AD B2B işbirliği kullanıcılarınız için kolayca kullanılabilir hale getirebilirsiniz.

Aşağıdakilerden her ikisini de yapmanız gerekir:

- [Azure Active Directory Uygulama galerisinde olmayan uygulamalarda çoklu oturum açmayı yapılandırma](../manage-apps/configure-single-sign-on-non-gallery-applications.md)başlığı altında açıklandığı gıbı, SAML uygulamasını Galeri dışı uygulama şablonunu kullanarak tümleştirin. **Oturum açma URL 'si** değeri için ne kullanacağınızı aklınızda olduğunuzdan emin olun.
-  Şirket içi uygulamayı, kimlik doğrulama kaynağı olarak yapılandırılmış **Azure Active Directory** birlikte yayımlamak için Azure AD uygulama ara sunucusu kullanın. Yönergeler için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](../manage-apps/application-proxy-publish-azure-portal.md). 

   **Iç URL** ayarını yapılandırdığınızda, Galeri dışı uygulama şablonunda belirttiğiniz oturum açma URL 'sini kullanın. Bu şekilde, kullanıcılar uygulamaya kuruluş sınırının dışından erişebilir. Uygulama proxy 'Si, şirket içi uygulama için SAML çoklu oturum açma işlemini gerçekleştirir.
 
   ![Şirket içi uygulama ayarlarını iç URL ve kimlik doğrulaması gösterir](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>IWA ve KCD uygulamalarına erişim

B2B kullanıcılarına, tümleşik Windows kimlik doğrulaması ve Kerberos kısıtlı temsilcisiyle güvenliği sağlanmış şirket içi uygulamalara erişim sağlamak için aşağıdaki bileşenlere ihtiyacınız vardır:

- **Azure AD uygulama ara sunucusu aracılığıyla kimlik doğrulaması**. B2B kullanıcılarının şirket içi uygulamada kimlik doğrulaması yapabilmesi gerekir. Bunu yapmak için şirket içi uygulamayı Azure AD Uygulama Ara Sunucusu aracılığıyla yayımlamanız gerekir. Daha fazla bilgi için bkz. [uygulama proxy 'si ile çalışmaya başlama ve](../manage-apps/application-proxy-enable.md) [Azure AD uygulama ara sunucusu kullanarak](../manage-apps/application-proxy-publish-azure-portal.md)bağlayıcı ve uygulama yayımlama.
- **Şirket içi dizindeki BIR B2B Kullanıcı nesnesi aracılığıyla yetkilendirme**. Uygulamanın kullanıcı erişim denetimleri gerçekleştirebilmesi ve doğru kaynaklara erişim izni vermesi gerekir. IWA ve KCD, bu yetkilendirmeyi tamamlaması için şirket içi Windows Server Active Directory bir kullanıcı nesnesi gerektirir. [KCD ile çoklu oturum açma konusunda](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works)açıklandığı gibi, uygulama proxy 'si, kullanıcının kimliğine bürünmesi ve uygulamaya Kerberos belirteci alması için bu kullanıcı nesnesine ihtiyaç duyuyor. 

   B2B Kullanıcı senaryosunda, şirket içi dizinde yetkilendirme için gerekli olan Konuk kullanıcı nesnelerini oluşturmak için kullanabileceğiniz iki yöntem vardır:

   - Microsoft Graph için Microsoft Identity Manager (MıM) ve MıM Yönetim Aracısı. 
   - [Bir PowerShell betiği](#create-b2b-guest-user-objects-through-a-script-preview). Betiği kullanmak, MıM gerektirmeyen daha basit bir çözümdür. 

Aşağıdaki diyagramda, B2B kullanıcılarına şirket içi ıWA ve KCD uygulamalarınıza erişim sağlamak için Azure AD Uygulama Ara Sunucusu 'nin ve şirket içi dizindeki B2B Kullanıcı nesnesinin oluşturulmasına ilişkin üst düzey bir genel bakış sağlanmaktadır. Numaralandırılmış adımlar, diyagramın altında ayrıntılı olarak açıklanmaktadır.

![MıM ve B2B betik çözümlerinin diyagramı](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Bir iş ortağı kuruluştan (Fabrikam kiracısı) bir Kullanıcı contoso kiracısına davet edilir.
2.  Contoso kiracısında bir Konuk Kullanıcı nesnesi oluşturulur (örneğin, guest_fabrikam UPN 'si olan bir kullanıcı nesnesi). com # EXT #@contoso.onmicrosoft.com).
3.  Fabrikam konuğu, contoso 'dan MıM 'e veya B2B PowerShell betiği aracılığıyla içeri aktarılır.
4.  Fabrikam Konuk Kullanıcı nesnesinin (konuk # EXT #) temsili veya "ayak izi", MıM aracılığıyla veya B2B PowerShell betiği aracılığıyla şirket içi dizininde oluşturulur.
5.  Konuk Kullanıcı, app.contoso.com Şirket içi uygulamasına erişir.
6.  Kimlik doğrulama isteği, Kerberos kısıtlanmış temsili kullanılarak uygulama proxy 'Si aracılığıyla yetkilendirilir. 
7.  Konuk Kullanıcı nesnesi yerel olarak mevcut olduğundan kimlik doğrulaması başarılı olur.

### <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetim ilkeleri

Şirket içi B2B Kullanıcı nesnelerini yaşam döngüsü yönetim ilkeleri aracılığıyla yönetebilirsiniz. Örneğin:

- Uygulama proxy kimlik doğrulaması sırasında MFA kullanılacak şekilde Konuk Kullanıcı için Multi-Factor Authentication (MFA) ilkeleri ayarlayabilirsiniz. Daha fazla bilgi için bkz. [B2B işbirliği kullanıcıları Için koşullu erişim](conditional-access.md).
- Bulut B2B kullanıcısı üzerinde gerçekleştirilen tüm sponsorships, erişim incelemeleri, hesap doğrulamaları vb. Şirket içi kullanıcılar için geçerlidir. Örneğin, bulut kullanıcısı yaşam döngüsü yönetimi ilkeleriniz aracılığıyla silinirse, şirket içi Kullanıcı de MıM eşitleme veya Azure AD Connect eşitleme aracılığıyla silinir. Daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleri ile konuk erişimini yönetme](../governance/manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim"></a>MıM aracılığıyla B2B Konuk Kullanıcı nesneleri oluşturma

MıM 2016 hizmet paketi 1 ' i ve MıM Yönetim Aracısı 'nı şirket içi dizinde Konuk Kullanıcı nesneleri oluşturmak üzere Microsoft Graph için kullanma hakkında bilgi için bkz. Azure [uygulama proxy 'si ile Microsoft Identity Manager (MIM) 2016 SP1 Ile Azure AD işletmeden işletmeye (B2B) işbirliği](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Betik aracılığıyla B2B Konuk Kullanıcı nesneleri oluşturma (Önizleme)

Şirket içi Active Directory Konuk kullanıcı nesnelerini oluşturmak için başlangıç noktası olarak kullanabileceğiniz bir PowerShell örnek betiği bulunur.

Betiği ve Benioku dosyasını [Indirme merkezinden](https://www.microsoft.com/download/details.aspx?id=51495)indirebilirsiniz. **Azure AD B2B kullanıcıları on-Prem. zip dosyasını çekmek Için betiği ve Benioku** dosyasını seçin.

Betiği kullanmadan önce, ilişkili Readme dosyasındaki önkoşulları ve önemli konuları gözden geçirdiğinizden emin olun. Ayrıca, betiğin yalnızca örnek olarak kullanılabileceğini de anlayın. Geliştirme takımınızın veya iş ortağının, çalıştırmadan önce betiği özelleştirmesi ve incelemesi gerekir.

## <a name="license-considerations"></a>Lisans konuları

Şirket içi uygulamalara erişen dış Konuk kullanıcılar için doğru Istemci erişim lisanslarına (CAL) sahip olduğunuzdan emin olun. Daha fazla bilgi için, [Istemci erişim lisansları ve yönetim lisanslarının](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx)"dış Bağlayıcılar" bölümüne bakın. Özel lisans gereksinimleriniz hakkında Microsoft temsilcinize veya yerel satıcınızla görüşün.

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için B2B işbirliği Azure Active Directory](hybrid-organizations.md)

- Azure AD Connect genel bir bakış için bkz. Şirket [içi dizinlerinizi Azure Active Directory Ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

