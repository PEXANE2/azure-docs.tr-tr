---
title: 'Öğretici: Check Point CloudGuard Dome9 Arc ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Check Point CloudGuard Dome9 Arc arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b46ac34db21576c7e2de2271a468e3e782ff6aa9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73885344"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-check-point-cloudguard-dome9-arc"></a>Öğretici: Check Point CloudGuard Dome9 Arc ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Check Point CloudGuard Dome9 Arc'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Check Point CloudGuard Dome9 Arc'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Check Point CloudGuard Dome9 Arc'a erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Point CloudGuard Dome9 Arc'a otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Point CloudGuard Dome9 Arc tek oturum açma (SSO) özellikli aboneliği denetleyin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Check Point CloudGuard Dome9 Arc **SP ve IDP** SSO başlatılan destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>Galeriden Check Point CloudGuard Dome9 Arc ekleme

Check Point CloudGuard Dome9 Arc'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Check Point CloudGuard Dome9 Arc eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Point CloudGuard Dome9 Arc'ı** yazın.
1. Sonuç panelinden **Point CloudGuard Dome9 Arc'ı kontrol** edin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-check-point-cloudguard-dome9-arc"></a>Check Point CloudGuard Dome9 Arc için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Check Point CloudGuard Dome9 Arc ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında Check Point CloudGuard Dome9 Arc'ta bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Check Point CloudGuard Dome9 Arc ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Check Point CloudGuard Dome9 Arc SSO'yu yapılandırın.](#configure-check-point-cloudguard-dome9-arc-sso)**
    1. **[Check Point CloudGuard Dome9 Arc test kullanıcısını oluşturun](#create-check-point-cloudguard-dome9-arc-test-user)** - Kullanıcının Azure AD gösterimine bağlı Check Point CloudGuard Dome9 Arc'ta B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Check Point CloudGuard Dome9 Arc** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://secure.dome9.com/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://secure.dome9.com/sso/saml/<yourcompanyname>`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Değeri, daha `<company name>` sonra öğreticide açıklanan **Configure Check Point CloudGuard Dome9 Arc SSO** bölümünden alırsınız. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Point CloudGuard Dome9 Arc uygulamasını kontrol edin, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Check Point CloudGuard Dome9 Arc uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.
    
    | Adı |  Kaynak Özniteliği|
    | ---------------| --------------- |
    | Memberof | user.assignedroles |

    >[!NOTE]
    >Azure AD'de nasıl roller oluşturacağınıöğrenmek için [burayı](https://docs.microsoft.com/azure/active-directory/saas-apps/apptio-tutorial) tıklatın.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Check **Point CloudGuard Dome9 Arc'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Check Point CloudGuard Dome9 Arc'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Check **Point CloudGuard Dome9 Arc'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-check-point-cloudguard-dome9-arc-sso"></a>YapılaşıL,Check Point CloudGuard Dome9 Arc SSO

1. Denetimi Noktası CloudGuard Dome9 Arc içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Setup Check Point CloudGuard Dome9 Arc'a** tıklayın ve sizi Check Point CloudGuard Dome9 Arc uygulamasına yönlendirecektir. Buradan, Check Point CloudGuard Dome9 Arc'ta oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Check Point CloudGuard Dome9 Arc'ı el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve yönetici olarak Check Point CloudGuard Dome9 Arc şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

2. Sağ üst köşedeki **Profil Ayarları'na** tıklayın ve ardından **Hesap Ayarları'nı**tıklatın. 

    ![Check Point CloudGuard Dome9 Arc Yapılandırması](./media/dome9arc-tutorial/configure1.png)

3. **SSO'ya** gidin ve ardından **ETKINLEŞTIR'i**tıklatın.

    ![Check Point CloudGuard Dome9 Arc Yapılandırması](./media/dome9arc-tutorial/configure2.png)

4. SSO Yapılandırma bölümünde aşağıdaki adımları gerçekleştirin:

    ![Check Point CloudGuard Dome9 Arc Yapılandırması](./media/dome9arc-tutorial/configure3.png)

    a. **Hesap Kimliği** metin kutusuna şirket adını girin. Bu değer, Azure portalının **Temel SAML Yapılandırması** bölümünde belirtilen **URL'de** **Yanıtla** ve İşaretle'de kullanılacaktır.

    b. **Veren** metin kutusunda, Azure portalını kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    c. **Idp endpoint url** textbox'ına, Azure portalını kopyaladığınız **Giriş URL'sinin**değerini yapıştırın.

    d. İndirilen Base64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından X.509 sertifika** metin kutusuna yapıştırın.

    e. **Kaydet**'e tıklayın.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Check Point CloudGuard Dome9 Arc test kullanıcıoluşturma

Azure AD kullanıcılarının Point CloudGuard Dome9 Arc'ta oturum açabilmeleri için uygulamaya dahil edilmeleri gerekir. Point CloudGuard Dome9 Arc'ın tam zamanında sağlamayı desteklediğini denetleyin, ancak bunun düzgün çalışması için, kullanıcının belirli **Rolü** seçmesi ve aynı rolü kullanıcıya ataması gerekir.

   >[!Note]
   >**Rol** oluşturma ve diğer ayrıntılar için [Check Point CloudGuard Dome9 Arc Client destek ekibi](mailto:Dome9@checkpoint.com)ile iletişime geçin.

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Check Point CloudGuard Dome9 Arc şirket sitenizde yönetici olarak oturum açın.

2. **Kullanıcılar & Rolleri'ne** tıklayın ve ardından **Kullanıcılar'ı**tıklatın.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user1.png)

3. **KULLANICI EKLE'yi**tıklatın.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user2.png)

4. Kullanıcı **Oluştur** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/dome9arc-tutorial/user3.png)

    a. **E-posta** metin kutusuna, kullanıcının B.Simon@contoso.come-postasını yazın.

    b. Ad **metin** kutusunda, kullanıcının adını B gibi yazın.

    c. **Soyadı** metin kutusuna, Kullanıcının soyadını Simon gibi yazın.

    d. **SSO Kullanıcı** olarak **olun .**

    e. **OLUŞTUR'u**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Check Point CloudGuard Dome9 Arc döşemesini tıklattığınızda, SSO'yu kurduğunuz Check Point CloudGuard Dome9 Arc'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Point CloudGuard Dome9 Arc'ı kontrol edin](https://aad.portal.azure.com/)