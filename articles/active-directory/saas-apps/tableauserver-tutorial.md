---
title: 'Öğretici: Tableau Server ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Tableau Server arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: f9ef179c1a93d8b2f97c47eb4c68d0312d55d3d1
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825988"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Öğretici: Tableau Server ile tümleştirme Azure Active Directory

Bu öğreticide, Tableau Server 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Tableau Server 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Tableau sunucusuna erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Tableau Server 'a (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Tableau Server ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Tableau sunucusu çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Tableau sunucusu **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau Server ekleme

Tableau Server tümleştirmesini Azure AD ile yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize Tableau sunucusu eklemeniz gerekir.

**Galeriden Tableau sunucusu eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau Server**yazın, sonuç panelinden **Tableau Server** ' ı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuç listesinde Tableau sunucusu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Tableau Server Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Tableau Server 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı Tableau Server ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Tableau sunucusu çoklu oturum açmayı yapılandırın](#configure-tableau-server-single-sign-on)** .
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan Tableau Server 'da Britta Simon 'ın bir karşılığı olacak şekilde **[Tableau Server test kullanıcısı oluşturun](#create-tableau-server-test-user)** .
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Tableau Server ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Tableau Server** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tableau Server etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://azure.<domain name>.link`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://azure.<domain name>.link`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > Yukarıdaki değerler gerçek değer değildir. Değerleri, Öğreticinin ilerleyen kısımlarında açıklanan Tableau sunucu yapılandırma sayfasından gerçek URL ve tanımlayıcıyla güncelleştirin.

5. Tableau Server uygulaması, aşağıda belirtildiği gibi tanımlanması gereken özel bir talep **Kullanıcı adı** bekliyor. Bu, benzersiz kullanıcı tanımlayıcısı talebi yerine Kullanıcı tanımlayıcısı olarak kullanılır. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri & talepler** bölümünde yönetebilirsiniz. **Kullanıcı öznitelikleri & talepler** iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri & talepler** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, YUKARıDAKI görüntüde gösterildiği gibi SAML belirteci özniteliğini yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği | Ad Alanı |
    | ---------------| --------------- | ----------- |
    | kullanıcı adı | User. UserPrincipalName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanı** değerini girin.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. **Tableau sunucusunu ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-tableau-server-single-sign-on"></a>Tableau sunucusu çoklu oturum açmayı yapılandırma

1. Uygulamanız için yapılandırılmış SSO 'yu almak için, Tableau sunucu kiracınızda yönetici olarak oturum açmanız gerekir.

2. **Yapılandırma** sekmesinde, **Kullanıcı kimliği & erişim**' i seçin ve ardından **kimlik doğrulama** yöntemi sekmesini seçin.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. **Yapılandırma** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. **Kimlik doğrulama yöntemi**için SAML ' yi seçin.

    b. **Sunucu IÇIN SAML kimlik doğrulamasını etkinleştir**onay kutusunu seçin.

    c. Tableau sunucusu dönüş URL 'SI — gibi Tableau Server kullanıcılarına erişim <http://tableau_server>verilecek URL. Kullanılması `http://localhost` önerilmez. Sondaki eğik çizgiyle (örneğin, `http://tableau_server/`) bir URL kullanılması desteklenmez. **Tableau sunucusu geri dönüş URL 'sini** kopyalayın ve Azure Portal **temel SAML yapılandırması** bölümünde URL metin kutusunu **oturum** açmak için yapıştırın

    d. SAML varlık KIMLIĞI — varlık KIMLIĞI, Tableau sunucu yüklemenizi IDP 'ye benzersiz şekilde tanımlar. İsterseniz Tableau sunucu URL 'nizi buraya girebilirsiniz, ancak Tableau Server URL 'niz olması gerekmez. **SAML VARLıK kimliğini** kopyalayın ve Azure Portal **temel SAML yapılandırması** bölümünde **tanımlayıcı** metin kutusuna yapıştırın

    e. **XML meta verilerini Indir dosyasına** tıklayın ve metin düzenleyici uygulamasında açın. Onaylama tüketici hizmeti URL 'sini http post ve Index 0 ile bulun ve URL 'YI kopyalayın. Şimdi bunu, Azure portal **temel SAML yapılandırması** bölümünde **yanıt URL** metin kutusuna yapıştırın

    f. Azure portal 'ten indirilen Federasyon meta veri dosyanızı bulun ve **SAML IDP meta veri dosyasına**yükleyin.

    g. IDP 'nin Kullanıcı adlarını, görünen adları ve e-posta adreslerini tutmak için kullandığı özniteliklerin adlarını girin.

    h. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Müşterinin, Tableau Server SAML SSO yapılandırmasındaki herhangi bir sertifikayı karşıya yüklemesi gerekir ve bu, SSO akışında yok sayılır. Tableau sunucusunda SAML yapılandırması için yardıma ihtiyacınız varsa lütfen [SAML 'Yi yapılandırma](https://help.tableau.com/current/server/en-gb/saml_config_steps_tsm_ui.htm)makalesine başvurun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Tableau sunucusuna erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve **Tableau sunucusu**' nu seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau Server**' ı seçin.

    ![Uygulamalar listesindeki Tableau sunucusu bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-tableau-server-test-user"></a>Tableau Server test kullanıcısı oluştur

Bu bölümün amacı, Tableau Server 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. Tableau sunucusundaki tüm kullanıcıları sağlamanız gerekir.

Kullanıcının Kullanıcı adı, Kullanıcı **adının**Azure AD özel özniteliğinde yapılandırdığınız değerle eşleşmelidir. Doğru eşleme ile tümleştirme, Azure AD çoklu oturum açma yapılandırması ile çalışmalıdır.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, kuruluşunuzda Tableau sunucu yöneticisiyle iletişim kurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Tableau sunucu kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Tableau sunucusunda otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

