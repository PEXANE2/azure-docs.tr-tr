---
title: 'Öğretici: ThirdLight ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve ThirdLight arasında tek oturum açma yı nasıl yapılandırabileceğinizi öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 448d46cd21a63488c4f567d5555fe6406fc0fa73
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67089098"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Öğretici: ThirdLight ile Azure Active Directory entegrasyonu

Bu eğitimde, ThirdLight'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Bu tümleştirme şu avantajları sağlar:

* Azure AD'yi kullanarak ThirdLight'a kimlerin erişebileceğini kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak ThirdLight'ta (tek oturum açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Etkin Dizini'ndeki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ThirdLight ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma özelliği etkin olan bir ThirdLight aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* ThirdLight, SP tarafından başlatılan SSO'yı destekler.

## <a name="add-thirdlight-from-the-gallery"></a>Galeriden ThirdLight ekleme

ThirdLight'ın Azure AD'ye entegrasyonunu ayarlamak için, galeriden ThirdLight'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **ThirdLight**girin. Arama sonuçlarında **ThirdLight'ı** seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD oturum açma işlemini ThirdLight ile yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, Bir Azure REKLAM kullanıcısı ile ThirdLight'taki ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturum açma işlemlerini ThirdLight ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında **[ThirdLight tek oturum](#configure-thirdlight-single-sign-on)** açmayı yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir ThirdLight test kullanıcısı oluşturun.](#create-a-thirdlight-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini ThirdLight ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)ThirdLight uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları tamamlayın.

    ![Temel SAML Yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:
    
          `https://<subdomain>.thirdlight.com/`

    1. Tanımlayıcı **(Entity ID)** kutusuna, bu desene bir URL girin:

       `https://<subdomain>.thirdlight.com/saml/sp`

       > [!NOTE]
       > Bu değerler yer tutuculardır. Gerçek oturum açma URL'sini ve tanımlayıcıyı kullanmanız gerekir. Değerleri almak için [ThirdLight destek ekibine](https://www.thirdlight.com/support) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Federation **Metadata XML'nin**yanındaki **İndir** bağlantısını seçin ve dosyayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/metadataxml.png)

6. **ThirdLight'ı ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-thirdlight-single-sign-on"></a>ThirdLight tek oturum açmayı yapılandırma

1. Yeni bir web tarayıcısı penceresinde, Yönetici olarak ThirdLight şirket sitenizde oturum açın.

1. **Yapılandırma** > **Sistemi Yönetimi** > **SAML2**gidin:

    ![Sistem Yönetimi](./media/thirdlight-tutorial/ic805843.png "Sistem Yönetimi")

1. SAML2 yapılandırma bölümünde aşağıdaki adımları izleyin.
  
    ![SAML2 yapılandırma bölümü](./media/thirdlight-tutorial/ic805844.png "SAML2 yapılandırma bölümü")

    1. **SAML2 Tek Oturum Aç'ı Etkinleştir'i**seçin.

    1. **IdP Metadata kaynak**altında, **XML'den Load IdP Metaverilerini**seçin.

    1. Önceki bölümde Azure portalından indirdiğiniz meta veri dosyasını açın. Dosyanın içeriğini kopyalayın ve **IdP Metadata XML** kutusuna yapıştırın.

    1. **SAML2 ayarlarını kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Pencerenin üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde Britta Simon'ın ThirdLight'a erişimini sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **ThirdLight'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ThirdLight'ı**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından pencerenin altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Pencerenin altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-thirdlight-test-user"></a>ThirdLight test kullanıcısı oluşturma

Azure AD kullanıcılarının ThirdLight'ta oturum açabilmesini sağlamak için bunları ThirdLight'a eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları izleyin:

1. Yönetici olarak ThirdLight şirket sitenizde oturum açın.

1. **Kullanıcılar** sekmesine gidin.

1. **Kullanıcıları ve Grupları**seçin.

1. **Yeni Kullanıcı Ekle'yi**seçin.

1. Kullanıcı adını, adı veya açıklamayı ve sağlamak istediğiniz geçerli bir Azure REKLAM hesabının e-posta adresini girin. Bir Önceden Ayarlanmış veya Yeni Üye Grubu seçin.

1. **Oluştur'u**seçin.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için ThirdLight tarafından sağlanan herhangi bir kullanıcı hesabı oluşturma aracını veya API'yi kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki ThirdLight döşemesini seçtiğinizde, SSO'yu kurduğunuz ThirdLight örneğinde otomatik olarak oturum açmalısınız. Access Paneli hakkında daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
