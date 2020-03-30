---
title: 'Öğretici: Riskware ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Riskware arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 6eaa1be81d3ac0733c0829bc45e1b62f8aae5755
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72027106"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Öğretici: Riskware ile Azure Active Directory entegrasyonu

Bu eğitimde, Riskware'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Riskware'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Riskware erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Riskware'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Riskware ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Riskware tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Riskware **SP** başlatılan SSO destekler

## <a name="adding-riskware-from-the-gallery"></a>Galeriden Riskware ekleme

Riskware'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Riskware eklemeniz gerekir.

**Galeriden Riskware eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Riskware**yazın, sonuç panelinden **Riskware'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinderiskware](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Riskware'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Riskware ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Riskware Tek Oturum](#configure-riskware-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Riskware test kullanıcı oluşturun](#create-riskware-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Riskware Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Riskware ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Riskware** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Riskware Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | Ortam| URL Deseni|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | Prod| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | Demo| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. Tanımlayıcı **(Entity ID)** metin kutusuna URL'yi yazın:
    
    | Ortam| URL Deseni|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | Prod| `https://riskcloud.net/prod` |
    | Demo| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > URL değerindeki Oturum gerçek değildir. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Riskware İstemci destek ekibine](mailto:support@pansoftware.com.au) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Riskware'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-riskware-single-sign-on"></a>Riskware Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Riskware şirket sitenizde yönetici olarak oturum açın.

1. Sağ üstte, bakım sayfasını açmak için **Bakım'ı** tıklatın.

    ![Riskware Yapılandırmaları korumak](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Bakım sayfasında Kimlik **Doğrulama'yı**tıklatın.

    ![Riskware Yapılandırma authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. **Kimlik Doğrulama Yapılandırması** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Riskware Yapılandırma authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Kimlik doğrulaması için **SAML** olarak **Yazın'ı** seçin.

    b. **Kod** metin kutusuna kodunuzu AZURE_UAT gibi yazın.

    c. **Açıklama** metin kutusuna, açıklamanızı SSO için AZURE Yapılandırması gibi yazın.

    d. **Tek Oturum Aç metin** kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL** değerini yapıştırın.

    e. **Sayfayı Oturum Aç** metin kutusunda, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    f. Form **Alanı sonrası** metin kutusuna, SamLResponse gibi SAML içeren Yanıt Sonrası'nda bulunan alan adını yazın

    g. **XML Identity Tag Name** textbox'ta, NameID gibi SAML yanıtındaki benzersiz tanımlayıcıyı içeren tür özniteliği.

    h. İndirilen **Metadata Xml'i** not defterinde Azure portalından açın, Sertifikayı Meta veri dosyasından kopyalayın ve **Sertifika** metin kutusuna yapıştırın

    i. **Tüketici URL** textbox'ına, destek ekibinden aldığınız **YanıtLA URL**değerini yapıştırın.

    j. **Veren** metin kutusunda, destek ekibinden aldığınız **Tanımlayıcının**değerini yapıştırın.

    > [!Note]
    > Bu değerleri almak için [Riskware İstemci destek ekibine](mailto:support@pansoftware.com.au) başvurun

    k. POST onay kutusunu **kullan'ı** seçin.

    l. **SAML İstek** onay kutusunu kullan'ı seçin.

    m. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Riskware'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Riskware'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Riskware'i**seçin.

    ![Uygulamalar listesindeki Riskware bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-riskware-test-user"></a>Riskware test kullanıcısı oluşturma

Azure AD kullanıcılarının Riskware'de oturum açmalarını sağlamak için Riskware'de oturum açmaları gerekir. Riskware'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak Riskware'de oturum açın.

1. Sağ üstte, bakım sayfasını açmak için **Bakım'ı** tıklatın. 

    ![Riskware Yapılandırması,](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Bakım sayfasında **Kişiler'i**tıklatın.

    ![Riskware Yapılandırma sı](./media/riskware-tutorial/tutorial_riskware_people.png)

1. **Ayrıntılar** sekmesini seçin ve aşağıdaki adımları gerçekleştirin:

    ![Riskware Yapılandırma ayrıntıları](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Çalışan gibi **Kişi Türü'nü** seçin.

    b. **Ad metin** **kutusuna, Britta**gibi kullanıcının ilk adını girin.

    c. **Soyadı** textbox,Simon gibi kullanıcının **Simon**soyadını girin.

1. **Güvenlik** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Riskware Yapılandırma güvenliği](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. **Kimlik Doğrulama** bölümünde, SSO için AZURE Yapılandırması gibi kurulumunuz olan **Kimlik Doğrulama** modunu seçin.

    b. **Logon Details** bölümünün altında, **Kullanıcı Kimliği** metin kutusuna, kullanıcının e-postasını girin. `brittasimon@contoso.com`

    c. **Parola** metin kutusuna, kullanıcının parolasını girin.

1. **Kuruluş** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Riskware Yapılandırma org](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. **Düzey1** kuruluşu olarak seçeneği seçin.

    b. **Kişinin Birincil İşyeri** bölümünde, **Konum** metin kutusunda konumunuzu yazın.

    c. **Çalışan** bölümü altında, Casual gibi **Çalışan Durumu'nun**

    d. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Riskware döşemesini tıklattığınızda, SSO'yu kurduğunuz Riskware'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
