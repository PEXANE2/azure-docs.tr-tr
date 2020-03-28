---
title: 'Öğretici: Lifesize Cloud ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Lifesize Cloud arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e886a1fa3f590ac94dbf088520e6770690ee21ac
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159563"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Öğretici: Lifesize Cloud ile Azure Active Directory entegrasyonu

Bu eğitimde, Lifesize Cloud'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Lifesize Cloud'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Lifesize Cloud erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Lifesize Cloud 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Lifesize Cloud ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Lifesize Cloud tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Lifesize **Cloud, SP** tarafından başlatılan SSO'ya destek verdi

* Lifesize **Cloud, Otomatik** kullanıcı sağlamayı destekler

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Galeriden Lifesize Bulut ekleme

Lifesize Cloud'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Lifesize Cloud'u eklemeniz gerekir.

**Galeriden Lifesize Cloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Lifesize Cloud**yazın, sonuç panelinden **Lifesize Cloud'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Lifesize Cloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Lifesize Cloud ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Lifesize Cloud'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Lifesize Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Lifesize Cloud Tek Oturum](#configure-lifesize-cloud-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Lifesize Cloud test kullanıcısını oluşturun](#create-lifesize-cloud-test-user)** - Kullanıcının Azure AD gösterimine bağlı Lifesize Cloud'daki Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Lifesize Cloud ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Lifesize Cloud** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Lifesize Cloud Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-relay.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://login.lifesizecloud.com/ls/?acs`

    b. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.lifesizecloud.com/<companyname>`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://webapp.lifesizecloud.com/?ent=<identifier>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Röle Durumu ile güncelleştirin. Oturum Açma URL'si ve Tanımlayıcı değerlerini almak için [Lifesize Cloud Client destek ekibine](https://www.lifesize.com/en/support) başvurun ve daha sonra öğreticide açıklanan SSO Yapılandırmasından Relay State değerini alabilirsiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Lifesize Cloud'u ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-lifesize-cloud-single-sign-on"></a>Lifesize Cloud Tek İşaretle Yapılandır

1. SSO'nun uygulamanız için yapılandırılmasını sağlamak için, Yönetici ayrıcalıklarıyla Lifesize Cloud uygulamasına giriş yapın.

2. Sağ üst köşede adınızı n üzerine tıklayın ve ardından **Önceden Ayarlar'a**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

3. Gelişmiş Ayarlar'da artık **SSO Yapılandırma** bağlantısına tıklayın. Örneğiniz için SSO Yapılandırma sayfasını açar.

    ![Tek İşaret-On'u Yapılandır](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

4. Şimdi SSO yapılandırma UI'de aşağıdaki değerleri yapılandırın.

    ![Tek İşaret-On'u Yapılandır](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    a. **Kimlik Sağlayıcı Veren** metin kutusuna, Azure portalından kopyalamış olduğunuz Azure Reklam **Tanımlayıcısı** değerini yapıştırın.

    b.  **Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    c. Azure portalından indirilen not defterinde base-64 kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve **ardından X.509 Sertifikası** metin kutusuna yapıştırın.
  
    d. Ad metin kutusunun SAML Öznitelik eşlemelerinde değeri`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    e. **Soyadı** metin kutusunun SAML Öznitelik eşlemesinde değeri`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    f. **E-posta** metin kutusu için SAML Öznitelik eşleme olarak değeri girin`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

5. Yapılandırmayı kontrol etmek için **Test** düğmesine tıklayabilirsiniz.

    >[!NOTE]
    >Başarılı sınama için Yapılandırma sihirbazını Azure AD'de tamamlamanız ve ayrıca testi gerçekleştirebilecek kullanıcılara veya gruplara erişim sağlamanız gerekir.

6. SSO'yu etkinleştir düğmesini işaretleyerek **SSO'yu** etkinleştirin.

7. Şimdi tüm ayarların kaydedilen için **Güncelleştir** düğmesine tıklayın. Bu, RelayState değerini oluşturur. Metin kutusunda oluşturulan RelayState değerini kopyalayın, **Lifesize Cloud Etki Alanı ve URL'ler** bölümünün altındaki **Relay State** metin kutusuna yapıştırın.

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

Bu bölümde, Britta Simon'ın Lifesize Cloud'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Lifesize Cloud'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Lifesize Cloud'u**seçin.

    ![Uygulamalar listesindeki Lifesize Cloud bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-lifesize-cloud-test-user"></a>Lifesize Cloud test kullanıcısı oluşturma

Bu bölümde, Lifesize Cloud'da Britta Simon adında bir kullanıcı oluşturursunuz. Lifesize bulut otomatik kullanıcı sağlama destekler. Azure AD'de başarılı bir kimlik doğrulaması yaptıktan sonra, kullanıcı uygulamada otomatik olarak kullanılabilir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Lifesize Cloud döşemesini tıklattığınızda, Lifesize Cloud uygulamasının giriş sayfasını almalısınız. Burada kullanıcı adınızı girmeniz gerekir ve bundan sonra uygulama ana sayfasına yönlendirilir.

Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
