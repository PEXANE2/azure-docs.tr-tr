---
title: 'Öğretici: Azure Active Directory freshservice ile tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Freshservice arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e08ef72dca09f873ad1cfcc91e132063b88406b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227531"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-freshservice"></a>Öğretici: Freshservice ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Freshservice'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Freshservice'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Freshservice erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Freshservice'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Freshservice tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Freshservice **SP** başlatılan SSO destekler

## <a name="adding-freshservice-from-the-gallery"></a>Galeriden Freshservice ekleme

Freshservice'in Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Yönetilen SaaS uygulamaları listenize Freshservice eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Freshservice** yazın.
1. Sonuçlar panelinden **Freshservice'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-freshservice"></a>Freshservice için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Freshservice ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Freshservice'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Freshservice ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Freshservice SSO'yu yapılandırır](#configure-freshservice-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Freshservice test kullanıcısını oluşturun](#create-freshservice-test-user)** - Kullanıcının Azure AD gösterimine bağlı Freshservice'de B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Freshservice** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<democompany>.freshservice.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<democompany>.freshservice.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Freshservice İstemci destek ekibine](https://support.freshservice.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. SSO'nun çalışması için SHA-256 parmak izi gerekmektedir. SHA-256 parmak izi almak için aşağıdaki adımları gerçekleştirin:

    ![Parmak izi](./media/freshservice-tutorial/ic790821.png "Parmak izi")

    1. [Bağlantıyı](https://www.samltool.com/fingerprint.php) farklı web tarayıcısında açın.

    1. İndirilen sertifika (Base64) dosyasını Not Defteri'nde açın ve **X.509 sertifika** textbox'ındaki içeriği yapıştırın.

    1. Algoritma için açılır dosyadan **sha256'yı** seçin.

    1. **PARMAK İzİnİzİ HESAPLAYIN'a**tıklayın.

    1. Oluşturulan **FingerPrint'i** kopyalamak ve bilgisayarınıza kaydetmek için kopya simgesine tıklayın.

1. **Azure portalındaki** **Taze Hizmeti Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Freshservice'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Freshservice'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-freshservice-sso"></a>Freshservice SSO'nun yapılandırılması

1. Freshservice içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Setup Freshservice'e** tıklayın ve sizi Freshservice uygulamasına yönlendirecektir. Buradan, Freshservice'e oturum açmaları için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Freshservice'i el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Freshservice şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Üstteki menüde **Yönetici'yi**tıklatın.

    ![Yönetici](./media/freshservice-tutorial/ic790814.png "Yönetici")

5. Müşteri **Portalı'nda** **Güvenlik'i**tıklatın.

    ![Güvenlik](./media/freshservice-tutorial/ic790815.png "Güvenlik")

6. **Güvenlik** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu Oturum Açma](./media/freshservice-tutorial/ic790816.png "Çoklu Oturum Açma")

    a. **Tek İşareti Aç.**

    b. **SAML SSO'yi**seçin.

    c. **SAML Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    d. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    e. **Güvenlik Sertifikası Parmak Izi** metin kutusunda, daha önce oluşturduğunuz Parmak **Yazdır** değerini yapıştırın.

    f. **Kaydet'i** tıklatın

### <a name="create-freshservice-test-user"></a>Freshservice test kullanıcısı oluşturma

Azure AD kullanıcılarının FreshService'de oturum açabilmeleri için FreshService'de oturum açmaları gerekir. FreshService durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **FreshService** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **Yönetici'yi**tıklatın.

    ![Yönetici](./media/freshservice-tutorial/ic790814.png "Yönetici")

3. Kullanıcı **Yönetimi** bölümünde **İstekliler'i**tıklatın.

    ![İstekliler](./media/freshservice-tutorial/ic790818.png "İstekliler")

4. **Yeni İstekli'yi**tıklatın.

    ![Yeni İstekliler](./media/freshservice-tutorial/ic790819.png "Yeni İstekliler")

5. Yeni **İstekçi** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni İstekli](./media/freshservice-tutorial/ic790820.png "Yeni İstekli")  

    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure Etkin Dizin hesabının **Ad** ve **E-posta** özniteliklerini girin.

    b. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure Active Directory hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bir bağlantı içeren bir e-posta alır
    >  

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için FreshService tarafından sağlanan diğer FreshService kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Freshservice döşemesini tıklattığınızda, SSO'yu kurduğunuz Freshservice'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Freshservice'i deneyin](https://aad.portal.azure.com/)