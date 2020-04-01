---
title: 'Öğretici: RolePoint ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve RolePoint arasında tek oturum açma yapmayı öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/15/2019
ms.author: jeedes
ms.openlocfilehash: 0b6fd17d2f8577532778733866260f43e9ac7685
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67092728"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Öğretici: RolePoint ile Azure Active Directory entegrasyonu

Bu eğitimde, RolePoint'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bu tümleştirme şu avantajları sağlar:

* RolePoint'e kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla RolePoint'te (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini RolePoint ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma etkinleştirilmiş bir RolePoint aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* RolePoint, SP tarafından başlatılan SSO'yı destekler.

## <a name="add-rolepoint-from-the-gallery"></a>Galeriden RolePoint ekleme

RolePoint'in Azure AD'ye entegrasyonunu ayarlamak için, galeriden yönetilen SaaS uygulamaları listenize RolePoint eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **RolePoint**girin. Arama sonuçlarında **RolePoint'i** seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak RolePoint ile Azure AD oturum açma işlemini yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, RolePoint'teki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturum açma işlemlerini RolePoint ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında **[RolePoint tek oturum](#configure-rolepoint-single-sign-on)** açma yı yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir RolePoint test kullanıcısı oluşturun.](#create-a-rolepoint-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini RolePoint ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında](https://portal.azure.com/)RolePoint uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Temel SAML Yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:

       `https://<subdomain>.rolepoint.com/login`

    1. Tanımlayıcı **(Entity ID)** kutusuna, bu desene bir URL girin:

       `https://app.rolepoint.com/<instancename>`

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'sini ve tanımlayıcıyı kullanmanız gerekir. Tanımlayıcıda benzersiz bir dize değeri kullanmanızı öneririz. Bu değerleri almak için [RolePoint destek ekibine](mailto:info@rolepoint.com) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Federation **Metadata XML'nin**yanındaki **İndir** bağlantısını seçin ve dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme linki](common/metadataxml.png)

6. **RolePoint'i ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.


### <a name="configure-rolepoint-single-sign-on"></a>RolePoint tek oturum açma yı yapılandırma

RolePoint tarafında tek oturum açma ayarlamak için [RolePoint destek ekibiyle](mailto:info@rolepoint.com)çalışmanız gerekir. Bu ekibe Federasyon Metadata XML dosyasını ve Azure portalından aldığınız URL'leri gönderin. Saml SSO bağlantısının her iki tarafta da düzgün şekilde ayarlandığından emin olmak için RolePoint'i yapılandıracaklar.

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

Bu bölümde, Britta Simon'A RolePoint erişimi vererek Azure tek oturum açma olanağı nı kullanacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **RolePoint'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **RolePoint'i**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından pencerenin altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Pencerenin altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-rolepoint-test-user"></a>RolePoint test kullanıcısı oluşturma

Ardından, RolePoint'te Britta Simon adında bir kullanıcı oluşturmanız gerekir. Kullanıcıları RolePoint'e eklemek için [RolePoint destek ekibiyle](mailto:info@rolepoint.com) birlikte çalışın. Tek oturum açma dan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki RolePoint döşemesini seçtiğinizde, SSO'yu kurduğunuz RolePoint örneğinde otomatik olarak oturum açmış olmanız gerekir. Access Paneli hakkında daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
