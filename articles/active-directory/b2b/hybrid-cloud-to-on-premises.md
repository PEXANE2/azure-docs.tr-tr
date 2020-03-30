---
title: B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme - Azure AD
description: Azure AD B2B işbirliği ile bulut B2B kullanıcılarına şirket içi uygulamalara nasıl erişilir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272792"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Azure AD'deki B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme

Ortak kuruluşlardan gelen konuk kullanıcıları Azure AD'nize davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş olarak, artık bu B2B kullanıcılarına şirket içi uygulamalara erişim sağlayabilirsiniz. Bu şirket içi uygulamalar, Saml tabanlı kimlik doğrulamayı veya Entegre Windows Kimlik Doğrulaması'nı (IWA) Kerberos kısıtlı delegasyonu (KCD) ile kullanabilir.

## <a name="access-to-saml-apps"></a>SAML uygulamalarına erişim

Şirket içi uygulamanız SAML tabanlı kimlik doğrulamasını kullanıyorsa, Azure portalı aracılığıyla bu uygulamaları Azure AD B2B işbirliği kullanıcılarınız için kolayca kullanılabilir hale getirebilirsiniz.

Aşağıdakilerin her ikisini de yapmalısınız:

- [Azure Active Directory uygulama galerisinde olmayan uygulamalarda tek oturum açma](../manage-apps/configure-single-sign-on-non-gallery-applications.md)yapılandırmada açıklandığı gibi, galeri dışı uygulama şablonu kullanarak SAML uygulamasını tümleştirin. **Oturum Açma URL** değeri için ne kullandığınızı not aldığınızdan emin olun.
-  Azure **Active Directory** kimlik doğrulama kaynağı olarak yapılandırılan şirket içi uygulamayı yayınlamak için Azure AD Application Proxy'yi kullanın. Yönergeler için [bkz.](../manage-apps/application-proxy-publish-azure-portal.md) 

   **Dahili Url** ayarını yapılandırDığınızda, galeri dışı uygulama şablonunda belirttiğiniz oturum açma URL'sini kullanın. Bu şekilde, kullanıcılar uygulamaya kuruluş sınırı dışından erişebilir. Application Proxy, şirket içi uygulama için SAML tek oturum açma gerçekleştirir.
 
   ![Şirket içi uygulama ayarlarını dahili URL ve kimlik doğrulamagösterir](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>IWA ve KCD uygulamalarına erişim

B2B kullanıcılarının Entegre Windows Kimlik Doğrulaması ve Kerberos kısıtlı delegasyonu ile güvenli olan şirket içi uygulamalara erişebilmeniz için aşağıdaki bileşenlere ihtiyacınız vardır:

- **Azure AD Uygulama Proxy ile kimlik doğrulama.** B2B kullanıcıları şirket içi uygulamaya kimlik doğrulaması yapabilmeli. Bunu yapmak için şirket içi uygulamayı Azure AD Uygulama Proxy'si aracılığıyla yayımlamanız gerekir. Daha fazla bilgi için Bkz. [Uygulama Proxy'yi başlatın ve bağlayıcıyı yükleyin](../manage-apps/application-proxy-enable.md) ve [Uygulamaları Azure AD Application Proxy'yi kullanarak yayımlayın.](../manage-apps/application-proxy-publish-azure-portal.md)
- **Şirket içi dizinde bir B2B kullanıcı nesnesi üzerinden yetkilendirme.** Uygulama, kullanıcı erişim denetimleri gerçekleştirebilmeli ve doğru kaynaklara erişim verebilmelidir. IWA ve KCD, bu yetkilendirmeyi tamamlamak için şirket içi Windows Server Active Directory'de bir kullanıcı nesnesi gerektirir. [KCD ile tek oturum açma nın nasıl çalıştığı](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works)konusunda açıklandığı gibi, Application Proxy'nin kullanıcıyı taklit etmesi ve uygulamaya Kerberos belirteci alması için bu kullanıcı nesnesine ihtiyacı vardır. 

   B2B kullanıcı senaryosu için, şirket içi dizinde yetkilendirme için gerekli olan konuk kullanıcı nesnelerini oluşturmak için kullanabileceğiniz iki yöntem vardır:

   - Microsoft Identity Manager (MIM) ve Microsoft Graph için MIM yönetim aracısı. 
   - [Bir PowerShell komut dosyası](#create-b2b-guest-user-objects-through-a-script-preview). Komut dosyasını kullanmak, MIM gerektirmeyen daha hafif bir çözümdür. 

Aşağıdaki diyagram, B2B kullanıcılarına şirket içi IWA ve KCD uygulamalarınıza erişim izni vermek için Azure AD Application Proxy ve şirket içi dizindeki B2B kullanıcı nesnesinin oluşumunun birlikte nasıl çalıştığına ilişkin üst düzey bir genel bakış sağlar. Numaralanan adımlar diyagramın altında ayrıntılı olarak açıklanmıştır.

![MIM ve B2B komut dosyası çözümleri diyagramı](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Ortak bir kuruluştan (Fabrikam kiracısı) bir kullanıcı Contoso kiracısına davet edilir.
2.  Contoso kiracısında bir konuk kullanıcı nesnesi oluşturulur (örneğin, UPN guest_fabrikam.com#EXT#@contoso.onmicrosoft.comiçeren bir kullanıcı nesnesi).
3.  Fabrikam konuğu Contoso'dan MIM veya B2B PowerShell komut dosyası ile ithal edilir.
4.  Fabrikam konuk kullanıcı nesnesinin (Guest#EXT#) bir gösterimi veya "ayak izi", şirket içi dizinde, Contoso.com, MIM aracılığıyla veya B2B PowerShell komut dosyası aracılığıyla oluşturulur.
5.  Konuk kullanıcı şirket içi başvuruya app.contoso.com.
6.  Kimlik doğrulama isteği, Kerberos kısıtlı delegasyonu kullanılarak Uygulama Proxy'si aracılığıyla yetkilendirilir. 
7.  Konuk kullanıcı nesnesi yerel olarak var olduğundan, kimlik doğrulaması başarılı olur.

### <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetimi politikaları

Şirket içi B2B kullanıcı nesnelerini yaşam döngüsü yönetimi ilkeleri yle yönetebilirsiniz. Örnek:

- Uygulama Proxy kimlik doğrulaması sırasında MFA'nın kullanılması için Konuk kullanıcı için çok faktörlü kimlik doğrulama (MFA) ilkeleri ayarlayabilirsiniz. Daha fazla bilgi [için B2B işbirliği kullanıcıları için Koşullu Erişim'e](conditional-access.md)bakın.
- Bulut B2B kullanıcısı üzerinde gerçekleştirilen tüm sponsorluklar, erişim değerlendirmeleri, hesap doğrulamaları vb. şirket içi kullanıcılar için geçerlidir. Örneğin, bulut kullanıcısı yaşam döngüsü yönetimi ilkeleriniz aracılığıyla silinirse, şirket içi kullanıcı MIM Sync veya Azure AD Connect eşitlemi yoluyla da silinir. Daha fazla bilgi için bkz: [Azure AD erişim incelemeleri ile konuk erişimini yönet.](../governance/manage-guest-access-with-access-reviews.md)

### <a name="create-b2b-guest-user-objects-through-mim"></a>MIM aracılığıyla B2B konuk kullanıcı nesneleri oluşturma

Şirket dizininde konuk kullanıcı nesneleri oluşturmak için MIM 2016 Service Pack 1 ve Microsoft Graph için MIM yönetim aracısının nasıl kullanılacağı hakkında bilgi için, Azure Ad iş yeri [(B2B) işbirliği ile Microsoft Identity Manager (MIM) 2016 SP1 ile Azure Application Proxy'ye](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario)bakın.

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Komut dosyası aracılığıyla B2B konuk kullanıcı nesneleri oluşturma (Önizleme)

Şirket içi Active Directory'nizde konuk kullanıcı nesnelerini oluşturmak için başlangıç noktası olarak kullanabileceğiniz bir PowerShell örnek komut dosyası mevcuttur.

Komut dosyasını ve Readme dosyasını [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=51495)indirebilirsiniz. Azure **AD B2B kullanıcılarını prem.zip dosyasına çekmek için Komut Dosyası ve Readme'yi** seçin.

Komut dosyasını kullanmadan önce, ilişkili Readme dosyasındaki ön koşulları ve önemli hususları gözden aldığınızdan emin olun. Ayrıca, komut dosyasının yalnızca örnek olarak kullanılabildiğini anlayın. Geliştirme ekibinizin veya iş ortağınızın, siz çalıştırmadan önce komut dosyasını özelleştirmesi ve gözden geçirmesi gerekir.

## <a name="license-considerations"></a>Lisans hususları

Şirket içi uygulamalara erişen harici konuk kullanıcılar için doğru İstemci Erişim Lisanslarına (CALs) sahip olduğunuzdan emin olun. Daha fazla bilgi [için, İstemci Erişim Lisansları ve Yönetim Lisansları'nın](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx)"Harici Bağlayıcılar" bölümüne bakın. Özel lisans gereksinimleriniz için Microsoft temsilcinize veya yerel satıcınıza danışın.

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için Azure Active Directory B2B işbirliği](hybrid-organizations.md)

- Azure AD Connect'e genel bir bakış için [bkz.](../hybrid/whatis-hybrid-identity.md)

