---
title: 'Öğretici: Facebook tarafından İşyeri ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve İşyeri arasında Facebook tarafından tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de84f2aee5f59d14ab70cb1687968643c4cdb31e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79136389"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workplace-by-facebook"></a>Öğretici: Facebook tarafından İşyeri ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Facebook tarafından İşyeri'ni Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Facebook'a göre İşyeri'ni Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Facebook tarafından İşyeri erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Facebook tarafından İşyerine otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Facebook tarafından işyeri tek oturum açma (SSO) aboneliği etkin.

> [!NOTE]
> Facebook'un iki ürünü vardır: İşyeri Standardı (ücretsiz) ve İşyeri Premium (ücretli). Herhangi bir İşyeri Premium kiracı maliyet veya lisans gerekli başka etkileri ile SCIM ve SSO entegrasyonu yapılandırabilirsiniz. SSO ve SCIM, İşyeri Standardı örneklerinde kullanılamaz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Facebook tarafından İşyeri **SP** başlatılan SSO destekler
* Facebook tarafından İşyeri **tam zamanında sağlamayı** destekler
* Facebook tarafından İşyeri ** [otomatik Kullanıcı Sağlama](workplacebyfacebook-provisioning-tutorial.md) destekler**
* Facebook Mobile uygulaması tarafından işyeri artık SSO'yu etkinleştirmek için Azure AD ile yapılandırılabilir. Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.
* İşyeri'ni Facebook tarafından yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Galeriden Facebook'a İşyeri Ekleme

