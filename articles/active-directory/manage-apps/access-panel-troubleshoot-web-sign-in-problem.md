---
title: Erişim paneli web sitesine giriş sorunu | Microsoft Dokümanlar
description: Erişim Panelini kullanmak için oturum açmaya çalışırken karşılaşabileceğiniz sorunları giderme kılavuzu
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65784318"
---
# <a name="problem-signing-in-to-the-access-panel-website"></a>Erişim paneli web sitesine giriş sorunu

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) iş veya okul hesabı olan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. Azure AD sürümlerine sahip bir kullanıcı, Access Paneli aracılığıyla self servis grubu ve uygulama yönetimi özelliklerini de kullanabilir. Erişim Paneli Azure portalından ayrıdır ve kullanıcıların Azure aboneliğine sahip olmasını gerektirmez.

Kullanıcılar Azure AD'de bir çalışmaları veya okul hesapları varsa Access Panel'de oturum açabilir.

-   Kullanıcılar doğrudan Azure AD tarafından kimlik doğrulaması yapılabilir.

-   Kullanıcılar Active Directory Federation Services (AD FS) kullanılarak kimlik doğrulaması yapılabilir.

-   Kullanıcıların kimliği Windows Server Active Directory tarafından doğrulanabilir.

Bir kullanıcının Azure veya Office 365 aboneliği varsa ve Azure portalını veya Office 365 uygulamasını kullanıyorsa, yeniden oturum açmasına gerek kalmadan Access Panelini sorunsuz bir şekilde kullanabilir. Kimliği doğrulanamayan kullanıcıların Azure AD'deki hesaplarının kullanıcı adı ve parolasını kullanarak oturum açmaları istenir. Kuruluş federasyonu yapılandırmışsa, kullanıcı adını yazmak yeterlidir.

## <a name="general-issues-to-check-first"></a>Önce kontrol etmek için genel konular 

-   Kullanıcının **doğru URL'de**oturum açmıştından emin olun:<https://myapps.microsoft.com>

-   Kullanıcının tarayıcısının URL'yi **güvenilir sitelerine eklediklerinden** emin olun

-   Oturum açma için kullanıcının hesabının **etkin** olduğundan emin olun.

-   Kullanıcının hesabının **kilitlendirolmadığından** emin olun.

-   Kullanıcının **parolasının süresinin dolmadığından veya unutulmadığından** emin olun.

-   Çok **Faktörlü Kimlik Doğrulama'nın** kullanıcı erişimini engellemediğinden emin olun.

-   **Koşullu Erişim ilkesinin** veya **Kimlik Koruma** ilkesinin kullanıcı erişimini engellemediğinden emin olun.

-   Çok Faktörlü Kimlik Doğrulama veya Koşullu Erişim ilkelerinin uygulanmasına izin vermek için kullanıcının **kimlik doğrulama iletişim bilgisinin** güncel olduğundan emin olun.

-   Tarayıcınızın çerezlerini temizlemeyi ve yeniden oturum açmaya çalışmayı da denediğinden emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim Paneli için tarayıcı gereksinimlerini karşılama

Erişim Paneli, JavaScript'i destekleyen ve CSS etkinleştirilmiş bir tarayıcı gerektirir. Access Paneli'nde parola tabanlı tek oturum açma (SSO) kullanmak için Erişim Masası uzantısı kullanıcının tarayıcısına yüklenmiş olmalıdır. Kullanıcı parola tabanlı SSO için yapılandırılan bir uygulamayı seçtiğinde bu uzantı otomatik olarak indirilir.

Parola tabanlı SSO için, son kullanıcının tarayıcıları şu olabilir:

-   Internet Explorer 8, 9, 10, 11 -- Windows 7 veya sonraki

-   Windows 10 Anniversary Edition veya sonraki sürümde Microsoft Edge 

-   Chrome -- Windows 7 veya sonraki ve MacOS X veya daha sonra

-   Firefox 26.0 veya sonraki -- Windows XP SP2 veya sonraki ve Mac OS X 10.6 veya sonraki


## <a name="problems-with-the-users-account"></a>Kullanıcının hesabındaki sorunlar

Erişim Paneli'ne erişim, kullanıcının hesabındaki bir sorun nedeniyle engellenebilir. Aşağıda, kullanıcılarla ve hesap ayarlarıyla ilgili sorunları gidermenin ve çözebileceğiniz bazı yollar verilmiştir:

