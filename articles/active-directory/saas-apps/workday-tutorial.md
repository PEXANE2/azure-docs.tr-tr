---
title: 'Öğretici: İş Günü ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve İş Günü arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2f39b6f58b250d68a3b2ce962f158c7df36d812
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77046613"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday"></a>Öğretici: İş Günü ile Azure Active Directory tek oturum açma (SSO) tümleştirmesi

Bu eğitimde, Çalışma Günü'nü Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. İş Günü'nü Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Çalışma Günü'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Çalışma Günü'nde otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* İş günü tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* İş **günü, SP** tarafından başlatılan SSO'ya destek veriyor.

* İş Gününü yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-workday-from-the-gallery"></a>Galeriden İş Günü Ekleme

İş Günü'nün Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize İş Günü eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **İş Günü** yazın.
1. Sonuç panelinden **İş Günü'nü** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-workday"></a>İş Günü için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak İş Günü ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile İş Günü'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu İş Günü ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[Çalışma Gününü yapılandırın.](#configure-workday)**
    1. **[İş Günü'nde](#create-workday-test-user)** B.Simon'ın, kullanıcının Azure REKLAM gösterimine bağlı bir muadili olması için İş Günü testi kullanıcısı oluşturun.
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **İş Günü** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://impl.workday.com/<tenant>/login-saml2.flex`

    b. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`http://www.workday.com`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Yanıt URL'nizin bir alt etki alanı olmalıdır: www, wd2, wd3, wd3-impl, wd5, wd5-impl).
    > Gibi `http://www.myworkday.com` bir şey `http://myworkday.com` kullanarak çalışır ama yok. Bu değerleri almak için [İş Günü İstemci destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. İş günü uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. İş günü uygulaması **nameidentifier** **user.mail**, **UPN**, vb ile eşlenen bekliyor, bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşleme değiştirmek gerekir.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Burada varsayılan olarak UPN (user.userprincipalname) ile Ad Kimliği eşledik. SSO'nun başarılı bir şekilde çalışması için Ad Kimliğini İş Günü hesabınızdaki gerçek Kullanıcı Kimliği (e-postanız, UPN'iniz vb.) ile eşlemeniz gerekir.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **İmzalama** seçeneklerini gereksiniminize göre değiştirmek için **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-certificate.png) 

    ![image](./media/workday-tutorial/signing-option.png)

    a. **İmzalama Seçeneği**için **IMZA SAML yanıtını ve iddiasını** seçin.

    b. **Kaydet'i** tıklatın

1. İş **Günü Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın İş Günü'ne erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **İş Günü'nü**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-workday"></a>İş Gününü Yapılandırma

1. Farklı bir web tarayıcısı penceresinde, İş Günü şirket sitenizde yönetici olarak oturum açın.

2. Ana sayfanın sol üst tarafındaki **Kiracı Kurulumunu Düzenle adı** ile **Arama kutusu** aramasında güvenlik.

    ![Kiracı Güvenliğini Edin](./media/workday-tutorial/IC782925.png "Kiracı Güvenliğini Edin")

3. Yönlendirme **URL'leri** bölümünde aşağıdaki adımları gerçekleştirin:

    ![YÖNLENDIRME URL'leri](./media/workday-tutorial/IC7829581.png "YÖNLENDIRME URL'leri")

    a. **Satır Ekle'yi**tıklatın.

    b. Giriş **Giriş Yönlendirme URL'sinde**, **Timeout Redirect URL** ve **Mobil Yönlendirme URL** textbox'ına, Azure portalının İş **Günü Ayarla** bölümünden kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    c. Giriş **Çıkış Yönlendirme URL** metin kutusuna, Azure portalının İş Günü **Ayarla** bölümünden kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    d. **Ortamlar için Kullanılan** textbox'ta ortam adını seçin.  

   > [!NOTE]
   > Çevre özniteliğinin değeri kiracı URL'sinin değerine bağlıdır:  
   > -İş Günü kiracı URL'sinin etki alanı adı impl ile başlarsa örneğin: * https://www.myworkday.com/"kiracı"/login-saml2.htmld),* **Çevre** özniteliği Uygulama olarak ayarlanmalıdır.  
   > -Alan adı başka bir şeyle başlarsa, eşleşen **Ortam** değerini almak için [Workday Client destek ekibine](https://www.workday.com/en-us/partners-services/services/support.html) başvurmanız gerekir.

4. **SAML Kurulumu** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Kurulumu](./media/workday-tutorial/IC782926.png "SAML Kurulumu")

    a.  **SAML Kimlik Doğrulamasını Etkinleştir'i**seçin.

    b.  **Satır Ekle'yi**tıklatın.

5. **SAML Kimlik Sağlayıcıları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Kimlik Sağlayıcıları](./media/workday-tutorial/IC7829271.png "SAML Kimlik Sağlayıcıları")

    a. Kimlik **Sağlayıcı Adı** metin kutusunda bir sağlayıcı adı yazın (örneğin: *SPInitiatedSSO).*

    b. Azure portalında, **İş Günü Ayarla** bölümünde, **Azure AD Tanımlayıcı** değerini kopyalayın ve ardından **Veren** metin kutusuna yapıştırın.

    ![SAML Kimlik Sağlayıcıları](./media/workday-tutorial/IC7829272.png "SAML Kimlik Sağlayıcıları")

    c. Azure portalında, **İş Günü'nü ayarla** bölümünde, **Oturum Açma URL** değerini kopyalayın ve ardından Oturum Açma Yanıtı **URL** metin kutusuna yapıştırın.

    d. Azure portalında, **İş Günü'nü ayarla** **bölümünde, Giriş URL** değerini kopyalayın ve ardından **IdP SSO Hizmeti URL** textbox'ına yapıştırın.

    e. **Ortamlar için Kullanılan** textbox'ta ortam adını seçin.

    f. **Kimlik Sağlayıcı Ortak Anahtar Sertifikası'nı**tıklatın ve sonra **Oluştur'u**tıklatın.

    ![Oluştur](./media/workday-tutorial/IC782928.png "Oluşturma")

    g. **x509 Ortak Anahtar Oluştur'u**tıklatın.

    ![Oluştur](./media/workday-tutorial/IC782929.png "Oluşturma")

6. Görünüm **x509 Ortak Anahtar** bölümünde aşağıdaki adımları gerçekleştirin:

    ![view x509 Ortak Anahtar](./media/workday-tutorial/IC782930.png "view x509 Ortak Anahtar")

    a. **Ad** metin kutusunda, sertifikanız için bir ad yazın (örneğin: *PPE\_SP).*

    b. Geçerli **Gönderen** metin kutusuna, geçerliyi sertifikanızın öznitelik değerinden yazın.

    c.  **Geçerliiçin** textbox'ına, sertifikanızın geçerli değerini yazın.

    > [!NOTE]
    > İndirilen sertifikadan geçerliyi, tarihi de çift tıklayarak alabilirsiniz.  Tarihler **Ayrıntılar** sekmesi altında listelenir.
    >
    >

    d.  Taban-64 kodlanmış sertifikanızı not defterinde açın ve içeriğini kopyalayın.

    e.  **Sertifika** metin kutusuna panonuzun içeriğini yapıştırın.

    f.  **Tamam**'a tıklayın.

7. Aşağıdaki adımları uygulayın:

    ![SSO yapılandırması](./media/workday-tutorial/WorkdaySSOConfiguratio.png "SSO yapılandırması")

    a.  Servis **Sağlayıcı Kimliği** metin kutusuna, **http://www.workday.com**yazın.

    b. **SP tarafından başlatılan Kimlik Doğrulama İsteğini**Söndürmeyin'i seçin.

    c. **Kimlik Doğrulama İstek İmza Yöntemi**olarak, **SHA256'yı**seçin.

    ![Kimlik Doğrulama İstek İmza Yöntemi](./media/workday-tutorial/WorkdaySSOConfiguration.png "Kimlik Doğrulama İstek İmza Yöntemi")

    d. **Tamam**'a tıklayın.

    ![Tamam](./media/workday-tutorial/IC782933.png "Tamam")

    > [!NOTE]
    > Lütfen tek bir oturum açmayı doğru ayarladığınızdan emin olun. Yanlış kurulumla tek oturum açmayı etkinleştirmeniz durumunda, başvuruya kimlik bilgilerinizle giremeyebilir ve kilitlenebilirsiniz. Bu durumda, Workday, kullanıcıların normal kullanıcı adlarını ve parolalarını kullanarak aşağıdaki biçimde oturum açabilecekleri bir yedekleme giriş url'si sağlar:[İş GÜNÜ URL'niz]/login.flex?redirect=n

### <a name="create-workday-test-user"></a>İş Günü testi kullanıcısı oluşturma

Bu bölümde, İş Günü'nde B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları İş Günü platformuna eklemek için [İş Günü İstemci destek ekibiyle](https://www.workday.com/partners-services/services/support.html) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki İş Günü döşemesini seçtiğinizde, SSO'yu kurduğunuz Çalışma Günü'nde otomatik olarak oturum açmış olmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile İş Günü'nü deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle İş Günü nasıl korunur?](https://docs.microsoft.com/cloud-app-security/protect-workday)