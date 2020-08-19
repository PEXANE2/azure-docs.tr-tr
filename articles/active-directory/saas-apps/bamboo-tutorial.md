---
title: 'Öğretici: Bamboo için SAML SSO ile tümleştirme GmbH Azure Active Directory | Microsoft Docs'
description: Çözüm GmbH tarafından Bamboo için SAML SSO ve Azure Active Directory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: ed587e8ee54aeb36b6790314e849f38b1bab1007
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88549409"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: çözüm GmbH tarafından Bamboo için SAML SSO ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Bamboo için SAML SSO 'yu çözüm GmbH ile tümleştirmeyi öğreneceksiniz.
Azure AD ile Bamboo için SAML SSO 'yu çözüm GmbH ile tümleştirme, aşağıdaki avantajları sağlar:

* Bamboo için SAML SSO 'ya erişimi olan Azure AD 'de çözüm GmbH ' i denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla çözüm GmbH (çoklu oturum açma) tarafından Bamboo için SAML SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bamboo tarafından çözümlenmek üzere SAML SSO ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Çözüm GmbH çoklu oturum açma özellikli aboneliğe göre Bamboo için SAML SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Bamboo tarafından çözümlenmek üzere SAML SSO GmbH **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Bamboo by çözümüne göre SAML SSO GmbH **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Galeriden Bamboo için SAML SSO 'SU çözümleme GmbH ekleniyor

Bamboo için SAML SSO 'yu çözüm GmbH tarafından Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize çözüm GmbH tarafından Bamboo için SAML SSO 'SU eklemeniz gerekir.

**Bamboo için SAML SSO 'yu, Galeriden çözüm GmbH ile eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Bamboo Için SAML SSO 'yu çözüm GmbH**yazın, sonuç panelinden **Bamboo için SAML SSO 'su** ' nu seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

    ![Sonuç listesinde çözüm GmbH tarafından Bamboo için SAML SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına bağlı olarak çözüm GmbH tarafından Bamboo IÇIN SAML SSO Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve Bamboo için SAML SSO 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Bamboo tarafından çözümlenmek üzere SAML SSO ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Bamboo IÇIN SAML SSO 'yu çözüm GmbH çoklu oturum açma Için yapılandırın](#configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Bamboo by Resolution GmbH test KULLANıCıSıNA SAML SSO 'Su oluşturun](#create-saml-sso-for-bamboo-by-resolution-gmbh-test-user)** ve bu, kullanıcının Azure AD gösterimine bağlı olan Bamboo tarafından çözümlenme GmbH için SAML SSO 'su Ile Ilgili Britta Simon 'un bir karşılığı olur.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Bamboo tarafından çözümlenmek üzere SAML SSO ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Bamboo için SAML SSO çözüm GmbH** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Bamboo için SAML SSO ve çözümleme GmbH etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Bamboo için SAML SSO ve çözümleme GmbH etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Bamboo by Resolution GmbH Client support ekibine yönelik SAML SSO ile](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) iletişim kurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Bamboo by çözümü IÇIN SAML SSO 'Yu ayarla GmbH** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on"></a>Bamboo için SAML SSO 'yu çözüm GmbH çoklu oturum açma ile yapılandırma

1. Bamboo için SAML SSO 'da yönetici olarak çözüm GmbH Şirket sitesini oturum açın.

1. Ana araç çubuğunun sağ tarafında, **Ayarlar**Eklentiler ' e tıklayın  >  **Add-ons**.

    ![Ayarlar](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. GÜVENLIK bölümüne gidin, menü çubuğunda **SAML SingleSignOn** öğesine tıklayın.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. **SAML SIngleSignOn eklenti yapılandırması sayfasında** **IDP Ekle**' ye tıklayın.

    ![IDP ekleme](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. **SAML kimlik sağlayıcınızı seçin** sayfasında aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. **IDP türünü** **Azure AD**olarak seçin.

    b. **Ad** metin kutusuna adı yazın.

    c. **Açıklama** metin kutusuna açıklamayı yazın.

    d. **İleri**’ye tıklayın.

1. **Kimlik sağlayıcısı yapılandırma** sayfasında **İleri**' ye tıklayın.

    ![Kimlik yapılandırması](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1. Azure portal 'den indirdiğiniz **meta VERI XML** dosyasını karşıya yüklemek Için **SAML IDP meta verilerini Içeri aktar** sayfasında **Dosya Yükle** ' ye tıklayın.

    ![Idpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. **İleri**’ye tıklayın.

1. **Ayarları kaydet**’e tıklayın.

    ![Kaydet](./media/bamboo-tutorial/tutorial_bamboo_save.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, çözüm GmbH tarafından Bamboo için SAML SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra **Bamboo tarafından çözüm GmbH için SAML SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **çözüm GmbH tarafından Bamboo Için SAML SSO**' yı seçin.

    ![Uygulamalar listesindeki Bamboo by Resolution GmbH LINK için SAML SSO](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Çözüm GmbH test kullanıcısı tarafından Bamboo için SAML SSO oluşturma

Bu bölümün amacı, Bamboo by Resolution GmbH tarafından SAML SSO 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. Bamboo tarafından çözümlenmek üzere SAML SSO GmbH, tam zamanında sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir, gereksiniminize göre, [Bamboo Için SAML SSO 'Su GmbH istemci destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Bamboo tarafından çözüm için SAML SSO GmbH kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız çözüm GmbH tarafından Bamboo için SAML SSO 'ya otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
