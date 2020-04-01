---
title: 'Öğretici: Aha ile Azure Active Directory entegrasyonu! | Microsoft Belgeleri'
description: Azure Active Directory ve Aha arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin!.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a63872680d28664c6d5a7ff109f6de72817173d5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68989656"
---
# <a name="tutorial-integrate-aha-with-azure-active-directory"></a>Öğretici: Entegre Aha! Azure Active Directory ile

Bu öğreticide, Nasıl Aha entegre öğreneceksiniz! Azure Etkin Dizin (Azure AD) ile. Aha'yı entegre ettiğinde! Azure AD ile şunları yapabilirsiniz:

* Aha erişimi olan Azure AD'de denetim!.
* Kullanıcılarınızın Aha'da otomatik olarak oturum açmış olmasını sağla! Azure AD hesaplarıyla birlikte.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Aha! tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Aha! **SP** başlatılan SSO destekler
* Aha! **Just In Time** kullanıcı sağlama destekler

## <a name="adding-aha-from-the-gallery"></a>Aha ekleme! galeriden

Aha entegrasyon yapılandırmak için! Azure AD içine, Aha eklemeniz gerekir! galeriden yönetilen SaaS uygulamaları listenize kadar.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, **Aha yazın!** yazın.
1. **Aha'yı seçin!** sonuç panelinden ve sonra uygulama ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-aha"></a>Aha için Azure AD oturum açma işlemlerini yapılandırın ve test edin!

Azure AD SSO'nun Aha ile yapılandırılVe test edin! **B.Simon**adlı bir test kullanıcısı kullanarak. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Aha'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir!

Azure AD SSO'yu Aha! ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[Aha'yı yapılandır! SSO](#configure-aha-sso)** - uygulama tarafında Tek Oturum Açma ayarlarını yapılandırmak için.
    1. **[Aha oluşturun! test kullanıcı](#create-aha-test-user)** - Aha B.Simon bir meslektaşı olması! bu, kullanıcının Azure AD gösterimine bağlıdır.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Aha'da!** uygulama tümleştirme sayfasını, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.aha.io/session/new`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.aha.io`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Aha ile temasa [geçin! ](https://www.aha.io/company/contact)Bu değerleri almak için istemci destek ekibi. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Aha'yı kurşumda!** bölüm, gereksiniminize göre uygun URL(ler) kopyalayın.

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

Bu bölümde, B.Simon'ın Aha!'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız!

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Aha'yı seçin!**
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-aha-sso"></a>Aha'yı yapılandır! SSO

1. Aha içinde yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra **Kurulum Aha'ya tıklayın!** Seni Aha'ya yönlendirecek! Uygulama. Oradan, Aha oturum için yönetici kimlik bilgilerini sağlayın!. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Eğer Aha kurmak istiyorsanız! el ile, yeni bir web tarayıcısı penceresi açın ve Aha oturum açın! yönetici olarak şirket sitesi ve aşağıdaki adımları gerçekleştirin:

4. Üstteki menüde **Ayarlar'ı**tıklatın.

    ![Ayarlar](./media/aha-tutorial/IC798950.png "Ayarlar")

5. **Hesap'ı**tıklatın.
  
    ![Profil](./media/aha-tutorial/IC798951.png "Profil")

6. **Güvenlik'i ve tek oturum açma'yı**tıklatın.

    ![Güvenlik ve tek oturum açma](./media/aha-tutorial/IC798952.png "Güvenlik ve tek oturum açma")

7. Kimlik **Sağlayıcısı**olarak **Tek Oturum Açma** bölümünde **SAML2.0'ı**seçin.

    ![Güvenlik ve tek oturum açma](./media/aha-tutorial/IC798953.png "Güvenlik ve tek oturum açma")

8. Tek **Oturum Açma** yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma](./media/aha-tutorial/IC798954.png "Çoklu Oturum Açma")

    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın.

    b. **Kullanarak Yapılandırmak**için **Meta veri dosyalarını**seçin.

    c. İndirdiğiniz meta veri dosyanızı yüklemek için **Gözat'ı**tıklatın.

    d. **Güncelleştir**’e tıklayın.

### <a name="create-aha-test-user"></a>Aha'yı yarat! test kullanıcısı

Bu bölümde, B.Simon adlı bir kullanıcı Aha oluşturulur!. Aha! varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Aha!'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Aha'ya tıkladığınızda! Erişim Paneli'ndeki döşeme, otomatik olarak Aha'da oturum açmış olmalısınız! bunun için SSO kurmak. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

