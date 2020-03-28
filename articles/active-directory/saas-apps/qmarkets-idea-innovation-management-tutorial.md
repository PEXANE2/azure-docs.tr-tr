---
title: 'Öğretici: Qmarkets Idea & İnovasyon Yönetimi ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Qmarkets Idea & İnovasyon Yönetimi arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5ca125c7-f778-4a2d-a573-7cebe1ff634d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d3e3f86d761a686993e6ecf32718aa2e15dac92
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74536291"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-qmarkets-idea--innovation-management"></a>Öğretici: Qmarkets Idea & İnovasyon Yönetimi ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Qmarkets Idea & Yenilik Yönetimi'ni Azure Etkin Dizin (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. Qmarkets Idea & İnovasyon Yönetimi'ni Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Qmarkets Idea & İnovasyon Yönetimi'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Qmarkets Idea & İnovasyon Yönetimi'nde otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Qmarkets Idea & İnovasyon Yönetimi tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.



* Qmarkets Idea & İnovasyon Yönetimi **SP ve IDP** SSO'ya destek verdi
* Qmarkets Idea & İnovasyon Yönetimi **Just In Time** kullanıcı sağlama destekler


## <a name="adding-qmarkets-idea--innovation-management-from-the-gallery"></a>Galeriden Qmarkets Idea & İnovasyon Yönetimi ekleme

Qmarkets Idea & İnovasyon Yönetimi'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Qmarkets Idea & İnovasyon Yönetimi'ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Qmarkets Idea & İnovasyon Yönetimi** yazın.
1. Sonuç panelinden **Qmarkets Idea & İnovasyon Yönetimi'ni** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-qmarkets-idea--innovation-management"></a>Qmarkets Idea & İnovasyon Yönetimi için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'& Qmarkets Idea ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Qmarkets Idea & İnovasyon Yönetimi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Qmarkets Idea & İnovasyon Yönetimi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Qmarkets Idea'yı & İnovasyon Yönetimi SSO'yu uygulama](#configure-qmarkets-idea--innovation-management-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. **[Qmarkets Idea & İnovasyon Yönetimi test kullanıcısı oluşturun](#create-qmarkets-idea--innovation-management-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Qmarkets Idea & İnovasyon Yönetimi'nde B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Qmarkets Idea & İnovasyon Yönetimi** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<app_url>/sso/saml2/metadata/qmarkets_sp_<endpoint_id>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<app_url>/sso/saml2/acs/qmarkets_sp_<endpoint_id>`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<app_url>/sso/saml2/endpoint/qmarkets_sp_<endpoint_id>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Qmarkets Idea & İnovasyon Yönetimi Müşteri destek ekibiyle](mailto:support@qmarkets.net) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın Qmarkets Idea & İnovasyon Yönetimi'ne erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **Qmarkets Idea & İnovasyon Yönetimi'ni**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-qmarkets-idea--innovation-management-sso"></a>Qmarkets Fikir & İnovasyon Yönetimi SSO yapılandırılması

**Qmarkets Idea & İnovasyon Yönetimi** tarafında tek oturum açma yapmak **için, App Federation Metadata Url'yi** [Qmarkets Idea & İnovasyon Yönetimi destek ekibine](mailto:support@qmarkets.net)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-qmarkets-idea--innovation-management-test-user"></a>Qmarkets Idea & İnovasyon Yönetimi test kullanıcısı oluşturun

Bu bölümde, Britta Simon adlı bir kullanıcı Qmarkets Idea & İnovasyon Yönetimi oluşturulur. Qmarkets Idea & İnovasyon Yönetimi, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Qmarkets Idea & İnovasyon Yönetimi'nde bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Qmarkets Idea & İnovasyon Yönetimi karosu'nu tıklattığınızda, SSO'yu kurduğunuz Qmarkets Idea & İnovasyon Yönetimi'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Qmarkets Idea & İnovasyon Yönetimini Deneyin](https://aad.portal.azure.com/)

