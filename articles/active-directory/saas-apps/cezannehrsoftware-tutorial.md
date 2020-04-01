---
title: 'Öğretici: Cezanne İK Yazılımı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Cezanne HR Software arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa689e6138f8d965e59f7cfa7a85e0835301086c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158707"
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Öğretici: Cezanne İK Yazılımı ile Azure Active Directory entegrasyonu

Bu eğitimde, Cezanne İK YazılımLarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Cezanne HR Yazılımını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Cezanne HR Yazılımına erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Cezanne İk Yazılımında (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Cezanne HR Yazılımı ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Cezanne İk Yazılımı tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Cezanne İk Yazılımı **SP** başlatılan SSO destekler

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Galeriden Cezanne İk Yazılımı Ekleme

Cezanne HR Yazılımının Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Cezanne İK Yazılımını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Cezanne İK Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Cezanne HR Yazılımı**yazın, sonuç panelinden **Cezanne HR Yazılımı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Cezanne İk Yazılımı sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Cezanne İK Yazılımı ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Cezanne İk Yazılımı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Cezanne HR Yazılımı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Cezanne HR Software Tek Oturum Açma](#configure-cezanne-hr-software-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Cezanne HR Software test kullanıcıoluşturun](#create-cezanne-hr-software-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Cezanne İk Yazılımı Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Cezanne HR Yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Cezanne İk Yazılımı** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Cezanne İk Yazılım Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna URL'yi yazın:`https://w3.cezanneondemand.com/CezanneOnDemand/`

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Cezanne HR Software Client destek ekibine](https://cezannehr.com/services/support/) başvurun.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Cezanne İk Yazılımı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-cezanne-hr-software-single-sign-on"></a>Cezanne HR Yazılımı Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, Cezanne İK Yazılım kiracınızı yönetici olarak oturum açın.

2. Soldaki gezinti bölmesine Sistem **Kurulumu'nu**tıklatın. Güvenlik **Ayarları'na**gidin. Ardından **Tek Oturum Açma Yapılandırmasına**gidin.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

3. Kullanıcıların **aşağıdaki Tek Oturum Açma (SSO) Hizmeti panelini kullanarak oturum açmalarına izin** ver,SAML **2.0** kutusunu işaretleyin ve **Gelişmiş Yapılandırma** seçeneğini seçin.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

4. **Yeni Ekle** düğmesini tıklatın.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

5. **SAML 2.0 KİmLİk SADIKLARI** bölümünde aşağıdaki adımları gerçekleştirin.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    a. **Görüntü Adı**olarak Kimlik Sağlayıcınızın adını girin.

    b. Varlık **Tanımlayıcısı** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure Reklam Tanımlayıcısı** değerini yapıştırın.

    c. **SAML Bağlama'yı** 'POST' olarak değiştirin.

    d. Güvenlik **Belirteci Hizmeti Bitiş Noktası** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    e. Kullanıcı Kimliği Öznitelik Adı textbox'ına . `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`

    f. İndirilen sertifikayı Azure portalından yüklemek için **Yükle** simgesini tıklatın.

    g. **Tamam** düğmesine tıklayın.

6. **Kaydet** düğmesini tıklatın.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

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

Bu bölümde, Britta Simon'ın Cezanne HR Yazılımına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **cezanne Hr Yazılımı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Cezanne İk Yazılımı'nı**seçin.

    ![Uygulamalar listesinde cezanne İk Yazılım bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-cezanne-hr-software-test-user"></a>Cezanne İK Yazılım test kullanıcıoluşturma

Azure AD kullanıcılarının Cezanne İK Yazılımı'na giriş yapabilmeleri için Cezanne HR Yazılımı'na dahil edilmeleri gerekir. Cezanne İk Yazılımı söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Cezanne HR Software şirket sitenize yönetici olarak giriş yapın.

2. Soldaki gezinti bölmesine Sistem **Kurulumu'nu**tıklatın. Kullanıcıları **Yönet'e**gidin. Ardından **Yeni Kullanıcı Ekle'ye**gidin.

    ![Yeni Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Yeni Kullanıcı")

3. **Kİşİ BİlGİlerİ** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Yeni Kullanıcı")

    a. **Dahili Kullanıcı'yı** KAPALI olarak ayarlayın.

    b. Ad **metin** kutusuna, **Britta**gibi kullanıcının Adını yazın.  

    c. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını yazın.

    d. E-posta metin kutusuna, kullanıcının **e-posta** adresini şöyle Brittasimon@contoso.comyazın.

4. **Hesap Bilgileri** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Yeni Kullanıcı")

    a. Kullanıcı **adı** metin kutusuna, kullanıcının Brittasimon@contoso.come-postasını yazın.

    b. **Parola** metin kutusuna, kullanıcının parolasını yazın.

    c. **Güvenlik Rolü**olarak **İk Profesyonelini** seçin.

    d. **Tamam**'a tıklayın.

5. Tek **Oturum Açma** sekmesine gidin ve **SAML 2.0 Tanımlayıcılar** alanında **Yeni Ekle'yi** seçin.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Kullanıcı")

6. **Kimlik Sağlayıcınız** için Kimlik Sağlayıcınızı seçin ve **Kullanıcı Tanımlayıcısı**metin kutusuna Britta Simon hesabının e-posta adresini girin.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Kullanıcı")

7. **Kaydet** düğmesini tıklatın.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Kullanıcı")

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Cezanne İk Yazılımı döşemesini tıklattığınızda, SSO'yu kurduğunuz Cezanne İk Yazılımı'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
