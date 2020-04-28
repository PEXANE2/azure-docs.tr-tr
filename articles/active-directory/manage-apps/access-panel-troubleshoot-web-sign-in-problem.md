---
title: Erişim paneli web sitesinde oturum açma sorunu | Microsoft Docs
description: Erişim panelini kullanmaya oturum açmaya çalışırken karşılaşabileceğiniz sorunları gidermeye yönelik kılavuz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviwer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7c6a9c3f26c8939176197a2ecf2fcd6026e9928
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "65784318"
---
# <a name="problem-signing-in-to-the-access-panel-website"></a>Erişim paneli web sitesinde oturum açma sorunu

Erişim paneli, Azure Active Directory (Azure AD) içinde bir iş veya okul hesabına sahip olan bir kullanıcının, Azure AD yöneticisinin erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve kullanmasını sağlayan Web tabanlı bir portaldır. Azure AD sürümleri olan bir Kullanıcı, erişim paneli aracılığıyla self servis grup ve uygulama yönetimi özelliklerini de kullanabilir. Erişim paneli Azure portal ayrıdır ve kullanıcıların bir Azure aboneliğine sahip olmasını gerektirmez.

Kullanıcılar, Azure AD 'de iş veya okul hesabına sahip olmaları durumunda erişim panelinde oturum açabilirler.

-   Kullanıcılara Azure AD tarafından doğrudan kimlik doğrulaması yapılabilir.

-   Kullanıcılara Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanılarak kimlik doğrulaması yapılabilir.

-   Kullanıcılara Windows Server Active Directory tarafından kimlik doğrulaması yapılabilir.

Bir kullanıcının Azure veya Office 365 aboneliğine sahip olması ve Azure portal veya bir Office 365 uygulamasını kullanması durumunda, yeniden oturum açmaya gerek kalmadan erişim panelini sorunsuz bir şekilde kullanabilir. Kimliği doğrulanmamış kullanıcıların, Azure AD 'de hesabı için Kullanıcı adı ve parola kullanarak oturum açması istenir. Kuruluş Federasyonu yapılandırmışsa, Kullanıcı adını yazmak yeterlidir.

## <a name="general-issues-to-check-first"></a>Önce denetlenecek genel sorunlar 

-   Kullanıcının **doğru URL**'de oturum açarak emin olun:<https://myapps.microsoft.com>

-   Kullanıcının tarayıcısının URL 'sini **Güvenilen sitelerine** eklediğinizden emin olun

-   Oturum açma işlemlerinin Kullanıcı hesabının **etkinleştirildiğinden** emin olun.

-   Kullanıcı hesabının **kilitlenmediğinden** emin olun.

-   Kullanıcının **parolasının dolmadığından veya unutulmuş** olduğundan emin olun.

-   **Multi-Factor Authentication** Kullanıcı erişimini engellemediğinden emin olun.

-   **Koşullu erişim ilkesi** veya **kimlik koruma** ilkesinin Kullanıcı erişimini engellemediğinden emin olun.

-   Multi-Factor Authentication veya koşullu erişim ilkelerinin zorlanmasını sağlamak için kullanıcının **kimlik doğrulaması iletişim bilgilerinin** güncel olduğundan emin olun.

-   Ayrıca, tarayıcınızın tanımlama bilgilerini temizlemeyi ve yeniden oturum açmayı denediğinizden emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim paneli için toplantı tarayıcısı gereksinimleri

Erişim paneli JavaScript 'ı destekleyen ve CSS 'nin etkinleştirildiği bir tarayıcı gerektirir. Erişim panelinde parola tabanlı çoklu oturum açma 'yı (SSO) kullanmak için, erişim paneli uzantısının kullanıcının tarayıcısına yüklenmesi gerekir. Bu uzantı, bir kullanıcı parola tabanlı SSO için yapılandırılmış bir uygulamayı seçtiğinde otomatik olarak indirilir.

Parola tabanlı SSO için son kullanıcının tarayıcıları şunları yapabilir:

-   Internet Explorer 8, 9, 10, 11--Windows 7 veya sonraki sürümlerde

-   Windows 10 yıldönümü sürümü veya sonraki sürümlerde Microsoft Edge 

-   Chrome--Windows 7 veya üzeri sürümlerde ve MacOS X veya üzeri sürümlerde

-   Firefox 26,0 veya üzeri--Windows XP SP2 veya üzeri sürümlerde ve Mac OS X 10,6 veya üzeri


## <a name="problems-with-the-users-account"></a>Kullanıcının hesabıyla ilgili sorunlar

Erişim paneline erişim, kullanıcının hesabındaki bir sorun nedeniyle engelleniyor olabilir. Aşağıda, kullanıcılarla ilgili sorunları giderebilmeniz ve sorunları ve bunların hesap ayarlarını çözmeniz için kullanabileceğiniz bazı yollar verilmiştir:

