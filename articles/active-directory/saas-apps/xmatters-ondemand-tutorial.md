---
title: 'Öğretici: xMatters OnDemand ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve xMatters OnDemand arasında tek oturum açma yı nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: e8ae31122d59238ac104d7d873cf56f32977c9af
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086512"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Öğretici: xMatters OnDemand ile Azure Active Directory entegrasyonu

Bu eğitimde, xMatters OnDemand'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
xMatters OnDemand'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* xMatters OnDemand erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla xMatters OnDemand (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini xMatters OnDemand ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* xMatters OnDemand tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* xMatters OnDemand **IDP** başlatılan SSO destekler

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden xMatters OnDemand ekleme

xMatters OnDemand'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize xMatters OnDemand eklemeniz gerekir.

**Galeriden xMatters OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **xMatters OnDemand**yazın, sonuç panelinden **xMatters OnDemand'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![xMatters OnDemand sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak azure AD tek oturum açma işlemini xMatters OnDemand ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışması için, bir Azure AD kullanıcısı ile xMatters OnDemand'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı xMatters OnDemand ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[xMatters OnDemand Tek Oturum Açma'yı yapılandırın.](#configure-xmatters-ondemand-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[xMatters OnDemand test kullanıcısını oluşturun](#create-xmatters-ondemand-test-user)** - kullanıcının Azure AD gösterimine bağlı xMatters OnDemand'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini xMatters OnDemand ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **xMatters OnDemand** uygulama tümleştirme **sayfasında, Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![xMatters OnDemand Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|
    | |

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [xMatters OnDemand İstemci destek ekibine](https://www.xmatters.com/company/contact-us/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!IMPORTANT]
    > Sertifikayı [xMatters OnDemand destek ekibine](https://www.xmatters.com/company/contact-us/)iletmeniz gerekir. Tek oturum açma yapılandırmasını sonuçlandırabilmesi için sertifikanın xMatters destek ekibi tarafından yüklenmesi gerekir.

6. **xMatters OnDemand'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-xmatters-ondemand-single-sign-on"></a>yapılandırma xMatters OnDemand Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, XMatters OnDemand şirket sitenizde yönetici olarak oturum açın.

2. Üstteki araç çubuğunda **Yönetici'yi**tıklatın ve ardından sol taraftaki gezinti çubuğundaki **Şirket Ayrıntıları'nı** tıklatın.

    ![Yönetici](./media/xmatters-ondemand-tutorial/IC776795.png "Yönetici")

3. **SAML Yapılandırma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![SAML yapılandırması](./media/xmatters-ondemand-tutorial/IC776796.png "SAML yapılandırması")

    a. **SAML'yi Etkinleştir'i**seçin.

    b. Kimlik **Sağlayıcı Kimliği** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. **URL'de Tek Oturum** Açma metin kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. Tek **Giriş URL** metin kutusunda, Azure portalından kopyaladığınız **Logout URL'sini**yapıştırın.

    e. Şirket Ayrıntıları sayfasında, en üstte **Değişiklikleri Kaydet'i**tıklatın.

    ![Şirket bilgileri](./media/xmatters-ondemand-tutorial/IC776797.png "Şirket bilgileri")

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

Bu bölümde, Britta Simon'ın xMatters OnDemand'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından xMatters OnDemand'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, xMatters OnDemand'ı**seçin.

    ![Uygulamalar listesindeki xMatters OnDemand bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-xmatters-ondemand-test-user"></a>xMatters OnDemand test kullanıcıoluşturma

Bu bölümün amacı xMatters OnDemand Britta Simon adlı bir kullanıcı oluşturmaktır.

**Kullanım kılavuzunu el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:**

1. **XMatters OnDemand** kiracınızda oturum açın.

2. **Kullanıcılar** sekmesini tıklatın ve ardından **Kullanıcı Ekle'yi**tıklatın.

    ![Kullanıcılar](./media/xmatters-ondemand-tutorial/IC781048.png "Kullanıcılar")

3. Kullanıcı **Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Ekleme](./media/xmatters-ondemand-tutorial/IC781049.png "Kullanıcı Ekleme")

    a. **Etkin'i**seçin.

    b. Kullanıcı **Kimliği** metin kutusuna, kullanıcı kimliğini Brittasimon@contoso.com'ye benzer şekilde yazın.

    c. Ad **metin** kutusuna, Britta gibi kullanıcının ilk adını yazın.

    d. **Soyadı** metin kutusuna, Kullanıcının soyadını Simon gibi yazın.

    e. **Site** metin kutusuna, sağlamak istediğiniz geçerli bir Azure REKLAM hesabının geçerli sitesini girin.

    f. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki xMatters OnDemand döşemesini tıklattığınızda, SSO'yu kurduğunuz xMatters OnDemand'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