-   [Azure Etkin Dizini'nde bir kullanıcı hesabı olup olmadığını denetleme](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kullanıcının hesap durumunu denetleme](#check-a-users-account-status)

-   [Kullanıcının parolanı sıfırlama](#reset-a-users-password)

-   [Kendi kendine parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status)

-   [Kullanıcının kimlik doğrulama kişi bilgilerini kontrol edin](#check-a-users-authentication-contact-info)

-   [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

-   [Kullanıcının atanmış lisanslarını denetleme](#check-a-users-assigned-licenses)

-   [Kullanıcıya lisans atama](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir kullanıcı hesabı olup olmadığını denetleme

Bir kullanıcının hesabının olup olmadığını kontrol etmek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  Kullanıcı nesnesinin özelliklerini kontrol edin, beklediğiniz gibi göründüklerinden ve hiçbir veri eksik olmadığından emin olun.

### <a name="check-a-users-account-status"></a>Kullanıcının hesap durumunu denetleme

Bir kullanıcının hesap durumunu denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  **Profili**tıklatın.

8.  **Ayarlar** **altında, Oturum Aç'ı Oturum Aç'ın** **Hayır**olarak ayarlı olduğundan emin olun.

### <a name="reset-a-users-password"></a>Kullanıcının parolanı sıfırlama

Bir kullanıcının parolasını sıfırlamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcı bölmesinin üst kısmındaki **parolayı sıfırla** düğmesini tıklatın.

8.  görünen **parola** bölmesindeki **Parolayı Sıfırla** düğmesini tıklatın.

9.  Geçici **parolayı** kopyalayın veya kullanıcı için **yeni bir parola girin.**

10. Bu yeni parolayı kullanıcıya iletin, bir sonraki oturum açma sırasında bu parolayı Azure Active Directory'de değiştirmeleri gerekir.

### <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Self servis parola sıfırlamayı etkinleştirmek için aşağıdaki dağıtım adımlarını izleyin:

-   [Kullanıcıların Azure Etkin Dizin parolalarını sıfırlamalarını sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Kullanıcıların Etkin Dizini şirket içi parolalarını sıfırlamalarına veya değiştirmelerine olanak sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme

Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. bölmenin üst kısmındaki **Çok Faktörlü Kimlik Doğrulama** düğmesini tıklatın.

7. Çok **Faktörlü Kimlik Doğrulama Yönetimi Portalı** yüklendikten sonra **Kullanıcılar** sekmesinde olduğundan emin olun.

8. Arama yaparak, filtreleyerek veya sıralayarak kullanıcıyı kullanıcı listesinde bulun.

9. Kullanıcı listesinden kullanıcıyı seçin **ve**çok faktörlü kimlik doğrulamasını istediğiniz gibi etkinleştirin veya **zorleyin.** **Disable**

   >[!NOTE]
   >Bir kullanıcı **Zorlanmış** durumdaysa, hesabına geri dönmelerine izin vermek için geçici olarak **Devre Dışı Bırak'a** ayarlayabilirsiniz. Geri döndüklerinde, bir sonraki oturum açma sırasında kişi bilgilerini yeniden kaydetmelerini istemek için durumlarını yeniden **Enabled** olarak değiştirebilirsiniz. Alternatif olarak, bu verileri doğrulamak veya onlar için ayarlamak için [kullanıcının kimlik doğrulama iletişim bilgilerini](#check-a-users-authentication-contact-info) denetle'deki adımları izleyebilirsiniz.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Kullanıcının kimlik doğrulama kişi bilgilerini kontrol edin

Çok faktörlü kimlik doğrulama, Koşullu Erişim, Kimlik Koruması ve Parola Sıfırlama için kullanılan kullanıcının kimlik doğrulama iletişim bilgilerini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  **Profili**tıklatın.

8.  **Kimlik Doğrulama iletişim bilgilerine**aşağı kaydırın.

9.  Kullanıcı için kayıtlı verileri **gözden geçirin** ve gerektiğinde güncelleştirin.

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini kontrol edin

Bir kullanıcının grup üyeliklerini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının hangi gruplara üye olduğunu görmek için **Gruplar'ı** tıklatın.

### <a name="check-a-users-assigned-licenses"></a>Kullanıcının atanmış lisanslarını denetleme

Bir kullanıcının atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

### <a name="assign-a-user-a-license"></a>Kullanıcıya lisans atama 

Bir kullanıcıya lisans atamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8.  **Atla** düğmesini tıklatın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. Ürünleri parçalı olarak atamak için **atama seçenekleri** öğesini **isteğe bağlı** olarak tıklatın. Bu tamamlandığında **Tamam'ı** tıklatın.

11. Bu lisansları bu kullanıcıya atamak için **Atla** düğmesini tıklatın.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmezse

varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata oluşur sırasında saat dilimi ve saat/zaman dilimi oluşur

-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
