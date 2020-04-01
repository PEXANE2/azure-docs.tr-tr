---
title: 'Öğretici: O.C. ile Azure Active Directory tümleştirmesi Tanner - AppreciateHub | Microsoft Dokümanlar'
description: Azure Active Directory ve O.C. arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin. Tanner - AppreciateHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/28/2020
ms.author: jeedes
ms.openlocfilehash: 768756e013a4301ea11f5f9478178bdad46f9d67
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76984056"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-oc-tanner---appreciatehub"></a>Öğretici: O.C. ile Azure Active Directory tek oturum açma (SSO) tümleştirmesi Tanner - AppreciateHub

Bu eğitimde, O.C.'yi nasıl entegre edeceğinizi öğreneceksiniz. Tanner - Azure Etkin Dizin (Azure AD) ile AppreciateHub. O.C.'yi entegre ettiğinde. Tanner - Azure AD ile AppreciateHub, şunları yapabilirsiniz:

* Azure AD'de O.C.'ye erişimi olan denetim Tanner - AppreciateHub.
* Kullanıcılarınızın O.C.'de otomatik olarak oturum açmış olmasını sağlar. Tanner - Azure AD hesaplarıyla AppreciateHub.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* O.C. Tanner - AppreciateHub tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* O.C. Tanner - AppreciateHub **IDP** başlatılan SSO destekler

* O.C.'yi yapılandırdıktan sonra. Tanner - AppreciateHub, kuruluşunuzun hassas verilerinin sızma ve sızmalarını gerçek zamanlı olarak koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. ekleme Tanner - Galeriden AppreciateHub

O.C.'nin entegrasyonunu yapılandırmak için. Tanner - Azure AD içine AppreciateHub, O.C eklemeniz gerekir. Tanner - AppreciateHub galeriden yönetilen SaaS uygulamaları listenize kadar.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **O.C. Tanner - AppreciateHub** yazın.
1. Sonuç panelinden **O.C. Tanner - AppreciateHub'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-oc-tanner---appreciatehub"></a>Azure AD oturum açma'yı O.C. için yapılandırın ve test edin. Tanner - AppreciateHub

Azure AD SSO'su O.C. ile yapılandırın ve test edin. Tanner - AppreciateHub **B.Simon**adlı bir test kullanıcısı kullanarak . SSO'nun çalışması için, bir Azure AD kullanıcısı ile O.C.'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir. Tanner - AppreciateHub.

Azure AD SSO'u O.C. ile yapılandırmak ve test etmek için. Tanner - AppreciateHub, aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[O.C. Tanner -AppreciateHub SSO- yapılandırın.](#configure-oc-tanner---appreciatehub-sso)**
    * **[O.C. Tanner oluşturun - AppreciateHub test kullanıcı](#create-oc-tanner---appreciatehub-test-user)** - O.C B.Simon bir meslektaşı olması. Tanner - Kullanıcının Azure AD gösterimine bağlı AppreciateHub.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **O.C. Tanner - AppreciateHub** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **O.C. Tanner - AppreciateHub'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, B.Simon'ın O.C.'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız. Tanner - AppreciateHub.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar **listesinde, O.C. Tanner - AppreciateHub'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-oc-tanner---appreciatehub-sso"></a>O.C.'yi yapılandırın. Tanner - AppreciateHub SSO

**O.C. Tanner - AppreciateHub** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'i** ve uygun kopyalanmış URL'leri Azure portalından [O.C. Tanner - AppreciateHub destek ekibine](mailto:sso@octanner.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-oc-tanner---appreciatehub-test-user"></a>O.C. oluştur. Tanner - AppreciateHub test kullanıcısı

Bu bölümün amacı O.C.'de Britta Simon adında bir kullanıcı oluşturmaktır. Tanner - AppreciateHub.

**O.C. Tanner - AppreciateHub'da Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

[O.C. Tanner - AppreciateHub destek ekibinizden, Azure AD'deki](mailto:sso@octanner.com) Britta Simon kullanıcı adı ile aynı değere sahip bir kullanıcı oluşturmasını isteyin.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

O.C.'yi tıklattığında. Tanner - Access Paneli'ndeki AppreciateHub döşemesi, Otomatik Olarak O.C.'de oturum açmış olmalısınız. Tanner - SSO'ya kurup kurduğunuz AppreciateHub. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [O.C. Tanner deneyin - Azure AD ile AppreciateHub](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [O.C. Tanner nasıl korunur - Gelişmiş görünürlük ve kontrollerle AppreciateHub](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)