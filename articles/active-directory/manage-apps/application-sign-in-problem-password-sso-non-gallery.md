---
title: Access Paneli'nde parola tabanlı tek oturum açma (SSO) | Microsoft Dokümanlar
description: Parola tek oturum açma için yapılandırılan Azure AD Galerisi uygulamalarında oturum açmayla ilgili sorunları gidermek için kılavuz sağlayan sorunlu alanları tartışır.
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
ms.openlocfilehash: 9ca192c28757df189e531aee0ba2d8da288ba7e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68381229"
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Parola tek oturum açma için yapılandırılan bir Azure AD Galerisi uygulamasında oturum açma sorunları

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) iş veya okul hesabı olan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. Azure AD sürümlerine sahip bir kullanıcı, Access Paneli aracılığıyla self servis grubu ve uygulama yönetimi özelliklerini de kullanabilir. Erişim Paneli Azure portalından ayrıdır ve kullanıcıların Azure aboneliğine sahip olmasını gerektirmez.

Access Paneli'nde parola tabanlı tek oturum açma (SSO) kullanmak için Erişim Masası uzantısı kullanıcının tarayıcısına yüklenmiş olmalıdır. Kullanıcı parola tabanlı SSO için yapılandırılan bir uygulamayı seçtiğinde bu uzantı otomatik olarak indirilir.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim Paneli için tarayıcı gereksinimlerini karşılama

Erişim Paneli, JavaScript'i destekleyen ve CSS etkinleştirilmiş bir tarayıcı gerektirir. Access Paneli'nde parola tabanlı tek oturum açma (SSO) kullanmak için Erişim Masası uzantısı kullanıcının tarayıcısına yüklenmiş olmalıdır. Kullanıcı parola tabanlı SSO için yapılandırılan bir uygulamayı seçtiğinde bu uzantı otomatik olarak indirilir.

Parola tabanlı SSO için, son kullanıcının tarayıcıları şu olabilir:

-   Internet Explorer 8, 9, 10, 11 -- Windows 7 veya sonraki

-   Chrome -- Windows 7 veya sonraki ve MacOS X veya daha sonra

-   Firefox 26.0 veya sonraki -- Windows XP SP2 veya sonraki ve Mac OS X 10.6 veya sonraki

>[!NOTE]
>Tarayıcı uzantıları Microsoft Edge için desteklendiğinde, parola tabanlı SSO uzantısı Windows 10'da Microsoft Edge için kullanılabilir hale gelir.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Access Panel Tarayıcı uzantısı nasıl yüklenir?

Access Panel Tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Desteklenen tarayıcılardan birinde [Erişim Paneli'ni](https://myapps.microsoft.com) açın ve Azure AD'nizde **kullanıcı** olarak oturum açın.

2.  Access Paneli'ndeki bir **parola-SSO uygulamasını** tıklatın.

3.  Yazılımı yüklemek isteyen istemi, **Şimdi Yükle'yi**seçin.

4.  Tarayıcınıza bağlı olarak indirme linkine yönlendirilirsiniz. Uzantıyı tarayıcınıza **ekleyin.**

5.  Tarayıcınız sorarsa, uzantıyı **etkinleştir** veya **ona izin ver'i** seçin.

6.  Yüklendikten sonra tarayıcı oturumunuzu **yeniden başlatın.**

7.  Access Paneli'nde oturum açın ve parola-SSO uygulamalarınızı **başlatıp başlatamayacağınızı** görün

Chrome ve Firefox için uzantıyı aşağıdaki doğrudan linklerden de indirebilirsiniz:

-   [Chrome Erişim Paneli Uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox Erişim Paneli Uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir grup ilkesi ayarlama

Internet Explorer için Erişim Masası uzantısını kullanıcılarınızın makinelerine uzaktan yüklemenize olanak tanıyan bir grup ilkesi kurabilirsiniz.

Ön koşullar şunlardır:

-   [Active Directory Domain Services'ı](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)kurdunuz ve kullanıcılarınızın makinelerini etki alanınıza katıldınız.

-   Grup İlkesi Nesnesini (GPO) yeniden atamak için "Ayarları düzelt" iznine sahip olmalısınız. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri şu izne sahiptir: Etki Alanı Yöneticileri, Kurumsal Yöneticiler ve Grup İlkesi Oluşturucusu Sahipleri. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Grup ilkesini yapılandırma ve kullanıcılara dağıtma hakkında adım adım yönergeler için [Grup İlkesi'ni kullanarak Internet Explorer için Erişim Masası Uzantısını Nasıl DağıtAcağını](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) zinde öğretisini izleyin.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Internet Explorer'da Access Panelini Sorun Giderme

Bir tanılama aracına erişmek [için Internet Explorer için Erişim Masası Uzantısı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) kılavuzunu izleyin ve IE için uzantıyı yapılandırmayla ilgili adım adım yönergeleri izleyin.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Galeri dışı uygulama ekleme](#add-a-non-gallery-application)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

-   [Uygulamaya kullanıcı atama](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Galeri dışı uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Yönetici** veya **Ortak Yönetici** olarak oturum açın

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri Dışı uygulamasını tıklatın.**

7.  **Uygulamanızın** adını Ad metin kutusuna girin. **Ekle'yi seçin.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Parola tabanlı Oturum Açma modunu **seçin.**

9. Oturum **Açma URL'sini**girin. Bu, kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri URL'dir. Oturum açma alanlarının URL'de görünür olduğundan emin olun.

10. Kullanıcıları uygulamaya atayın.

11. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

### <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

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

11. Bir **onay kutusunu**ortaya çıkarmak için listedeki **kullanıcının** üzerine taşırın. Kullanıcınızı **Seçili** listeye eklemek için kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla kullanıcı eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam **ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin.

15. Uygulamayı seçili kullanıcılara atamak için **Atla** düğmesini tıklatın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar bu uygulamaları Access Paneli'nde başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmüyorsa

varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata oluşur sırasında saat dilimi ve saat/zaman dilimi oluşur

-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

