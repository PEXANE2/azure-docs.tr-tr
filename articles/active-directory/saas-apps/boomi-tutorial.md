---
title: 'Öğretici: Boomi ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Boomi arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/07/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a22a36d5e6c36008c3a574cbcf9be8ec4f52b82b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77086430"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-boomi"></a>Öğretici: Boomi ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Boomi'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Boomi'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Boomi erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Boomi'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Boomi tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Boomi **IDP'nin** Başlattığı SSO'ya destek verdi
* Boomi'yi yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-boomi-from-the-gallery"></a>Galeriden Boomi ekleme

Boomi'nin Azure AD'ye entegrasyonunu yapılandırmak için, boomi'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Boomi** yazın.
1. Sonuçlar panelinden **Boomi'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-boomi"></a>Boomi için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Boomi ile yapılandırın ve test edin. SSO'nun çalışması için, Boomi'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Boomi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Boomi SSO'yu yapılandırın](#configure-boomi-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Boomi test kullanıcıoluşturun](#create-boomi-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Boomi B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Boomi** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız** varsa ve **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel SAML Yapılandırması bölümünde otomatik olarak doldurulur.

    ![image](common/idp-intiated.png)

    > [!Note]
    > Hizmet Sağlayıcı **meta veri dosyasını,** daha sonra öğreticide açıklanan **YapılBoomi SSO** bölümünden alırsınız. **Tanımlayıcı** ve **Yanıt URL** değerleri otomatik olarak dosyalamıyorsa, gereksiniminize göre değerleri el ile doldurun.

1. Boomi uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıda ek olarak, Boomi uygulama aşağıda gösterilen SAML yanıtı geri geçirilecek birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------------|  --------- |
    | FEDERATION_ID | kullanıcı.posta |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Boomi'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Boomi'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Boomi'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-boomi-sso"></a>Boomi SSO'nun yapılandırılsın

1. Farklı bir web tarayıcısı penceresinde, Boomi şirket sitenizde yönetici olarak oturum açın.

1. Şirket **Adı'na** gidin ve **Ayarla'ya**gidin.

1. **SSO Seçenekleri sekmesini** tıklatın ve aşağıdaki adımları gerçekleştirin.

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/boomi-tutorial/tutorial_boomi_11.png)

    a. **SAML Tek Oturum Açma** onay kutusunu etkinleştir'i işaretleyin.

    b. İndirilen sertifikayı Azure AD'den **Kimlik Sağlayıcı Sertifikası'na**yüklemek için **İçe Aktar'ı** tıklatın.

    c. Kimlik **Sağlayıcı Giriş URL** textbox'ına Azure AD uygulama yapılandırma penceresinden **Giriş URL** değerini koyun.

    d. **Federasyon Kimlik Konumu**için, Federasyon Kimliği seçin FEDERATION_ID Attribute öğesi radyo **düğmesinde.**

    e. **AtomSphere MetaData URL'sini**kopyalayın, seçtiğiniz tarayıcı üzerinden **MetaData URL'sine** gidin ve çıktıyı bir dosyaya kaydedin. Azure portalındaki Temel **SAML Yapılandırması** bölümüne **MetaData URL'sini** yükleyin.

    f. **Kaydet** düğmesini tıklatın.

### <a name="create-boomi-test-user"></a>Boomi test kullanıcısı oluşturma

Azure AD kullanıcılarının Boomi'de oturum açabilmeleri için Boomi'ye dahil olmaları gerekir. Boomi durumunda, sağlama manuel bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Boomi şirket sitenizde yönetici olarak oturum açın.

1. Oturum açtıktan sonra **Kullanıcı Yönetimi'ne** gidin ve **Kullanıcılar'a**gidin.

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_001.png "Kullanıcılar")

1. Simgeyi tıklatın **+** ve **Kullanıcı Rolleri Ekle/Koru** iletişim kutusu açılır.

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_002.png "Kullanıcılar")

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_003.png "Kullanıcılar")

    a. Kullanıcı **e-posta adresi** B.Simon@contoso.comtextbox,.

    b. Ad **metin** kutusuna, B gibi kullanıcının adını yazın.

    c. **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını yazın.

    d. Kullanıcının Federasyon **Kimliğini**girin. Her kullanıcının, hesaptaki kullanıcıyı benzersiz olarak tanımlayan bir Federasyon Kimliği olmalıdır.

    e. **Standart Kullanıcı** rolünü kullanıcıya atayın. Yönetici rolünü atayın, çünkü bu onlara normal Atmosfer erişiminin yanı sıra tek oturum açma erişimi sağlar.

    f. **Tamam**'a tıklayın.

    > [!NOTE]
    > Kullanıcı, parolaları kimlik sağlayıcısı aracılığıyla yönetildiği için AtomSphere hesabında oturum açmak için kullanılabilecek bir parola içeren bir karşılama bildirim e-postası almaz. AAD kullanıcı hesaplarını sağlamak için Boomi tarafından sağlanan diğer Boomi kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Boomi döşemesini tıklattığınızda, SSO'yu kurduğunuz Boomi'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Azure AD ile Boomi'yi deneyin](https://aad.portal.azure.com/)
