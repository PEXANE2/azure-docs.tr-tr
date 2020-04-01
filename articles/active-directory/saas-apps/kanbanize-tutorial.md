---
title: 'Öğretici: Kanbanize ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Kanbanize arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c5a3a096c5a44f681d23587837ae31fd1af33b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373220"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-kanbanize"></a>Öğretici: Kanbanize ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Kanbanize'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Kanbanize'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Kanbanize erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Kanbanize'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kanbanize tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Kanbanize **SP ve IDP'nin** SSO'yu başlattığını destekliyor
* Kanbanize **Just In Time** kullanıcı sağlama destekler

## <a name="adding-kanbanize-from-the-gallery"></a>Galeriden Kanbanize ekleme

Kanbanize'in Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Kanbanize'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Kanbanize** yazın.
1. Sonuçlar panelinden **Kanbanize'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-kanbanize"></a>Kanbanize için Azure AD oturum açma işlemlerini yapılandırma ve test etme

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Kanbanize ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Kanbanize'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Kanbanize ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Kanbanize SSO'yu yapılandırın.](#configure-kanbanize-sso)**
    1. **[Kanbanize test kullanıcısını oluşturun](#create-kanbanize-test-user)** - Kullanıcının Azure AD gösterimine bağlı kanbanize'deki B.Simon'Un bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Kanbanize** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

     a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.kanbanize.com/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.kanbanize.com/saml/acs`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna bir URL yazın:`/ctrl_login/saml_login`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.kanbanize.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Kanbanize Müşteri destek ekibine](mailto:support@ms.kanbanize.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Kanbanize uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, nameidentifier **user.userprincipalname**ile eşlenir gibi. Kanbanize uygulaması nameidentifier **user.mail**ile eşlenen bekliyor , bu yüzden düzenleme simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşleme değiştirmek gerekir.

    ![image](common/edit-attribute.png)

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Kanbanize'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Kanbanize'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Kanbanize'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-kanbanize-sso"></a>Kanbanize SSO'yu yapılandırın

1. Kanbanize içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'i tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Kanbanize'i** ayarla'yı tıklatın ve sizi Kanbanize uygulamasına yönlendirecektir. Oradan, Kanbanize oturum için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Kanbanize'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Kanbanize şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst bölümüne gidin, **Ayarlar** logosuna tıklayın.

    ![Kanbanize ayarları](./media/kanbanize-tutorial/tutorial-kanbanize-set.png)

5. Menünün sol tarafındaki Yönetim paneli sayfasında **Tümleştirmeler'i** tıklatın ve ardından **Tek Oturum Açma'yı etkinleştirin.**

    ![Kanbanize entegrasyonları](./media/kanbanize-tutorial/tutorial-kanbanize-admin.png)

6. Tümleştirmeler bölümünde, Tek Oturum **Açma Tümleştirme** sayfasını açmak için **CONFIGURE'yi** tıklatın.

    ![Kanbanize config](./media/kanbanize-tutorial/tutorial-kanbanize-config.png)

7. **Yapılandırmalar**altındaki **Tek Oturum Açma Tümleştirme** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Kanbanize entegrasyonları](./media/kanbanize-tutorial/tutorial-kanbanize-save.png)

    a. **Idp Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    b. **Idp Giriş Bitiş Noktası** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. **Idp Logout Endpoint** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    d. E-posta textbox **için Öznitelik adına,** bu değeri girin`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. Ad textbox **için Öznitelik adına,** bu değeri girin`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    f. Soyadı textbox **için Öznitelik adına,** bu değeri girin`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    > [!Note]
    > Bu değerleri, Azure portalındaki Kullanıcı öznitelikleri bölümünden ilgili öznitelik ad alanı ve ad değerlerini birleştirerek alabilirsiniz.

    g. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlu sertifikayı açın, içeriğini kopyalayın (başlangıç ve bitiş işaretçileri olmadan) ve ardından **Idp X.509 Sertifika** kutusuna yapıştırın.

    h. **Hem SSO hem de Kanbanize ile girişi etkinleştir'i denetleyin.**

    i. **Ayarları Kaydet'i**tıklatın.

### <a name="create-kanbanize-test-user"></a>Kanbanize test kullanıcısı oluşturma

Bu bölümde Kanbanize'de B.Simon adında bir kullanıcı oluşturuldu. Kanbanize, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Kanbanize'de bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur. El ile bir kullanıcı oluşturmanız gerekiyorsa, [Kanbanize İstemci destek ekibine](mailto:support@ms.kanbanize.com)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Kanbanize karosu tıklattığınızda, SSO'yu kurduğunuz Kanbanize'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Kanbanize'i deneyin](https://aad.portal.azure.com/)

