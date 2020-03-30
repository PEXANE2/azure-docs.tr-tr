---
title: SSO için yapılandırılan Azure AD galerisi uygulamasında oturum açma sorunu | Microsoft Dokümanlar
description: Parola tek oturum açma için yapılandırılan bir Azure AD Galerisi uygulamasıyla ilgili sorunları giderme sorunları nasıl giderilir?
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
ms.openlocfilehash: e9fd17d9e066be6a1abff5165436a09b8921184e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68381301"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>SSO için yapılandırılan bir Azure REKLAM galerisi uygulamasıyla oturum açma sorunları

Access Panel web tabanlı bir portaldır. Azure Active Directory (Azure AD) çalışmasına veya okul hesaplarına sahip kullanıcıların izinleri olan bulut tabanlı uygulamalara erişmesini sağlar. Azure AD sürümlerine sahip kullanıcılar, Access Panel aracılığıyla self servis grubu ve uygulama yönetimi özelliklerini de kullanabilir.

Access Panel, Azure portalından ayrıdır. Kullanıcıların Access Panel'i kullanmak için Azure aboneliğine ihtiyacı yoktur.

Access Panel'de parola tabanlı tek oturum açma (SSO) kullanmak için Tarayıcınızda Access Panel uzantısı yüklenmiş olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulamayı seçtiğinizde otomatik olarak indirilir.

## <a name="browser-requirements-for-access-panel"></a>Access Panel için tarayıcı gereksinimleri

Access Panel, JavaScript'i destekleyen ve CSS özellikli bir tarayıcı gerektirir.

Aşağıdaki tarayıcılar parola tabanlı SSO'yu destekler:

- Windows 7 veya sonraki internet explorer 8, 9, 10 ve 11

- Windows 7 veya sonraki veya MacOS X veya daha sonra Chrome

- Firefox 26.0 veya daha sonra Windows XP SP2 veya daha sonra veya Mac OS X 10.6 veya daha sonra

>[!NOTE]
>Tarayıcı uzantıları desteği Microsoft Edge'e eklendiğinde, parola tabanlı SSO uzantısı Windows 10'da Microsoft Edge için kullanılabilir hale gelir.

## <a name="install-the-access-panel-browser-extension"></a>Access Panel Tarayıcı uzantısını yükleme

Şu adımları uygulayın:

