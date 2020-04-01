---
title: 'Öğretici: Discovery Benefits SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Discovery Benefits SSO arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a788cd07-0eed-4067-b79d-19b840e8836d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64c4a6811ef5d7ed4f29c7dae89561616895a42a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72266146"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-discovery-benefits-sso"></a>Öğretici: Discovery Benefits SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Discovery Benefits SSO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Discovery Faydaları SSO'su Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Bulma Avantajları SSO'ya erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Discovery Benefits SSO'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Discovery Benefits SSO tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Discovery Benefits **SSO, IDP** tarafından başlatılan SSO'ya destek verdi

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-discovery-benefits-sso-from-the-gallery"></a>Galeriden Discovery Faydaları SSO ekleme

Discovery Benefits SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Discovery Faydaları SSO'yu eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Discovery Benefits SSO** yazın.
1. Sonuç panelinden **Discovery Benefits SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-discovery-benefits-sso"></a>Bulma Avantajları SSO için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Discovery Benefits SSO ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Discovery Benefits SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Discovery Benefits SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Yapılandırma Discovery Faydaları SSO SSO](#configure-discovery-benefits-sso-sso)** - uygulama tarafında tek oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcının Azure AD gösterimine bağlı Discovery Benefits SSO'da B.Simon'ın bir örneğine sahip olmak için **[Discovery Benefits SSO test kullanıcısı oluşturun.](#create-discovery-benefits-sso-test-user)**
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Discovery Benefits SSO** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Discovery Benefits SSO uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

    a. **Benzersiz Kullanıcı Tanımlayıcısı (Ad Kimliği)** iletişim kutusunu açmak için **Edit** simgesine tıklayın.

    ![Discovery Faydaları SSO yapılandırma](./media/discovery-benefits-sso-tutorial/attribute01.png)

    ![Discovery Faydaları SSO yapılandırma](./media/discovery-benefits-sso-tutorial/attribute02.png)

    b. **Dönüşümleri Yönet** iletişim kutusunu açmak için **Yap** simgesini tıklatın.

    c. **Dönüşüm** metin kutusunda, bu satır için gösterilen **ToUppercase()** yazın.

    d. **Parametre 1** metin kutusuna, parametreyi ' yi ' ye benzer şekilde `<Name Identifier value>`yazın

    e. **Ekle**’ye tıklayın.

    > [!NOTE]
    > Discovery Benefits SSO, bu tümleştirmenin çalışması için **Benzersiz Kullanıcı Tanımlayıcısı (Ad Kimliği)** alanında geçirilecek sabit bir dize değerinin olmasını gerektirir. Azure AD şu anda bu özelliği desteklemiyor, bu nedenle çözüm olarak, ekran görüntüsünde yukarıda gösterildiği gibi sabit bir dize değeri ayarlamak için NameID'nin **ToUpper** veya **ToLower** dönüşümlerini kullanabilirsiniz.

    f. SSO yapılandırması için gerekli olan ek talepleri otomatik`SSOInstance` `SSOID`olarak doldurduk ( ve ). Kuruluşunuza göre değerleri eşlemek için **Düzenleme** simgesini kullanın.

    ![Discovery Faydaları SSO yapılandırma](./media/discovery-benefits-sso-tutorial/attribute03.png)

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Discovery **Benefits SSO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Discovery Benefits SSO'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Discovery Benefits SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-discovery-benefits-sso-sso"></a>Yapılandırma Discovery Faydaları SSO SSO SSO

**Discovery Benefits SSO** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından [Discovery Benefits SSO destek ekibine](mailto:Jsimpson@DiscoveryBenefits.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-discovery-benefits-sso-test-user"></a>Bulma Avantajları Oluşturma SSO test kullanıcısı

Bu bölümde, Discovery Benefits SSO'da Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Discovery Benefits SSO platformuna eklemek için [Discovery Benefits SSO destek ekibiyle](mailto:Jsimpson@DiscoveryBenefits.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Discovery Benefits SSO döşemesini tıklattığınızda, SSO'yu kurduğunuz Discovery Benefits SSO'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SSO'ya Discovery Avantajlarını Deneyin](https://aad.portal.azure.com/)

