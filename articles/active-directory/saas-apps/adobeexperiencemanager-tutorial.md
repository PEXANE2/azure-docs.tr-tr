---
title: 'Öğretici: Adobe Experience Manager ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Adobe Experience Manager arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f39751f40b32c5da24e13d75d2607d7da0a57ad3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73154107"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Öğretici: Adobe Experience Manager ile Azure Active Directory entegrasyonu

Bu eğitimde, Adobe Experience Manager'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Adobe Experience Manager'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Adobe Experience Manager erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Adobe Experience Manager (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Adobe Experience Manager ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Adobe Experience Manager tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Adobe Experience **Manager, SP ve IDP** tarafından başlatılan SSO'nun desteklerini

* Adobe Experience **Manager, Just In Time** kullanıcı sağlamayı destekler

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Galeriden Adobe Experience Manager ekleme

Adobe Experience Manager'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Adobe Experience Manager'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Adobe Experience Manager eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda Adobe **Experience Manager**yazın, sonuç panelinden Adobe Experience **Manager'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Adobe Experience Manager](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre [Uygulama adı] ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile [Uygulama adı]'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Adobe Experience Manager Single Sign-On](#configure-adobe-experience-manager-single-sign-on)** 'u uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Adobe Experience Manager test kullanıcısını oluşturun](#create-adobe-experience-manager-test-user)** - Adobe Experience Manager'da kullanıcının Azure AD gösterimine bağlı britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Adobe Experience Manager** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Adobe Experience Manager Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, AEM sunucunuzda tanımladığınız benzersiz bir değer yazın.

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<AEM Server Url>/saml_login`

    > [!NOTE]
    > YanıtURL değeri gerçek değil. YanıtLA URL değerini gerçek yanıt URL'si ile güncelleştirin. Bu değeri elde etmek için, bu değeri almak için [Adobe Experience Manager Client destek ekibine](https://helpx.adobe.com/support/experience-manager.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Adobe Experience Manager Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna Adobe Experience Manager sunucu URL'nizi yazın.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Adobe **Experience Manager'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-adobe-experience-manager-single-sign-on"></a>Adobe Experience Manager Tek Oturum Açma'yı Yapılandır

1. Başka bir tarayıcı penceresinde Adobe **Experience Manager** yönetici portalını açın.

2. **Ayarlar** > **Güvenlik** > **Kullanıcıları'nı**seçin.

    ![Tek oturum açma kaydet düğmesini yapılandırma](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

3. **Yönetici'yi** veya diğer ilgili kullanıcıyı seçin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

4. **Hesap ayarlarını** > seçin**TrustStore'u yönetin.**

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

5. **CER dosyasından Sertifika Ekle dosyasının**altında **Sertifika Dosyalarını Seçin'i**tıklatın. Azure portalından indirdiğiniz sertifika dosyasına göz atın ve seçin.

    ![Tek oturum açma kaydet düğmesini yapılandırma](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

6. Sertifika TrustStore'a eklenir. Sertifikanın diğer adını not edin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

7. **Kullanıcılar** sayfasında kimlik **doğrulama hizmeti**seçin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

8. **Hesap ayarlarını** > seçin**KeyStore oluştur/yönet.** Parola sağlayarak KeyStore'u oluşturun.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

9. Yönetici ekranına geri dön. Ardından **Ayarlar** > **İşlemleri** > **Web Konsolu'nun**'u seçin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Bu yapılandırma sayfasını açar.

    ![Tek oturum açma kaydet düğmesini yapılandırma](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

10. **Adobe Granit SAML 2.0 Kimlik Doğrulama Handibu**bul. Ardından **Ekle** simgesini seçin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

11. Bu sayfada aşağıdaki eylemleri alın.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. **Yol** kutusuna girin. **/**

    b. **IDP URL** kutusuna Azure portalından kopyaladığınız **Giriş URL** değerini girin.

    c. **IDP Sertifika Takma Ad kutusuna,** TrustStore'a eklediğiniz **Sertifika Diğer Ad değeri'ni** girin.

    d. Güvenlik **Sağlanan Varlık Kimliği** kutusuna, Azure portalında yapılandırdığınız benzersiz **Azure Reklam Tanımlayıcı** değerini girin.

    e. İddia **Tüketici Hizmeti URL** kutusuna, Azure portalında yapılandırdığınız **Yanıtla URL** değerini girin.

    f. Key **Store** Şifresi kutusuna, KeyStore'da ayarladığınız **Parola'yı** girin.

    g. Kullanıcı **Öznitelik kimliği** kutusuna, **Ad Kimliği'ni** veya durumunuzla alakalı başka bir kullanıcı kimliği girin.

    h. **Otomatik CRX Kullanıcıları**seçin.

    i. Oturum **Açma URL** kutusuna Azure portalından aldığınız benzersiz **Logout URL** değerini girin.

    j. **Kaydet'i**seçin.

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

Bu bölümde, Britta Simon'ın Adobe Experience Manager'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından Adobe **Experience Manager'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Adobe Experience Manager'ı**seçin.

    ![Uygulamalar listesindeki Adobe Experience Manager bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-adobe-experience-manager-test-user"></a>Adobe Experience Manager test kullanıcısı oluşturma

Bu bölümde, Adobe Experience Manager'da Britta Simon adında bir kullanıcı oluşturursunuz. Otomatik Oluşturma **CRX Kullanıcıları** seçeneğini seçtiyseniz, kullanıcılar başarılı kimlik doğrulamadan sonra otomatik olarak oluşturulur.

Kullanıcıları el ile oluşturmak istiyorsanız, kullanıcıları Adobe Experience Manager platformuna eklemek için [Adobe Experience Manager destek ekibiyle](https://helpx.adobe.com/support/experience-manager.html) birlikte çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Adobe Deneyim Yöneticisi döşemesini tıklattığınızda, SSO'yu kurduğunuz Adobe Experience Manager'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
