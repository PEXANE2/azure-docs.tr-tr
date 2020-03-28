---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) google cloud (G Suite) Bağlayıcısı ile entegrasyon | Microsoft Dokümanlar'
description: Azure Active Directory ve Google Cloud (G Suite) Bağlayıcısı arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/14/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b3282dd88b62a6811031e95672638d67702215a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048455"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-google-cloud-g-suite-connector"></a>Öğretici: Google Cloud (G Suite) Bağlayıcısı ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Google Cloud (G Suite) Bağlayıcısını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Google Cloud (G Suite) Bağlayıcısı'nı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Google Cloud (G Suite) Bağlayıcısı'na erişimi olan denetimi kontrol edin.
* Kullanıcılarınızın Azure AD hesaplarıyla Google Cloud (G Suite) Bağlayıcısı'nda otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği.
- Google Cloud (G Suite) Bağlayıcısı tek oturum açma (SSO) aboneliğini etkinleştirildi.
- Bir Google Apps aboneliği veya Google Bulut Platformu aboneliği.

> [!NOTE]
> Bu öğreticideki adımları sınamak için bir üretim ortamı kullanmanızı önermiyoruz. Bu belge, yeni kullanıcı Tek Oturum Açma deneyimi kullanılarak oluşturuldu. Hala eskisini kullanıyorsanız, kurulum farklı görünecektir. G-Suite uygulamasının Tek Oturum Açma ayarlarında yeni deneyimi etkinleştirebilirsiniz. Azure **AD, Kurumsal uygulamalara**gidin, **Google Cloud (G Suite)** Bağlayıcısı'nı seçin, **Tek Oturum Açma'yı** seçin ve ardından **yeni deneyimimizi deneyin'e**tıklayın.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

1. **S: Bu entegrasyon, Azure AD ile Google Cloud Platform SSO entegrasyonunu destekliyor mu?**

    C: Evet. Google Bulut Platformu ve Google Apps aynı kimlik doğrulama platformunu paylaşır. Bu nedenle GCP entegrasyonunu yapmak için SSO'yu Google Apps ile yapılandırmanız gerekir.

