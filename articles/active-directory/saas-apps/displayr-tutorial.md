---
title: 'Öğretici: Displayr ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Displayr arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: b739b4e3-1a37-4e3c-be89-c3945487f4c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3cd1785595cf2f6b2401837780106f52fdc97e36
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67103982"
---
# <a name="tutorial-integrate-displayr-with-azure-active-directory"></a>Öğretici: Ekrancıyı Azure Etkin Dizini ile tümleştir

Bu eğitimde, Displayr'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Displayr'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Görüntüleyici'ye erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Displayr'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Displayr tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. **Displayr, SP** tarafından başlatılan SSO'yı destekler.

## <a name="adding-displayr-from-the-gallery"></a>Galeriden Görüntüleyici Ekleme

Displayr'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Görüntüleyici eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Görüntüleyici** yazın.
1. Sonuç panelinden **Görüntüleyici'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak Displayr ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile Displayr'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Displayr ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[Displayr'i yapılandırın.](#configure-displayr)**
3. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Displayr'da Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için **[Displayr test kullanıcısını oluşturun.](#create-displayr-test-user)**
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Ekrancı** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Kurulum Tek Oturum** Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımı gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<YOURDOMAIN>.displayr.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<YOURDOMAIN>.displayr.com`

    >[!NOTE]
    >Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Displayr İstemci destek ekibine](mailto:support@displayr.com) başvurun. Azure portalındaki Temel SAML Yapılandırması bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** ile Kurulum Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Görüntüleyici uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Displayr uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Grup Talepleri (Önizleme)** iletişim kutusundaki **Kullanıcı Öznitelikleri & Talepler** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Talepte döndürülen Gruplar'ın**yanındaki **kalemi** tıklatın.

    ![image](./media/displayr-tutorial/config04.png)

    ![image](./media/displayr-tutorial/config05.png)

    b. Radyo listesinden **Tüm Gruplar'ı** seçin.

    c. **Grup Kimliğinin**Kaynak **Özniteliği'ni** seçin.

    d. **Grup talebinin adını özelleştirin.**

    e. **Rol talepleri olarak Emit gruplarını**denetleyin.

    f. **Kaydet**'e tıklayın.

1. Ayar **görüntüleyici** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-displayr"></a>Görüntüleyiciyi Yapılandır

1. Displayr içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Setup Displayr'ı** tıklatın ve sizi Ekrancı uygulamasına yönlendirecektir. Buradan, Görüntüleyici'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Ekrancı'yı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Ekrancı şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. **Ayarlar'a** tıklayın ve **Ardından Hesap'a**gidin.

    ![Yapılandırma](./media/displayr-tutorial/config01.png)

5. Üst menüden **Ayarlar'a** geçin ve Yapılandırma **Tek İşaretini Aç (SAML)** tıklatmak için sayfayı aşağı kaydırın.

    ![Yapılandırma](./media/displayr-tutorial/config02.png)

6. Tek **İşaret Açma (SAML)** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma](./media/displayr-tutorial/config03.png)

    a. Tek **İşaret Açma (SAML)** kutusunu etkinleştir kutusunu işaretleyin.

    b. Azure AD'nin **Temel SAML Yapılandırması** bölümündeki gerçek **Tanımlayıcı** değerini kopyalayın ve **Veren** metin kutusuna yapıştırın.

    c. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    d. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    e. Sertifikayı (Base64) Not Defteri'nde açın, içeriğini kopyalayın ve **Sertifika** metin kutusuna yapıştırın.

    f. **Grup eşlemeleri** isteğe bağlıdır.

    g. **Kaydet**'e tıklayın.  

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Displayr'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Görüntüleyici'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-displayr-test-user"></a>Displayr test kullanıcıoluşturma

Azure AD kullanıcılarının Displayr'da oturum açmalarını etkinleştirmek için, görüntüleyenler olarak sağlanmalıdır. Görüntüleyici'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Görüntüleyici'de oturum açın.

2. **Ayarlar'a** tıklayın ve **Ardından Hesap'a**gidin.

    ![Görüntüleyici Yapılandırması](./media/displayr-tutorial/config01.png)

3. Üst menüden **Ayarlar'a** geçin ve **Kullanıcılar** bölümüne kadar sayfayı aşağı kaydırın ve **Yeni Kullanıcı'yı**tıklatın.

    ![Görüntüleyici Yapılandırması](./media/displayr-tutorial/config07.png)

4. Yeni **Kullanıcı** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Görüntüleyici Yapılandırması](./media/displayr-tutorial/config06.png)

    a. **Ad** metin **kutusuna, Brittasimon**gibi kullanıcının adını girin.

    b. **E-posta** metin kutusuna, gibi `Brittasimon@contoso.com`kullanıcının e-posta girin.

    c. Uygun **Grup üyeliğinizi**seçin.

    d. **Kaydet**'e tıklayın.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Ekrancı döşemesini seçtiğinizde, SSO'yu kurduğunuz Ekrangörüntüsü'nda otomatik olarak oturum açmış olmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
