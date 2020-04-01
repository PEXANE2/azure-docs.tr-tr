---
title: 'Öğretici: Onit ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Onit arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e908cb76a57f027494230edc648b69da0730ac27
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70164235"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-onit"></a>Öğretici: Onit ile Azure Active Directory tek oturum açma (SSO) tümleştirmesi

Bu eğitimde, Onit'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Onit'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Onit erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Onit'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Onit tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Onit **SP** başlatılan SSO destekler

## <a name="adding-onit-from-the-gallery"></a>Galeriden Onit ekleme

Onit'in Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Yönetilen SaaS uygulamaları listenize Onit'i eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Onit** yazın.
1. Sonuçlar panelinden **Onit'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-onit"></a>Onit için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Onit ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Onit'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Onit ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Onit SSO'yu yapılandırın.](#configure-onit-sso)**
    1. **[Onit test kullanıcısını oluşturun](#create-onit-test-user)** - Onit'te kullanıcının Azure AD gösterimine bağlı b.simon'Un bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Onit** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<sub-domain>.onit.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<sub-domain>.onit.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Onit Client destek ekibine](https://www.onit.com/support) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, Parmak İzi Değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. **Onit'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Onit'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Onit'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-onit-sso"></a>Onit SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Onit şirket sitenize giriş yapın.

2. Üstteki menüde **İdare'yi**tıklatın.
   
    ![Yönetim](./media/onit-tutorial/IC791174.png "Yönetim")

3. **Şirketi Edit'i**tıklatın.
   
    ![Şirket'i Edit](./media/onit-tutorial/IC791175.png "Şirket'i Edit")
   
4. **Güvenlik** sekmesine tıklayın.
    
    ![Şirket Bilgilerini Edit](./media/onit-tutorial/IC791176.png "Şirket Bilgilerini Edit")

5. **Güvenlik** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma](./media/onit-tutorial/IC791177.png "Çoklu Oturum Açma")

    a. **Kimlik Doğrulama Stratejisi**olarak, Tek Oturum Açma ve **Parola'yı**seçin.
    
    b. **Idp Target URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. **Idp giriş URL** textbox'ına, Azure portalından kopyaladığınız **Logout URL**değerini yapıştırın.

    d. **Idp Cert Fingerprint (SHA1)** textbox'ına, Azure portalından kopyalamış olduğunuz sertifikanın **Parmak Izi** değerini yapıştırın.

### <a name="create-onit-test-user"></a>Onit test kullanıcıoluşturma

Azure AD kullanıcılarının Onit'e giriş yapabilmelerini sağlamak için, Bunların Onit'e dahil edilmesi gerekir. Onit durumunda, sağlama manuel bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak **Onit** şirket sitenizde oturum açın.

2. **Kullanıcı Ekle**'ye tıklayın.

    ![Yönetim](./media/onit-tutorial/IC791180.png "Yönetim")

3. Kullanıcı **Ekle** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Ekle](./media/onit-tutorial/IC791181.png "Kullanıcı Ekleme")

    a. Sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **Adını** ve **E-posta Adresini** ilgili metin kutularına yazın.

    b. **Oluştur'u**tıklatın.

    > [!NOTE]
    > Azure Etkin Dizin hesabı sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Onit döşemesini tıklattığınızda, SSO'yu kurduğunuz Onit'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Onit'i deneyin](https://aad.portal.azure.com/)