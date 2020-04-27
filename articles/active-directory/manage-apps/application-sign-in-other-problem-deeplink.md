---
title: Derin bağlantı kullanarak bir uygulamada oturum açma sorunları | Microsoft Docs
description: Azure AD kullanarak bir uygulamaya ayrıntılı bağlantı URL 'sinden erişme sorunları nasıl giderilir?
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
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44825f32a13db0a221252c042dc9f23ec43a9c8f
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "65825424"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Ayrıntılı bağlantı kullanarak bir uygulamada oturum açma sorunları

Erişim paneli, Azure Active Directory (Azure AD) ' de iş veya okul hesabı olan bir kullanıcının Azure AD yöneticisinin erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan Web tabanlı bir portaldır. 

Bu uygulamalar, Azure AD portalındaki Kullanıcı adına yapılandırılır. Uygulama, uygulamayı erişim paneli 'nde görmek için bir Kullanıcı ya da Kullanıcı üyesi olan bir gruba atanmalıdır.

Derin bağlantılar veya Kullanıcı erişimi URL 'Leri, kullanıcılarınızın parola SSO uygulamalarına doğrudan tarayıcıların URL çubuklarından erişmek için kullanabileceği bağlantılardır. Bu bağlantıya giderek, kullanıcılar önce erişim paneline gitmek zorunda kalmadan otomatik olarak uygulamada oturum açmalıdır. Bu, kullanıcıların Office 365 uygulama başlatıcısı 'ndan bu uygulamalara erişmek için kullandığı bağlantıdır.

## <a name="general-issues-to-check-first"></a>Önce denetlenecek genel sorunlar

-   Erişim paneli için en düşük gereksinimleri karşılayan bir **tarayıcı** kullandığınızdan emin olun.

-   Kullanıcının tarayıcısının uygulamanın URL 'sini **Güvenilen sitelerine**eklediğinizden emin olun.

-   Uygulamanın doğru şekilde **yapılandırıldığını** kontrol ettiğinizden emin olun.

-   Oturum açma işlemlerinin Kullanıcı hesabının **etkinleştirildiğinden** emin olun.

-   Kullanıcı hesabının **kilitlenmediğinden** emin olun.

-   Kullanıcının **parolasının dolmadığından veya unutulmuş** olduğundan emin olun.

-   **Multi-Factor Authentication** Kullanıcı erişimini engellemediğinden emin olun.

-   **Koşullu erişim ilkesi** veya **kimlik koruma** ilkesinin Kullanıcı erişimini engellemediğinden emin olun.

-   Multi-Factor Authentication veya koşullu erişim ilkelerinin zorlanmasını sağlamak için kullanıcının **kimlik doğrulaması iletişim bilgilerinin** güncel olduğundan emin olun.

-   Ayrıca, tarayıcınızın tanımlama bilgilerini temizlemeyi ve yeniden oturum açmayı denediğinizden emin olun.

## <a name="checking-the-deeplink"></a>Ayrıntılı bağlantı denetleniyor

Doğru ayrıntılı bağlantı olup olmadığını denetlemek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

7. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

8. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

9. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

10. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

    * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

11. Ayrıntılı bağlantısını kontrol etmek istediğiniz uygulamayı seçin.

12. **Kullanıcı erişim URL 'si**etiketini bulun. Ayrıntılı bağlantınız Bu URL ile eşleşmelidir.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını nasıl yüklenir

Erişim paneli tarayıcı uzantısını yüklemek için şu adımları izleyin:

1.  Desteklenen tarayıcılardan birinde [erişim panelini](https://myapps.microsoft.com) açın ve Azure AD 'de **Kullanıcı** olarak oturum açın.

2.  Erişim panelinde bir **Password-SSO uygulamasına** tıklayın.

3.  Yazılımı yüklemek isteyip istememe isteminde **Şimdi yüklensin**' i seçin.

4.  Tarayıcınıza bağlı olarak, indirme bağlantısına yönlendirilirsiniz. Uzantıyı tarayıcınıza **ekleyin** .

5.  Tarayıcınız istediğinde, uzantıyı **etkinleştirmek** veya **izin vermek** için seçin.

6.  Yüklendikten sonra tarayıcı oturumunuzu **yeniden başlatın** .

7.  Erişim panelinde oturum açın ve parola SSO uygulamalarınızı **başlatıp başlatamadıysanız** bkz.

Chrome ve Firefox uzantısını şu doğrudan bağlantılardan de indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması için parola çoklu oturum açmayı yapılandırma

Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure AD Galerisi 'nden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulamayı parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galerisi 'nden uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için şu adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeriden Ekle** bölümünden **bir ad girin** metin kutusuna uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce **ad metin kutusundan adını değiştirebilirsiniz** .

9.  Uygulamayı eklemek için **Ekle**' ye tıklayın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulamayı parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. Mod **parola tabanlı oturum açmayı seçin.**

9. [Kullanıcılara uygulama atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcıların adına Kullanıcı adına kimlik bilgileri de sağlayabilirsiniz ve **kimlik bilgilerini güncelleştir** ' i tıklatarak ve Kullanıcı adına Kullanıcı adını ve parolayı girebilirsiniz. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola çoklu oturum açmayı yapılandırma

Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Galeri dışı bir uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulamayı parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için şu adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeri dışı uygulama** ' ya tıklayın.

7.  **Ad** metin kutusuna uygulamanızın adını girin. Ekle ' yi seçin **.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulamayı parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

    1.  Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8.  Mod **parola tabanlı oturum açmayı seçin.**

9.  Oturum açmak için kullanıcıların kullanıcı adını ve parolasını girmesi için **oturum açma URL**'sini girin. Oturum açma alanlarının URL 'de görünür olduğundan emin olun.

10. Kullanıcılara uygulama atama.

11. Ayrıca, kullanıcıların adına Kullanıcı adına kimlik bilgileri de sağlayabilirsiniz ve **kimlik bilgilerini güncelleştir** ' i tıklatarak ve Kullanıcı adına Kullanıcı adını ve parolayı girebilirsiniz. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcıyı doğrudan uygulamaya atama

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Listeden Kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9. **atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresine göre ara** arama kutusuna, ilgilendiğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**açığa çıkarmak için listedeki **kullanıcının** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için kullanıcının profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **Isteğe bağlı:** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Kullanıcıları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

14. **Isteğe bağlı:** seçtiğiniz kullanıcılara atanacak bir rol seçmek Için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

15. Uygulamayı seçili kullanıcılara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar erişim panelinde bu uygulamaları başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmezse. 

varsa, aşağıdaki bilgilerle bir destek bileti açın:

-   Bağıntı hata KIMLIĞI

-   UPN (Kullanıcı e-posta adresi)

-   Değerine

-   Tarayıcı türü

-   Hata sırasında saat dilimi ve zaman/zaman dilimi

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama proxy 'Si ile uygulamalarınıza çoklu oturum açma sağlama](application-proxy-configure-single-sign-on-with-kcd.md)
