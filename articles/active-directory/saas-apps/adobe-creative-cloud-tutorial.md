---
title: 'Öğretici: Adobe Creative Cloud ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Adobe Creative Cloud arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f3fdd1aa4fa655cfffb9e9a2f29d551133613a8
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73154002"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Öğretici: Adobe Creative Cloud ile tümleştirme Azure Active Directory

Bu öğreticide, Adobe Creative Cloud Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Adobe Creative Cloud Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Adobe Creative Cloud erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Adobe Creative Cloud (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Adobe Creative Cloud ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Çoklu oturum açma özellikli abonelik Adobe Creative Cloud

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Adobe Creative Cloud **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Galeriden Adobe Creative Cloud ekleme

Adobe Creative Cloud tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Adobe Creative Cloud yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Adobe Creative Cloud eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Adobe Creative Cloud**yazın, sonuç panelinden **Adobe Creative Cloud** ' i seçin, sonra uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

    ![Sonuçlar listesinde Adobe Creative Cloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Adobe Creative Cloud ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Adobe Creative Cloud ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Adobe Creative Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Adobe Creative Cloud çoklu oturum açmayı yapılandırın](#configure-adobe-creative-cloud-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı Adobe Creative Cloud ' de Britta Simon 'un bir karşılığı olacak **[Adobe Creative Cloud test kullanıcısı oluşturun](#create-adobe-creative-cloud-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Adobe Creative Cloud ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Adobe Creative Cloud** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Etki alanı ve URL 'Ler Adobe Creative Cloud çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusuna değeri şöyle yazın: `https://adobe.com`.

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://www.okta.com/saml2/service-provider/<token>`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değil. Bu değeri gerçek tanımlayıcıyla güncelleştirin. Bu değeri almak için [Adobe Creative Cloud istemci destek ekibine](https://www.adobe.com/au/creativecloud/business/teams/plans.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. Adobe Creative Cloud uygulama, SAML onaylamalarını belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri** bölümünden yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, YUKARıDAKI görüntüde gösterildiği gibi SAML belirteci özniteliğini yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak özniteliği|
    |----- | --------- |
    | firstName | Kullanıcı. |
    | Soyadı | User. soyadı |
    | E-posta | Kullanıcı. Mail

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam 'a** tıklayın

    g. **Kaydet** düğmesine tıklayın.

    > [!NOTE]
    > Kullanıcı, SAML yanıtında doldurulacak e-posta talebi değeri için geçerli bir Office 365 ExO lisansına sahip olmalıdır.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **Adobe Creative Cloud ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum açma URL 'SI

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-adobe-creative-cloud-single-sign-on"></a>Adobe Creative Cloud çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) 'nda oturum açın.

2. Üst gezinti çubuğundaki **Ayarlar** ' a gidin ve ardından **kimlik**' i seçin. Etki alanlarının listesi açılır. Etki alanınız için bağlantıyı **Yapılandır** seçeneğine tıklayın. Ardından, **Çoklu oturum açma yapılandırması gerekli** bölümünde aşağıdaki adımları gerçekleştirin. Daha fazla bilgi için bkz. [etki alanı kurma](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Ayarlar](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "Ayarlar")

    a. Azure AD 'den **IDP sertifikasına**indirilen sertifikayı yüklemek Için, **Araştır** ' a tıklayın.

    b. **IDP veren** metin kutusunda, Azure Portal içinde **oturum açma yapılandırma** bölümünden kopyaladığınız **SAML varlık kimliğinin** değerini yerleştirin.

    c. **IDP oturum açma URL** 'si metin kutusunda, Azure Portal içinde **oturum açma yapılandırma** bölümünden kopyaladığınız **SAML SSO hizmeti URL 'si** değerini yerleştirin.

    d. **IDP bağlama**olarak **http-Redirect** ' i seçin.

    e. **Kullanıcı oturum açma ayarı**olarak **e-posta adresini** seçin.

    f. **Kaydet** düğmesine tıklayın.

3. Pano artık XML **"Indirme meta verileri"** dosyasını sunacak. Adobe 'un EntityDescriptor URL 'sini ve AssertionConsumerService URL 'sini içerir. Lütfen dosyayı açın ve Azure AD uygulamasında yapılandırın.

    ![Uygulama tarafında çoklu oturum açmayı yapılandırma](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. **Uygulama ayarlarını yapılandır** Iletişim kutusunda **tanımlayıcı** Için size sunulan EntityDescriptor değerini kullanın.

    b. **Uygulama ayarlarını yapılandır** Iletişim kutusunda **yanıt URL 'si** Için size sunulan assertionconsumerservice değerini kullanın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanında **brittasıon\@yourşirketnotlarıetki alanı. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Adobe Creative Cloud erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve **Adobe Creative Cloud**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Adobe Creative Cloud**' yi seçin.

    ![Uygulamalar listesindeki Adobe Creative Cloud bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-adobe-creative-cloud-test-user"></a>Adobe Creative Cloud test kullanıcısı oluştur

Azure AD kullanıcılarının Adobe Creative Cloud oturum açmasını sağlamak için, Adobe Creative Cloud sağlanması gerekir. Adobe Creative Cloud durumda, sağlama işlemi el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Yönetici olarak [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) sitesinde oturum açın.

2. Kullanıcıları, Adobe konsolunun içinde Federasyon KIMLIĞI olarak ekleyin ve bir ürün profiline atayın. Kullanıcı ekleme hakkında ayrıntılı bilgi için bkz. [Adobe Yönetici Konsolu 'nda Kullanıcı ekleme](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Bu noktada, e-posta adresinizi/UPN 'nizi Adobe SignIn formuna yazın, sekme tuşuna basın ve Azure AD 'ye geri Federasyon oluşturmanız gerekir:
   * Web erişimi: www\.adobe.com > oturum açma
   * Masaüstü uygulaması yardımcı programı içinde > oturum açma
   * Uygulama içinde > Yardım > oturum açma

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Adobe Creative Cloud kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Adobe Creative Cloud otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
  
- [Etki alanı ayarlama (adobe.com)](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
  
- [Azure 'ı Adobe SSO ile kullanım için yapılandırma (adobe.com)](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)
