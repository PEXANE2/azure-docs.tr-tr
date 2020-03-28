---
title: 'Öğretici: Üyeler için Mavi Erişim ile Azure Active Directory tek oturum açma (SSO) entegrasyonu (BAM) | Microsoft Dokümanlar'
description: Azure Etkin Dizini ile Üyeler için Mavi Erişim (BAM) arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2680ada0-88e4-46bd-8f08-e9e7ae8aafcd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7dd09f80721bb4180eb5f2c25f3315e6211c63cc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74085474"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-blue-access-for-members-bam"></a>Öğretici: Üyeler için Mavi Erişim ile Azure Active Directory tek oturum açma (SSO) entegrasyonu (BAM)

Bu eğitimde, Üyeler için Mavi Erişim'i (BAM) Azure Etkin Dizini (Azure AD) ile nasıl entegre ettiğinizi öğreneceksiniz. Üyeler için Mavi Erişim'i (BAM) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Üyeler için Mavi Erişim'e (BAM) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Üyeler için Mavi Erişim 'de (BAM) otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Üyeler için Mavi Erişim (BAM) tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.


* Üyeler için Mavi Erişim (BAM) **IDP** başlatılan SSO destekler




## <a name="adding-blue-access-for-members-bam-from-the-gallery"></a>Galeriden Üyeler için Mavi Erişim (BAM) ekleme

Üyeler için Mavi Erişim'in (BAM) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Üyeler için Mavi Erişim'i (BAM) yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Üyeler için Mavi Erişim (BAM)** yazın.
1. Sonuçlar panelinden **Üyeler için Mavi Erişim'i (BAM)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-blue-access-for-members-bam"></a>Üyeler için Mavi Erişim için Azure AD oturum açma işlemlerini yapılandırma ve test edin (BAM)

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Üyeler için Mavi Erişim (BAM) ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Üyeler için Mavi Erişim 'deki (BAM) ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Üyeler için Mavi Erişim (BAM) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Üyeler için Mavi Erişimi (BAM) SSO'yu yapılandırın](#configure-blue-access-for-members-bam-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Üyeler için Mavi Erişim (BAM) test kullanıcısı oluşturun](#create-blue-access-for-members-bam-test-user)** - kullanıcının Azure AD gösterimine bağlı Üyeler için Mavi Erişim'de (BAM) B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Üyeler için Mavi Erişim (BAM)** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<Custom Domain Value>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<CUSTOMURL>/affwebservices/public/saml2assertionconsumer`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<CUSTOMURL>/BAMSSOServlet/sso/BamInboundSsoServlet`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL ve Röle Durumu ile güncelleştirin. Bu değerleri almak [için Üyeler için Mavi Erişim (BAM) İstemci destek ekibine](https://www.bcbstx.com/contact-us) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Üyeler için Mavi Erişim (BAM) uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Üyeler için Mavi Erişim (BAM) uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ------------ | --------- |
    | ClientID | `<ClientID>` |
    | Uıd | `<UID>` |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üyeler **için Mavi Erişim Ayarla (BAM)** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Üyeler için Mavi Erişim 'e (BAM) erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde Üyeler **için Mavi Erişim (BAM)** seçeneğini belirleyin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-blue-access-for-members-bam-sso"></a>Üyeler için Mavi Erişimi Yapılandır (BAM) SSO

Üyeler için Mavi Erişim **(BAM)** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'i** ve uygun kopyalanmış URL'leri Azure portalından [Üyeler için Mavi Erişim (BAM) destek ekibine](https://www.bcbstx.com/contact-us)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-blue-access-for-members-bam-test-user"></a>Üyeler için Mavi Erişim (BAM) test kullanıcısı oluşturma

Bu bölümde, Üyeler için Mavi Erişim 'de (BAM) B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcılar üyeler [için Mavi Erişim (BAM) destek ekibiyle](https://www.bcbstx.com/contact-us) çalışarak Üyelerine Mavi Erişim (BAM) platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Üyeler için Mavi Erişim (BAM) döşemesini tıklattığınızda, SSO'yu ayarladığınız Üyeler için Mavi Erişim'de (BAM) otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Üyeler için Mavi Erişimi (BAM) deneyin](https://aad.portal.azure.com/)