---
title: 'Öğretici: Syncplicity ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Syncplicity arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: 663958ae367162eaeb336c819d1d219dc74a2cbe
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233278"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Öğretici: Syncplicity'yi Azure Etkin Dizini ile tümleştirin

Bu eğitimde, Syncplicity'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Syncplicity'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Syncplicity erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Eşitleme ile otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Syncplicity tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Syncplisite **SP** başlatılan SSO destekler.

## <a name="adding-syncplicity-from-the-gallery"></a>Galeriden Eşitleme Ekleme

Syncplicity'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Syncplicity eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Eşitleme** yazın.
1. Sonuç panelinden **Syncplicity'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO'su yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Syncplicity ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Syncplicity'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Syncplicity ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Syncplicity SSO'yu yapılandırır](#configure-syncplicity-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Syncplicity test kullanıcıoluşturun](#create-syncplicity-test-user)** - Kullanıcının Azure AD gösterimine bağlı Syncplicity B.Simon bir meslektaşı olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Syncplicity** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.syncplicity.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Syncplicity İstemci destek ekibine](https://www.syncplicity.com/contact-us) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Syncplicity'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso"></a>Syncplicity SSO'ya yapılandır

1. **Syncplicity** kiracınızda oturum açın.

1. Üstteki **menüde, admin'e**tıklayın, **ayarları**seçin ve ardından Özel etki alanı ve tek **oturum açma'yı**tıklatın.

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Tek **Oturum Açma (SSO)** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum \(Açma SSO\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    a. Özel **Etki Alanı** metin kutusuna etki alanınızın adını yazın.
  
    b. **Tek Oturum Açma Durumu**Olarak **Etkin'i** seçin.

    c. Varlık **Kimliği** metin kutusunda, Azure portalındaki **Temel SAML Yapılandırmasında** kullandığınız **Tanımlayıcı (Entity ID)** değerini yapıştırın.

    d. Oturum **Açma sayfası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    e. Oturum **Açma sayfası URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    f. **Kimlik Sağlayıcı Sertifikası'nda,** **dosyayı seç'i**tıklatın ve ardından Azure portalından indirdiğiniz sertifikayı yükleyin.

    g. **DEĞIŞIKLIKLERI KAYDET'i**tıklatın.

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

Bu bölümde, B.Simon'ın Syncplicity'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Eşitleme'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-syncplicity-test-user"></a>Syncplicity test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmesi için Eşitleme uygulamasına dahil edilmeleri gerekir. Bu bölümde, Syncplicity'de Azure AD kullanıcı hesaplarının nasıl oluşturulutamamı açıklanmaktadır.

**Bir kullanıcı hesabını Syncplicity'ye sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Syncplicity** kiracınızda oturum açın (örneğin: `https://company.Syncplicity.com`).

1. **Admin'i** tıklatın ve **kullanıcı hesaplarını** seçin ve ardından **KULLANICI EKLE'yi**tıklatın.

    ![Kullanıcıları Yönet](./media/syncplicity-tutorial/ic769764.png "Kullanıcıları Yönet")

1. Sağlamak istediğiniz bir Azure REKLAM hesabının **E-posta adreslerini** yazın, **Kullanıcı'yı Rol**olarak seçin ve ardından **NEXT'i**tıklatın. **User**

    ![Hesap Bilgileri](./media/syncplicity-tutorial/ic769765.png "Hesap Bilgileri")

    > [!NOTE]
    > Azure AD hesap sahibi, hesabı onaylamak ve etkinleştirmek için bir bağlantı içeren bir e-posta alır.

1. Şirketinizde yeni kullanıcınızın üyesi olması gereken bir grup seçin ve ardından **NEXT'i**tıklatın.

    ![Grup Üyeliği](./media/syncplicity-tutorial/ic769772.png "Grup Üyeliği")

    > [!NOTE]
    > Listede grup yoksa **NEXT'i**tıklatın.

1. Kullanıcının bilgisayarında Syncplicity denetimi altında yerleştirmek istediğiniz klasörleri seçin ve sonra **NEXT'i**tıklatın.

    ![Eşitleme Klasörleri](./media/syncplicity-tutorial/ic769773.png "Eşitleme Klasörleri")

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Syncplicity tarafından sağlanan diğer Syncplicity kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki Eşitleme döşemesini seçtiğinizde, SSO'yu kurduğunuz Syncplicity'de otomatik olarak oturum açmış olmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)