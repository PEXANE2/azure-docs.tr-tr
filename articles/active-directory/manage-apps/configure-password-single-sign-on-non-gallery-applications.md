---
title: Azure AD uygulamaları için parola tabanlı çoklu oturum açmayı yapılandırma
description: Microsoft Identity platformunda (Azure AD) Azure AD uygulamalarınız için parola tabanlı çoklu oturum açma (SSO) yapılandırma
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 07/29/2020
ms.author: kenwith
ms.openlocfilehash: c3f9f96c6429d4925c60a56cd450a9c2ee7dde24
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87419963"
---
# <a name="configure-password-based-single-sign-on"></a>Parola tabanlı çoklu oturum açmayı yapılandırma

Uygulama yönetiminde [hızlı başlangıç serisinde](view-applications-portal.md) , Azure AD 'yi bir uygulama Için kimlik sağlayıcısı (IDP) olarak kullanmayı öğrendiniz. Hızlı başlangıç kılavuzunda SAML tabanlı SSO 'yu ayarlarsınız. SAML 'ya ek olarak, parola tabanlı SSO için bir seçenek vardır. Bu makale, çoklu oturum açma için parola tabanlı seçenek hakkında daha fazla ayrıntıya gider. 

Bu seçenek, HTML oturum açma sayfasına sahip tüm Web siteleri için kullanılabilir. Parola oluşturma olarak da bilinen parola tabanlı SSO, Kimlik Federasyonu desteklemeyen Web uygulamalarına Kullanıcı erişimini ve parolalarını yönetmenizi sağlar. Ayrıca, çeşitli kullanıcıların, kuruluşunuzun sosyal medya uygulaması hesapları gibi tek bir hesabı paylaşması gereken senaryolar için de kullanışlıdır. 

Parola tabanlı SSO, uygulamaları Azure AD 'ye hızlı bir şekilde tümleştirmenize başlamak için harika bir yoldur ve şunları yapmanızı sağlar:

- Azure AD ile tümleştirmiş olduğunuz uygulama için Kullanıcı adlarını ve parolaları güvenli bir şekilde depolayıp kaydederek kullanıcılarınız için çoklu oturum açmayı etkinleştirin

- Yalnızca Kullanıcı adı ve parola alanlarının oturum açmasını gerektiren uygulamalar için çoklu oturum açma alanları gerektiren uygulamalar desteklenir

- Kullanıcılarınızın kimlik bilgilerini girerken [uygulama erişimi panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) göreceği Kullanıcı adı ve parola giriş alanlarının etiketlerini özelleştirin

- Kullanıcılarınızın, el ile yazdıkları mevcut uygulama hesapları için kendi kullanıcı adlarını ve parolalarını sağlamasına izin verin.

- İş grubunun bir üyesinin, [self servis uygulama erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) özelliğini kullanarak bir kullanıcıya atanan kullanıcı adlarını ve parolaları belirtmesini sağlar

-   Bir yöneticinin kimlik bilgilerini güncelleştir özelliğini kullanarak uygulamada oturum açarken bireyler veya gruplar tarafından kullanılacak kullanıcı adını ve parolayı belirtmesini sağlar 

## <a name="before-you-begin"></a>Başlamadan önce

Kimlik sağlayıcınız olarak Azure AD 'nin kullanılması ve çoklu oturum açma (SSO) ayarlama, kullanılan uygulamaya bağlı olarak basit veya karmaşık olabilir. Bazı uygulamalar yalnızca birkaç eylem ile ayarlanabilir. Başkalarının derinlemesine yapılandırılması gerekir. Hızlı bir şekilde hızlandırmak için uygulama yönetiminde [hızlı başlangıç serisini](view-applications-portal.md) gözden geçirebilirsiniz. Eklemekte olduğunuz uygulama basittir, büyük olasılıkla bu makaleyi okumanız gerekmez. Eklemekte olduğunuz uygulama özel yapılandırma gerektiriyorsa ve parola tabanlı SSO kullanmanız gerekiyorsa, bu makale sizin için de kullanılır.

