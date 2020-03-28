---
title: 'Öğretici: Uyarlanabilir Öngörülerle Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Uyarlanabilir Öngörüler arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf05fc30a2b111a12c0b8e131db5117ec784075b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74232134"
---
# <a name="tutorial-integrate-adaptive-insights-with-azure-active-directory"></a>Öğretici: Uyarlanabilir Öngörüleri Azure Active Directory ile Tümleştirin

Bu eğitimde, Uyarlanabilir Öngörüleri Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Uyarlanabilir Öngörüleri Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Uyarlanabilir Öngörüler'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Uyarlanabilir Öngörüler'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Adaptive Insights tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Adaptive **Insights, IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-adaptive-insights-from-the-gallery"></a>Galeriden Uyarlanabilir Öngörüler Ekleme

Uyarlanabilir Öngörüler'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Adaptive Insights eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Uyarlanabilir Öngörüler** yazın.
1. Sonuç panelinden **Adaptive Insights'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Uyarlanabilir Öngörülerle yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Uyarlamalı Öngörüler'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Uyarlamalı Öngörülerle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Uyarlanabilir Öngörüler SSO'yu yapılandırır](#configure-adaptive-insights-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı Adaptive Insights'ta B.Simon'ın bir muadili olması için **[Uyarlanabilir Öngörüler test kullanıcısı oluşturun.](#create-adaptive-insights-test-user)**
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Uyarlanabilir Öngörüler** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    > [!NOTE]
    > Adaptif Insights'ın **SAML SSO Ayarları** sayfasından Tanımlayıcı (Entity ID) ve Yanıt URL değerlerini alabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Adaptive Insights'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-adaptive-insights-sso"></a>Adaptif Öngörüleri Yapılandırma SSO

1. Farklı bir web tarayıcısı penceresinde, Uyarlanabilir Öngörüler şirket sitenizde yönetici olarak oturum açın.

2. **İdareye**git.

    ![Yönetici](./media/adaptivesuite-tutorial/ic805644.png "Yönetici")

3. Kullanıcılar **ve Roller** bölümünde **SAML SSO Ayarları'nı**tıklatın.

    ![SAML SSO Ayarlarını Yönet](./media/adaptivesuite-tutorial/ic805645.png "SAML SSO Ayarlarını Yönet")

4. **SAML SSO Ayarları** sayfasında aşağıdaki adımları gerçekleştirin:

    ![SAML SSO Ayarları](./media/adaptivesuite-tutorial/ic805646.png "SAML SSO Ayarları")

    a. Kimlik **sağlayıcı adı** textbox'ına yapılandırmanız için bir ad yazın.

    b. Azure portalından kopyalanan **Azure AD Tanımlayıcı** değerini Kimlik **sağlayıcısı Entity IDENTITY** textbox'a yapıştırın.

    c. Azure portalından kopyalanan **Giriş URL** değerini **Kimlik sağlayıcısı SSO URL** textbox'a yapıştırın.

    d. Azure portalından kopyalanan **Logout URL** değerini **Özel oturum açma URL** textbox'ına yapıştırın.

    e. İndirdiğiniz sertifikayı yüklemek için **dosyayı seç'i**tıklatın.

    f. Aşağıdakileri seçin:

     * **SAML kullanıcı kimliği**, **Kullanıcının Adaptive Insights kullanıcı adını**seçin.

     * **SAML kullanıcı kimliği konumu**, **Konunun NameID kullanıcı kimliğini**seçin .

     * **SAML NameID biçimi,** **E-posta adresini**seçin.

     * **SAML'yi etkinleştirin,** **SAML SSO'ya İzin Ver'i ve doğrudan Adaptif Öngörüler oturum açmasını**seçin.

    g. **Uyarlanabilir Öngörüler SSO URL'sini** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki Tanımlayıcı **(Entity ID)** ve **Yanıtla URL** metin kutularına yapıştırın.

    h. **Kaydet**'e tıklayın.

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

Bu bölümde, B.Simon'ın Uyarlanabilir Öngörülere erişim sağlayarak Azure oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar **listesinde, Uyarlanabilir Öngörüler'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-adaptive-insights-test-user"></a>Adaptive Insights test kullanıcısı oluşturma

Azure AD kullanıcılarının Uyarlanabilir Öngörüler'de oturum açmalarını sağlamak için, Uyarlamalı Öngörüler'e dahil edilmeleri gerekir. Adaptive Insights durumunda, sağlama manuel bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **Adaptive Insights** şirket sitenizde yönetici olarak oturum açın.

2. **İdareye**git.

   ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "Yönetici")

3. Kullanıcılar **ve Roller** **bölümünde, Kullanıcılar'ı**tıklatın.

   ![Kullanıcı Ekle](./media/adaptivesuite-tutorial/IC805648.png "Kullanıcı Ekleme")

4. Yeni **Kullanıcı** bölümünde aşağıdaki adımları gerçekleştirin:

   ![Gönder](./media/adaptivesuite-tutorial/IC805649.png "Gönder")

   a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure Active Directory kullanıcısının **Adını,** Kullanıcı **Adını,** **E-postasını,** **Parolasını** yazın.

   b. Bir **Rol**seçin.

   c. **Gönder'i**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Adaptive Insights tarafından sağlanan diğer Uyarlamalı Öngörüler kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Adaptif Öngörüler döşemesini tıklattığınızda, SSO'yu kurduğunuz Adaptif Öngörüler'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

