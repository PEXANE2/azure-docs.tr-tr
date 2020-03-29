---
title: Deeplink kullanarak bir uygulamaya oturum açma sorunları | Microsoft Dokümanlar
description: Azure AD'yi kullanarak deeplink URL'den bir uygulamaya erişen sorunları giderme sorunları nasıl giderilir?
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65825424"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Deeplink kullanarak bir uygulamada oturum açma sorunları

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) çalışan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. 

Bu uygulamalar Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün bir şekilde yapılandırılmalı ve kullanıcının üyesi olduğu bir gruba Access Panel'de uygulamayı görmek için atanmalıdır.

Derin bağlantılar veya Kullanıcı erişim URL'leri, kullanıcılarınızın parola-SSO uygulamalarına doğrudan tarayıcılarının URL çubuklarından erişmek için kullanabileceği bağlantılardır. Bu bağlantıya yönlendirilerek, kullanıcılar önce Access Paneli'ne gitmek zorunda kalmadan uygulamada otomatik olarak oturum alabilmelerini sağlar. Bu, kullanıcıların bu uygulamalara Office 365 uygulama başlatıcısından erişmek için kullandıkları bağlantıdır.

## <a name="general-issues-to-check-first"></a>Önce kontrol etmek için genel konular

-   Erişim Paneli için minimum gereksinimleri karşılayan bir **tarayıcı** kullandığınızdan emin olun.

-   Kullanıcının tarayıcısının uygulamanın URL'sini **güvenilir sitelerine**eklemiştir.

-   Uygulamanın doğru şekilde **yapılandırıldığından** emin olun.

-   Oturum açma için kullanıcının hesabının **etkin** olduğundan emin olun.

-   Kullanıcının hesabının **kilitlendirolmadığından** emin olun.

-   Kullanıcının **parolasının süresinin dolmadığından veya unutulmadığından** emin olun.

-   Çok **Faktörlü Kimlik Doğrulama'nın** kullanıcı erişimini engellemediğinden emin olun.

-   **Koşullu Erişim ilkesinin** veya **Kimlik Koruma** ilkesinin kullanıcı erişimini engellemediğinden emin olun.

-   Çok Faktörlü Kimlik Doğrulama veya Koşullu Erişim ilkelerinin uygulanmasına izin vermek için kullanıcının **kimlik doğrulama iletişim bilgisinin** güncel olduğundan emin olun.

-   Tarayıcınızın çerezlerini temizlemeyi ve yeniden oturum açmaya çalışmayı da denediğinden emin olun.

## <a name="checking-the-deeplink"></a>deeplink kontrol

Doğru deeplink'e sahip olup olmadığını kontrol etmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

7. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

8. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

9. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

10. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

    * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

11. Deeplink'i kontrol etmek istediğiniz uygulamayı seçin.

12. Kullanıcı **Erişimi URL**etiketini bulun. Deeplink'iniz bu URL ile eşleşmelidir.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Access Panel Tarayıcı uzantısı nasıl yüklenir?

Access Panel Tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Desteklenen tarayıcılardan birinde [Erişim Paneli'ni](https://myapps.microsoft.com) açın ve Azure AD'nizde **kullanıcı** olarak oturum açın.

2.  Access Paneli'ndeki bir **parola-SSO uygulamasını** tıklatın.

3.  Yazılımı yüklemek isteyen istemi, **Şimdi Yükle'yi**seçin.

4.  Tarayıcınıza bağlı olarak indirme linkine yönlendirilirsiniz. Uzantıyı tarayıcınıza **ekleyin.**

5.  Tarayıcınız sorarsa, uzantıyı **etkinleştir** veya **ona izin ver'i** seçin.

6.  Yüklendikten sonra tarayıcı oturumunuzu **yeniden başlatın.**

7.  Access Paneli'nde oturum açın ve parola-SSO uygulamalarınızı **başlatıp başlatamayacağınızı** görün

Chrome ve Firefox'un uzantısını aşağıdaki doğrudan bağlantılardan da indirebilirsiniz:

-   [Chrome Erişim Paneli Uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox Erişim Paneli Uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD galerisi uygulaması için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmalısınız:

-   [Azure REKLAM galerisinden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure REKLAM galerisinden uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri bölümünden ekle** bölümünden bir ad textbox **girin,** uygulamanın adını yazın.

7.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce, adını **Ad** metin kutusundan değiştirebilirsiniz.

9.  Uygulamayı eklemek için **Ekle'yi**tıklatın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Parola tabanlı Oturum Açma modunu **seçin.**

9. [Kullanıcıları uygulamaya atayın.](#how-to-assign-a-user-to-an-application-directly)

10. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmalısınız:

-   [Galeri dışı uygulama ekleme](#add-a-non-gallery-application)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Galeri dışı uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri Dışı uygulamasını tıklatın.**

7.  **Uygulamanızın** adını Ad metin kutusuna girin. **Ekle'yi seçin.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

    1.  Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6.  Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8.  Parola tabanlı Oturum Açma modunu **seçin.**

9.  Oturum **Açma**URL'sini, kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri URL'yi girin. Oturum açma alanlarının URL'de görünür olduğundan emin olun.

10. Kullanıcıları uygulamaya atayın.

11. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcıyı doğrudan bir uygulamaya atama

Bir veya daha fazla kullanıcıyı doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Listeden bir kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

8. **Atama** **Ekle** bölmesini açmak için Kullanıcılar **ve Gruplar** listesinin üst kısmındaki Ekle düğmesini tıklatın.

9. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçicisini tıklatın.

10. Ada veya **e-posta adresi** arama kutusuna atamak istediğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**ortaya çıkarmak için listedeki **kullanıcının** üzerine taşırın. Kullanıcınızı **Seçili** listeye eklemek için, kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla kullanıcı eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam **ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin.

15. Uygulamayı seçili kullanıcılara atamak için **Atla** düğmesini tıklatın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar bu uygulamaları Access Paneli'nde başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmüyorsa. 

varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata oluşur sırasında saat dilimi ve saat/zaman dilimi oluşur

-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
