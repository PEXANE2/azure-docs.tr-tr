---
title: 'Öğretici: Zscaler ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcf0341e03a5d95abbe8b1a8ce69379fef8251b7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68989063"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zscaler"></a>Öğretici: Zscaler ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Zscaler'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Zscaler'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Zscaler erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Zscaler'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Zscaler **SP** başlatılan SSO destekler
* Zscaler **Just In Time** kullanıcı sağlama destekler

## <a name="adding-zscaler-from-the-gallery"></a>Galeriden Zscaler ekleme

Zscaler'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Zscaler** yazın.
1. Sonuç panelinden **Zscaler'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zscaler"></a>Zscaler için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Zscaler ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile Zscaler'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Zscaler ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Zscaler SSO'yu yapılandırır](#configure-zscaler-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
    1. **[Zscaler test kullanıcısını oluşturun](#create-zscaler-test-user)** - Zscaler'deki B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Zscaler** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.zscaler.net`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Zscaler İstemci destek ekibine](https://www.zscaler.com/company/contact) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Zscaler uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Zscaler uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği |
    | ---------| ------------ |
    | Memberof     | user.assignedroles |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure AD'de Rolü nasıl yapılandıracağınıöğrenmek için [lütfen buraya](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) tıklayın

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Zscaler'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Britta Simon'ın Zscaler'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Zscaler'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler'i**seçin.

    ![Uygulamalar listesindeki Zscaler bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, listeden **Britta Simon** gibi kullanıcıyı seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-tutorial/tutorial_zscaler_users.png)

6. Rolü **Seç** iletişim kutusundan listedeki uygun kullanıcı rolünü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-tutorial/tutorial_zscaler_roles.png)

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini seçin.

    ![image](./media/zscaler-tutorial/tutorial_zscaler_assign.png)

## <a name="configure-zscaler-sso"></a>Zscaler SSO'nun yapılandırılsın

1. Zscaler içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten **sonra, Kurulum Zscaler'e** tıklayın ve sizi Zscaler uygulamasına yönlendirecektir. Buradan, Zscaler'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum SSO](common/setup-sso.png)

1. Zscaler'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Zscaler şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Yönetim **> Kimlik Doğrulama ayarları>** gidin ve aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-tutorial/ic800206.png "Yönetim")

    a. Kimlik Doğrulama Türü altında **SAML'yi**seçin.

    b. **SAML'yi Yapılandır'ı**tıklatın.

1. **SAML'yi Edit** penceresinde aşağıdaki adımları gerçekleştirin ve Kaydet'i tıklatın.  

    ![Kullanıcıları & Kimlik Doğrulamayı Yönetin](./media/zscaler-tutorial/ic800208.png "Kullanıcıları & Kimlik Doğrulamayı Yönetin")
    
    a. **SAML Portal URL** textbox'ına Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    b. Giriş **Adı Özniteliği** metin kutusuna **NameID'i**girin.

    c. Azure portalından indirdiğiniz Azure SAML imza sertifikasını **Genel SSL Sertifikası'na**yüklemek için **Yükle'yi**tıklatın.

    d. **SAML Otomatik Sağlama'yı Etkinleştir'i**geçişin.

    e. Kullanıcı **Görüntülü Reklam Adı Özniteliği** metin kutusuna, displayName öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **displayName'i** girin.

    f. Grup **Adı Özniteliği** textbox'ına, üye Of öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **üyeOf** girin.

    g. Bölüm **Adı Özniteliği** Bölüm Girin **bölümünde,** bölüm öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız.

    h. **Kaydet**'e tıklayın.

1. Kullanıcı **Kimlik Doğrulaması'nı Yapılandırma** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**tıklatın.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'da proxy ayarlarını yapılandırmak için

1. **Internet Explorer'ı başlatın.**

1. **Internet Seçenekleri** iletişim kutusunu açmak için **Araçlar** menüsünden **Internet seçeneklerini** seçin.

    ![İnternet Seçenekleri](./media/zscaler-tutorial/ic769492.png "İnternet Seçenekleri")

1. **Bağlantılar** sekmesini tıklatın.
  
    ![Bağlantılar](./media/zscaler-tutorial/ic769493.png "Bağlantılar")

1. **LAN Ayarları** iletişim kutusunu açmak için **LAN ayarlarını** tıklatın.

1. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   

    ![Proxy sunucusu](./media/zscaler-tutorial/ic769494.png "Proxy sunucu")

    a. **LAN'ınız için proxy sunucusu kullan'ı**seçin.

    b. Adres metin kutusuna **gateway.zscaler.net**yazın.

    c. Bağlantı Noktası metin kutusunda **80**yazın.

    d. **Yerel adresler için Bypass proxy sunucusu'ni**seçin.

    e. **Yerel Alan Ağı (LAN) Ayarları** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

1. **Internet Seçenekleri** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

### <a name="create-zscaler-test-user"></a>Zscaler test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Zscaler oluşturulur. Zscaler, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Zscaler'de bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Zscaler destek ekibine](https://www.zscaler.com/company/contact)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zscaler döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Zscaler'i deneyin](https://aad.portal.azure.com/)
