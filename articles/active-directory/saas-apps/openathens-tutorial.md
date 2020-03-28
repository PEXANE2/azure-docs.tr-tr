---
title: 'Öğretici: OpenAthens ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve OpenAthens arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43fc2272a81672ea613bdcbe17c5381e99cafbff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73053207"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-openathens"></a>Öğretici: OpenAthens ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, OpenAthens'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. OpenAthens'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* OpenAthens erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla OpenAthens'da otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* OpenAthens tek oturum açma (SSO) aboneliği etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **OpenAthens'den IDP'nin** başlattığı SSO'ya destek
* OpenAthens **Just In Time** kullanıcı sağlama destekler

## <a name="adding-openathens-from-the-gallery"></a>Galeriden OpenAthens ekleme

OpenAthens'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize OpenAthens eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **OpenAthens** yazın.
1. Sonuçlar panelinden **OpenAthens'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-openathens"></a>OpenAthens için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak OpenAthens ile yapılandırın ve test edin. SSO'nun çalışması için, OpenAthens'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu OpenAthens ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[OpenAthens SSO'yu yapılandırır](#configure-openathens-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[OpenAthens test kullanıcısını oluşturun](#create-openathens-test-user)** - Kullanıcının Azure AD gösterimine bağlı OpenAthens'da B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **OpenAthens** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, bu öğreticide daha sonra belirtilen adımları, **Hizmet Sağlayıcı meta veri dosyasını**yükleyin.

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![openathens yükleme meta veri](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![Openathens yükleme meta verilerine göz atın](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** değeri **Temel SAML Yapılandırması** bölümü textbox'ta otomatik olarak doldurulur:

    ![OpenAthens Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **OpenAthens'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın OpenAthens'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **OpenAthens'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-openathens-sso"></a>OpenAthens SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, OpenAthens şirket sitenizde yönetici olarak oturum açın.

1. **Yönetim** sekmesi altındaki listeden **Bağlantılar'ı** seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application1.png)

1. **SAML 1.1/2.0'ı**seçin ve sonra **Yapıla** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application2.png)

1. Yapılandırmayı eklemek için Azure portalından indirdiğiniz meta data .xml dosyasını yüklemek için **Gözat** düğmesini seçin ve ardından **Ekle'yi**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application3.png)

1. **Ayrıntılar** sekmesi altında aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. **Görüntü adı eşlemesinde,** **Özniteliği kullan'ı**seçin.

    b. Görüntü **adı özniteliği** metin kutusuna `http://schema.microsoft.com/identity/claims/displayname`değeri girin.

    c. **Benzersiz kullanıcı eşlemesinde,** **Özniteliği Kullan'ı**seçin.

    d. Benzersiz **kullanıcı özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`değeri girin.

    e. **Durum'** da, üç onay kutusunun hepsini seçin.

    f. **Yerel hesaplar oluştur'da,** otomatik **olarak**seçin.

    g. **Değişiklikleri kaydet**'i seçin.

    h. **</> Relying Party** sekmesinden **Metadata URL'sini** kopyalayın ve **SP meta data XML** dosyasını indirmek için bunu tarayıcıda açın. Bu SP meta veri dosyasını Azure AD'deki **Temel SAML Yapılandırması** bölümüne yükleyin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application5.png)

### <a name="create-openathens-test-user"></a>OpenAthens test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı OpenAthens oluşturulur. OpenAthens, varsayılan olarak etkinleştirilen **tam zamanında kullanıcı sağlamayı**destekler. Bu bölümde sizin için bir eylem öğesi yoktur. OpenAthens'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki OpenAthens döşemesini tıklattığınızda, SSO'yu kurduğunuz OpenAthens'da otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile OpenAthens'ı deneyin](https://aad.portal.azure.com/)