> [!IMPORTANT] 
> **Çoklu oturum açma** seçeneğinin **Kurumsal uygulamalarda**bir uygulama için gezinmede olmadığı bazı senaryolar vardır. 
>
> Uygulama **uygulama kayıtları** kullanılarak kaydedilmişse, çoklu oturum açma özelliği varsayılan olarak OIDC OAuth kullanacak şekilde ayarlanır. Bu durumda, **Çoklu oturum açma** seçeneği, **Kurumsal uygulamalar**altındaki gezinmede gösterilmez. Özel uygulamanızı eklemek için **uygulama kayıtları** kullandığınızda, bildirim dosyasındaki seçenekleri yapılandırırsınız. Bildirim dosyası hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory uygulama bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest). SSO standartları hakkında daha fazla bilgi edinmek için bkz. [Microsoft Identity platform kullanarak kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/active-directory/develop/authentication-vs-authorization#authentication-and-authorization-using-microsoft-identity-platform). 
>
> Bir uygulamanın başka bir kiracıda barındırıldığı veya hesabınızın gerekli izinleri (genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi) yoksa, **Çoklu oturum açma** 'nın gezinmede eksik olduğu diğer senaryolar. İzinler Ayrıca **Çoklu oturum** açmayı açabiliyor ancak kaydedemeyeceksiniz bir senaryoya neden olabilir. Azure AD Yönetim rolleri hakkında daha fazla bilgi için bkz https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) . (.


## <a name="basic-configuration"></a>Temel yapılandırma

[Hızlı başlangıç serisinde](view-applications-portal.md), Azure AD 'nin uygulama Için kimlik sağlayıcısı (IDP) olarak kullanıldığını bilmesi için kiracınıza bir uygulama eklemeyi öğrendiniz. Bazı uygulamalar önceden yapılandırılmış ve Azure AD galerisinde gösteriliyor. Diğer uygulamalar galeride değildir ve genel bir uygulama oluşturup el ile yapılandırmanız gerekir. Uygulamaya bağlı olarak, parola tabanlı SSO seçeneği kullanılamayabilir. Uygulamanın çoklu oturum açma sayfasında parola tabanlı seçenek listesini görmüyorsanız, kullanılabilir değildir.

Parola tabanlı SSO yapılandırma sayfası basittir. Yalnızca uygulamanın kullandığı oturum açma sayfasının URL 'sini içerir. Bu dize, Kullanıcı adı giriş alanını içeren sayfa olmalıdır.

URL 'YI girdikten sonra **Kaydet**' i seçin. Azure AD, Kullanıcı adı ve parola giriş alanları için oturum açma sayfasının HTML 'sini ayrıştırır. Deneme başarılı olursa işiniz bitti demektir.
 
Sonraki adımınız, [uygulamaya Kullanıcı veya grup atamak](methods-for-assigning-users-and-groups.md)olur. Kullanıcılar ve gruplar atadıktan sonra, uygulamada oturum açtıklarında Kullanıcı adına kullanılacak kimlik bilgilerini sağlayabilirsiniz. **Kullanıcılar ve gruplar**' ı seçin, kullanıcının veya grubun satırı için onay kutusunu işaretleyin ve ardından **kimlik bilgilerini güncelleştir**' i seçin. Ardından Kullanıcı veya Grup adına kullanılacak kullanıcı adını ve parolayı girin. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.
 

## <a name="manual-configuration"></a>El ile yapılandırma

Azure AD 'nin Ayrıştırma girişimi başarısız olursa, oturum açmayı el ile yapılandırabilirsiniz.

1. ** \<application name> Yapılandırma**altında, **oturum açmayı Yapılandır** sayfasını göstermek Için ** \<application name> parola çoklu oturum açma ayarlarını yapılandır** ' ı seçin. 

2. **Oturum açma alanlarını el ile Algıla**' yı seçin. Oturum açma alanlarının el ile algılanmasını açıklayan ek yönergeler görüntülenir.

   ![Parola tabanlı çoklu oturum açma için el ile yapılandırma](./media/configure-password-single-sign-on/password-configure-sign-on.png)
3. **Kayıt açma alanlarını yakala '** yı seçin. **Şu anda devam eden ileti meta veri yakalama olduğunu**gösteren yeni bir sekmede yakalama durumu sayfası açılır.

4. **Erişim paneli uzantısı gerekli** kutusu yeni bir sekmede görünürse **uygulamalarım güvenli oturum açma uzantısı** tarayıcı uzantısını yüklemek için **Şimdi yüklensin** ' i seçin. (Tarayıcı uzantısı Microsoft Edge, Chrome veya Firefox gerektirir.) Sonra uzantıyı yükleyip etkinleştirin, sonra da yakalama durumu sayfasını yenileyin.

   Tarayıcı uzantısı daha sonra, girilen URL 'YI görüntüleyen bir sekme açar.
5. Girilen URL 'nin bulunduğu sekmede, oturum açma işlemine gidin. Kullanıcı adı ve parola alanlarını doldurup oturum açmayı deneyin. (Doğru parolayı sağlamanız gerekmez.)

   Bir istem, yakalanan oturum açma alanlarını kaydetmenizi ister.
6. **Tamam**’ı seçin. Tarayıcı uzantısı, **uygulama için Ileti meta verileri güncelleştirildiğinde**yakalama durumu sayfasını güncelleştirir. Tarayıcı sekmesi kapanır.

7. Azure AD **oturum açma yapılandırma** sayfasında **Tamam ' ı seçin, uygulamada başarıyla oturum açabildim**.

8. **Tamam**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamaya Kullanıcı veya Grup atama](methods-for-assigning-users-and-groups.md)
- [Otomatik Kullanıcı hesabı sağlamayı yapılandırma](../app-provisioning/configure-automatic-user-provisioning-portal.md)
