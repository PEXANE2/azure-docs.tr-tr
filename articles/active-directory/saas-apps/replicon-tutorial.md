---
title: 'Öğretici: Replicon ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Replicon arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b27615b0c76b5c23bbc79788431b0e909b8bf22a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67092761"
---
# <a name="tutorial-integrate-replicon-with-azure-active-directory"></a>Öğretici: Replicon'u Azure Active Directory ile tümleştirin

Bu eğitimde, Replicon'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Replicon'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Replicon erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Replicon'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Replicon tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Replicon, **SP** tarafından başlatılan SSO'yu destekler.

## <a name="adding-replicon-from-the-gallery"></a>Galeriden Replicon ekleme

Replicon'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Replicon eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Replicon** yazın.
1. Sonuçlar panelinden **Replicon'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Replicon ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Replicon'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Replicon ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Replicon SSO'yu yapılandırın](#configure-replicon-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Replicon test kullanıcıoluşturun](#create-replicon-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Replicon B.Simon bir meslektaşı olması.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Replicon** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    1. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://global.replicon.com/!/saml2/<client name>/sp-sso/post`

    1. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://global.replicon.com/!/saml2/<client name>`

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://global.replicon.com/!/saml2/<client name>/sso/post`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Replicon Client destek ekibine](https://www.replicon.com/customerzone/contact-support) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Ayarları yeniden yapmak için **SAML İmza Sertifikası** için edit/kalem simgesini tıklatın.

    ![İmza Algoritması](common/signing-algorithm.png)

    1. **İmzalama Seçeneği**olarak **SIGN SAML iddiasını** seçin.

    1. **İmza Algoritması**olarak **SHA-256'yı** seçin.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-replicon-sso"></a>Replicon SSO'yu yapılandır

1. Farklı bir web tarayıcısı penceresinde, Replicon şirket sitenizde yönetici olarak oturum açın.

2. SAML 2.0'ı yapılandırmak için aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulamasını etkinleştirme](./media/replicon-tutorial/ic777805.png "SAML kimlik doğrulamasını etkinleştirme")

    a. **EnableSAML Authentication2** iletişim kutusunu görüntülemek için, şirket anahtarınızdan sonra aşağıdakileri URL'nize ekleyebilir:`/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Aşağıdaki tam URL şema gösterir:`https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. **+** **v20Configuration** bölümünü genişletmek için tıklatın.

   c. **+** **metaDataConfiguration** bölümünü genişletmek için tıklatın.

   d. xmlSignatureAlgorithm için **SHA256'yı** seçin

   e. Kimlik sağlayıcınız meta data XML dosyanızı seçmek için **Dosyayı Seçin'i**tıklatın ve **Gönder'i**tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Replicon'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Replicon'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-replicon-test-user"></a>Replicon test kullanıcısı oluşturma

Bu bölümün amacı Replicon B.Simon adlı bir kullanıcı oluşturmaktır.

**Kullanım kılavuzunu el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde, Yönetici olarak Replicon şirket sitenizde oturum açın.

2. ** \> Yönetim Kullanıcıları'na**gidin.

    ![Kullanıcılar](./media/replicon-tutorial/ic777806.png "Kullanıcılar")

3. **+Kullanıcı Ekle'yi**tıklatın.

    ![Kullanıcı Ekle](./media/replicon-tutorial/ic777807.png "Kullanıcı Ekleme")

4. Kullanıcı **Profili** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı profili](./media/replicon-tutorial/ic777808.png "Kullanıcı profili")

    a. Giriş **Adı** metin kutusuna, aşağıdaki gibi `B.Simon@contoso.com`sağlamak istediğiniz Azure AD kullanıcısının Azure AD e-posta adresini yazın.

    > [!NOTE]
    > Oturum Açma Adı'nın Azure AD'deki kullanıcının e-posta adresiyle eşleşmesi gerekiyor

    b. **Kimlik Doğrulama Türü**olarak **SSO'yı**seçin.

    c. Kimlik Doğrulama Kimliğini Oturum Açma Adı (Kullanıcının Azure AD e-posta adresi) ile aynı değere ayarlama

    d. **Bölüm** metin kutusuna, kullanıcının bölümünü yazın.

    e. **Çalışan Türü**olarak, **Yönetici'yi**seçin.

    f. **Kullanıcı Profilini Kaydet'i**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Replicon tarafından sağlanan diğer Replicon kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki Replicon karosu seçtiğinizde, SSO'yu kurduğunuz Replicon'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)