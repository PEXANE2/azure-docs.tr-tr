---
title: 'Öğretici: Özet ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Özet arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb167ab-d769-4661-a8cb-ae371cb63d2a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81886fa9165269d89bde8306c5829be41952c190
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68302596"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Öğretici: Özet'i Azure Active Directory ile tümleştir

Bu eğitimde, Özet'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Özet'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Özet'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Özet'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Soyut tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Özet, **SP ve IDP'nin** SSO'ya başlattığı destekler

## <a name="adding-abstract-from-the-gallery"></a>Galeriden Özet Ekleme

Özet'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Özet eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Özet** yazın.
1. Sonuç panelinden **Özet'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Özet ile yapılandırın ve test edin. SSO'nun çalışması için, Özet'te bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Özet ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Özet SSO'yu yapılandırın.](#configure-abstract-sso)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Özet test kullanıcısı oluşturun](#create-abstract-test-user)** - Kullanıcının Azure AD gösterimine bağlı Soyut'ta Britta Simon'ın bir örneğine sahip olmak için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Özet** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.abstract.com/signin`

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-abstract-sso"></a>Soyut SSO'nun yapılandırılsın

Özet'te SSO'yu yapılandırmanız gerektiğinden, sizin ve Azure portalından sizin `App Federation Metadata Url` ve azure `Azure AD Identifier` portalından aldığınızdan emin olun.

Bu bilgileri SAML sayfası **yla Tek Oturum Açma'yı ayarla'da** bulabilirsiniz:

* `App Federation Metadata Url` Saml **İmza Sertifikası** bölümünde yer alır.
* `Azure AD Identifier` **Özet'i Ayarla** bölümünde yer alır.


Artık SSO'ya Özet'te yapılandırmaya hazırsınız:

>[!Note]
>Özet'teki SSO ayarlarına erişmek için bir kuruluş Yöneticisi hesabıyla kimlik doğrulamanız gerekir.

1. Soyut [web uygulamasını](https://app.abstract.com/)açın.
2. Sol taraftaki çubuktaki **İzinler** sayfasına gidin.
3. **SSO'yu Yapılandır** bölümünde **Meta veri URL'nizi** ve **Entity Id'inizi**girin.
4. Sahip olabileceğiniz el ile özel durumları girin. Manuel özel durumlar bölümünde listelenen e-postalar SSO'yu atlar ve e-posta ve parolaile giriş yapabilecektir. 
5. **Değişiklikleri Kaydet**’e tıklayın.

>[!Note] 
>El ile özel durumlar listesinde birincil e-posta adreslerini kullanmanız gerekir. Listedeki e-posta kullanıcının ikincil e-postasıysa SSO etkinleştirme başarısız olur. Bu durumda, başarısız hesabın birincil e-postasıyla birlikte bir hata iletisi görürsünüz. Kullanıcıyı tanıdığınızı doğruladıktan sonra bu birincil e-postayı el ile özel durumlar listesine ekleyin.

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

Bu bölümde, B.Simon'ın Özet'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Özet'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-abstract-test-user"></a>Özet test kullanıcısı oluşturma

SSO'u Özet'te test etmek için:

1. Soyut [web uygulamasını](https://app.abstract.com/)açın.
2. Sol taraftaki çubuktaki **İzinler** sayfasına gidin.
3. **Hesabımla Test'i**tıklatın. Test başarısız olursa, lütfen [destek ekibimizle iletişime geçin.](https://www.abstract.com/help/contact/)

>[!Note]
>Özet'teki SSO ayarlarına erişmek için bir kuruluş Yöneticisi hesabıyla kimlik doğrulamanız gerekir.
Bu kuruluş Yöneticisi hesabının Azure portalında Özet'e atanması gerekir.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Özet döşemeyi tıklattığınızda, SSO'yu kurduğunuz Özet'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

