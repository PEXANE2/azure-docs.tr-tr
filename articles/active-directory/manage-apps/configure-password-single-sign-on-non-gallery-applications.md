---
title: Azure AD uygulamaları için parola tek oturum açma nasıl yapılandırılır | Microsoft Dokümanlar
description: Microsoft kimlik platformundaki (Azure AD) Azure AD kurumsal uygulamalarınızda parola tek oturum açma (SSO) yapılandırma
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 563bda275b73f76b042b5e57a9909ca78c504bb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063535"
---
# <a name="configure-password-single-sign-on"></a>Parolayı tek oturum açma yapılandırma

Azure AD Kurumsal Uygulamalarınıza bir galeri uygulaması veya [galeri dışı bir web uygulaması](add-non-gallery-app.md) [eklediğinizde,](add-gallery-app.md) kullanabileceğiniz tek oturum açma seçeneklerinden biri parola tabanlı tek [oturum açmadır.](what-is-single-sign-on.md#password-based-sso) Bu seçenek, HTML oturum açma sayfası olan tüm web siteleri için kullanılabilir. Parola atlama olarak da adlandırılan parola tabanlı SSO, kimlik federasyonunu desteklemeyen web uygulamalarına kullanıcı erişimini ve parolalarını yönetmenize olanak tanır. Ayrıca, kuruluşunuzun sosyal medya uygulama hesapları gibi birden çok kullanıcının tek bir hesabı paylaşması gereken senaryolar için de yararlıdır. 

Parola tabanlı SSO, uygulamaları Azure AD'ye hızla entegre etmeye başlamak için harika bir yoldur ve şunları yapmanızı sağlar:

-   Azure AD ile entegre ettiğiniz uygulama için kullanıcı adlarını ve parolaları güvenli bir şekilde depolayarak ve yeniden oynatarak **kullanıcılarınız için Tek Oturum** Açma'yı etkinleştirin

-   Oturum **açması** için yalnızca kullanıcı adı ve parola alanlarından fazlasını gerektiren uygulamalar için birden çok oturum açma alanı gerektiren uygulamaları destekleme

-   Kullanıcılarınızın kimlik bilgilerini girerken [Uygulama Erişim Paneli'nde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) gördükleri kullanıcı adı ve parola giriş alanlarının **etiketlerini özelleştirin**

-   **Kullanıcılarınızın** [Uygulama Erişim Paneli'nde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) el ile yazdıkları mevcut uygulama hesapları için kendi kullanıcı adlarını ve parolalarını sağlamasına izin verin

-   İş grubunun bir üyesinin [Self Servis Uygulama Erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) **özelliğini** kullanarak kullanıcıya atanan kullanıcı adlarını ve parolaları belirtmesine izin ver

-   Kimlik Bilgilerini Güncelleştir özelliğini kullanarak uygulamada oturum açtığınızda bir **yöneticinin** kişiler veya gruplar tarafından kullanılacak bir kullanıcı adı ve parola belirtmesine izin ver 

## <a name="before-you-begin"></a>Başlamadan önce

Uygulama Azure AD kiracınıza eklenmediyse, [bkz.](add-gallery-app.md) [Add a non-gallery app](add-non-gallery-app.md)

## <a name="open-the-app-and-select-password-single-sign-on"></a>Uygulamayı açın ve parolayı tek oturum açma'yı seçin

1. [Azure portalında](https://portal.azure.com) bulut uygulaması yöneticisi veya Azure AD kiracınız için bir uygulama yöneticisi olarak oturum açın.

2. Azure **Active Directory** > **Enterprise uygulamalarına**gidin. Azure AD kiracınızdaki uygulamaların rasgele bir örneği görüntülenir. 

3. Uygulama **Türü** menüsünde **Tüm uygulamaları**seçin ve sonra **Uygula'yı**seçin.

4. Arama kutusuna uygulamanın adını girin ve ardından sonuçlardan uygulamayı seçin.

5. **Yönet** bölümünde, **Tek oturum açma'yı**seçin. 

6. **Parola tabanlı'yı**seçin.

7. Uygulamanın web tabanlı oturum açma sayfasının URL'sini girin. Bu dize, kullanıcı adı giriş alanını içeren sayfa olmalıdır.

   ![Parola tabanlı tek oturum açma](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

8. **Kaydet'i**seçin. Azure AD, oturum açma sayfasını kullanıcı adı girişi ve parola girişi için ayrışdırmaya çalışır. Deneme başarılı olursa, bitti. 
 
> [!NOTE]
> Bir sonraki adım, [kullanıcıları veya grupları uygulamaya atamaktır.](methods-for-assigning-users-and-groups.md) Kullanıcıları ve grupları atadıktan sonra, uygulamada oturum açtıklarında kullanıcı adına kullanılacak kimlik bilgileri sağlayabilirsiniz. **Kullanıcıları ve grupları**seçin, kullanıcının veya grubun satırı için onay kutusunu seçin ve ardından Kimlik Bilgilerini **Güncelleştir'i**tıklatın. Ardından, kullanıcı veya grup adına kullanılacak kullanıcı adı ve parolayı girin. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.
 

## <a name="manual-configuration"></a>El ile yapılandırma

Azure AD'nin ayrıştırma girişimi başarısız olursa, oturum açma işlemlerini el ile yapılandırabilirsiniz.

1. ** \<Uygulama adı> Configuration**altında, Yapılandırma oturum açma sayfasını görüntülemek için ** \<Uygulama Adını> Parola Tek Oturum Açma Ayarlarını Yapıla'yı** seçin. **Configure sign-on** 

2. **Oturum açma alanlarını El ile algıla'yı**seçin. Oturum açma alanlarının el ile algılanmasını açıklayan ek yönergeler görüntülenir.

   ![Parola tabanlı tek oturum açmanın el ile yapılandırması](./media/configure-password-single-sign-on/password-configure-sign-on.png)
3. **Oturum açma alanlarını kaydedin.** Yakalama durumu sayfası yeni bir sekmede açılır ve meta veri yakalama iletisinin **şu anda devam ettiğini**gösterir.

4. Access **Panel Uzantısı Gerekli** kutu yeni bir sekmede görünüyorsa, **Uygulamalarım Güvenli Oturum Açma Uzantısı** tarayıcı uzantısını yüklemek için Şimdi **Yükle'yi** seçin. (Tarayıcı uzantısı Microsoft Edge, Chrome veya Firefox gerektirir.) Ardından uzantıyı yükleyin, başlatın ve etkinleştirin ve yakalama durumu sayfasını yenileyin.

   Tarayıcı uzantısı daha sonra girilen URL'yi görüntüleyen başka bir sekmeyi açar.
5. Girilen URL ile sekmede, oturum açma işleminden geçin. Kullanıcı adı ve parola alanlarını doldurun ve oturum açmayı deneyin. (Doğru parolayı sağlamanız gerekmez.)

   Bir komut istemi yakalanan oturum açma alanlarını kaydetmenizi ister.
6. **Tamam'ı**seçin. Tarayıcı uzantısı, **Metadata'nın uygulama için güncelleştirildiğini**ile yakalama durumu sayfasını güncelleştirir. Tarayıcı sekmesi kapanır.

7. Azure AD **Yapılandırma oturum açma** **sayfasında, Tamam'ı seçin, uygulamada başarılı bir şekilde oturum açabildim.**

8. **Tamam'ı**seçin.

Oturum açma sayfasının ele geçirilmesinden sonra, kullanıcıları ve grupları atayabilir ve normal [parola SSO uygulamaları](what-is-single-sign-on.md)gibi kimlik bilgileri ayarlayabilirsiniz.

> [!NOTE]
> Uygulama için **Yapıl'ı Ayarla** sekmesindeki **Logo yükle** düğmesini kullanarak uygulama için bir döşeme logosu yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı veya grupları uygulamaya atama](methods-for-assigning-users-and-groups.md)
- [Otomatik kullanıcı hesabı sağlama yapılandırma](../app-provisioning/configure-automatic-user-provisioning-portal.md)
