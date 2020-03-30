---
title: 'Öğretici: AirWatch ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AirWatch arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 772b37816b83c275bae927d825434dc3ca76a35c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74231983"
---
# <a name="tutorial-integrate-airwatch-with-azure-active-directory"></a>Öğretici: AirWatch'u Azure Active Directory ile tümleştirin

Bu eğitimde, AirWatch'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. AirWatch'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* AirWatch erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla AirWatch'ta otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* AirWatch tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. **AirWatch, SP** tarafından başlatılan SSO'ya destek veriyor.

## <a name="adding-airwatch-from-the-gallery"></a>Galeriden AirWatch ekleme

AirWatch'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize AirWatch eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **AirWatch** yazın.
1. Sonuçlar panelinden **AirWatch'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak AirWatch ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile AirWatch'taki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu AirWatch ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[AirWatch SSO'yu yapılandırın.](#configure-airwatch-sso)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. **[AirWatch test kullanıcısını oluşturun](#create-airwatch-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı AirWatch'ta Britta Simon'ın bir muadili olması için.
5. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **AirWatch** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    1. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    1. Tanımlayıcı **(Entity ID)** metin kutusuna değeri aşağıdaki gibi yazın:`AirWatch`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değeri almak için [AirWatch Client destek ekibiyle](https://www.air-watch.com/company/contact-us/) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. AirWatch uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

1. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, **Edit simgesini** kullanarak talepleri düzenleme veya yukarıdaki resimde gösterildiği gibi SAML belirteç özniteliğini yapılandırmak için **yeni talep ekle** kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:

    | Adı |  Kaynak Özniteliği|
    |---------------|----------------|
    | Uıd | user.userprincipalname |
    | | |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'i** bulun ve Metadata XML'yi indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **AirWatch'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-airwatch-sso"></a>AirWatch SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, AirWatch şirket sitenizde yönetici olarak oturum açın.

1. Ayarlar sayfasında. **Ayarlar > Kurumsal Tümleştirme > Dizin Hizmetleri'ni**seçin.

   ![Ayarlar](./media/airwatch-tutorial/ic791921.png "Ayarlar")

1. **Temel DN** textbox'ta **Kullanıcı** sekmesini tıklatın, etki alanı adınızı yazın ve sonra **Kaydet'i**tıklatın.

   ![Kullanıcı](./media/airwatch-tutorial/ic791922.png "Kullanıcı")

1. **Sunucu** sekmesini tıklatın.

   ![Sunucu](./media/airwatch-tutorial/ic791923.png "Sunucu")

1. **LDAP** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Karşıya yükle](./media/airwatch-tutorial/ic791924.png "LDAP")   

    a. **Dizin Türü**olarak, **Yok'u**seçin.

    b. **Kimlik Doğrulama için SAML'yi kullan'ı**seçin.

1. SAML **2.0** bölümünde, indirilen sertifikayı yüklemek için **Yükleyin'i**tıklatın.

    ![Karşıya yükle](./media/airwatch-tutorial/ic791932.png "Karşıya Yükle")

1. **İstek** bölümünde aşağıdaki adımları gerçekleştirin:

    ![İstek](./media/airwatch-tutorial/ic791925.png "İstek")  

    a. **İstek Bağlama Türü**olarak POST'u seçin. **POST**

    b. Azure **portalında, AirWatch** iletişim sayfasında ki Yapılandırma tek oturum açma **sayfasında, Giriş URL** değerini kopyalayın ve ardından Kimlik Sağlayıcı Tek **İşareti URL** metin kutusuna yapıştırın.

    c. **NameID Biçimi**olarak, **E-posta Adresi**seçin.

    d. **Kimlik Doğrulama İstek Güvenliği**olarak, **Yok'u**seçin.

    e. **Kaydet**'e tıklayın.

1. **Kullanıcı** sekmesini yeniden tıklatın.

    ![Kullanıcı](./media/airwatch-tutorial/ic791926.png "Kullanıcı")

1. **Öznitelik** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Öznitelik](./media/airwatch-tutorial/ic791927.png "Öznitelik")

    a. Nesne **Tanımlayıcı** textbox'ına `http://schemas.microsoft.com/identity/claims/objectidentifier`.

    b. Kullanıcı **adı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    c. Görüntü **Adı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    d. Ad **metin** kutusuna , `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. **Soyadı** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`, yazın.

    f. **E-posta** metin kutusuna, yazın. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    g. **Kaydet**'e tıklayın.

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

Bu bölümde, B.Simon'ın AirWatch'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **AirWatch'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-airwatch-test-user"></a>AirWatch test kullanıcıoluşturun

Azure AD kullanıcılarının AirWatch'ta oturum açabilmeleri için AirWatch'a dahil edilmeleri gerekir. AirWatch durumunda, sağlama manuel bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **AirWatch** şirket sitenizde yönetici olarak oturum açın.

2. Sol taraftaki gezinti bölmesinde **Hesaplar'ı**tıklatın ve ardından **Kullanıcılar'ı**tıklatın.
  
   ![Kullanıcılar](./media/airwatch-tutorial/ic791929.png "Kullanıcılar")

3. **Kullanıcılar** menüsünde **Liste Görünümü'nü**tıklatın ve ardından Kullanıcı **Ekle >'ı**tıklatın.
  
   ![Kullanıcı Ekle](./media/airwatch-tutorial/ic791930.png "Kullanıcı Ekleme")

4. Kullanıcı **Ekle / Edit** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı Ekle](./media/airwatch-tutorial/ic791931.png "Kullanıcı Ekleme")

   a. Geçerli bir Azure Active Directory hesabının **Kullanıcı Adı,** **Parola,** **Şifreyi Onayla,** **Ad,** **Soyad,** **E-posta Adresini** girin ilgili metin kutularına girin.

   b. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için AirWatch tarafından sağlanan diğer AirWatch kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki AirWatch döşemesini seçtiğinizde, SSO'yu kurduğunuz AirWatch'ta otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
