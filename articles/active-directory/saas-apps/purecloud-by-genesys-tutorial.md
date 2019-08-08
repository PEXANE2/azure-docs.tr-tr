---
title: 'Öğretici: Genesys tarafından Pureci ile tümleştirmeyi Azure Active Directory | Microsoft Docs'
description: Genesys tarafından Azure Active Directory ve Purecg arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 92df57737b75ab2c9bb9992dd3f223f55dbc39bb
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68826128"
---
# <a name="tutorial-azure-active-directory-integration-with-purecloud-by-genesys"></a>Öğretici: Genesys tarafından Purecre ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Genesys tarafından Purecre 'yi tümleştirmeyi öğreneceksiniz.
Azure AD ile Genesys tarafından Purecre 'yi tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de Genesys tarafından Purecre 'ye erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Genesys (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Genesys tarafından Purecı ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Genesys çoklu oturum açma özellikli aboneliğe göre Purecg

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Genesys tarafından Purecm **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Galeriden Genesys 'e göre Purecg ekleme

Genesys tarafından Azure AD 'ye göre purecı tümleştirmesini yapılandırmak için, Genesys tarafından yönetilen SaaS uygulamaları listenize göre Poesys 'e göre pureci 'yi eklemeniz gerekir.

**Galeriden Genesys 'e göre Pureck eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Genesys 'e göre purecı**yazın, sonuç panelinden **Maesys tarafından pureck** ' ı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuçlar listesinde Genesys tarafından Pureci](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Genesys Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Genesys ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Genesys ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Genesys çoklu oturum açma 'ya göre Purecg 'ı yapılandırın](#configure-purecloud-by-genesys-single-sign-on)** .
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Genesys test kullanıcısına göre Purecon](#create-purecloud-by-genesys-test-user)** , kullanıcının Azure AD gösterimine bağlı olan Genesys tarafından potta Simon 'ın bir karşılığı olacak şekilde.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, Genesys tarafından Purecg ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Genesys** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Genesys etki alanı ve URL 'Ler çoklu oturum açma bilgileri tarafından Purecg](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, bölgenize göre bir URL yazın:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. **Yanıt URL 'si** metin kutusuna, bölgenize göre bir URL yazın:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml` |

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Genesys etki alanı ve URL 'Ler çoklu oturum açma bilgileri tarafından Purecg](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, bölgenize göre bir URL yazın:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

6. Genesys uygulamasına göre Purecyüksek, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

7. Genesys uygulamasına göre Purecsesli, daha fazla özniteliğin SAML yanıtına geri geçirilmesini bekler. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde AŞAĞıDAKI tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği|
    | ---------------| --------------- |
    | Email | User. userprinicipalname |
    | OrganizationName | `Your organization name` |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. **Genesys tarafından Purecg ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-purecloud-by-genesys-single-sign-on"></a>Genesys çoklu oturum açma ile Purecg 'yi yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak Genesys tarafından Purecy 'de oturum açın.

2. Üstteki **yönetici** ' ye tıklayın ve **tümleştirmeler**altında **Çoklu oturum açma** ' ya gidin.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure01.png)

3. **ADFS/Azure AD (Premium)** sekmesine geçin ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Azure portal indirdiğiniz Base-64 kodlu sertifikayı **ADFS sertifikasına**yüklemek Için, **Araştır** ' a tıklayın.

    b. **ADFS ıSSUER URI** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.

    c. **Hedef URI** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    d. **Bağlı olan taraf tanımlayıcı** değeri için Azure Portal gitmeniz gerekir, **Genesys** uygulama tümleştirmesi sayfasında, **ÖZELLIKLER** sekmesine tıklayın ve **uygulama kimliği** değerini kopyalayın. **Bağlı olan taraf tanımlayıcısı** metin kutusuna yapıştırın. 

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. **Kaydet**'e tıklayın.   

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına **bricompansıon\@yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Genesys tarafından Purecg 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra **Genesys 'e göre pureck**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Genesys tarafından Purecı**' yi seçin.

    ![Uygulamalar listesindeki Genesys bağlantısına göre Purecme bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-purecloud-by-genesys-test-user"></a>Genesys test kullanıcısına göre Purecsesli oluştur

Azure AD kullanıcılarının Genesys tarafından Pureck 'da oturum açmasını sağlamak için, Genesys tarafından Purecre 'ye sağlanması gerekir. Genesys tarafından Pureci tarafından sağlama, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Genesys tarafından yönetici olarak Pureci 'de oturum açın.

2. Üstteki **yönetici** ' ye tıklayın ve **kişiler & izinler**' in altındaki **kişilere** gidin.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure03.png)

3. Kişiler sayfasında **kişi ekle**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure04.png)

4. **Kuruluşa kişi ekleme** açılır penceresinde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. **Tam ad** metin kutusuna, **Brittasıon**gibi kullanıcının adını girin.

    b. **E-posta** metin kutusuna, **\@brittasıon contoso.com**gibi kullanıcının e-postasını girin.
    
    c.           **Oluştur**'a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Genesys kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Genesys tarafından otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

