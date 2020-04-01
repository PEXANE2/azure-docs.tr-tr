---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) zoom ile tümleştirme | Microsoft Dokümanlar'
description: Azure Active Directory ve Zoom arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f9d727154adf0a2099d7a9144c109cef9c91238
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70743978"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zoom"></a>Öğretici: Azure Active Directory tek oturum açma (SSO) tümleştirmesi ile Yakınlaştırma

Bu eğitimde, Yakınlaştırma'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Yakınlaştırma'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Yakınlaştırma'ya erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Yakınlaştırma için otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) özellikli aboneliği yakınlaştırma.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Zoom, **SP** tarafından başlatılan SSO'ya ve 
* [ **Yakınlaştırma, Otomatik** kullanıcı sağlamayı](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)destekler.

## <a name="adding-zoom-from-the-gallery"></a>Galeriden Yakınlaştırma Ekleme

Azure AD'ye Yakınlaştırma'nın tümleştirilmesini yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Yakınlaştırma eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Yakınlaştır'ı** yazın.
1. Sonuçlar panelinden **Yakınlaştır'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zoom"></a>Azure AD oturum açma özelliğini Yakınlaştırma için yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Yakınlaştırma ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Yakınlaştırma'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Yakınlaştırma ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Yakınlaştırma SSO'yu yapılandırın.](#configure-zoom-sso)**
    1. **[Yakınlaştırma testi kullanıcısını oluşturun](#create-zoom-test-user)** - Kullanıcının Azure AD gösterimine bağlı Yakınlaştırma'daki B.Simon'ın bir örneğine sahip olmak için.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Yakınlaştırma** uygulaması tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.zoom.us`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<companyname>.zoom.us`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Yakınlaştırma İstemci destek ekibine](https://support.zoom.us/hc/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Yakınlaştırma yı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

> [!NOTE]
> Azure AD'de Rolü nasıl yapılandırılabildiğini öğrenmek [için, kurumsal uygulamalar için SAML belirtecinde verilen rol iddiasını yapılandır'a](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)bakın.

> [!NOTE]
> Yakınlaştırma, SAML yükünde bir grup talebi bekleyebilir. Herhangi bir grup oluşturduysanız, grup bilgilerini kendi uçlarında yapılandırabilmeleri için grup bilgileriyle [yakınlaştırma Istemcisi destek ekibine](https://support.zoom.us/hc/) başvurun. Ayrıca, nesne kimliğini kendi uçlarında yapılandırabilmeleri için [Yakınlaştırma İstemci destek ekibine](https://support.zoom.us/hc/) Nesne Kimliği sağlamanız gerekir. Nesne Kimliğini almak için Azure [ile YakınlaştırmaYı Yapılandırma](https://support.zoom.us/hc/articles/115005887566)konusuna bakın.

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

Bu bölümde, B.Simon'ın Yakınlaştırma'ya erişim sağlayarak Azure tek oturum açma özelliğini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Yakınlaştırma'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-zoom-sso"></a>Yakınlaştırma SSO'su yapılandır

1. Farklı bir web tarayıcısı penceresinde, Zoom şirket sitenizde yönetici olarak oturum açın.

2. Tek **Oturum Açma** sekmesini tıklatın.

    ![Tek oturum açma sekmesi](./media/zoom-tutorial/ic784700.png "Çoklu oturum açma")

3. Güvenlik **Denetimi** sekmesini tıklatın ve ardından **Tek Oturum Açma** ayarlarına gidin.

4. Tek Oturum Açma bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek oturum açma bölümü](./media/zoom-tutorial/ic784701.png "Çoklu oturum açma")

    a. Oturum **Açma sayfası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    b. **Oturum açma sayfası URL** değeri için Azure portalına gitmeniz ve soldaki Azure Etkin **Dizini'ni** tıklatmanız ve ardından **Uygulama kayıtlarına gitmeniz**gerekir.

    ![Azure Etkin Dizin düğmesi](./media/zoom-tutorial/appreg.png)

    c. Bitiş **Noktaları'na** tıklayın

    ![Bitiş noktası düğmesi](./media/zoom-tutorial/endpoint.png)

    d. **SAML-P SIGN-OUT ENDPOINT'i** kopyalayın ve **Oturum açsayfa URL** textbox'a yapıştırın.

    ![Bitiş noktasını kopyala düğmesi](./media/zoom-tutorial/endpoint1.png)

    e. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Kimlik sağlayıcısı sertifika** metin kutusuna yapıştırın.

    f. **Veren** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın** değerini yapıştırın. 

    g. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Daha fazla bilgi için yakınlaştırma belgelerini ziyaret edin[https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

### <a name="create-zoom-test-user"></a>Yakınlaştırma testi kullanıcısı oluşturma

Bu bölümün amacı Zoom B.Simon adlı bir kullanıcı oluşturmaktır. Yakınlaştırma, varsayılan olarak etkin olan otomatik kullanıcı sağlamayı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial) fazla bilgi bulabilirsiniz.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Yakınlaştırma İstemci destek ekibine](https://support.zoom.us/hc/) başvurmanız gerekir

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Yakınlaştırma döşemesini tıklattığınızda, SSO'yu kurduğunuz Yakınlaştırma'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Yakınlaştırma'yı deneyin](https://aad.portal.azure.com/)
