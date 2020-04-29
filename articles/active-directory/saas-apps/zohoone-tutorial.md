---
title: 'Öğretici: Zoho One ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Zoho One arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 0a37789e7c7efeb71770ff0e8061d57e6603b6c4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67086245"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Öğretici: Zoho One ile tümleştirme Azure Active Directory

Bu öğreticide, Zoho 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Zoho 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, Zoho One erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla bir (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zoho One ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Zoho bir çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Zoho One, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-zoho-one-from-the-gallery"></a>Galeriden Zoho ekleme

Zoho 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden bir tane Zoho 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden bir tane Zoho eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zoho One**yazın, sonuç panelinden **Zoho One** ' ı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuç listesinde bir adet Zoho](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Zoho ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Zoho ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açmayı, Zoho One ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Zoho 'Yi bir çoklu oturum açma yapılandırması](#configure-zoho-one-single-sign-on)** , uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Zoho **[One test kullanıcısı oluşturun](#create-zoho-one-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Zoho 'Da Britta Simon 'un bir karşılığı olacak şekilde.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, Zoho One ile birlikte yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Zoho One** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Bir etki alanı ve URL 'Ler çoklu oturum açma bilgilerini Zoho](common/idp-relay.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`one.zoho.com`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://accounts.zoho.com/samlresponse/<saml-identifier>`

    > [!NOTE]
    > Önceki **yanıt URL 'si** değeri gerçek değil. #Step4, daha sonra `<saml-identifier>` Öğreticinin ilerleyen kısımlarında açıklanan, **Zoho yapılandırma bir çoklu oturum açma** bölümünün değerden yararlanacaktır.

    c. **Ek URL 'Ler ayarla**' ya tıklayın.

    d. **Geçiş durumu** metin kutusuna bir URL yazın:`https://one.zoho.com`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:


    ![Bir etki alanı ve URL 'Ler çoklu oturum açma bilgilerini Zoho](common/both-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 

    > [!NOTE] 
    > Önceki **oturum açma URL 'si** değeri gerçek değil. Bu değeri, Öğreticinin ilerleyen kısımlarında açıklanan **bir çoklu oturum açma yapılandırma** bölümünde yer alan gerçek oturum açma URL 'si ile güncelleşceksiniz. 

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Zoho 'Yi ayarlama** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-zoho-one-single-sign-on"></a>Zoho 'Yi tek bir oturum açma yapılandırması

1. Farklı bir Web tarayıcısı penceresinde, Zoho One şirket sitenizde yönetici olarak oturum açın.

2. **Kuruluş** sekmesinde, **SAML kimlik doğrulaması**altında **Kurulum** ' a tıklayın.

    ![Zoho One org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

3. Açılır sayfada aşağıdaki adımları uygulayın:

    ![Zoho bir SIG](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. Oturum açma **URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    b. Oturum kapatma **URL 'si** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'si**değerini yapıştırın.

    c. Azure portal indirdiğiniz **sertifikayı (base64)** karşıya yüklemek Için, **Gözden** geçirme ' ye tıklayın.

    d. **Kaydet**’e tıklayın.

4. SAML kimlik doğrulaması kurulumunu kaydettikten sonra, **SAML tanımlayıcı** değerini kopyalayın ve ' `<saml-identifier>`nin yerine `https://accounts.zoho.com/samlresponse/one.zoho.com` **Yanıt** URL 'sini ekleyin ve oluşturulan değeri **temel SAML yapılandırması** bölümünün altındaki yanıt **URL 'si** metin kutusuna yapıştırın.

    ![Zoho bir SAML](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

5. Etki **alanları** sekmesine gidin ve ardından **etki alanı Ekle**' ye tıklayın.

    ![Bir etki alanı Zoho](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

6. **Etki alanı Ekle** sayfasında, aşağıdaki adımları uygulayın:

    ![Zoho One etki alanı Ekle](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. **Etki alanı adı** metin kutusuna contoso.com gibi bir etki alanı yazın.

    b. **Ekle**'ye tıklayın.

    >[!Note]
    >Etki alanını ekledikten sonra, etki alanınızı doğrulamak için [aşağıdaki](https://www.zoho.com/one/help/admin-guide/domain-verification.html) adımları izleyin. Etki alanı doğrulandıktan sonra, Azure portal ' deki **temel SAML yapılandırması** bölümünde **oturum açma URL 'si** içinde etki alanı adınızı kullanın.

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

Bu bölümde, Zotta tek oturum açma özelliğini kullanarak Zoho SIG 'ye erişim izni vermiş olursunuz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Zoho One**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Zoho One**' ı seçin.

    ![Uygulamalar listesinde Zoho One bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-zoho-one-test-user"></a>Zoho One test kullanıcısı oluşturma

Azure AD kullanıcılarının, Zoho 'da oturum açmasını sağlamak için, bunların Zoho One 'a sağlanması gerekir. Zoho One 'da, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bir güvenlik yöneticisi olarak Zoho 'da oturum açın.

2. **Kullanıcılar** sekmesinde **Kullanıcı logosu**' na tıklayın.

    ![Bir Kullanıcı Zoho](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. **Kullanıcı Ekle** sayfasında, aşağıdaki adımları uygulayın:

    ![Zoho One Kullanıcı Ekle](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. **Ad** metin kutusuna, **Britta Simon**gibi kullanıcının adını girin.
    
    b. **E-posta adresi** metin kutusuna kullanıcının e-postasını girin brittasimon@contoso.com.

    >[!Note]
    >Etki alanı listesinden doğrulanmış etki alanınızı seçin.

    c. **Ekle**'ye tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Zoho One kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Zoho 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

