---
title: 'Öğretici: Questetra BPM Suite ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Questetra BPM Suite arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: f58d6cbc6ec04e51e105662dff31c60ff502584c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67093355"
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Öğretici: Questetra BPM Suite ile Azure Active Directory entegrasyonu

Bu eğitimde, Questetra BPM Suite'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Questetra BPM Suite'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Questetra BPM Suite'e erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Questetra BPM Suite'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Questetra BPM Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Questetra BPM Suite tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Questetra BPM **Suite, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Galeriden Questetra BPM Suite ekleme

Questetra BPM Suite'in Azure AD'ye entegrasyonunu yapılandırmak için, questetra BPM Suite'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Questetra BPM Suite eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Questetra BPM Suite**yazın, sonuç panelinden **Questetra BPM Suite'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Questetra BPM Suite](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Questetra BPM Suite ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Questetra BPM Suite'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumunuzu Questetra BPM Suite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Questetra BPM Suite Tek Oturum Açma](#configure-questetra-bpm-suite-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Questetra BPM Suite test kullanıcısını oluşturun](#create-questetra-bpm-suite-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Questetra BPM Suite'te Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Questetra BPM Suite ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Questetra BPM Suite** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Questetra BPM Suite Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.questetra.net/saml/SSO/alias/bpm`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.questetra.net/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve Tanımlayıcıile güncelleştirin. Bu değerleri, daha sonra öğreticide açıklanan **Questetra BPM Suite** şirket sitenizdeki **SP Information** bölümünden edinebilir veya [Questetra BPM Suite Müşteri destek ekibiyle](https://www.questetra.com/contact/)iletişime geçebilirsiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Questetra BPM Suite'i Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-questetra-bpm-suite-single-sign-on"></a>Questetra BPM Suite Tek İşaretli Yapıla

1. Farklı bir web tarayıcısı penceresinde, **Questetra BPM Suite** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde Sistem **Ayarları'nı**tıklatın. 
   
    ![Azure AD Tek Oturum Açma][10]

3. **SingleSignOnSAML** sayfasını açmak için **SSO (SAML) sayfasını**tıklatın. 
   
    ![Azure AD Tek Oturum Açma][11]

4. **Questetra BPM Suite** şirket sitenizde, **SP Information** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **ACS URL'sini**kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Oturum Aç URL** metin kutusuna yapıştırın.
    
    b. Varlık **Kimliğini**kopyalayın ve Azure portalından **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

5. **Questetra BPM Suite** şirket sitenizde aşağıdaki adımları gerçekleştirin: 
   
    ![Tek İşaret-On'u Yapılandır][15]
   
    a. **Tek Oturum Aç'ı Etkinleştir'i**seçin.
   
    b. **Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz Azure **AD Tanımlayıcısı** değerini yapıştırın.
    
    c. **Oturum Açma sayfası URL** textbox'ına, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.
    
    d. **Oturum Açma sayfası URL** textbox'ına, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.
    
    e. **NameID biçimindeki** textbox'a `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Azure portalından indirilen not defterinde **Base-64** kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve ardından **Doğrulama sertifikası** metin kutusuna yapıştırın. 

    g. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Questetra BPM Suite'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Questetra BPM Suite'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Questetra BPM Suite'i**seçin.

    ![Uygulamalar listesindeki Questetra BPM Suite bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-questetra-bpm-suite-test-user"></a>Questetra BPM Suite test kullanıcısı oluşturma

Bu bölümün amacı Questetra BPM Suite Britta Simon adlı bir kullanıcı oluşturmaktır.

**Questetra BPM Suite'te Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Questetra BPM Suite şirket sitenizde yönetici olarak oturum açın.

2. Yeni **Kullanıcı > Kullanıcı Listesi > Sistem Ayarları'na**gidin.
 
3. Yeni Kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin: 
   
    ![Test kullanıcısı oluşturma][300] 
   
    a. **Ad** metin kutusunda, **name** kullanıcının britta.simon@contoso.comadını yazın.
   
    b. **E-posta** metin kutusunda, kullanıcının britta.simon@contoso.com **e-posta** yazın.
   
    c. **Parola** metin kutusuna, kullanıcının **parolasını** yazın.
    
    d. **Yeni kullanıcı ekle'yi**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Questetra BPM Suite döşemesini tıklattığınızda, SSO'yu kurduğunuz Questetra BPM Suite'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[10]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[300]: ./media/questetra-bpm-suite-tutorial/questera_bpm_suite_11.png