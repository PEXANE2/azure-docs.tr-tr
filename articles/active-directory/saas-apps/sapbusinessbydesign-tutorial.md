---
title: 'Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SAP Business ByDesign arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0b830f0760b768826b2d937b4a8b2ffbd8e9e2a2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67091684"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme

Bu öğreticide SAP Business ByDesign 'ın Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz.
SAP Business ByDesign 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* SAP Business ByDesign 'a erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Business ByDesign (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

SAP Business ByDesign ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SAP Business ByDesign çoklu oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* SAP Business ByDesign **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Galeriden SAP Business ByDesign ekleme

SAP Business ByDesign 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden SAP Business ByDesign 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SAP Business ByDesign eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP Business Bydesign**yazın, sonuç panelinden **SAP Business Bydesign** ' ı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![SAP Business ByDesign for Results List](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açma 'yı, **Britta Simon**adlı bir test KULLANıCıSıNA göre SAP Business Bydesign ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve SAP Business ByDesign içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

SAP Business ByDesign ile Azure AD çoklu oturum açma 'yı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[SAP Business ByDesign Single oturum açma ayarlarını yapılandırma](#configure-sap-business-bydesign-single-sign-on)** uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. SAP Business Bydesign **[test kullanıcısı oluşturma](#create-sap-business-bydesign-test-user)** -kullanıcının Azure AD gösterimine bağlı olan SAP Business Bydesign 'Ta Britta Simon 'a sahip olmak için.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

SAP Business ByDesign ile Azure AD çoklu oturum açma 'yı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **SAP Business Bydesign** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<servername>.sapbydesign.com`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<servername>.sapbydesign.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [SAP Business ByDesign Client destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. SAP Business ByDesign uygulaması, SAML onaylamalarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri** bölümünden yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Ad tanımlayıcı değerini**düzenlemek için **Düzenle** simgesine tıklayın.

    ![image](media/sapbusinessbydesign-tutorial/mail-prefix1.png)

7. **Kullanıcı taleplerini Yönet** bölümünde aşağıdaki adımları uygulayın: ![görüntü](media/sapbusinessbydesign-tutorial/mail-prefix2.png)

    a. **Kaynak**olarak **dönüşüm** ' i seçin.

    b. **Dönüştürme** açılan listesinde **Extractmailprefix ()** öğesini seçin.

    c. **Parametre 1** açılan listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin. Örneğin, EmployeeID 'yi benzersiz kullanıcı tanımlayıcısı olarak kullanmak istiyorsanız ve öznitelik değerini ExtensionAttribute2 içinde depoladıysanız User. ExtensionAttribute2 ' yi seçin.

    d. **Kaydet**’e tıklayın.

8. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. **SAP Business ByDesign 'ı ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-sap-business-bydesign-single-sign-on"></a>SAP Business ByDesign Single oturum açmayı yapılandırma

1. SAP Business ByDesign Portal 'da yönetici haklarıyla oturum açın.

2. **Uygulama ve Kullanıcı yönetimi ortak görevi** ' ne gidin ve **kimlik sağlayıcısı** sekmesine tıklayın.

3. **Yeni kimlik sağlayıcısı** ' na tıklayın ve Azure Portal indirdiğiniz meta veri xml dosyasını seçin. Meta verileri içeri aktararak, sistem gerekli imza sertifikasını ve şifreleme sertifikasını otomatik olarak karşıya yükler.

    ![Çoklu oturum açmayı yapılandırma](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

4. **Onaylama tüketici hizmeti URL 'SINI** SAML isteğine dahil etmek Için, **onaylama tüketici hizmeti URL 'sini dahil et**' i seçin.

5. **Çoklu oturum açmayı etkinleştir**' e tıklayın.

6. Yaptığınız değişiklikleri kaydedin.

7. **Sistemim** sekmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

8. **Azure AD oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    ![Çoklu oturum açmayı yapılandırma](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

9. Çalışanın, **El Ile kimlik sağlayıcı seçimi**seçerek Kullanıcı kimliği ve parola veya SSO ile oturum açma arasında el ile seçim yapıp kullanamayacağını belirtin.

10. **SSO URL 'si** bölümünde, çalışan tarafından sistemde oturum açmaya yönelik olarak kullanılması gereken URL 'yi belirtin.
    Çalışanlara gönderilen URL açılır listesinde, aşağıdaki seçenekler arasından seçim yapabilirsiniz:

    **SSO olmayan URL**

    Sistem, çalışana yalnızca normal sistem URL 'sini gönderir. Çalışan, SSO kullanarak yeniden oturum alamaz ve bunun yerine parola veya sertifika kullanmalıdır.

    **SSO URL 'SI**

    Sistem yalnızca SSO URL 'sini çalışana gönderir. Çalışan SSO 'yu kullanarak oturum açabilir. Kimlik doğrulama isteği IDP aracılığıyla yeniden yönlendirildi.

    **Otomatik seçim**

    SSO etkin değilse, Sistem normal sistem URL 'sini çalışana gönderir. SSO etkinse, sistem, çalışanın bir parolaya sahip olup olmadığını denetler. Bir parola varsa, çalışana hem SSO URL 'si hem de SSO olmayan URL gönderilir. Ancak, çalışanın parolası yoksa, çalışana yalnızca SSO URL 'SI gönderilir.

11. Yaptığınız değişiklikleri kaydedin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SAP Business ByDesign 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **SAP Business Bydesign**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Business ByDesign**' ı seçin.

    ![Uygulamalar listesindeki SAP Business ByDesign bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-sap-business-bydesign-test-user"></a>SAP Business ByDesign test kullanıcısı oluşturma

Bu bölümde, SAP Business ByDesign 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. SAP Business ByDesign platformunda kullanıcıları eklemek için lütfen [SAP Business Bydesign Client destek ekibi](https://www.sap.com/products/cloud-analytics.support.html) ile çalışın. 

> [!NOTE]
> Lütfen NameID değerinin SAP Business ByDesign platformunda username alanıyla eşleştiğinden emin olun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde SAP Business ByDesign kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SAP Business ByDesign ' ta otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
