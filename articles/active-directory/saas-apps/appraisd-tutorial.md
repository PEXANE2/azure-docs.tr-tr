---
title: 'Öğretici: Appraisd ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Appraisd arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08453928ab000cf906c451fa6c1cd619a00ee4ca
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67561192"
---
# <a name="tutorial-integrate-appraisd-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile Appraisd entegre

Bu eğitimde, Appraisd'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Appraisd'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Appraisd'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Appraisd'e otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Appraisd tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Appraisd **SP ve IDP** SSO başlatılan destekler.

## <a name="adding-appraisd-from-the-gallery"></a>Galeriden Appraisd ekleme

Appraisd'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Appraisd eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Appraisd** yazın.
1. Sonuç panelinden **Appraisd'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B. Simon**adlı bir test kullanıcısı kullanarak Appraisd ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Appraisd'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Appraisd ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[Appraisd'i yapılandırın.](#configure-appraisd)**
3. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Appraisd test kullanıcısını,](#create-appraisd-test-user)** Kullanıcının Azure AD gösterimine bağlı Appraisd'deki B. Simon'ın bir muadili olması için oluşturun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Appraisd** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının Kaydet düğmesini tıklatarak yapılandırmayı kaydetmesi ve aşağıdaki adımları gerçekleştirmesi gerekir:

    a. **Ek URL'ler ayarla'yı**tıklatın.

    b. **Röle Durumu** metin kutusuna bir URL yazın:`<TENANTCODE>`

    c. Uygulamayı **SP** başlatılan modda, **Oturum Aç URL** metin kutusunda yapılandırmak istiyorsanız, aşağıdaki deseni kullanarak bir URL yazın:`https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > Daha sonra öğreticide açıklanan Appraisd SSO Yapılandırma sayfasında gerçek Oturum Açma URL'si ve Röle Durumu değerini alırsınız.

1. Appraisd uygulama, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. Appraisd uygulama **nameidentifier** **user.mail**ile eşlenen bekliyor , bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşleme değiştirmek gerekir.

    ![image](common/edit-attribute.png)

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Appraisd'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-appraisd"></a>TakdirI Yapılandır

1. Appraisd içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Appraisd'e** tıklayın ve sizi Appraisd uygulamasına yönlendirecektir. Buradan Appraisd'e oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Appraisd'i el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Appraisd şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst kısmında **Ayarlar** simgesine tıklayın ve ardından **Yapılandırma'ya**gidin.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

5. Menünün sol tarafından **SAML tek oturum açma'ya**tıklayın.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

6. **SAML 2.0 Tek Oturum Açma yapılandırma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Varsayılan **Geçiş Durumu** değerini kopyalayın ve Azure portalındaki **Temel SAML** Yapılandırması'ndaki **Relay State** textbox'a yapıştırın.

    b. Hizmet **tarafından başlatılan giriş URL** değerini kopyalayın ve Azure portalındaki **Temel SAML** Yapılandırması'ndaki **Oturum Açma URL** textbox'ına yapıştırın.

7. **Kullanıcıları Tanımlama**altında aynı sayfayı aşağı kaydırın, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. Kimlik **Sağlayıcı Tek Oturum Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin**değerini yapıştırın ve **Kaydet'i**tıklatın.

    b. Kimlik **Sağlayıcı Veren URL** textbox'ına, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın ve **Kaydet'i**tıklatın.

    c. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlu sertifikayı açın, içeriğini kopyalayın ve **ardından X.509 Sertifika** kutusuna yapıştırın ve **Kaydet'i**tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B. Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B. Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın Appraisd'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Appraisd'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B. Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-appraisd-test-user"></a>Appraisd test kullanıcısı oluşturma

Azure AD kullanıcılarının Appraisd'de oturum açabilmeleri için, bu kullanıcıların Appraisd'e uygun olarak sağlanması gerekir. Appraisd'de, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak Appraisd'de oturum açın.

2. Sayfanın sağ üst kısmında **Ayarlar** simgesine tıklayın ve ardından **Yönetim merkezine**gidin.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Sayfanın üst kısmındaki araç çubuğunda **Kişiler'i**tıklatın ve **ardından yeni bir kullanıcı ekle'ye**gidin.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Yeni **bir kullanıcı ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)

    a. **Ad metin** **kutusuna, Britta**gibi kullanıcının ilk adını girin.

    b. **Soyadı** metin kutusuna, **simon**gibi kullanıcının soyadını girin.

    c. **E-posta** metin kutusuna, gibi `B. Simon@contoso.com`kullanıcının e-posta girin.

    d. **Kullanıcı Ekle'yi**tıklatın.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki Appraisd karo'yu seçtiğinizde, SSO'yu kurduğunuz Appraisd'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
