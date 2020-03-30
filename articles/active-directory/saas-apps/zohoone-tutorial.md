---
title: 'Öğretici: Zoho One ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zoho One arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086245"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Öğretici: Zoho One ile Azure Active Directory entegrasyonu

Bu eğitimde, Zoho One'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zoho One'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zoho One erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zoho One 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zoho One ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Zoho One one tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zoho One **SP** ve **IDP** SSO başlatılan destekler

## <a name="adding-zoho-one-from-the-gallery"></a>Galeriden Zoho Bir ekleme

Zoho One'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Zoho One'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zoho One eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Zoho One**yazın, sonuç panelinden **Zoho One'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Zoho Bir sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Zoho One ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Zoho One'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Zoho One ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zoho One One One Sign-On](#configure-zoho-one-single-sign-on)** 'u yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zoho One test kullanıcısını oluşturun](#create-zoho-one-test-user)** - Kullanıcının Azure AD gösterimine bağlı Zoho One'daki Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Zoho One ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Zoho One** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Zoho One Domain ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`one.zoho.com`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://accounts.zoho.com/samlresponse/<saml-identifier>`

    > [!NOTE]
    > Önceki **Yanıt URL** değeri gerçek değil. Sen Yapılandırma `<saml-identifier>` **Zoho Bir Tek Sign-On** bölümünde #step4 değeri alırsınız , hangi öğretici daha sonra açıklanmıştır.

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna bir URL yazın:`https://one.zoho.com`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:


    ![Zoho One Domain ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 

    > [!NOTE] 
    > Önceki **Oturum Açma URL** değeri gerçek değildir. Değeri, daha sonra öğreticide açıklanan **Yapılandırılan Zoho One Single Sign-On** bölümünden gerçek Oturum Açma URL'si ile güncelleyeceksiniz. 

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Zoho One'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zoho-one-single-sign-on"></a>Zoho Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Zoho One şirket sitenizde oturum açın.

2. **Kuruluş** sekmesinde, **SAML Kimlik Doğrulaması**altında **Kurulum'u** tıklatın.

    ![Zoho Bir org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

3. Açılan sayfada aşağıdaki adımları gerçekleştirin:

    ![Zoho Bir sig](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    b. Oturumu **Çıkış URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. Azure portalından indirdiğiniz **Sertifikayı (Base64)** yüklemek için **Gözat'ı** tıklatın.

    d. **Kaydet**'e tıklayın.

4. SAML Kimlik Doğrulama kurulumunu kurtardıktan sonra, **SAML-Tanımlayıcı** değerini kopyalayın ve **Temel SAML Yapılandırması** bölümü altındaki **YanıtURL** metin kutusunda oluşturulan değeri beğenin `<saml-identifier>` `https://accounts.zoho.com/samlresponse/one.zoho.com` ve yapıştırın yerine Yanıt **URL'si** ile ekleyin.

    ![Zoho Bir saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

5. **Etki Alanları** sekmesine gidin ve ardından Etki **Alanı Ekle'yi**tıklatın.

    ![Zoho Bir etki alanı](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

6. Etki **Alanı Ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Zoho Bir etki alanı eklemek](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. Etki **Alanı Adı** metin kutusunda, etki alanını contoso.com gibi yazın.

    b. **Ekle**’ye tıklayın.

    >[!Note]
    >Etki alanını ekledikten sonra etki alanınızı doğrulamak için [aşağıdaki](https://www.zoho.com/one/help/admin-guide/domain-verification.html) adımları izleyin. Etki alanı doğrulandıktan sonra, Azure portalındaki Temel **SAML Yapılandırma** bölümünde **Oturum Aç URL'sinde** alan adınızı kullanın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Zoho One'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Zoho One'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zoho One'ı**seçin.

    ![Uygulamalar listesindeZoho Bir bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-zoho-one-test-user"></a>Zoho One test kullanıcısı oluşturma

Azure AD kullanıcılarının Zoho One'da oturum açabilmeleri için Zoho One'da oturum açmaları gerekir. Zoho Bir'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak Zoho One'da oturum açın.

2. **Kullanıcılar** sekmesinde, **kullanıcı logosunu**tıklatın.

    ![Zoho Bir kullanıcı](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. Kullanıcı **Ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Zoho Bir kullanıcı eklemek](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. **Ad** metin **kutusuna, Britta simon**gibi kullanıcının adını girin.
    
    b. **E-posta Adresi** metin kutusuna, brittasimon@contoso.comkullanıcının e-posta adresini girin.

    >[!Note]
    >Etki alanı listesinden doğrulanmış etki alanınızı seçin.

    c. **Ekle**’ye tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Zoho One döşemesini tıklattığınızda, SSO'yu kurduğunuz Zoho One'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

