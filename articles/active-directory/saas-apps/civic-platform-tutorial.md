---
title: 'Öğretici: Azure Active Directory entegrasyonu ile Toplumsal Platform | Microsoft Dokümanlar'
description: Azure Active Directory ve Toplumsal Platform arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d790454-143e-40ac-b3cb-5a256977b4db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ccf124c5a4160715df4e685e405dcd591c49ae7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68496829"
---
# <a name="tutorial-integrate-civic-platform-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile Toplumsal Platformu Tümleştirin

Bu eğitimde, Azure Active Directory (Azure AD) ile Toplumsal Platformu nasıl entegre acağınızı öğreneceksiniz. Toplumsal Platformu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de, Toplumsal Platform'a erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Civic Platform'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Civic Platform tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Toplumsal Platform **SP'den** başlatılan SSO'yu destekliyor





## <a name="adding-civic-platform-from-the-gallery"></a>Galeriden Toplumsal Platform Ekleme

Toplumsal Platformun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Civic Platform eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Toplumsal Platform** yazın.
1. Sonuç panelinden **Toplumsal Platform'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Toplumsal Platform ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Ilgili Kullanıcı Arasında Toplumsal Platform'da bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Toplumsal Platform ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Sivil Platform SSO'yu yapılandırın.](#configure-civic-platform-sso)**
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı Toplumsal Platform'da B.Simon'ın bir örneğine sahip olmak için **[Toplumsal Platform test kullanıcısını oluşturun.](#create-civic-platform-test-user)**
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Toplumsal Platform** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.accela.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna bir URL yazın:`civicplatform.accela.com`

    > [!NOTE]
    > URL değerindeki Oturum gerçek değildir. Bu değeri URL'deki gerçek Oturum'la güncelleştirin. Bu değeri almak için [Civic Platform Müşteri destek ekibine](mailto:skale@accela.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

1. Azure AD'deki **Azure Active Directory** > **App kayıtlarına** gidin, uygulamanızı seçin.

1. **Dizin (kiracı) kimliğini** kopyalayın ve Not Defteri'nde saklayın.

    ![Dizini (kiracı kimliği) kopyalayın ve uygulama kodunuzda saklayın](media/civic-platform-tutorial/directoryid.png)

1. Uygulama **Kimliğini** kopyalayın ve Not Defteri'ne saklayın.

   ![Başvuru (istemci) kimliğini kopyalama](media/civic-platform-tutorial/applicationid.png)

1. Azure AD'deki **Azure Active Directory** > **App kayıtlarına** gidin, uygulamanızı seçin. **Sertifikalar & sırları**seçin.

1. **İstemci sırlarını**seçin -> Yeni istemci sırrı .

1. Sırrın açıklamasını ve bir süreyi sağlayın. Bittiğinde **Ekle'yi**seçin.

   > [!NOTE]
   > İstemci sırrını kurtardıktan sonra istemci sırrının değeri görüntülenir. Anahtarı daha sonra alamadığınız için bu değeri kopyalayın.

   ![Bunu daha sonra alamadığınız için gizli değeri kopyalayın](media/civic-platform-tutorial/secretkey.png)

### <a name="configure-civic-platform-sso"></a>Sivil Platform SSO'yu Yapılandır

1. Yeni bir web tarayıcısı penceresi açın ve Atlassian Cloud şirket sitenizde yönetici olarak oturum açın.

1. Standart **Seçenekler'e**tıklayın.

    ![Sertifika indirme bağlantısı](media/civic-platform-tutorial/standard-choices.png)

1. Standart bir seçim **ssoconfig**oluşturun.

1. **Ssoconfig'i** arayın ve gönderin.

    ![Sertifika indirme bağlantısı](media/civic-platform-tutorial/sso-config.png)

1. Kırmızı nokta tıklayarak SSOCONFIG genişletin.

    ![Sertifika indirme bağlantısı](media/civic-platform-tutorial/sso-config01.png)

1. Aşağıdaki adımda SSO ile ilgili yapılandırma bilgilerini sağlayın:

    ![Sertifika indirme bağlantısı](media/civic-platform-tutorial/sso-config02.png)

    1. Uygulama **kimliği** alanında, Azure portalından kopyaladığınız **Uygulama Kimliği** değerini girin.

    1. **clientSecret** alanında, Azure portalından kopyaladığınız **Gizli** değeri girin.

    1. **Dizin** alanında, Azure portalından kopyaladığınız **Dizin (kiracı) Kimlik** değerini girin.

    1. idpName'yi girin. Örn:- `Azure`.

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

Bu bölümde, B.Simon'ın Toplumsal Platform'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Sivil Platform'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-civic-platform-test-user"></a>Toplumsal Platform test kullanıcıoluşturma

Bu bölümde, Toplumsal Platform'da B.Simon adında bir kullanıcı oluşturursunuz. Sivil Platform [Müşteri destek ekibindeki](mailto:skale@accela.com)kullanıcıları eklemek için Civic Platform destek ekibiyle birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Sivil Platform döşemesini tıklattığınızda, SSO'yu kurduğunuz Sivil Platform'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

