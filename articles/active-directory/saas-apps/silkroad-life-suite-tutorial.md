---
title: 'Öğretici: SilkRoad Life Suite ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SilkRoad Life Suite arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 63165da69815c77afb8692e1e68c1710beb8df8c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67090833"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Öğretici: SilkRoad Life Suite ile Azure Active Directory entegrasyonu

Bu eğitimde, SilkRoad Life Suite'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SilkRoad Life Suite'i Azure AD ile entegre etmek size aşağıdaki avantajları sağlar:

* SilkRoad Life Suite erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SilkRoad Life Suite'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SilkRoad Life Suite ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SilkRoad Life Suite tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SilkRoad Life **Suite, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Galeriden SilkRoad Life Suite ekleme

SilkRoad Life Suite'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize SilkRoad Life Suite eklemeniz gerekir.

**Galeriden SilkRoad Life Suite eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **SilkRoad Life Suite**yazın, sonuç panelinden **SilkRoad Life Suite'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![SilkRoad Life Suite sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, SilkRoad Life Suite ile Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile SilkRoad Life Suite'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını SilkRoad Life Suite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[SilkRoad Life Suite Tek Oturum Açma'yı yapılandırın.](#configure-silkroad-life-suite-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SilkRoad Life Suite test kullanıcısını oluşturun](#create-silkroad-life-suite-test-user)** - SilkRoad Life Suite'te Britta Simon'ın bir muadili için kullanıcının Azure AD temsiline bağlı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı SilkRoad Life Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **SilkRoad Life Suite** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Hizmet Sağlayıcı **meta veri dosyasını** daha sonra bu öğreticide açıkladı alırsınız.

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![image](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel SAML Yapılandırması bölümünde otomatik olarak doldurulur:

    ![image](common/sp-identifier-reply.png)

    > [!Note]
    > **Tanımlayıcı** ve **Yanıt URL** değerleri otomatik olarak yorumlanmıyorsa, gereksiniminize göre değerleri el ile doldurun.

    d. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.silkroad-eng.com/Authentication/`

5. Temel **SAML Yapılandırması** bölümünde, Servis **Sağlayıcı meta veri dosyanız**yoksa aşağıdaki adımları gerçekleştirin:

    ![SilkRoad Life Suite Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.silkroad-eng.com/Authentication/`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP`|
    | `https://<subdomain>.silkroad.com/Authentication/SP`|

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/`|
    | `https://<subdomain>.silkroad.com/Authentication/`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [SilkRoad Life Suite Client destek ekibiyle](https://www.silkroad.com/locations/) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Silkroad Life Suite'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-silkroad-life-suite-single-sign-on"></a>Yapılandırılan SilkRoad Life Suite Tek İşaret-On

1. Yönetici olarak SilkRoad şirket sitenizde oturum açın.

    > [!NOTE]
    > Federasyonu Microsoft Azure AD ile yapılandırmak için SilkRoad Life Suite Kimlik Doğrulama uygulamasına erişmek için lütfen SilkRoad Destek veya SilkRoad Hizmetleri temsilcinizle iletişime geçin.

1. Servis **Sağlayıcı'ya**gidin ve ardından **Federasyon Ayrıntıları'nı**tıklatın.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png)

1. **Federasyon Meta Verilerini İndir'i**tıklatın ve ardından meta veri dosyasını bilgisayarınıza kaydedin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde Hizmet **Sağlayıcısı meta veri dosyası** olarak İndirilen Federasyon Meta verilerini kullanın.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png)

1. **SilkRoad** uygulamanızda **Kimlik Doğrulama Kaynakları'nı**tıklatın.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png) 

1. **Kimlik Doğrulama Kaynağı Ekle'yi**tıklatın.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png)

1. Kimlik **Doğrulama Kaynağı Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png)
  
    a. **Seçenek 2 - MetaData File**altında, Azure portalından indirilen meta veri dosyasını yüklemek için **Gözat'ı** tıklatın.
  
    b. **Dosya Verilerini kullanarak Kimlik Sağlayıcısı Oluştur'u**tıklatın.

1. Kimlik **Doğrulama Kaynakları** **bölümünde, Edit'i**tıklatın.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png)

1. Kimlik **Doğrulama Kaynağını Edit** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png)

    a. **Etkin olarak**, **Evet'i**seçin.

    b. **EntityId** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD** Tanımlayıcısı'nın değerini yapıştırın.

    c. **IdP Açıklama** metin kutusunda, yapılandırmanız için bir açıklama yazın (örneğin: *Azure AD SSO).*

    d. Meta **data Dosyası** metin kutusunda, Azure portalından indirdiğiniz **meta veri** dosyasını yükleyin.
  
    e. **IdP Adı** metin kutusunda, yapılandırmanıza özgü bir ad yazın (örneğin: *Azure SP).*
  
    f. Oturum **Açma Hizmeti URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    g. Oturum **Açma hizmeti URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    h. **Kaydet**'e tıklayın.

1. Diğer tüm kimlik doğrulama kaynaklarını devre dışı bırakma.

    ![Azure AD Tek Oturum Açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png)

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

Bu bölümde, Britta Simon'ın SilkRoad Life Suite'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **SilkRoad Life Suite'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **SilkRoad Life Suite'i**seçin.

    ![Uygulamalar listesindeki SilkRoad Life Suite bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-silkroad-life-suite-test-user"></a>SilkRoad Life Suite test kullanıcıoluşturma

Bu bölümde, SilkRoad Life Suite'te Britta Simon adında bir kullanıcı oluşturursunuz. [SilkRoad Life Suite müşteri destek ekibiyle](https://www.silkroad.com/locations/) birlikte çalışarak kullanıcıları SilkRoad Life Suite platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SilkRoad Life Suite döşemesini tıklattığınızda, SSO'yu kurduğunuz SilkRoad Life Suite'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
