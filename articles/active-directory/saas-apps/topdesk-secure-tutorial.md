---
title: 'Öğretici: TOPdesk ile Azure Active Directory entegrasyonu - Secure | Microsoft Dokümanlar'
description: Azure Active Directory ve TOPdesk - Secure arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 393557531fe69a494a16e0f4c49ac07891e490ec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233394"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Öğretici: TOPdesk ile Azure Active Directory entegrasyonu - Güvenli

Bu eğitimde, TOPdesk - Secure ile Azure Etkin Dizini (Azure AD) entegre etmeyi öğrenirsiniz.
TOPdesk'i tümleştirme - Azure AD ile güvenli olmak size aşağıdaki avantajları sağlar:

* TOPdesk - Secure erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla TOPdesk - Secure (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

TOPdesk - Secure ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* TOPdesk - Güvenli tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* TOPdesk - **Secure, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-topdesk---secure-from-the-gallery"></a>TOPdesk ekleme - Galeriden güvenli

TOPdesk - Secure ile Azure AD'nin entegrasyonunu yapılandırmak için, topdesk - Secure'u galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**TOPdesk eklemek için - Galeriden güvenli, aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TOPdesk yazın - Güvenli**, **TOPdesk** seçin - Sonuç panelinden güvenli sonra uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![TOPdesk - Sonuç listesinde güvenli](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini TOPdesk - Secure ile **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test edeyimsiniz.
Tek oturum açmanın çalışması için, Bir Azure AD kullanıcısı ile TOPdesk'teki ilgili kullanıcı - Secure arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturum açma işlemlerini TOPdesk - Secure ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[TOPdesk -Secure Single Sign-On](#configure-topdesk---secure-single-sign-on)** - yapıla.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[TOPdesk oluşturun - Güvenli test kullanıcısı](#create-topdesk---secure-test-user)** - TOPdesk'te Britta Simon'ın bir muadili olması için - Kullanıcının Azure AD temsiline bağlı güvenli.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

TOPdesk - Secure ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **TOPdesk - Güvenli** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![TOPdesk - Güvenli Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.topdesk.net`

    b. Tanımlayıcı **URL** kutusunda, TOPdesk yapılandırmasından alabileceğiniz TOPdesk meta veri URL'sini doldurun. Aşağıdaki deseni kullanmalıdır:`https://<companyname>.topdesk.net/saml-metadata/<identifier>`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.topdesk.net/tas/secure/login/verify`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [TOPdesk - Secure Client destek ekibine](https://www.topdesk.com/us/support/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **ToPdesk- Secure** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-topdesk---secure-single-sign-on"></a>TOPdesk'i Yapılandır - Güvenli Tek Oturum Açma

1. **TOPdesk - Secure** şirket sitenizde yönetici olarak oturum açın.

2. **TOPdesk** menüsünde **Ayarlar'ı**tıklatın.

    ![Ayarlar](./media/topdesk-secure-tutorial/ic790598.png "Ayarlar")

3. **Giriş Ayarları'nı**tıklatın.

    ![Giriş Ayarları](./media/topdesk-secure-tutorial/ic790599.png "Giriş Ayarları")

4. Giriş **Ayarları** menüsünü genişletin ve ardından **Genel'i**tıklatın.

    ![Genel](./media/topdesk-secure-tutorial/ic790600.png "Genel")

5. **SAML oturum açma** yapılandırması bölümünün **Güvenli** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Teknik Ayarlar](./media/topdesk-secure-tutorial/ic790855.png "Teknik Ayarlar")

    a. Ortak meta veri dosyasını indirmek ve ardından bilgisayarınıza yerel olarak kaydetmek için **İndir'i** tıklatın.

    b. Meta veri dosyasını açın ve ardından **AssertionConsumerService** düğümini bulun.

    ![İddia Tüketici Hizmeti](./media/topdesk-secure-tutorial/ic790856.png "İddia Tüketici Hizmeti")

    c. **AssertionConsumerService** değerini kopyalayın, bu değeri **TOPdesk - Güvenli Etki Alanı ve URL'ler** bölümündeki YanıtURL textbox'ına yapıştırın.

6. Sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:

    ![Sertifika](./media/topdesk-secure-tutorial/ic790606.png "Sertifika")

    a. Azure portalından indirilen meta veri dosyasını açın.

    b. **Xsi:fed türü** olan **RoleScriptor** düğümünün **genişletin:ApplicationServiceType**.

    c. **X509Sertifika** düğümünün değerini kopyalayın.

    d. Kopyalanan **X509Sertifika** değerini bilgisayarınıza yerel olarak bir dosyaya kaydedin.

7. **Genel** bölümünde **Ekle'yi**tıklatın.

    ![Ekle](./media/topdesk-secure-tutorial/ic790607.png "Ekle")

8. **SAML yapılandırma yardımcısı** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![SAML Yapılandırma Asistanı](./media/topdesk-secure-tutorial/ic790608.png "SAML Yapılandırma Asistanı")

    a. İndirdiğiniz meta veri dosyanızı Azure portalından, **Federasyon Meta verileri**altında yüklemek için **Gözat'ı**tıklatın.

    b. Sertifika dosyanızı yüklemek için **Sertifika (RSA)** altında **Gözat'ı**tıklatın.

    c. **Özel anahtar (RSA, PKCS8, DER)** için kendi özel anahtarınızı yükleyebilir veya özel anahtarı almak için [TOPdesk - Secure Client destek ekibiyle](https://www.topdesk.com/us/support) iletişime geçebilirsiniz.

    d. TOPdesk destek ekibinden aldığınız logo dosyasını **Logo simgesinin**altına yüklemek için **Gözat'ı**tıklatın.

    e. Kullanıcı **adı özniteliği** textbox,. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    f. Görüntü **adı** metin kutusuna yapılandırmanız için bir ad yazın.

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
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın TOPdesk - Secure'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **TOPdesk - Secure**'u seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **TOPdesk - Secure**yazın ve seçin.

    ![TOPdesk - Uygulamalar listesindeki güvenli bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-topdesk---secure-test-user"></a>TOPdesk oluşturma - Güvenli test kullanıcısı

Azure AD kullanıcılarının TOPdesk - Secure'a giriş yapabilmeleri için, TOPdesk - Secure'da oturum açmaları gerekir.  
TOPdesk durumunda - Güvenli, sağlama manuel bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **TOPdesk - Secure** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **TOPdesk \> Yeni \> Destek \> Dosyaları Operatörü'nü**tıklatın.

    ![Işleç](./media/topdesk-secure-tutorial/ic790610.png "İşleç")

3. Yeni **Operatör** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Yeni Operatör](./media/topdesk-secure-tutorial/ic790611.png "Yeni İşleç")

    a. **Genel** sekmesini tıklatın.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının Soyadı yazın.

    c. **Konum** bölümündeki hesap için bir **Site** seçin.

    d. **TOPdesk Giriş** bölümünün **Giriş Adı** metin kutusuna, kullanıcınız için bir giriş adı yazın.

    e. **Kaydet**'e tıklayın.

> [!NOTE]
> Diğer TOPdesk -Güvenli kullanıcı hesabı oluşturma araçlarını veya TOPdesk tarafından sağlanan API'leri kullanabilirsiniz - Azure AD kullanıcı hesaplarını sağlamak için güvenli.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki GÜVENLI döşeme olan TOPdesk 'i tıklattığınızda, SSO'yu kurduğunuz TOPdesk - Secure'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

