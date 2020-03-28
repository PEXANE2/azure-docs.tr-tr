---
title: 'Öğretici: Adobe Creative Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Adobe Creative Cloud arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25dd638c15fecbef787e4ceabea9ae7cb4359582
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76120375"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-adobe-creative-cloud"></a>Öğretici: Adobe Creative Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

> [!NOTE]
> Bu makalede, Adobe Yönetici Konsolu'nun Azure Etkin Dizini (Azure AD) için özel SAML tabanlı kurulumu açıklanmaktadır. Yepyeni yapılandırmalar için [Azure AD Bağlayıcısı'nı](https://helpx.adobe.com/enterprise/using/sso-setup-azure.html)kullanmanızı öneririz. Azure AD Bağlayıcısı dakikalar içinde ayarlanabilir ve etki alanı talep, tek oturum açma kurulumu ve kullanıcı eşitleme işlemini kısaltır.

Bu eğitimde, Adobe Creative Cloud'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Adobe Creative Cloud'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Adobe Creative Cloud erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Adobe Creative Cloud'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Adobe Creative Cloud tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Adobe Creative **Cloud, SP** tarafından başlatılan SSO'ya destek verdi





## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Galeriden Adobe Creative Cloud ekleme

Adobe Creative Cloud'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Adobe Creative Cloud'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Adobe Creative Cloud** yazın.
1. Sonuç panelinden **Adobe Creative Cloud'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-adobe-creative-cloud"></a>Adobe Creative Cloud için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Adobe Creative Cloud ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Adobe Creative Cloud'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Adobe Creative Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Adobe Creative Cloud SSO'yu uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-adobe-creative-cloud-sso)**
    1. **[Adobe Creative Cloud test kullanıcısını oluşturun](#create-adobe-creative-cloud-test-user)** - Adobe Creative Cloud'da Kullanıcının Azure AD gösterimine bağlı b.simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Adobe Creative Cloud** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç'a** bir URL yazın:`https://adobe.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.okta.com/saml2/service-provider/<token>`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değildir. Bu değeri gerçek Tanımlayıcı ile güncelleştirin. Bu değeri elde etmek için [Adobe Creative Cloud Client destek ekibine](https://www.adobe.com/au/creativecloud/business/teams/plans.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Adobe Creative Cloud uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

1. Adobe Creative Cloud uygulaması, yukarıda ek olarak, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    |----- | --------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Email | kullanıcı.posta |

    > [!NOTE]
    > Kullanıcıların SAML yanıtında doldurulması için e-posta talep değeri için geçerli bir Office 365 ExO lisansına sahip olması gerekir.

1. **SAML** ile tek oturum açma işlemi nde, **SAML İmza Sertifikası** bölümünde Federasyon Veri **XML'ini**bulun ve ardından XML meta veri dosyasını indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Adobe **Creative Cloud'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Adobe Creative Cloud'a erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Adobe Creative Cloud'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-adobe-creative-cloud-sso"></a>Adobe Creative Cloud SSO'ya yapılandırın

1. Farklı bir web tarayıcısı penceresinde, sistem yöneticisi olarak [Adobe Yönetici Konsolu'nda](https://adminconsole.adobe.com) oturum açın.

1. Üstteki gezinti çubuğundaki **Ayarlar'a** gidin ve ardından **Kimlik'i**seçin. Dizin listesi açılır. İstediğiniz Federe dizini seçin.

1. **Dizin Ayrıntıları** **sayfasında, Yapıla'yı**seçin.

1. Varlık Kimliğini ve ACS URL'sini kopyalayın (Tüketici Hizmet URL'sini veya Yanıt URL'sini tarayın). URL'leri Azure portalındaki uygun alanlara girin.

    ![Uygulama tarafında tek oturum açma yapılandırma](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. **Adobe'nin, YapıLı Uygulama Ayarları** iletişim kutusundai **identifier için** verdiği Entity ID değerini kullanımını sürür.

    b. **Adobe'nin, YapılAşı Uygulama Ayarları** iletişim kutusunda **Yanıt URL'si** için sağladığı ACS URL'sini (İddia Tüketici Hizmeti URL'si) kullanın.

1. Sayfanın alt kısmında, Azure portalından indirdiğiniz **Federasyon Veri XML** dosyasını yükleyin. 

    ![Federasyon Veri XML dosyası](https://helpx.adobe.com/content/dam/help/en/enterprise/kb/configure-microsoft-azure-with-adobe-sso/jcr_content/main-pars/procedure/proc_par/step_228106403/step_par/image_copy/saml_signinig_certificate.png "IdP Meta data XML")

1. **Kaydet'i**seçin.


### <a name="create-adobe-creative-cloud-test-user"></a>Adobe Creative Cloud test kullanıcıoluşturma

Azure AD kullanıcılarının Adobe Creative Cloud'da oturum açabilmeleri için Adobe Creative Cloud'da oturum açmaları gerekir. Adobe Creative Cloud söz konusu olduğunda, sağlama el ile bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. [Adobe Admin Console](https://adminconsole.adobe.com) sitesinde yönetici olarak oturum açın.

2. Kullanıcıyı Adobe'nin konsoluna Federe Kimliği olarak ekleyin ve bir Ürün Profiline atayın. Kullanıcı ekleme hakkında ayrıntılı bilgi için [bkz.](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Bu noktada, e-posta adresinizi/yükseltmenizi Adobe oturumdaki forma yazın, sekmesine basın ve Azure AD'ye geri gönderilmelidir:
   * Web erişimi:\.www adobe.com > oturum açma
   * Masaüstü uygulama programı içinde oturum > oturum açma
   * Uygulama içinde > oturum > yardımcı

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Adobe Creative Cloud döşemesini tıklattığınızda, SSO'yu kurduğunuz Adobe Creative Cloud'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Adobe Creative Cloud'u deneyin](https://aad.portal.azure.com/)

- [Kimlik ayarlama (adobe.com)](https://helpx.adobe.com/enterprise/using/set-up-identity.html)
  
- [Azure'u Adobe SSO (adobe.com) ile kullanım için yapılandırma](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

