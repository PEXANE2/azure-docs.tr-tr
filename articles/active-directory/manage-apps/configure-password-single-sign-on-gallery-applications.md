---
title: Azure AD Galeri uygulaması için parola çoklu oturum açmayı yapılandırma | Microsoft Docs
description: Bir uygulamayı Azure AD uygulama galerisinde zaten listelendiğinde güvenli parola tabanlı çoklu oturum açma için yapılandırma
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
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: eb37fe247901b799a845ce75723a4a6b535cbb28
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422591"
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması için parola çoklu oturum açmayı yapılandırma

[Azure AD Uygulama Galerisi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)'nden bir uygulama eklediğinizde, kullanıcılarınızın bu uygulamada oturum açmasını istediğinizi tercih edersiniz. Bu seçeneği, [Azure Portal](https://portal.azure.com/)bir kurumsal uygulamadaki **Çoklu oturum açma** gezinti öğesini seçerek dilediğiniz zaman yapılandırabilirsiniz.

Kullanabileceğiniz çoklu oturum açma yöntemlerinden biri, [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) seçeneğidir. Bu, uygulamaları Azure AD 'ye hızla tümleştirmenize başlamak için harika bir yoldur ve şunları yapmanıza olanak sağlar:

-   Azure AD ile tümleştirmiş olduğunuz uygulama için Kullanıcı adlarını ve parolaları güvenli bir şekilde depolayıp kaydederek **kullanıcılarınız Için çoklu oturum açmayı** etkinleştirin

-   Yalnızca Kullanıcı adı ve parola alanlarının oturum açmasını gerektiren uygulamalar için **Çoklu oturum açma alanları gerektiren uygulamalar desteklenir**

-   Kullanıcılarınızın kimlik bilgilerini girerken [uygulama erişimi panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) göreceği Kullanıcı adı ve parola giriş alanlarının **etiketlerini özelleştirin**

-   **Kullanıcılarınızın** , [uygulama erişimi paneline](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) el ile yazdıkları mevcut uygulama hesapları için kendi kullanıcı adlarını ve parolalarını sağlamasına izin verin

-   **İş grubunun bir üyesinin** , [self servis uygulama erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) özelliğini kullanarak bir kullanıcıya atanan kullanıcı adlarını ve parolaları belirtmesini sağlar

-   Bir [uygulamaya Kullanıcı atarken](#assign-a-user-to-an-application-directly) kimlik bilgilerini güncelleştir özelliğini kullanarak bir **yöneticinin** bir kullanıcıya atanan kullanıcı adlarını ve parolaları belirtmesini sağlar

-   Bir [uygulamaya grup atarken](#assign-an-application-to-a-group-directly) kimlik bilgilerini güncelleştir özelliğini kullanarak bir grup kişi tarafından kullanılan paylaşılan kullanıcı adını veya parolayı belirtmesini sağlar

Aşağıdaki bölümde, zaten [Azure AD uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bulunan bir uygulamada [parola tabanlı çoklu oturum açmayı](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) nasıl etkinleştirebileceğinizi açıklanmaktadır.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure AD Galerisi 'nden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulamayı parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

-   Uygulamayı bir kullanıcıya veya gruba atama

    -   [Uygulamaya doğrudan kullanıcı atama](#assign-a-user-to-an-application-directly)

    -   [Bir gruba doğrudan bir uygulama atama](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galerisi 'nden uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için şu adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeriden Ekle** bölümünden **bir ad girin** metin kutusuna uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce **ad metin kutusundan** adını değiştirebilirsiniz.

9.  Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

## <a name="configure-the-application-for-password-single-sign-on"></a>Uygulamayı parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. Mod **parola tabanlı oturum açmayı seçin.**

9. [Kullanıcılara uygulama atama](#assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcıların adına Kullanıcı adına kimlik bilgileri de sağlayabilirsiniz ve **kimlik bilgilerini güncelleştir** ' i tıklatarak ve Kullanıcı adına Kullanıcı adını ve parolayı girebilirsiniz. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.

## <a name="assign-a-user-to-an-application-directly"></a>Uygulamaya doğrudan kullanıcı atama

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **Seçim** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

## <a name="assign-an-application-to-a-group-directly"></a>Bir gruba doğrudan bir uygulama atama

Bir uygulamaya doğrudan bir veya daha fazla grup atamak için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **Seçim** Birden **fazla grup eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam grup adı** yazın ve bu grubu **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Kısa bir süre sonra, seçtiğiniz kullanıcılar erişim panelinde bu uygulamaları başlatabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
