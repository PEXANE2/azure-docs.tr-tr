---
title: 'Öğretici: Moxtra ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Moxtra arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3e53ba11744b0e78287ffc46c4aac7b99b16b23
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74889644"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-moxtra"></a>Öğretici: Moxtra ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Moxtra'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Moxtra'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Moxtra erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Moxtra'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Moxtra tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Moxtra **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-moxtra-from-the-gallery"></a>Galeriden Moxtra ekleme

Moxtra'nın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Moxtra'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Moxtra** yazın.
1. Sonuç panelinden **Moxtra'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-moxtra"></a>Moxtra için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'sunu **B.Simon**adlı bir test kullanıcısı kullanarak Moxtra ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile Moxtra'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Moxtra ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Moxtra SSO'yu yapılandırın](#configure-moxtra-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Moxtra test kullanıcısını oluşturun](#create-moxtra-test-user)** - Moxtra'da B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Moxtra** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://www.moxtra.com/service/#login`

1. Moxtra uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Moxtra uygulaması SAML yanıtında birkaç özniteliğin daha geri geçirilmesini bekler. Kullanıcı Öznitelikleri iletişim kutusundaki Kullanıcı Talepleri bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği|
    | ------------------- | -------------------- |    
    | Ad | user.givenname |
    | Soyadı | user.surname |
    | idpid    | < Azure AD Tanımlayıcı >

    > [!Note]
    > **Idpid** özniteliğinin değeri gerçek değildir. Gerçek değeri **Kurulum Moxtra** bölümünden adım#8'den alabilirsiniz. 

    1. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    1. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    1. Ad **alanını** boş bırakın.

    1. **Kaynak'ı Öznitelik**olarak seçin.

    1. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    1. **Tamam'ı** tıklatın

    1. **Kaydet**'e tıklayın.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Moxtra'yı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Moxtra'ya erişim izni vererek Azure tek oturum açma özelliğini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Moxtra'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-moxtra-sso"></a>Moxtra SSO'nun yapılandırış

1. Başka bir tarayıcı penceresinde, Yönetici olarak Moxtra şirket sitenizde oturum açın.

2. Soldaki araç çubuğunda, **Admin Console > SAML Tek Oturum Açma'yı**tıklatın ve ardından **Yeni'yi**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

3. **SAML** sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. **Ad** metin kutusunda, yapılandırmanız için bir ad yazın (örn. *SAML).* 
  
    b. **IdP Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı** değerini yapıştırın. 
 
    c. **Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın. 
 
    d. **AuthnContextClassRef** textbox'ta, **vazo:vaha:adlar:tc:SAML:2.0:ac:classes:Password**. 
 
    e. **NameID Biçimli** metin kutusunda, **tür vazo:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**. 
 
    f. Not defterinde Azure portalından indirdiğiniz sertifikayı açın, içeriği kopyalayın ve **ardından Sertifika** metin kutusuna yapıştırın.    
 
    g. SAML e-posta etki alanı textbox'ına SAML e-posta etki alanınızı yazın.    
  
    >[!NOTE]
    >Etki alanını doğrulamak için adımları görmek için aşağıdaki "**i**" seçeneğini tıklayın.

    h. **Güncelleştir**’e tıklayın.

### <a name="create-moxtra-test-user"></a>Moxtra test kullanıcısı oluşturma

Bu bölümün amacı Moxtra'da B.simon adında bir kullanıcı oluşturmaktır.

**Moxtra'da B.simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Moxtra şirket sitenizde oturum açın.

1. Soldaki araç çubuğunda, **Yönetici Konsolu > Kullanıcı Yönetimi'ni**tıklatın ve ardından **Kullanıcı Ekle'yi**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
  
    a. Ad **metin** kutusunda, **B**yazın.
  
    b. **Soyadı** metin kutusuna **Simon**yazın.
  
    c. **E-posta** metin kutusuna, B.simon'ın e-posta adresini Azure portalındakiyle aynı yazın.
  
    d. **Bölüm** metin kutusuna **Dev**yazın.
  
    e. **Bölüm** metin kutusuna **BT**yazın.
  
    f. **Yönetici'yi**seçin.
  
    g. **Ekle**’ye tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Moxtra karosunu tıklattığınızda, SSO'yu kurduğunuz Moxtra'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Moxtra'yı deneyin](https://aad.portal.azure.com/)

