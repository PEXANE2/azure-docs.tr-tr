---
title: 'Öğretici: Canvas ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Canvas arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3ce5cff0de2939c25400d1d63138b23bc6c9822
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74232044"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas"></a>Öğretici: Canvas ile Azure Active Directory entegrasyonu

Bu eğitimde, Canvas'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Tuvali Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Canvas erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak Canvas'ta (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Canvas ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Tuval tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Canvas, **SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-canvas-from-the-gallery"></a>Galeriden Tuval Ekleme

Canvas'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Canvas eklemeniz gerekir.

**Galeriden Tuval eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Kanvas**yazın, sonuç panelinden **Tuval'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesindeki tuval](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırmak ve test etmek.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Canvas'taki ilgili kullanıcı arasında bir bağlantı ilişkisikurulması gerekir.

Azure AD oturumaçmayı Canvas ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Kanvas Tek Oturum](#configure-canvas-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Kullanıcının](#create-canvas-test-user)** Azure AD gösterimine bağlı Olan Canvas'ta Britta Simon'ın bir örneğine sahip olmak için Canvas test kullanıcısı oluşturun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Canvas ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Azure** [portalında,](https://portal.azure.com/)Canvas uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tuval Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant-name>.instructure.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant-name>.instructure.com/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Canvas İstemci destek ekibine](https://community.canvaslms.com/community/help) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

6. **SAML İmza Sertifikası** **bölümünde, THUMBPRINT'i** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

7. **Kanvas'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-canvas-single-sign-on"></a>YapıyapılandırKan Tek İşaret

1. Farklı bir web tarayıcısı penceresinde, Bir yönetici olarak Canvas şirket sitenizde oturum açın.

2. **Dersler \> Yönetilen Hesaplar \> Microsoft**gidin.

    ![Tuval](./media/canvas-lms-tutorial/ic775990.png "Tuval")

3. Soldaki gezinti bölmesinde Kimlik **Doğrulama'yı**seçin ve ardından **Yeni SAML Config ekle'yi**tıklatın.

    ![Kimlik doğrulaması](./media/canvas-lms-tutorial/ic775991.png "Kimlik doğrulaması")

4. Geçerli Tümleştirme sayfasında aşağıdaki adımları gerçekleştirin:

    ![Geçerli Tümleştirme](./media/canvas-lms-tutorial/ic775992.png "Geçerli Tümleştirme")

    a. **IdP Entity ID** textbox'ına, Azure portalından kopyalamış olduğunuz **Azure Reklam Tanımlayıcısı** değerini yapıştırın.

    b. **Url'de Giriş** kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    c. **Giriş Çıkış URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. **Parola Bağlantısını Değiştir** metin kutusuna, Azure portalından kopyalamış olduğunuz Parola **URL'sini Değiştir** değerini yapıştırın.

    e. **Sertifika Parmak İzi** metin kutusuna, Azure portalından kopyalamış olduğunuz sertifikanın **Parmak Izi** değerini yapıştırın.

    f. Giriş **Atföz** listesinden **NameID'i**seçin.

    g. Tanımlayıcı **Biçim** listesinden **e-posta Adresi'ni**seçin.

    h. **Kimlik Doğrulama Ayarlarını Kaydet'i**tıklatın.

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

Bu bölümde, Britta Simon'ın Canvas'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Canvas'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Kanvas'ı**seçin.

    ![Uygulamalar listesindeki Tuval bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-canvas-test-user"></a>Kanvas test kullanıcısı oluşturma

Azure AD kullanıcılarının Canvas'ta oturum açmasını sağlamak için, bunların Canvas'a sağlanması gerekir. Canvas durumunda, kullanıcı sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Canvas** kiracınıza giriş yapın.

2. **Dersler \> Yönetilen Hesaplar \> Microsoft**gidin.

   ![Tuval](./media/canvas-lms-tutorial/ic775990.png "Tuval")

3. **Kullanıcılar**’a tıklayın.

   ![Kullanıcılar](./media/canvas-lms-tutorial/ic775995.png "Kullanıcılar")

4. **Yeni Kullanıcı Ekle'yi**tıklatın.

   ![Kullanıcılar](./media/canvas-lms-tutorial/ic775996.png "Kullanıcılar")

5. Yeni Kullanıcı Ekle iletişim sayfasında aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı Ekle](./media/canvas-lms-tutorial/ic775997.png "Kullanıcı Ekleme")

   a. Tam **Ad** metin **kutusuna BrittaSimon**gibi kullanıcının adını girin.

   b. **E-posta** metin kutusuna, **\@brittasimon contoso.com**gibi kullanıcının e-posta girin.

   c. **Giriş** metin kutusuna, **brittasimon\@contoso.com**gibi kullanıcının Azure AD e-posta adresini girin.

   d. **Bu hesap oluşturma hakkında kullanıcıya E-posta**gönder'i seçin.

   e. **Kullanıcı Ekle**'ye tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Canvas tarafından sağlanan diğer Kanvas kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Kanvas döşemesini tıklattığınızda, SSO'yu kurduğunuz Tuval'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