-   [Azure Active Directory bir kullanıcı hesabının bulunup bulunmadığını denetle](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kullanıcının hesap durumunu denetleme](#check-a-users-account-status)

-   [Kullanıcının parolasını sıfırlama](#reset-a-users-password)

-   [Kendi kendine parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Kullanıcının Multi-Factor Authentication durumunu denetleme](#check-a-users-multi-factor-authentication-status)

-   [Kullanıcının kimlik doğrulaması iletişim bilgilerini denetleme](#check-a-users-authentication-contact-info)

-   [Kullanıcının grup üyeliklerini denetleme](#check-a-users-group-memberships)

-   [Kullanıcının atanan lisanslarını denetleme](#check-a-users-assigned-licenses)

-   [Kullanıcıya lisans atama](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Azure Active Directory bir kullanıcı hesabının bulunup bulunmadığını denetle

Bir kullanıcının hesabının mevcut olup olmadığını denetlemek için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  Beklenen şekilde göründüğünden ve hiç veri bulunmadığından emin olmak için Kullanıcı nesnesinin özelliklerini denetleyin.

### <a name="check-a-users-account-status"></a>Kullanıcının hesap durumunu denetleme

Kullanıcının hesap durumunu denetlemek için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  **profil**' e tıklayın.

8.  **Ayarlar** ' ın altında, **blok oturum açma** seçeneğinin **Hayır**olarak ayarlandığından emin olun.

### <a name="reset-a-users-password"></a>Kullanıcının parolasını sıfırlama

Bir kullanıcının parolasını sıfırlamak için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  Kullanıcı bölmesinin en üstündeki **Parolayı Sıfırla** düğmesine tıklayın.

8.  görüntülenen **parola sıfırlama** bölmesinde **Parolayı Sıfırla** düğmesine tıklayın.

9.  **Geçici parolayı** kopyalayın veya Kullanıcı için **Yeni bir parola girin** .

10. Bu yeni parolayı kullanıcıyla iletişim kurmak için, bir sonraki oturum açma sırasında Azure Active Directory için bu parolanın değiştirilmesi gerekir.

### <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Self servis parola sıfırlamayı etkinleştirmek için şu dağıtım adımlarını izleyin:

-   [Kullanıcıların Azure Active Directory parolalarını sıfırlamalarını sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Kullanıcıların şirket içi parolalarını Active Directory sıfırlamalarını veya değiştirmesini sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Kullanıcının Multi-Factor Authentication durumunu denetleme

Kullanıcının Multi-Factor Authentication durumunu denetlemek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. bölmenin üst kısmındaki **Multi-Factor Authentication** düğmesine tıklayın.

7. **Multi-Factor Authentication yönetim portalı** yüklendikten sonra, **Kullanıcılar** sekmesinde olduğunuzdan emin olun.

8. Kullanıcıları arayarak, filtreleyerek veya sıralayarak Kullanıcı listesinde bulun.

9. Kullanıcı listesinden kullanıcıyı seçin, istediğiniz şekilde Multi-Factor Authentication 'ı **etkinleştirin**, **devre dışı bırakın**veya **zorlayın** .

   >[!NOTE]
   >Bir Kullanıcı **zorlanan** durumdaysa, hesaplarına geri dönmek için onları geçici olarak **devre dışı** olarak ayarlayabilirsiniz. Yeniden **etkinleştikten** sonra, bir sonraki oturum açma sırasında iletişim bilgilerinin yeniden kaydolmasını gerektirmek için durumlarını tekrar etkin olarak değiştirebilirsiniz. Alternatif olarak, bu verileri doğrulamak veya ayarlamak için [kullanıcının kimlik doğrulaması iletişim bilgilerini denetleme](#check-a-users-authentication-contact-info) bölümündeki adımları izleyebilirsiniz.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Kullanıcının kimlik doğrulaması iletişim bilgilerini denetleme

Kullanıcının Multi-Factor Authentication, koşullu erişim, kimlik koruması ve parola sıfırlama için kullandığı kimlik doğrulama iletişim bilgilerini denetlemek için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  **profil**' e tıklayın.

8.  Aşağı kaydırarak **kimlik doğrulama iletişim bilgilerine**gidin.

9.  Kullanıcı için kaydedilen verileri **gözden geçirin** ve gerektiğinde güncelleştirin.

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini denetleme

Bir kullanıcının grup üyeliklerini denetlemek için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  kullanıcının üyesi olduğu grupları görmek için **gruplar** ' a tıklayın.

### <a name="check-a-users-assigned-licenses"></a>Kullanıcının atanan lisanslarını denetleme

Bir kullanıcının atanmış lisanslarını denetlemek için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  kullanıcının şu anda hangi lisansları atandığını görmek için **lisanslar** ' a tıklayın.

### <a name="assign-a-user-a-license"></a>Kullanıcıya lisans atama 

Bir kullanıcıya lisans atamak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  kullanıcının şu anda hangi lisansları atandığını görmek için **lisanslar** ' a tıklayın.

8.  **ata** düğmesine tıklayın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. **Isteğe bağlı** olarak, ürünleri daha fazla atamak için **atama seçenekleri** öğesine tıklayın. Bu tamamlandığında **Tamam** ' a tıklayın.

11. Bu lisansları bu kullanıcıya atamak için **ata** düğmesine tıklayın.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Bu sorun giderme adımları sorunu gidermezse

varsa, aşağıdaki bilgilerle bir destek bileti açın:

-   Bağıntı hata KIMLIĞI

-   UPN (Kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata sırasında saat dilimi ve zaman/zaman dilimi

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama proxy 'Si ile uygulamalarınıza çoklu oturum açma sağlama](application-proxy-configure-single-sign-on-with-kcd.md)