İşyeri'nin Facebook tarafından Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Facebook tarafından İşyeri'ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Facebook tarafından İşyeri** yazın.
1. Sonuç panelinden **Facebook'a göre İşyeri'ni** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-workplace-by-facebook"></a>Azure AD SSO'nun Facebook'a göre İşyeri için yapılandırılın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Facebook tarafından İşyeri ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Facebook tarafından İşyeri'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Facebook ile İşYeri ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[İşyeri'yi Facebook SSO tarafından yapılandırın.](#configure-workplace-by-facebook-sso)**
    * **[Facebook test kullanıcısı tarafından İşyeri oluşturun](#create-workplace-by-facebook-test-user)** - Kullanıcının Azure AD gösterimine bağlı Facebook'un Işyerinde B.Simon'ın bir örneğine sahip olması için.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Facebook tarafından İşyeri** uygulaması yla açılan [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<instancename>.facebook.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.facebook.com/company/<instanceID>`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.facebook.com/company/<instanceID>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. İşyeri topluluğunuz için doğru değerler için İşyeri Şirket Panosunun Kimlik Doğrulama sayfasına bakın, bu daha sonra öğreticide açıklanır.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Facebook **tarafından İşyeri Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Facebook tarafından İşyeri'ne erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Facebook **tarafından İşyeri'ni**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-workplace-by-facebook-sso"></a>Facebook SSO tarafından İşyeri Yapılandırma

1. Facebook tarafından İşyeri içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Facebook tarafından İşyeri'ni Kur'a** tıklayın ve facebook uygulaması yla Sizi İşyeri'ne yönlendirecektir. Buradan, Facebook tarafından İşyeri'nde oturum açmanın yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Facebook tarafından İşyeri'ni el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Facebook şirket sitesi tarafından Yönetici olarak İşyerinizde oturum açın ve aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > SAML kimlik doğrulama işleminin bir parçası olarak, İşyeri parametreleri Azure AD'ye aktarmak için 2,5 kilobayta kadar sorgu dizelerini kullanabilir.

1. Soldaki gezinti panelinde Güvenlik > **Kimlik Doğrulama** **sekmesine**gidin.

    ![Yönetici Paneli](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure01.png)

    a. Tek **oturum açma (SSO)** seçeneğini kontrol edin.
    
    b. **+Ekle yeni SSO Sağlayıcısı'na**tıklayın.
    > [!NOTE]
    > Parola giriş onay kutusunu da kontrol ettiğinizden emin olun. Yöneticilerin kendilerini kilitlenemelerini önlemek için sertifika rollover yaparken giriş için bu seçeneği gerektirebilir.

1. **Kimlik Doğrulama** sekmesi altında, Tek Oturum **Açma (SSO) seçeneğini** belirleyin ve aşağıdaki adımları gerçekleştirin:

    ![Kimlik Doğrulama Sekmesi](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure02.png)

    a. **SSO Sağlayıcısı adına**Azureadsso gibi SSO örnek adını girin.

    b. **SAML URL** textbox'ına, Azure portalından kopyaladığınız **Giriş URL'sinin**değerini yapıştırın.

    c. **SAML Veren URL** textbox'ına, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    d. Azure portalından indirilen not defterinde **base-64 kodlu sertifikanızı** açın, içeriğini panonuza kopyalayın ve **ardından SAML Sertifikası** metin kutusuna yapıştırın.

    e. Örnek için **Hedef Kitle URL'sini** kopyalayın ve Azure portalındaki Temel **SAML Yapılandırma** bölümündeki Tanımlayıcı **(Entity ID)** textbox'a yapıştırın.

    f. **Örneğiniz** için Alıcı URL'sini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma** bölümünde URL textbox'a **yapıştırın.**

    g. Acs **(Assertion Consumer Service) URL'sini** örneğiniz için kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** **bölümündeki YanıtLA URL** metin kutusuna yapıştırın.

    h. Bölümün altına gidin ve Test **SSO** düğmesini tıklatın. Bu, azure AD giriş sayfası sunulan bir açılır pencere yle sonuçlanır. Kimlik bilgilerinizi kimlik doğrulamaiçin normal olarak girin.

    **Sorun giderme:** Azure AD'den iade edilen e-posta adresinin, oturum açtığınız İşyeri hesabıyla aynı olduğundan emin olun.

    i. Test başarıyla tamamlandıktan sonra sayfanın altına gidin ve **Kaydet** düğmesini tıklatın.

    j. İşyeri'ni kullanan tüm kullanıcılara artık kimlik doğrulaması için Azure AD giriş sayfası sunulacaktır.

1. **SAML Giriş Yönlendirme (isteğe bağlı)** -

    Azure AD'nin oturum açma sayfasını işaret etmek için kullanılabilecek bir SAML Giriş Url'sini isteğe bağlı olarak yapılandırmayı seçebilirsiniz. Bu ayar etkinleştirildiğinde ve yapılandırıldığında, kullanıcı artık İşyeri oturum açma sayfasına yönlendirilecektir. Bunun yerine, kullanıcı SAML Logout Yönlendirme ayarına eklenen url'ye yönlendirilir.

### <a name="configuring-reauthentication-frequency"></a>Yeniden kimlik doğrulama sıklığını yapılandırma

İşyeri'ni her gün, üç gün, hafta, iki hafta, ay veya hiç saml denetimi için istinat edecek şekilde yapılandırabilirsiniz.

> [!NOTE]
> Mobil uygulamalardaki SAML denetimi için minimum değer bir hafta olarak ayarlanır.

Ayrıca düğmeyi kullanan tüm kullanıcılar için bir SAML sıfırlama zorlayabilir: Şimdi tüm kullanıcılar için SAML kimlik doğrulaması gerektirir.

### <a name="create-workplace-by-facebook-test-user"></a>Facebook test kullanıcısı tarafından İşyeri Oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Facebook tarafından Workplace oluşturulur. Facebook tarafından İşyeri, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler.

Bu bölümde sizin için herhangi bir işlem yoktur. Facebook tarafından İşyeri'nde bir kullanıcı yoksa, Facebook tarafından Workplace'e erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur.

>[!Note]
>Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Facebook Müşteri destek ekibi tarafından İşyeri](https://workplace.fb.com/faq/) ile Iletişim kurun

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Facebook döşemesi ile İşyeri'ni tıklattığınızda, SSO'yu kurduğunuz Facebook tarafından Otomatik olarak İşyeri'nde oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="test-sso-for-workplace-by-facebook-mobile"></a>Facebook tarafından İşyeri için Test SSO (mobil)

1. Facebook Mobile uygulaması ile İşyeri'ni açın. Oturum açma **sayfasında, Gİrİş**YAP'A tıklayın.

    ![Oturum açma](./media/workplacebyfacebook-tutorial/test05.png)

2. İşletmenizin e-postalarını girin ve **DEVAM'ı**tıklatın.

    ![E-posta](./media/workplacebyfacebook-tutorial/test02.png)

3. **SADECE Bİr KEZ TIKLAYIN**.

    ![Bir kez](./media/workplacebyfacebook-tutorial/test04.png)

4. **İzin ver**’e tıklayın.

    ![İzin Ver](./media/workplacebyfacebook-tutorial/test03.png)

5. Son olarak başarılı oturum açmadan sonra uygulama ana sayfası görüntülenir.    

    ![Giriş sayfası](./media/workplacebyfacebook-tutorial/test01.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı Sağlama'yı Yapılandır](workplacebyfacebook-provisioning-tutorial.md)

- [Azure AD ile Facebook tarafından İşyeri'ni deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
