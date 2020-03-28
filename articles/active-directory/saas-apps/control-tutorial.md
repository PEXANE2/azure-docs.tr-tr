---
title: 'Öğretici: Denetim ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Süreklilik Denetimi arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1cb7f505-0d06-44b0-95b1-65b470e97092
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5a3383c6c79f3cde901c244eafb39bb0f479176
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67164920"
---
# <a name="tutorial-integrate-continuity-control-with-azure-active-directory"></a>Öğretici: Azure Etkin Dizini ile Süreklilik Denetimini Tümleştir

Bu eğitimde, Süreklilik Denetimi'ni (Denetim) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Denetimi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Denetim erişimi olan Azure AD'de yönetin.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Denetim'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Bir Denetim tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Denetim, **SP** tarafından başlatılan SSO'yı destekler.

## <a name="adding-control-from-the-gallery"></a>Galeriden Denetim Ekleme

Denetim'in Azure AD'ye tümleştirilmesini yapmak için galeriden yönetilen SaaS uygulamaları listenize Denetim eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Denetim** yazın.
1. Sonuç panelinden **Denetimi'ni** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak Denetim ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Denetim'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Denetim ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Denetim SSO'yu yapılandırın](#configure-control-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Denetim testi kullanıcısını oluşturun](#create-control-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olan Britta Simon in Control'ün bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Uygulama** tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanın değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.continuity.net/auth/saml`

    > [!Note]
    > Değer gerçek değil. Değeri doğru alt etki alanıyla güncelleştirin. SSO alt etki alanınız [Denetim Kimlik Doğrulama Stratejileri'nde](https://control.continuity.net/settings/account_profile#tab/security)yapılandırılabilir. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, Thumbprint'i** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. Denetimi **Ayarla** bölümünde, Giriş URL'sini kopyalayın ve bilgisayarınıza kaydedin.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-control-sso"></a>Yapı denetim SSO

**Denetim** tarafında tek oturum açma yapılandırmak için, [Denetim Kimlik Doğrulama Stratejileri'nde](https://control.continuity.net/settings/account_profile#tab/security)tek oturum açma kimlik doğrulama ayarlarını güncelleştirmeniz gerekir. **SAML SSO URL'sini** **Giriş URL'si** ve Azure portalındaki **Parmak Izi değeriyle** **Sertifika Parmak İzi** ile güncelleyin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Denetim'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde Denetim'i **seçin.**
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-control-test-user"></a>Denetim testi kullanıcısı oluşturma

Bu bölümde, Kontrol Britta Simon adlı bir kullanıcı oluşturun. Kullanıcıları Denetim platformuna eklemek için [Denetim destek ekibiyle](mailto:help@continuity.net) birlikte çalışın. **Kimlik Sağlayıcı Kullanıcı Kimliğini** Denetimde doldurmak için Britta Simon'ın Azure AD Kullanıcı **adını** kullanın. Kullanıcıların tek oturum açma dan yararlanmadan önce Denetim'de kimlikleri ve **Kimlik Sağlayıcı Kullanıcı Kimliği** ayarlanmalıdır.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki Denetim döşemesini seçtiğinizde, SSO'yu kurduğunuz Denetim'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
