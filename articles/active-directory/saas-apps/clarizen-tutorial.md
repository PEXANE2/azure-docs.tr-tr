---
title: 'Öğretici: Clarizen ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Clarizen arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b34e6087f3a6e7f3cc0f46c53d4fe903838d210
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158605"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Öğretici: Clarizen ile Azure Active Directory entegrasyonu

Bu eğitimde, Clarizen'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Clarizen'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Clarizen erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Clarizen'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Clarizen ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Clarizen tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **Clarizen'den IDP'ye** destek

## <a name="adding-clarizen-from-the-gallery"></a>Galeriden Clarizen ekleme

Clarizen'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden clarizen'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Clarizen eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Clarizen**yazın, sonuç panelinden **Clarizen'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Clarizen sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Clarizen'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Clarizen ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Clarizen Tek Oturum Açma](#configure-clarizen-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Clarizen test kullanıcısını oluşturun](#create-clarizen-test-user)** - Kullanıcının Azure AD gösterimine bağlı Clarizen'deki Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Clarizen ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Clarizen** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![Clarizen Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir değer yazın:`Clarizen`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx`

    > [!NOTE]
    > Bunlar gerçek değerler değil. Gerçek tanımlayıcıyı kullanmanız ve URL'yi yanıtlamanız gerekir. Burada tanımlayıcı olarak bir dize benzersiz değerini kullanmanızı öneririz. Gerçek değerleri almak için [Clarizen destek ekibine](https://success.clarizen.com/hc/en-us/requests/new)başvurun.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Clarizen'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-clarizen-single-sign-on"></a>Clarizen Tek İşaretli Yapıla

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak Clarizen şirket sitenizde oturum açın.

1. Kullanıcı adınızı tıklatın ve ardından **Ayarlar'ı**tıklatın.

    ![Kullanıcı adınız altında "Ayarlar"ı tıklatma](./media/clarizen-tutorial/tutorial_clarizen_001.png "Ayarlar")

1. Genel **Ayarlar** sekmesini tıklatın. Daha sonra **Federe Kimlik Doğrulama'nın** **yanında, edit'i**tıklatın.

    !["Genel Ayarlar" sekmesi](./media/clarizen-tutorial/tutorial_clarizen_002.png "Genel Ayarlar")

1. **Federe Kimlik Doğrulama** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    !["Federe Kimlik Doğrulama" iletişim kutusu](./media/clarizen-tutorial/tutorial_clarizen_003.png "Federe Kimlik Doğrulama")

    a. **Federe Kimlik Doğrulamayı Etkinleştir'i**seçin.

    b. İndirilen sertifikanızı yüklemek için **Yükle'yi** tıklatın.

    c. Oturum **Açma URL** kutusuna Azure AD uygulama yapılandırma penceresinden **Giriş URL** değerini girin.

    d. Oturumu **Çıkış URL** kutusuna, Azure AD uygulama yapılandırma penceresinden **Oturum Açma URL** değerini girin.

    e. **POST'u kullan'ı**seçin.

    f. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Clarizen'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Clarizen'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Clarizen'i**seçin.

    ![Uygulamalar listesindeclarizen bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-clarizen-test-user"></a>Clarizen test kullanıcısı oluşturma

Bu bölümün amacı Clarizen Britta Simon adlı bir kullanıcı oluşturmaktır.

**El ile kullanım oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları gerçekleştirin:**

Azure AD kullanıcılarının Clarizen'de oturum açmasını sağlamak için kullanıcı hesapları sağlamanız gerekir. Clarizen durumunda, sağlama manuel bir görevdir.

1. Clarizen şirket sitenizde yönetici olarak oturum açın.

2. **Kişiler'i**tıklatın.

    !["Kişiler"e tıklayarak](./media/clarizen-tutorial/create_aaduser_001.png "People")

3. **Kullanıcıdavet**et'i tıklatın.

    !["Kullanıcıyı Davet Et" düğmesi](./media/clarizen-tutorial/create_aaduser_002.png "Kullanıcıları Davet Et")

1. Kişileri **Davet** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    !["Kişileri Davet Et" iletişim kutusu](./media/clarizen-tutorial/create_aaduser_003.png "Kişileri Davet Et")

    a. **E-posta** kutusuna Britta Simon hesabının e-posta adresini yazın.

    b. **Davet Et'i**tıklatın.

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.


### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Clarizen döşemesini tıklattığınızda, SSO'yu kurduğunuz Clarizen'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
