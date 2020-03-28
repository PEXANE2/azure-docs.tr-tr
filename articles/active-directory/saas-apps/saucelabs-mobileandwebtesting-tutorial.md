---
title: 'Öğretici: Sauce Labs ile Azure Active Directory entegrasyonu - Mobil ve Web Testi | Microsoft Dokümanlar'
description: Azure Active Directory ve Sos Laboratuvarları - Mobil ve Web Testi arasında tek oturum açma yı nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 8933cb90672e49305cd0fb7dc5e4f8f04f94093e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67091553"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Öğretici: Sauce Labs ile Azure Active Directory entegrasyonu - Mobil ve Web Testi

Bu eğitimde, Sos Laboratuvarları - Mobil ve Web Test'ini Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Sos Laboratuvarlarını Entegre Etme - Azure AD ile Mobil ve Web Testi aşağıdaki avantajları sağlar:

* Sos Laboratuvarları - Mobil ve Web Testi'ne erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Sos Laboratuvarları - Mobil ve Web Testi (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sauce Labs - Mobil ve Web Testi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Sauce Labs - Mobil ve Web Testi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Sos Labs - Mobil ve Web Testi **IDP** SSO başlatılan destekler
* Sauce Labs - Mobil ve Web Testi **Just In Time** kullanıcı sağlama destekler

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Sos Labs ekleme - Mobil ve Web Testi galeriden

Sos Laboratuvarları - Mobil ve Web Testi'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Sos Laboratuvarları - Mobil ve Web Testi eklemeniz gerekir.

**Galeriden Sos Laboratuvarları - Mobil ve Web Testi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Sauce Labs yazın - Mobil ve Web Testi**, sonuç panelinden **Sos Labs seçin - Mobil ve Web Testi** sonra uygulama eklemek için **ekle** düğmesini tıklatın.

    ![Sauce Labs - Mobil ve Web Test sonuçları listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini, **Britta Simon**adlı bir test kullanıcısına göre Sos Labs - Mobil ve Web Testi ile yapılandırıp test emzebilirsiniz.
Tek oturum açma nın işe yaraması için, Bir Azure AD kullanıcısı ile Sos Labs - Mobil ve Web Testi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumlarını Sauce Labs - Mobile ve Web Test ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Sos Laboratuvarlarını - Mobil ve Web Testi Tek İşaret-On](#configure-sauce-labs---mobile-and-web-testing-single-sign-on)** - yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Sos Laboratuvarları Oluşturun - Mobil ve Web Test test kullanıcısı](#create-sauce-labs---mobile-and-web-testing-test-user)** - Sos Labs Britta Simon bir meslektaşı olması - Mobil ve Web Testi kullanıcının Azure AD gösterimi ile bağlantılıdır.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Sos Labs - Mobil ve Web Testi ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Sauce Labs - Mobil ve Web Testi** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![Sos Labs - Mobil ve Web Test Etki Alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Sos Laboratuvarları Kur - Mobil ve Web Testi** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-sauce-labs---mobile-and-web-testing-single-sign-on"></a>Yapılandırılan Sos Labs - Mobil ve Web Test Tek Sign-On

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Sauce Labs - Mobile ve Web Testing şirket sitenizde oturum açın.

2. **Kullanıcı simgesine** tıklayın ve **Takım Yönetimi** sekmesini seçin.

    ![Tek İşaret-On'u Yapılandır](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

3. Metin kutusuna **Etki Alanı adınızı** girin.

    ![Tek İşaret-On'u Yapılandır](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

4. **Yapılsekmesini tıklatın.**

    ![Tek İşaret-On'u Yapılandır](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

5. Tek **İşaretI Yapve bölümünde** aşağıdaki adımları gerçekleştirin.

    ![Tek İşaret-On'u Yapılandır](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Azure AD'den **Gözat'ı** tıklatın ve indirilen meta veri dosyasını yükleyin.

    b. **İZİn VER'İ SEÇİn** VERİn VERİn SAĞLAMA onay kutusunu seçin.

    c. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Sauce Labs - Mobile ve Web Testing'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından Sauce Labs - Mobile ve **Web Testing'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Sauce Labs - Mobile ve Web Testing'i**seçin.

    ![Sos Labs - Uygulamalar listesinde Mobil ve Web Test bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sauce-labs---mobile-and-web-testing-test-user"></a>Sos Labs oluşturun - Mobil ve Web Test test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı Sauce Labs oluşturulur - Mobil ve Web Testi. Sauce Labs - Mobil ve Web Testi, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Sauce Labs - Mobile ve Web Test'te zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Sauce Labs - Mobil ve Web Testi destek ekibiyle](mailto:support@saucelabs.com)iletişime geçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Sos Laboratuvarları - Mobil ve Web Test döşemesini tıklattığınızda, SSO'yu kurduğunuz Sos Laboratuvarları - Mobil ve Web Testi'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