2. **S: Chromebook'lar ve diğer Chrome aygıtları Azure AD tek oturum açma ile uyumlu mu?**
  
    C: Evet, kullanıcılar Azure AD kimlik bilgilerini kullanarak Chromebook aygıtlarında oturum açabiliyor. Kullanıcıların neden iki kez kimlik bilgileri istinat edilebileceği hakkında bilgi almak için bu [Google Cloud (G Suite) Bağlayıcı destek makalesine](https://support.google.com/chrome/a/answer/6060880) bakın.

3. **S: Tek oturum açmayı etkinleştiriyorsam, kullanıcılar Google Classroom, GMail, Google Drive, YouTube gibi herhangi bir Google ürününde oturum açabilmek için Azure REKLAM kimlik bilgilerini kullanabilecek ler mi?**

    C: Evet, kuruluşunuz için etkinleştirmeyi veya devre dışı bırakmayı seçtiğiniz [Google Cloud (G Suite)](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) Bağlayıcısı'na bağlı olarak.

4. **S: Google Cloud (G Suite) Bağlayıcı kullanıcılarımın yalnızca bir alt kümesi için tek oturum açma yı etkinleştirebilir miyim?**

    C: Hayır, tek bir oturum açma, tüm Google Cloud (G Suite) Bağlayıcısı kullanıcılarınızın Azure AD kimlik bilgileriyle kimlik doğrulaması için hemen geçerli olmasını gerektirir. Google Cloud (G Suite) Bağlayıcısı birden çok kimlik sağlayıcısına sahip olmayı desteklemediği için, Google Cloud (G Suite) Bağlayıcı ortamınızın kimlik sağlayıcısı Azure AD veya Google olabilir , ancak ikisi aynı anda olmayabilir.

5. **S: Bir kullanıcı Windows üzerinden oturum açtıysa, otomatik olarak Google Cloud (G Suite) Bağlayıcısı'na şifre istenmeden kimlik doğrulaması mı yapılır?**

    C: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Azure Active [Directory Join](../device-management-introduction.md)aracılığıyla Windows 10 cihazlarında oturum açabilir. Alternatif olarak, kullanıcılar, [Etkin Dizin Federasyonu Hizmetleri (AD FS)](../hybrid/plan-connect-user-signin.md) dağıtımı aracılığıyla Azure AD'de tek oturum açma özelliğine etkinleştirilmiş şirket içi Active Directory'ye etki alanı yla birleştirilmiş Windows aygıtlarında oturum açabilir. Her iki seçenek de, Azure AD ve Google Cloud (G Suite) Bağlayıcısı arasında tek oturum açma yı etkinleştirmek için aşağıdaki öğreticideki adımları gerçekleştirmenizi gerektirir.

6. **S: "Geçersiz e-posta" hata iletisi aldığında ne yapmalıyım?**

    C: Bu kurulum için, kullanıcıların oturum açabilmesi için e-posta özniteliği gereklidir. Bu öznitelik el ile ayarlanamıyor.

    E-posta özniteliği, geçerli bir Exchange lisansına sahip herhangi bir kullanıcı için otomatik olarak doldurulur. Kullanıcı e-posta etkin değilse, uygulama nın erişim sağlamak için bu özniteliği alması gerektiğinden bu hata alınır.

    Yönetici hesabıyla portal.office.com'a gidebilir, ardından Yönetici merkezi, faturalandırma, abonelikler, Office 365 Aboneliğinizi seçebilir ve kullanıcılara atama kullanabilirsiniz, aboneliklerini kontrol etmek istediğiniz kullanıcıları seçebilir ve sağ bölmeye tıklayabilirsiniz. lisansları edin.

    O365 lisansı atandıktan sonra uygulanması birkaç dakika sürebilir. Bundan sonra, user.mail özniteliği otomatik olarak doldurulur ve sorun çözülmelidir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Google Cloud (G Suite) Konektörü **SP** tarafından başlatılan SSO'ya destek veriyor

* Google Cloud (G Suite) Bağlayıcısı [ **Otomatik** kullanıcı sağlama yı destekler](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)
* Google Cloud (G Suite) Bağlayıcısı'nı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-google-cloud-g-suite-connector-from-the-gallery"></a>Galeriden Google Cloud (G Suite) Bağlayıcısı Ekleme

Google Cloud (G Suite) Bağlayıcısı'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Google Cloud (G Suite) Bağlayıcısı'nı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Google Bulut (G Suite) Bağlayıcısı** yazın.
1. Sonuç panelinden **Google Cloud (G Suite)** Bağlayıcısı'nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-google-cloud-g-suite-connector"></a>Google Cloud (G Suite) Bağlayıcısı için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Google Cloud (G Suite) Bağlayıcısı ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Google Cloud (G Suite) Bağlayıcısı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Google Cloud (G Suite) Bağlayıcısı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Google Cloud (G Suite) BağlayıcıSI SSO'yu yapılandırın.](#configure-google-cloud-g-suite-connector-sso)**
    1. **[Google Cloud (G Suite) Bağlayıcı test kullanıcısını oluşturun](#create-google-cloud-g-suite-connector-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Google Cloud (G Suite) Bağlayıcısı'nda B.Simon'Un bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Google Cloud (G Suite) Bağlayıcı** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Gmail** için yapılandırma yapmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın: 

    | |
    |--|
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |


1. Temel **SAML Yapılandırması** bölümünde, **Google Bulut Platformu** için yapılandırma yapmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |
    
    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın: 
    
    | |
    |--|
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve Tanımlayıcıile güncelleştirin. Google Cloud (G Suite) Bağlayıcısı, Tek Oturum Açma yapılandırması üzerinde Entity ID/Tanımlayıcı değeri sağlamaz, bu nedenle etki alanına özgü `google.com` **veren** seçeneğinin işaretini aldığınızda Tanımlayıcı değeri olacaktır. Etki alanına **özel veren** seçeneğini kontrol `google.com/a/<yourdomainname.com>`ederseniz, bu. Etki alanına özgü **veren seçeneğini** kontrol etmek/denetlemeyi yapmak için, daha sonra öğreticide açıklanan Google Cloud **(G Suite) Bağlayıcısı SSO** bölümüne gitmeniz gerekir. Daha fazla bilgi için [Google Cloud (G Suite) Bağlayıcı İstemci destek ekibine](https://www.google.com/contact/)başvurun.

1. Google Cloud (G Suite) Bağlayıcısı uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü bunun için bir örnek gösterir. Benzersiz Kullanıcı **Tanımlayıcısı'nın** varsayılan değeri **user.userprincipalname'dir** ancak Google Cloud (G Suite) Bağlayıcısı bunun kullanıcının e-posta adresiyle eşlenmesini bekler. Bunun için listeden **user.mail** özniteliğini kullanabilir veya kuruluş yapılandırmanıza göre uygun öznitelik değerini kullanabilirsiniz.

    ![image](common/default-attributes.png)


1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Google **Cloud (G Suite) Bağlayıcısı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Google Cloud (G Suite) Bağlayıcısı'na erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Google Cloud (G Suite) Bağlayıcısı'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-google-cloud-g-suite-connector-sso"></a>Google Cloud (G Suite) Bağlayıcısı SSO'ya yapıla

1. Tarayıcınızda yeni bir sekme açın ve yönetici hesabınızı kullanarak [Google Cloud (G Suite) Bağlayıcı Yönetici Konsolu'nda](https://admin.google.com/) oturum açın.

2. **Güvenlik'i**tıklatın. Bağlantıyı görmüyorsanız, ekranın altındaki **Daha Fazla Denetimler** menüsünün altında gizlenmiş olabilir.

    ![Güvenlik'i tıklatın.][10]

3. **Güvenlik** sayfasında, **tek oturum açma (SSO) ayarla'yı tıklatın.**

    ![SSO'ya tıklayın.][11]

4. Aşağıdaki yapılandırma değişikliklerini gerçekleştirin:

    ![SSO'nun yapılandırılsın][12]

    a. **Üçüncü taraf kimlik sağlayıcısıyla Kurulum SSO'su'nun'u**seçin.

    b. Google Cloud (G Suite) Bağlayıcısı'ndaki **Oturum Açma sayfası URL** alanına, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.

    c. Google Cloud (G Suite) Bağlayıcısı'ndaki **Oturum Açma sayfası URL** alanına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. Google Cloud (G Suite) Bağlayıcısı'ndaki **parola URL'sini değiştir** alanına, Azure portalından kopyalamış olduğunuz Parola **URL'sini Değiştir** değerini yapıştırın.

    e. Doğrulama **sertifikası**için Google Cloud (G Suite) Bağlayıcısı'nda, Azure portalından indirdiğiniz sertifikayı yükleyin.

    f. Azure AD'de yukarıdaki **Temel SAML Yapılandırması** bölümünde belirtilen nota göre **etki alanına özgü bir veren** seçeneğini kullanma seçeneğini denetleyin/Kaldırın.

    g. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-google-cloud-g-suite-connector-test-user"></a>Google Cloud (G Suite) Bağlayıcı test kullanıcısı oluşturma

Bu bölümün amacı, [Google Cloud (G Suite) Bağlayıcısı](https://support.google.com/a/answer/33310?hl=en) B.Simon adında bir kullanıcı oluşturmaktır. Kullanıcı Google Cloud (G Suite) Bağlayıcısı'nda el ile oluşturulduktan sonra, kullanıcı artık Office 365 giriş kimlik bilgilerini kullanarak oturum açabilir.

Google Cloud (G Suite) Bağlayıcısı otomatik kullanıcı sağlamayı da destekler. Otomatik kullanıcı sağlama yapılandırmak için, öncelikle [otomatik kullanıcı sağlama için Google Cloud (G Suite) Bağlayıcısı yapılandırmanız](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)gerekir.

> [!NOTE]
> Azure AD'de sağlama Tek Oturum Açma'yı test etmeden önce açık değilse, kullanıcınızın Google Cloud (G Paketi) Bağlayıcısı'nda zaten bulunduğundan emin olun.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Google destek ekibine](https://www.google.com/contact/)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Google Bulut (G Suite) Bağlayıcı döşemesini tıklattığınızda, SSO'yu kurduğunuz Google Cloud (G Suite) Bağlayıcısı'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir.](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı Sağlama'yı Yapılandır](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

- [Azure AD ile Google Cloud (G Suite) Bağlayıcısı'nı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle Google Cloud (G Suite) Bağlayıcısı nasıl korunur?](https://docs.microsoft.com/cloud-app-security/protect-gsuite)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png