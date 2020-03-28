---
title: 'Öğretici: Sprinklr ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Sprinklr arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 9e4025d040783bff1cd85fb46d571e3a89967892
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67089653"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Öğretici: Sprinklr ile Azure Active Directory entegrasyonu

Bu eğitimde, Sprinklr'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Sprinklr'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Sprinklr erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Sprinklr 'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sprinklr ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Sprinklr tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Sprinklr **SP** başlatılan SSO destekler

## <a name="adding-sprinklr-from-the-gallery"></a>Galeriden Sprinklr ekleme

Sprinklr'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Sprinklr'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Sprinklr eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Sprinklr**yazın, sonuç panelinden **Sprinklr'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Sprinklr](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Sprinklr ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Sprinklr'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Sprinklr ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Sprinklr Tek Oturum](#configure-sprinklr-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Sprinklr'da Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için **[Sprinklr test kullanıcısı oluşturun.](#create-sprinklr-test-user)**
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumlarını Sprinklr ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Sprinklr** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Sprinklr Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.sprinklr.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.sprinklr.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Sprinklr İstemci destek ekibine](https://www.sprinklr.com/contact-us/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Sprinklr'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-sprinklr-single-sign-on"></a>Sprinklr Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Sprinklr şirket sitenizde oturum açın.

1. ** \> Yönetim Ayarları'na**gidin.

    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

1. Sol bölmeden **İş Ortağı \> Tek İşaretini Yönet'e** gidin.

    ![İş Ortağını Yönet](./media/sprinklr-tutorial/ic782908.png "İş Ortağını Yönet")

1. **+Tek İşaret Lisa Ekle'yi**tıklatın.

    ![Tek İşaretler](./media/sprinklr-tutorial/ic782909.png "Tek İşaretler")

1. Sayfadaki **Tek İşaret'te** aşağıdaki adımları gerçekleştirin:

    ![Tek İşaretler](./media/sprinklr-tutorial/ic782910.png "Tek İşaretler")

    a. **Ad** metin kutusunda, yapılandırmanız için bir ad yazın (örneğin: *WAADSSOTest*).

    b. **Etkin**'i seçin.

    c. **Yeni SSO Sertifikası kullan'ı**seçin.

    d. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Kimlik Sağlayıcı Sertifikası** metin kutusuna yapıştırın.

    e. Azure Portalı'ndan kopyaladığınız **Azure AD Tanımlayıcı** değerini Entity **Id** textbox'a yapıştırın.

    f. Azure **Portalı'ndan** kopyaladığınız Giriş URL değerini **Kimlik Sağlayıcı Giriş URL** textbox'ına yapıştırın.

    g. Azure Portalı'ndan kopyaladığınız **Giriş URL** değerini **Kimlik Sağlayıcısı Giriş URL** textbox'ına yapıştırın.

    h. **SAML Kullanıcı Kimliği Türü**olarak, Assertion'ı seçin **kullanıcının sprinklr.com kullanıcı adını içerir.**

    i. **SAML Kullanıcı Kimliği Konumu**olarak, kullanıcı kimliğini seçin **Konu deyiminin Ad Tanımlayıcı öğesindedir.**

    j. **Kaydet**'e tıklayın.

    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Sprinklr'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Sprinklr'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Sprinklr'ı**seçin.

    ![Uygulamalar listesindeki Sprinklr bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sprinklr-test-user"></a>Sprinklr test kullanıcısı oluşturma

1. Yönetici olarak Sprinklr şirket sitenize giriş yapın.

1. ** \> Yönetim Ayarları'na**gidin.

    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

1. Sol bölmeden **İstemci \> KullanıcıLarını Yönet'e** gidin.

    ![Ayarlar](./media/sprinklr-tutorial/ic782914.png "Ayarlar")

1. **Kullanıcı Ekle**'ye tıklayın.

    ![Ayarlar](./media/sprinklr-tutorial/ic782915.png "Ayarlar")

1. **Kullanıcı** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Kullanıcıyı edin](./media/sprinklr-tutorial/ic782916.png "Kullanıcıyı edin")

    a. **E-posta**, **Ad** ve **Soyadı** metin kutularında, sağlamak istediğiniz bir Azure REKLAM kullanıcı hesabının bilgilerini yazın.

    b. **Parola Devre Dışı'yı**seçin.

    c. **Dil'i**seçin.

    d. **Kullanıcı Türünü**Seçin.

    e. **Güncelleştir**’e tıklayın.

    > [!IMPORTANT]
    > **Bir** kullanıcının bir Kimlik sağlayıcısı üzerinden oturum açmasını sağlamak için Parola Devre Dışı seçilmelidir. 

1. **Role**gidin ve ardından aşağıdaki adımları gerçekleştirin:

    ![Ortak Rolleri](./media/sprinklr-tutorial/ic782917.png "Ortak Rolleri")

    a. **Genel** listeden **ALL_Permissions'** yi seçin.  

    b. **Güncelleştir**’e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Sprinklr tarafından sağlanan diğer Sprinklr kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Sprinklr döşemesini tıklattığınızda, SSO'yu kurduğunuz Sprinklr'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
