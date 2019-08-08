---
title: 'Öğretici: SD öğeleriyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SD öğeleri arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: a0272de238055151e73c13687f25745c3f402eb4
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68826151"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Öğretici: SD öğeleriyle Azure Active Directory tümleştirme

Bu öğreticide SD öğelerini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
SD öğelerini Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* SD öğelerine erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SD öğelerine (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini SD öğeleriyle yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* SD öğeleri çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* SD öğeleri **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-sd-elements-from-the-gallery"></a>Galeriden SD öğeleri ekleme

SD öğelerinin Azure AD ile tümleştirilmesini yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize SD öğeleri eklemeniz gerekir.

**Galeriden SD öğeleri eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SD öğeleri**yazın, sonuç panelinden **SD öğeleri** ' ni seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuçlar listesindeki SD öğeleri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test KULLANıCıSıNA göre SD öğeleriyle yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve SD öğelerindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı SD öğeleriyle birlikte yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SD öğelerini tek oturum açma yapılandırma](#configure-sd-elements-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan SD öğelerinde Britta Simon 'un bir karşılığı olacak şekilde, **[SD öğeleri test kullanıcısı oluşturun](#create-sd-elements-test-user)** .
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı SD öğeleriyle yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **SD öğeleri** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki adımları uygulayın:

    ![SD öğeleri etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [SD öğeleri istemci destek ekibine](mailto:support@sdelements.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. SD öğeleri uygulamanız belirli bir biçimde SAML onayları bekler, bu da SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektirir. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, yukarıdaki görüntüde gösterildiği gibi, **Düzen simgesini** kullanarak talepleri DÜZENLEYIN veya aşağıdaki resimde gösterildiği gibi SAML belirteci özniteliğini yapılandırmak için **yeni talep Ekle** ' yi kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:

    | Ad |  Kaynak özniteliği|
    | --- | --- |
    | email |Kullanıcı. Mail |
    | FirstName |Kullanıcı. |
    | Soyadı |User. soyadı |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **SD öğelerini ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sd-elements-single-sign-on"></a>SD öğelerini çoklu oturum açma yapılandırma

1. Çoklu oturum açmayı etkinleştirmek için [SD öğelerinin destek ekibine](mailto:support@sdelements.com) başvurun ve indirilen sertifika dosyasını sağlayın.

1. Farklı bir tarayıcı penceresinde, SD öğeleri kiracınızda yönetici olarak oturum açın.

1. Üstteki menüde, **sistem**' e ve ardından **Çoklu oturum açma**' ya tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/sd-elements-tutorial/tutorial_sd-elements_09.png)

1. **Çoklu oturum açma ayarları** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/sd-elements-tutorial/tutorial_sd-elements_10.png)

    a. **SSO türü**olarak **SAML**' yi seçin.

    b. **Kimlik sağlayıcısı VARLıK kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    c. **Kimlik sağlayıcısı çoklu oturum açma hizmeti** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SD öğelerine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **SD öğeleri**' ni seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SD öğeleri**' ni seçin.

    ![Uygulamalar listesindeki SD öğeleri bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-sd-elements-test-user"></a>SD öğeleri test kullanıcısı oluştur

Bu bölümün amacı SD öğelerinde Britta Simon adlı bir Kullanıcı oluşturmaktır. SD öğelerinde, SD öğelerinin oluşturulması, el ile gerçekleştirilen bir görevdir.

**SD öğelerinde Britta Simon oluşturmak için aşağıdaki adımları uygulayın:**

1. Bir Web tarayıcısı penceresinde, SD öğeleri şirket sitenizde yönetici olarak oturum açın.

1. Üstteki menüden **Kullanıcı yönetimi**' ne ve ardından **Kullanıcılar**' a tıklayın.

    ![SD öğeleri test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. **Yeni Kullanıcı Ekle**' ye tıklayın.

    ![SD öğeleri test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)

1. **Yeni Kullanıcı Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![SD öğeleri test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 

    a. **E-posta** metin kutusunda, kullanıcının **brittasimon@contoso.com** e-postasını girin.

    b. **Ad** metin kutusuna, ilk Kullanıcı adını **Britta**gibi girin.

    c. **Soyadı** metin kutusunda, **Simon**gibi kullanıcı adının soyadını girin.

    d. **Rol**olarak **Kullanıcı**' yı seçin.

    e. **Kullanıcı oluştur**' a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SD öğeleri kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SD öğelerinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)