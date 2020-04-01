---
title: 'Öğretici: HighGear ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve HighGear arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 55dcd2fb-96b7-46ec-9e69-eee71c535773
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed06586435315935566ca0b1519b182d4fc47d39
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159043"
---
# <a name="tutorial-azure-active-directory-integration-with-highgear"></a>Öğretici: HighGear ile Azure Active Directory entegrasyonu

Bu eğitimde, HighGear'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğrenebilirsiniz.
HighGear'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* HighGear erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla HighGear'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini HighGear ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Kurumsal veya Sınırsız lisansa sahip bir HighGear sistemi

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, bir test ortamında Azure AD tek oturum açma işlemlerini yapılandırmayı ve test etmeyi öğrenebilirsiniz.

* **HighGear, SP ve IdP** tarafından başlatılan SSO'ları destekliyor

## <a name="adding-highgear-from-the-gallery"></a>Galeriden HighGear ekleme

HighGear'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize HighGear eklemeniz gerekir.

**Galeriden HighGear eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Etkin Dizin** simgesini tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **HighGear**yazın, sonuç panelinden **HighGear'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde HighGear](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Azure AD tek oturum açma işlemini HighGear sisteminizle nasıl yapılandırabileceğinizi ve test edebilirsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile HighGear sisteminizdeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumunu HighGear sisteminizle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. HighGear uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[HighGear Tek Oturum Açma'yı yapılandırın.](#configure-highgear-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[HighGear test kullanıcısını oluşturun](#create-highgear-test-user)** - HighGear'da Britta Simon'ın, kullanıcının Azure REKLAM gösterimine bağlı bir muadili olması için. 
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı nasıl etkinleştirebileceğinizi öğrenebilirsiniz.

Azure AD oturumunu HighGear sisteminizle yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **HighGear** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum** Açma'da, **Temel SAML Yapılandırmailetişim** kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![HighGear Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, HighGear sisteminizdeki Tek Oturum Açma Ayarları sayfasındaki **Servis Sağlayıcı Varlık Kimliği** alanının değerini yapıştırın.

    ![Servis Sağlayıcı Varlık Kimliği alanı](media/highgear-tutorial/service-provider-entity-id-field.png)
    
    > [!NOTE]
    > Tek Oturum Açma Ayarları sayfasına erişmek için HighGear sisteminizde oturum açmanız gerekir. Oturum açtıktan sonra Farenizi HighGear'da Yönetim sekmesinin üzerinde hareket ettirin ve Tek Oturum Açma Ayarları menü öğesini tıklatın.
    
    ![Tek Oturum Açma Ayarları menü öğesi](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

    b. **Yanıtla URL** metin kutusuna, HighGear sisteminizdeki Tek Oturum Açma Ayarları sayfasından **Assertion Consumer Service (ACS) URL** değerini yapıştırın.

    ![İddia Tüketici Hizmeti (ACS) URL alanı](media/highgear-tutorial/assertion-consumer-service-url-field.png)

    c. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

     ![HighGear Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

     Oturum **Açma URL** metin kutusuna, HighGear sisteminizdeki Tek Oturum Açma Ayarları sayfasındaki **Servis Sağlayıcı Varlık Kimliği** alanının değerini yapıştırın. (Bu Varlık Kimliği aynı zamanda SP tarafından başlatılan oturum açma için kullanılacak Olan HighGear sisteminin temel URL'sidir.)

    ![Servis Sağlayıcı Varlık Kimliği alanı](media/highgear-tutorial/service-provider-entity-id-field.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri HighGear sisteminizdeki **Tek Oturum Açma Ayarları** sayfasından gerçek Tanımlayıcı, Yanıt URL'si ve Oturum Açma URL'si ile güncelleştirin. Yardıma ihtiyacınız varsa, lütfen [HighGear Destek Ekibi](mailto:support@highgear.com)ile irtibata geçin.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Sertifikayı indirmek **(Base64)** ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın. Tek Oturum Açma yapılandırmasının sonraki bir adımında ihtiyacınız olacak.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **HighGear'ı ayarla** bölümünde, aşağıdaki URL'lerin konumuna dikkat edin.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Giriş URL'si. Aşağıdaki **HighGear Tek Oturum Aç'ı Yapılandırın** altında Adım #2'da bu değere ihtiyacınız olacaktır.

    b. Azure AD Tanımlayıcısı. Aşağıdaki **HighGear Tek Oturum Aç'ı Yapılandırın** altında Adım #3'da bu değere ihtiyacınız olacaktır.

    c. Giriş URL'si. Aşağıdaki **HighGear Tek Oturum Aç'ı Yapılandırın** altında Adım #4'da bu değere ihtiyacınız olacaktır.

### <a name="configure-highgear-single-sign-on"></a>HighGear Tek İşaretli Yapıla

Tek Oturum Açma için HighGear'ı yapılandırmak için lütfen HighGear sisteminize giriş yapın. Oturum açtıktan sonra Farenizi HighGear'da Yönetim sekmesinin üzerinde hareket ettirin ve Tek Oturum Açma Ayarları menü öğesini tıklatın.

![Tek Oturum Açma Ayarları menü öğesi](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

1. Kimlik **Sağlayıcı**Adı'nda, Giriş sayfasında HighGear'ın Tek Oturum Açma düğmesinde görünecek kısa bir açıklama yazın. Örneğin: Azure AD

2. HighGear'daki **Tek Oturum Açma (SSO) URL** alanına, Azure'da **HighGear'ı ayarla** bölümündeki **Giriş URL** alanından değeri yapıştırın.

3. HighGear'daki **Kimlik Sağlayıcı Varlık Kimliği** alanına, Azure'da **HighGear'ı ayarla** bölümündeki **Azure AD Tanımlayıcı** alanından değeri yapıştırın.

4. HighGear'daki **Tek Oturum Açma (SLO) URL** alanına, Azure'da **HighGear'ı ayarla** bölümündeki **Logout URL** alanından değeri yapıştırın.

5. Azure'daki **SAML İmza Sertifikası** bölümünden indirdiğiniz sertifikayı açmak için Not Defteri'ni kullanın. **Sertifika (Base64)** biçimini indirmiş olmalısınız. Sertifikanın içeriğini Notepad'den kopyalayın ve HighGear'daki **Kimlik Sağlayıcı Sertifikası** alanına yapıştırın.

6. HighGear Sertifikanızı istemesi için [HighGear Destek Ekibine](mailto:support@highgear.com) e-posta gönder. **HighGear Sertifikası** ve **HighGear Sertifika Şifresi** alanlarını doldurmak için onlardan aldığınız talimatları uygulayın.

7. HighGear Tek Oturum Açma yapılandırmanızı kaydetmek için **Kaydet** düğmesini tıklatın.

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

    c. **Parolayı Göster** onay kutusunu seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın HighGear'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **HighGear'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **HighGear'ı**seçin.

    ![Uygulamalar listesindeki HighGear bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-highgear-test-user"></a>HighGear test kullanıcısı oluşturun

Tek Oturum Açma yapılandırmanızı test etmek için bir HighGear test kullanıcısı oluşturmak için lütfen HighGear sisteminize giriş yapın.

1. Yeni **Kişi Oluştur** düğmesini tıklatın.

    ![Yeni Kişi Oluştur düğmesi](media/highgear-tutorial/create-new-contact-button.png)

    Oluşturmak istediğiniz kişi türünü seçmenize olanak tanıyan bir menü görüntülenir.

2. Bir HighGear kullanıcısı oluşturmak için **Bireysel** menü öğesini tıklatın.

    Yeni kullanıcının bilgilerini girebilmeniz için sağda bir bölme açılır.  
    ![Yeni İletişim formu](media/highgear-tutorial/new-contact-form.png)

3. **Ad** alanına, ilgili kişi için bir ad yazın. Örneğin: Britta Simon

4. Diğer **Seçenekler** menüsüne tıklayın ve **Hesap Bilgileri** menü öğesini seçin.

    ![Hesap Bilgileri menü öğesini tıklatma](media/highgear-tutorial/account-info-menu-item.png)

5. Can **Giriş** alanını Evet olarak ayarlayın.

    **Tek Oturum Açma** alanını etkinleştir mesuliyesi otomatik olarak Evet olarak da ayarlanır.

6. Tek **Oturum Açma Kullanıcı Kimliği** alanına, kullanıcının kimliğini yazın. Örneğin, BrittaSimon@contoso.com

    Hesap Bilgileri bölümü şimdi şuna benzer olmalıdır:  
    ![Bitmiş Hesap Bilgileri bölümü](media/highgear-tutorial/finished-account-info-section.png)

7. İlgili kişi kaydetmek için bölmenin altındaki **Kaydet** düğmesini tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki HighGear döşemesini tıklattığınızda, SSO'yu kurduğunuz HighGear'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