1. Desteklenen bir tarayıcıda [Erişim Paneli'ni](https://myapps.microsoft.com) açın ve Azure AD'de kullanıcı olarak oturum açın.

2. Access Panel'de parola SSO özellikli bir uygulama seçin.

3. Sizden istendiğinde, Şimdi **Yükle'yi**seçin.

4. Tarayıcınıza dayalı bir indirme bağlantısına yönlendirilirsiniz. Tarayıcı uzantısını yüklemek için **Ekle'yi** seçin.

5. Sizden istenirse, **Etkinleştir** veya **İzin Ver'i**seçin.

6. Yüklemeden sonra tarayıcınızı yeniden başlatın.

7.  Access Panel'de oturum açın ve parola SSO özellikli uygulamalarınızı başlatıp başlatamayacağınızı görün.

Ayrıca chrome ve firefox uzantılarını doğrudan şu bağlantılar aracılığıyla indirebilirsiniz:

-   [Chrome Erişim Paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox Erişim Paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir grup ilkesi ayarlama

Internet Explorer için Erişim Masası uzantısını kullanıcılarınızın makinelerine uzaktan yüklemenize olanak tanıyan bir grup ilkesi ayarlayabilirsiniz.

Bu ön koşullar şunlardır:

-   [Etkin Dizin Etki Alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) ayarlanmalıdır ve kullanıcılarınızın makineleri etki alanınıza katılmalı.

-   Grup İlkesi Nesnesini (GPO) yeniden atamak için "Ayarları düzelt" izniniz vardır. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri şu izne sahiptir: Etki Alanı Yöneticileri, Kurumsal Yöneticiler ve Grup İlkesi Oluşturucusu Sahipleri. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Grup ilkesini yapılandırmak ve kullanıcılara dağıtmak [için, grup ilkesini kullanarak Internet Explorer için Erişim Masası uzantısını nasıl dağıtılayabilirsiniz'](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy)e bakın.

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Internet Explorer'da Sorun Giderme Access Paneli

Uzantıyı yapılandırmak için bir tanılama aracına ve yönergelerine erişmek için Internet [Explorer için Access Panel uzantısını giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting)sorununa bakın.

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Azure AD galerisi uygulaması için parola SSO'su yapılandırma

Azure AD galerisinden bir uygulamayı yapılandırmak için aşağıdakileri yapmanız gerekir:

-   Uygulamayı Azure REKLAM galerisinden ekleme
-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-app-for-password-sso)
-   [Uygulamaya kullanıcı atama](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>Uygulamayı Azure REKLAM galerisinden ekleme

Şu adımları uygulayın:

1. Azure [portalını](https://portal.azure.com) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti bölmesinin üst kısmındaki **Tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinden **Kurumsal Uygulamalar'ı** seçin.

5. **Kurumsal Uygulamalar** bölmesinin sağ üst köşesinde **Ekle'yi** seçin.

6. Galeri **bölümünden Ekle bölümüne,** **ad ekle** kutusuna uygulamanın adını yazın.

7. SSO için yapılandırmak istediğiniz uygulamayı seçin.

8. *İsteğe bağlı:* Uygulamayı eklemeden önce **Ad** kutusunda adını değiştirebilirsiniz.

9. Uygulamayı eklemek için **Ekle**’ye tıklayın.

   Kısa bir gecikmeden sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-app-for-password-sso"></a>SSO şifresi için uygulamayı yapılandırma

Şu adımları uygulayın:

1. Azure [portalını](https://portal.azure.com/) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti bölmesinin üst kısmındaki **Tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol tarafındaki bölmede **Tek oturum** açma'yı seçin.

8. **Parola tabanlı oturum açma modunu** seçin.

9. Kullanıcıları uygulamaya atayın.

10. Ayrıca kullanıcılar için kimlik bilgileri sağlayabilirsiniz. (Aksi takdirde, kullanıcılardan uygulama başlangıç sırasında kimlik bilgilerini girmeleri istenir.) Bunu yapmak için, kullanıcıların satırlarını seçin. Ardından **Kimlik Bilgilerini Güncelleştir'i** seçin ve kullanıcı adlarını ve parolalarını girin.

### <a name="assign-users-to-the-app"></a>Uygulamaya kullanıcı atama

Kullanıcıları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com/) açın ve genel yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti **acısındaki tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. **Uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. Listeden, bir kullanıcıyı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti bölmesinden **Kullanıcılar ve Gruplar'ı** seçin.

8. **Atama Ekle** bölmesini açmak için Kullanıcılar **ve Gruplar** listesinin en üstüne **Ekle'yi** seçin.

9. **Atama Ekle** bölmesinde Kullanıcıları **ve grupları** seçin.

10. **Ada veya e-posta adresine göre arama** kutusuna, atamak istediğiniz kullanıcının tam adını veya e-posta adresini yazın.

11. Listedeki kullanıcının üzerine titretin. Kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu seçin ve bu kullanıcıyı **Seçili** listeye ekleyin.

12. *İsteğe bağlı:* Başka bir kullanıcı eklemek için, **Ada veya e-posta adresi** kutusuna başka bir ad veya e-posta adresi yazın ve ardından bu kullanıcıyı **Seçili** listeye eklemek için onay kutusunu seçin.

13. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanan kullanıcılar ve gruplar listesine eklemek için **Seç'i** tıklatın.

14. *İsteğe bağlı:* Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rolü Seç'i** tıklatın.

15. Uygulamayı seçili kullanıcılara atamak için **Ata'yı** seçin.

    Kısa bir gecikmeden sonra, kullanıcılar bu uygulamalara Access Panel'den erişebilecektir.

## <a name="request-support"></a>Destek isteyin 
SSO'u kurup kullanıcıları atadığınızda bir hata iletisi alırsanız, bir destek bileti açın. Aşağıdaki bilgilerin mümkün olduğunca çoğunu ekleyin:

-   Korelasyon hatası kimliği
-   UPN (kullanıcı e-posta adresi)
-   Kiracı Kimliği
-   Tarayıcı türü
-   Hata oluştuğunda saat dilimi ve saat/saat çerçevesi
-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
