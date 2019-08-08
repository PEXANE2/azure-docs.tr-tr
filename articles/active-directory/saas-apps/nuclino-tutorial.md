---
title: 'Öğretici: Nuclino ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Nuclino arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.openlocfilehash: cce4857dd270f6868b8ae4d0de2117e46c20ce7c
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68826309"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Öğretici: Nuclino ile tümleştirme Azure Active Directory

Bu öğreticide, Nuclino Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz.
Nuclino 'ın Azure AD ile tümleştirilmesi aşağıdaki avantajları sağlar:

* Azure AD 'de, Nuclino 'ya erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Nuclino 'nun (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Nuclino ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Nuclino çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Nuclino, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekliyor

* Nuclino **, tam zamanında** Kullanıcı sağlamayı destekliyor

## <a name="adding-nuclino-from-the-gallery"></a>Galeriden Nuclino ekleniyor

Nuclino 'un tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki Nuclino 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Nuclino eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **nuclino**yazın, sonuç panelinden **nuclino** ' u seçin, sonra da uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuçlar listesinde nuclino](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Nuclino Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Nuclino 'da ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Nuclino ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Nuclino çoklu oturum açmayı yapılandırın](#configure-nuclino-single-sign-on)** .
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı Nuclino 'da Britta Simon 'ın bir karşılığı olacak şekilde **[nuclino test kullanıcısı oluşturun](#create-nuclino-test-user)** .
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Nuclino ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **nuclino** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Nuclino etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, Bu öğreticinin ilerleyen kısımlarında açıklanan **kimlik doğrulama** bölümündeki gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin.

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Nuclino etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://app.nuclino.com/<UNIQUE-ID>/login`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Bu değeri almak için [Nuclino istemci destek ekibine](mailto:contact@nuclino.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. Nuclino uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

7. Yukarıdakine ek olarak, Nuclino uygulaması SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekliyor. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde AŞAĞıDAKI tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Ad |  Kaynak özniteliği|
    | ---------------| --------- |
    | first_name | Kullanıcı. |
    | last_name | User. soyadı |

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

9. **Set up Nuclino** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-nuclino-single-sign-on"></a>Nuclino çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Nuclino şirket sitenizde yönetici olarak oturum açın.

2. **Simgeye**tıklayın.

    ![Nuclino yapılandırması](./media/nuclino-tutorial/configure1.png)

3. **Azure AD SSO** ' ya tıklayın ve açılan listeden **ekip ayarları** ' nı seçin.

    ![Nuclino yapılandırması](./media/nuclino-tutorial/configure2.png)

4. Sol gezinti bölmesinden **kimlik doğrulaması** ' nı seçin.

    ![Nuclino yapılandırması](./media/nuclino-tutorial/configure3.png)

5. **Kimlik doğrulama** bölümünde aşağıdaki adımları uygulayın:

    ![Nuclino yapılandırması](./media/nuclino-tutorial/configure4.png)

    a. **SAML tabanlı çoklu oturum açma (SSO)** seçeneğini belirleyin.

    b. **ACS URL 'sini kopyalayın (bunu SSO sağlayıcınıza kopyalamanız ve bu Sağlayıcınızdaki bir değere yapıştırmanız gerekir)** ve Azure Portal, **temel SAML YAPıLANDıRMASı** bölümünün **yanıt URL** metin kutusuna yapıştırmanız gerekir.

    c. **VARLıK kimliğini Kopyala (bunu SSO sağlayıcınıza kopyalamanız ve bu projenize yapıştırmanız gerekir)** değerini, Azure Portal **temel SAML yapılandırması** bölümünün **tanımlayıcı** metin kutusuna yapıştırmanız gerekir.

    d. **SSO URL** metin kutusuna, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    e. **VARLıK kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    f. İndirilen **sertifika (base64)** Dosyanızı Not defteri 'nde açın. İçeriğini panonuza kopyalayın ve sonra **genel sertifika** metin kutusuna yapıştırın.

    g. **Değişiklikleri Kaydet**' e tıklayın.

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

Bu bölümde, Nuclino 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve **nuclino**' u seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Nuclino**' ı seçin.

    ![Uygulamalar listesindeki Nuclino bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-nuclino-test-user"></a>Nuclino test kullanıcısı oluştur

Bu bölümde, Nuclino 'da Britta Simon adlı bir Kullanıcı oluşturulur. Nuclino, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı Nuclino 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Nuclino destek ekibine](mailto:contact@nuclino.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Nuclino kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Nuclino 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

