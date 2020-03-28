---
title: 'Öğretici: Yeni Emanet ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Yeni Emanet arasında tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: dfa5fbcf5df8ee314aab3e4a8228d81e7e14565b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233527"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Öğretici: Yeni Emanet ile Azure Active Directory entegrasyonu

Bu eğitimde, Yeni Emanet'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Yeni Emanet'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Yeni Emanet'e erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik olarak Yeni Emanet 'te (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Yeni Emanet ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Yeni Emanet tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Yeni Emanet **SP** başlatılan SSO destekler

## <a name="adding-new-relic-from-the-gallery"></a>Galeriden Yeni Emanet Ekleme

Yeni Emanet'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Yeni Emanet eklemeniz gerekir.

**Galeriden Yeni Emanet eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda Yeni **Emanet**yazın, sonuç panelinden **Yeni Emanet'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde yeni Emanet](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yeni Emanet ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Yeni Emanet'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi Yeni Emanet ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Yeni Kalıntı Tek Oturum](#configure-new-relic-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Yeni Emanet testi kullanıcısı oluşturun](#create-new-relic-test-user)** - Yeni Emanet'teki Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumlarını Yeni Emanet ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Yeni Emanet** uygulama tümleştirme sayfasında, **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Emanet Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL **metin** kutusunda Oturum Aç'a aşağıdaki deseni `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` kullanarak bir URL yazın: - Kendi Yeni Emanet Hesap Kimliğinizi değiştirip yerine mutlaka.

    b. Tanımlayıcı **(Entity ID)** metin kutusuna bir URL yazın:`rpm.newrelic.com`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Yeni **Emanet'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-new-relic-single-sign-on"></a>Yeni Emanet Tek İşaret-On Yapıla

1. Farklı bir web tarayıcısı penceresinde, **Yeni Emanet** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde Hesap **Ayarları'nı**tıklatın.
   
    ![Hesap Ayarları](./media/new-relic-tutorial/ic797036.png "Hesap Ayarları")

3. Güvenlik **ve kimlik doğrulama** sekmesini tıklatın ve sonra sekmedeki **Tek işaret'i** tıklatın.
   
    ![Tek Oturum Açma](./media/new-relic-tutorial/ic797037.png "Çoklu Oturum Açma")

4. SAML iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. İndirilen Azure Active Directory sertifikanızı yüklemek için **Dosyayı Seçin'i** tıklatın.

    b. Uzak **oturum açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.
   
    c. **Logout iniş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    d. **Değişikliklerimi Kaydet'i**tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Yeni Emanet'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Yeni Emanet'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde Yeni **Emanet'i**seçin.

    ![Uygulamalar listesindeki Yeni Emanet bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-new-relic-test-user"></a>Yeni Emanet testi kullanıcısı oluşturma

Azure Active Directory kullanıcılarının Yeni Emanet'te oturum açabilmeleri için Yeni Emanet'te yer alması gerekir. Yeni Emanet söz konusu olduğunda, sağlama manuel bir görevdir.

**Yeni Emanet'e bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Yeni Emanet** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde Hesap **Ayarları'nı**tıklatın.
   
    ![Hesap Ayarları](./media/new-relic-tutorial/ic797040.png "Hesap Ayarları")

3. Sol taraftaki **Hesap** bölmesinde **Özet'i**tıklatın ve ardından **kullanıcı ekle'yi**tıklatın.
   
    ![Hesap Ayarları](./media/new-relic-tutorial/ic797041.png "Hesap Ayarları")

4. Etkin **kullanıcılar** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Etkin Kullanıcılar](./media/new-relic-tutorial/ic797042.png "Etkin Kullanıcılar")
   
    a. **E-posta** metin kutusuna, sağlamak istediğiniz geçerli bir Azure Active Directory kullanıcısının e-posta adresini yazın.

    b. **Rol** olarak **Kullanıcı**seçin.

    c. **Bu kullanıcıyı ekle'yi**tıklatın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Yeni Emanet tarafından sağlanan diğer Yeni Emanet kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Yeni Emanet karosu tıklattığınızda, SSO'yu kurduğunuz Yeni Emanet'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

