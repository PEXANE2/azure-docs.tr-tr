---
title: 'Öğretici: Ralli Yazılımı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Rally Software arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: de84d03c3e0e433dbe7bc24c47b1766b32ad7bc4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67093171"
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Öğretici: Ralli Yazılımı ile Azure Active Directory entegrasyonu

Bu eğitimde, Ralli Yazılımlarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Ralli Yazılımını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Ralli Yazılımına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik olarak Ralli Yazılımı (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Rally Software ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Rally Software tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Rally Software **SP** başlatılan SSO destekler

## <a name="adding-rally-software-from-the-gallery"></a>Galeriden Ralli Yazılımı Ekleme

Rally Software'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Rally Yazılımı eklemeniz gerekir.

**Galeriden Rally Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Ralli Yazılımı**yazın, sonuç panelinden **Ralli Yazılımı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Ralli Yazılımı sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Ralli Yazılımı ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Rally Software'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçma işlemlerini Ralli Yazılımı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Ralli Yazılımı Tek Oturum Açma'yı **[yapılandırın.](#configure-rally-software-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Ralli Yazılımı oluşturma test kullanıcısı oluşturun](#create-rally-software-test-user)** - Kullanıcının Azure AD gösterimine bağlı Ralli Yazılımı'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Rally Software ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Ralli Yazılımı** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Rally Software Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant-name>.rally.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant-name>.rally.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Rally Software Client destek ekibine](https://help.rallydev.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Ralli **Yazılımı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-rally-software-single-sign-on"></a>Ralli Yazılımı Tek Oturum Açma'yı Yapılandır

1. **Rally Software** kiracınızda oturum açın.

2. Üstteki araç çubuğunda **Kurulum'u**tıklatın ve ardından **Abonelik'i**seçin.
   
    ![Abonelik](./media/rally-software-tutorial/ic769531.png "Abonelik")

3. **Eylem** düğmesini tıklatın. Araç çubuğunun sağ üst tarafındaki **Aboneliği Edit'i** seçin.

4. **Abonelik** iletişim sayfasında aşağıdaki adımları gerçekleştirin ve ardından **Kaydet & Kapat'ı**tıklatın:
   
    ![Kimlik Doğrulaması](./media/rally-software-tutorial/ic769542.png "Kimlik Doğrulaması")
   
    a. Kimlik Doğrulama açılır tarafından **Rally veya SSO kimlik doğrulaması'nı** seçin.

    b. Kimlik **sağlayıcısı URL** textbox'ına, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın. 

    c. **SSO Giriş** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

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

Bu bölümde, Britta Simon'ın Ralli Yazılımı'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Ralli Yazılımı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Rally Software'i**seçin.

    ![Uygulamalar listesindeki Ralli Yazılımı bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-rally-software-test-user"></a>Rally Software test kullanıcıoluşturma

Azure AD kullanıcılarının oturum açabilmesi için, Azure Active Directory kullanıcı adlarını kullanarak Ralli Yazılımı uygulamasına dahil edilmeleri gerekir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Rally Software kiracınızda oturum açın.

2. **Kurulum \> KULLANICILARI'na**gidin ve ardından **+ Yeni Ekle'yi**tıklatın.
   
    ![Kullanıcılar](./media/rally-software-tutorial/ic781039.png "Kullanıcılar")

3. Yeni Kullanıcı metin kutusuna adı yazın ve ardından **Ayrıntılarla Ekle'yi**tıklatın.

4. Kullanıcı **Oluştur** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı Oluştur](./media/rally-software-tutorial/ic781040.png "Kullanıcı Oluştur")

    a. Kullanıcı **Adı** metin kutusuna **Brittsimon**gibi kullanıcının adını yazın.
   
    b. **E-posta Adresi** textbox, gibi brittasimon@contoso.comkullanıcının e-posta girin .

    c. **Ad metin** kutusuna, **Britta**gibi kullanıcının ilk adını girin.

    d. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    e. **Kaydet ve Kapat**’a tıklayın.

   >[!NOTE]
   >Azure AD kullanıcı hesaplarını sağlamak için Rally Software tarafından sağlanan diğer Rally Software kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Ralli Yazılımı döşemesini tıklattığınızda, SSO'yu kurduğunuz Ralli Yazılımı'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

