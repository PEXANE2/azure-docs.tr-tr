---
title: 'Öğretici: Azure AD için BlueJeans ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure AD için Azure Active Directory ve BlueJeans arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfc38f63c5b6361122c236543320b91d22faa70a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72595042"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-bluejeans-for-azure-ad"></a>Öğretici: Azure AD için BlueJeans ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Azure AD için BlueJeans'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Azure AD için BlueJeans'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD için BlueJeans erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure AD için BlueJeans ile otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure AD için BlueJeans tek oturum açma (SSO) aboneliği etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Azure AD için **BlueJeans, SP** tarafından başlatılan SSO'ya destek veriyor

* Azure AD için BlueJeans [ **Otomatik** kullanıcı sağlama yı destekler](bluejeans-provisioning-tutorial.md)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-bluejeans-for-azure-ad-from-the-gallery"></a>Galeriden Azure AD için BlueJeans ekleme

Azure AD için BlueJeans'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Azure AD için BlueJeans'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Azure AD için BlueJeans** yazın.
1. Sonuçlar panelinden **Azure AD için BlueJeans'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-bluejeans-for-azure-ad"></a>Azure AD için BlueJeans için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Azure AD için BlueJeans ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Azure AD kullanıcısı ile Azure AD için BlueJeans'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Azure AD için BlueJeans ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Azure AD SSO için BlueJeans yapılandırın](#configure-bluejeans-for-azure-ad-sso)** - uygulama tarafında ki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Azure AD test kullanıcısı için BlueJeans oluşturun](#create-bluejeans-for-azure-ad-test-user)** - Kullanıcının Azure AD gösterimine bağlı Azure AD için BlueJeans'de B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/)Azure REKLAM uygulama tümleştirme **sayfasındaki BlueJeans,** **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.bluejeans.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak [için Azure AD İstemci destek ekibi için BlueJeans](https://support.bluejeans.com/contact) ile iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Azure **AD için BlueJeans'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Azure AD için BlueJeans erişimi sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde Azure **AD için BlueJeans'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-bluejeans-for-azure-ad-sso"></a>Azure AD SSO için BlueJeans'i yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Azure AD şirket sitenizde yönetici olarak **BlueJeans for BlueJeans'de** oturum açın.

2. **ADMIN \> GROUP AYARLARI \> GÜVENLİğİ'ne**gidin.

    ![Yönetici](./media/bluejeans-tutorial/ic785868.png "Yönetici")

3. **GÜVENLİk** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Tek Tabela](./media/bluejeans-tutorial/ic785869.png "SAML Tek Tabela")

    a. **SAML Tek İşareti'ni**seçin.

    b. **Otomatik sağlamayı etkinleştir'i**seçin.

4. Aşağıdaki adımları ile hareket edin:

    ![Sertifika Yolu](./media/bluejeans-tutorial/ic785870.png "Sertifika Yolu")

    a. Azure portalından indirdiğiniz base-64 kodlu sertifikayı yüklemek için **Dosyayı Seç'i**tıklatın.

    b. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    c. Parola **Değiştir URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Parola URL'sini Değiştir** değerini yapıştırın.

    d. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

5. Aşağıdaki adımları ile hareket edin:

    ![Değişiklikleri Kaydet](./media/bluejeans-tutorial/ic785874.png "Değişiklikleri Kaydet")

    a. Kullanıcı **Kimliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. **E-posta** metin kutusuna, yazın. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`

    c. **DEĞIŞIKLIKLERI KAYDET'i**tıklatın.

### <a name="create-bluejeans-for-azure-ad-test-user"></a>Azure AD test kullanıcısı için BlueJeans oluşturma

Bu bölümün amacı, Azure AD için BlueJeans'de B.Simon adında bir kullanıcı oluşturmaktır. Azure AD için BlueJeans, varsayılan olarak etkinleştirilen otomatik kullanıcı sağlamayı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](bluejeans-provisioning-tutorial.md) fazla bilgi bulabilirsiniz.

**Kullanım kılavuzunu el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:**

1. Azure AD şirket siteniz **için BlueJeans'de** yönetici olarak oturum açın.

2. ADMIN **MANAGE \> KULLANICILARI \> EKLE KULLANICI**'ya gidin.

    ![Yönetici](./media/bluejeans-tutorial/ic785877.png "Yönetici")

    > [!IMPORTANT]
    > **ADD USER** sekmesi yalnızca **SECUTIRY sekmesinde**otomatik **sağlamayı etkinleştirin** işaretli değilse kullanılabilir.

3. ADD **USER** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Ekle](./media/bluejeans-tutorial/ic785886.png "Kullanıcı Ekleme")

    a. **Ad metin** kutusuna, **B**gibi kullanıcının ilk adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    c. Azure AD Kullanıcı Adı metin kutusu **için BlueJeans seçin'de** **Brittasimon** gibi kullanıcı adını girin

    d. Parola metin kutusu **oluştur'da** parolanızı girin.

    e. **Şirket** metin kutusuna, Şirketiniz'e girin.

    f. **E-posta Adresi** metin kutusuna, `b.simon\@contoso.com`kullanıcının e-posta adresini girin.

    g. **Azure REKLAM Toplantısı Kimlik Kutusu için BlueJeans Oluştur'da** toplantı kimliğinizi girin.

    h. **Moderatör Parolası** metin kutusunu seçin'de parolanızı girin.

    i. **DEVAM'ı**tıklatın.

    ![Kullanıcı Ekle](./media/bluejeans-tutorial/ic785887.png "Kullanıcı Ekleme")

    J. **KULLANICI EKLE'yi**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Azure AD kullanıcı hesabı oluşturma araçları veya Azure AD için BlueJeans tarafından sağlanan API'ler için diğer BlueJeans'leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Azure REKLAM döşemesi için BlueJeans'i tıklattığınızda, SSO'yu ayarladığınız Azure AD için BlueJeans'te otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Azure AD için BlueJeans'i deneyin](https://aad.portal.azure.com/)

