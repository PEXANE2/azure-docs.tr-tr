---
title: 'Öğretici: NetDocuments ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve NetDocuments arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71e5e13485c4a10664d98363e8e99bfd3b4f4bcf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72035713"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netdocuments"></a>Öğretici: NetDocuments ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, NetDocuments'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. NetDocuments'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* NetDocuments erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla NetDocuments'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* NetDocuments tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* NetDocuments **SP** başlatılan SSO destekler

## <a name="adding-netdocuments-from-the-gallery"></a>Galeriden NetDocuments Ekleme

NetDocuments'un Azure AD'ye entegrasyonunu yapılandırmak için, galerideki NetDocuments'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **NetDocuments** yazın.
1. Sonuç panelinden **NetDocuments'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netdocuments"></a>NetDocuments için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak NetDocuments ile yapılandırma ve test edin. SSO'nun çalışması için, NetDocuments'ta bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu NetDocuments ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[NetDocuments SSO'yu yapılandırın](#configure-netdocuments-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[NetDocuments test kullanıcısını oluşturun](#create-netdocuments-test-user)** - Kullanıcının Azure AD gösterimine bağlı NetDocuments'ta B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **NetDocuments** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<Repository ID>`
    
    c. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`http://netdocuments.com/VAULT`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL'de gerçek Oturum Ve YanıtURL'i ile güncelleştirin. Depo **kimliği, CA** ile başlayan ve ardından NetDocuments Deponuzla ilişkili 8 karakter koduyla başlayan bir değerdir. Daha fazla bilgi için [NetDocuments Federe Kimlik destek belgesini](https://support.netdocuments.com/hc/en-us/articles/205220410-Federated-Identity-Login) kontrol edebilirsiniz. Alternatif olarak, yukarıdaki bilgileri kullanarak yapılandırmada güçlük yaşıyorsanız, bu değerleri almak için [NetDocuments İstemci destek ekibine](https://support.netdocuments.com/hc/) başvurabilirsiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. NetDocuments uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. NetDocuments **uygulaması, nameidentifier** **employeeid** veya **nameidentifier**olarak Kuruluşunuz için geçerli olan başka bir iddia ile eşlenecek bekliyor, bu nedenle **Düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşlemini değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **NetDocuments'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, NetDocuments'a erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **NetDocuments'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-netdocuments-sso"></a>NetDocuments SSO'da Yapılandır

1. Farklı bir web tarayıcısı penceresinde, NetDocuments şirket sitenizde yönetici olarak oturum açın.

2. **Admin'e**git.

3. **Ekle'yi tıklatın ve kullanıcıları ve grupları kaldırın.**
   
    ![Depo](./media/netdocuments-tutorial/ic795047.png "Depo")

4. **Gelişmiş kimlik doğrulama seçeneklerini yapıya kyapılandır'ı**tıklatın.
    
    ![Gelişmiş kimlik doğrulama seçeneklerini yapılandırma](./media/netdocuments-tutorial/ic795048.png "Gelişmiş kimlik doğrulama seçeneklerini yapılandırma")

5. **Federe Kimlik** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Federe Kimlik](./media/netdocuments-tutorial/ic795049.png "Federal Kimlik")
   
    a. **Federe kimlik sunucusu türü olarak,** **Active Directory Federation Services'ı**seçin.
   
    b. Azure portalından indirdiğiniz meta veri dosyasını yüklemek için **dosyayı seç'i**tıklatın.
   
    c. **Tamam**'a tıklayın.

### <a name="create-netdocuments-test-user"></a>NetDocuments test kullanıcısı oluşturma

Azure AD kullanıcılarının NetDocuments'da oturum açabilmeleri için NetDocuments'a dahil edilmeleri gerekir.  
NetDocuments durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **NetDocuments** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **Yönetici'yi**tıklatın.
   
    ![Yönetici](./media/netdocuments-tutorial/ic795051.png "Yönetici")

3. **Ekle'yi tıklatın ve kullanıcıları ve grupları kaldırın.**
   
    ![Depo](./media/netdocuments-tutorial/ic795047.png "Depo")

4. **E-posta Adresi** metin kutusuna, sağlamak istediğiniz geçerli bir Azure Etkin Dizin hesabının e-posta adresini yazın ve ardından **Kullanıcı Ekle'yi**tıklatın.
   
    ![E-posta Adresi](./media/netdocuments-tutorial/ic795053.png "E-posta Adresi")
   
    >[!NOTE]
    >Azure Active Directory hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bir bağlantı içeren bir e-posta alır. Azure Active Directory kullanıcı hesaplarını sağlamak için NetDocuments tarafından sağlanan diğer NetDocuments kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki NetDocuments döşemesini tıklattığınızda, SSO'yu kurduğunuz NetDocuments'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile NetDocuments'ı deneyin](https://aad.portal.azure.com/)

