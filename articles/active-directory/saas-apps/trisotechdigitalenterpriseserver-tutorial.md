---
title: 'Öğretici: Trisotech Digital Enterprise Server ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Trisotech Digital Enterprise Server arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 56399f99ede611c4a120603cce3a3eede2728c6d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67088257"
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Öğretici: Trisotech Digital Enterprise Server ile Azure Active Directory entegrasyonu

Bu eğitimde, Trisotech Digital Enterprise Server'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Trisotech Digital Enterprise Server'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Trisotech Digital Enterprise Server'a erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Trisotech Digital Enterprise Server'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Trisotech Digital Enterprise Server ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Trisotech Digital Enterprise Server tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Trisotech Digital Enterprise **Server, SP** tarafından başlatılan SSO'ya destek veriyor

* Trisotech Digital Enterprise Server **Just In Time** kullanıcı sağlama destekler

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Galeriden Trisotech Digital Enterprise Server ekleme

Trisotech Digital Enterprise Server'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Trisotech Digital Enterprise Server'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Trisotech Digital Enterprise Server eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Trisotech Digital Enterprise Server**yazın, sonuç panelinden **Trisotech Digital Enterprise Server'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Trisotech Digital Enterprise Server sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Trisotech Digital Enterprise Server ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Trisotech Digital Enterprise Server'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Trisotech Digital Enterprise Server ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Trisotech Digital Enterprise Server Tek Oturum Açma](#configure-trisotech-digital-enterprise-server-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Trisotech Digital Enterprise Server test kullanıcısını oluşturun](#create-trisotech-digital-enterprise-server-test-user)** - Kullanıcının Azure AD temsiline bağlı Trisotech Digital Enterprise Server'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Trisotech Digital Enterprise Server ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Trisotech Digital Enterprise Server** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Trisotech Digital Enterprise Server Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.trisotech.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.trisotech.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Trisotech Digital Enterprise Server Client destek ekibine](mailto:support@trisotech.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-trisotech-digital-enterprise-server-single-sign-on"></a>Trisotech Digital Enterprise Server Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Trisotech Digital Enterprise Server Configuration şirket sitenizde oturum açın.

2. **Menü simgesine** tıklayın ve ardından **İdare'yi**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/trisotechdigitalenterpriseserver-tutorial/user1.png)

3. **Kullanıcı Sağlayıcı'yı**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/trisotechdigitalenterpriseserver-tutorial/user2.png)

4. Kullanıcı **Sağlayıcı Yapılandırmaları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. **Kimlik Doğrulama Yöntemi'ndeki**açılır bırakma dan **Güvenli İddia İşaretle dili 2 (SAML 2)** seçin.

    b. **Metadata URL** metin kutusuna, Azure portalını kopyaladığınız **Uygulama Federasyonu Metadata Url** değerini yapıştırın.

    c. Uygulama **Kimliği** metin kutusuna aşağıdaki deseni kullanarak `https://<companyname>.trisotech.com`URL'yi girin: .

    d. **Kaydet'i** tıklatın

    e. İzin Verilen Etki **Alanları (boş herkes anlamına gelir)** textbox alan adını girin, otomatik olarak İzin Verilen Etki Alanları eşleşen kullanıcılar için lisansatar

    f. **Kaydet'i** tıklatın

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

Bu bölümde, Britta Simon'ın Trisotech Digital Enterprise Server'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Trisotech Digital Enterprise Server'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Trisotech Digital Enterprise Server'ı**seçin.

    ![Uygulamalar listesindeki Trisotech Digital Enterprise Server bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-trisotech-digital-enterprise-server-test-user"></a>Trisotech Digital Enterprise Server test kullanıcısı oluşturun

Bu bölümde, Britta Simon adlı bir kullanıcı Trisotech Digital Enterprise Server oluşturulur. Trisotech Digital Enterprise Server, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Trisotech Digital Enterprise Server'da zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Trisotech Digital Enterprise Server destek ekibine](mailto:support@trisotech.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Trisotech Digital Enterprise Server döşemesini tıklattığınızda, SSO'yu kurduğunuz Trisotech Digital Enterprise Server'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

