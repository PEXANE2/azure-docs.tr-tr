---
title: 'Öğretici: Predictix Ürün Yelpazesi Planlama ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Etkin Dizin ve Predictix Ürün Yelpazesi Planlama arasında tek oturum açma nın nasıl yapılandırılabildiğini öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: bc3ea2f6fddc233a69d96c0c885ab310ed1e77c2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094162"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Öğretici: Predictix Ürün Yelpazesi Planlama ile Azure Active Directory entegrasyonu

Bu eğitimde, Predictix Ürün Yelpazesi Planlamasını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bu tümleştirme şu avantajları sağlar:

* Predictix Ürün Yelpazesi Planlama'ya kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Predictix Ürün Yelpazesi Planlama (tek oturum açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Predictix Ürün Yelpazesi Planlamaile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/pricing/free-trial/)bir hesap alabilirsiniz.
* Tek oturum açma özelliği etkin olan Predictix Ürün Yelpazesi Planlama aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* Predictix Ürün Yelpazesi Planlama SP tarafından başlatılan SSO'yu destekler.

## <a name="add-predictix-assortment-planning-from-the-gallery"></a>Galeriden Predictix Ürün YelpazesiNe Planlama ekleme

Predictix Ürün Yelpazesi Planlama'nın Azure AD'ye entegrasyonunu ayarlamak için, galeriden Predictix Ürün Yelpazesi Planlamasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **Predictix Ürün Yelpazesi NeşterPlanlaması'nı**girin. Arama sonuçlarında **Predictix Ürün Yelpazesi Planlama'yı** seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Predictix Ürün Yelpazesi Planlamaile Azure AD oturum açma işlemini yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, Predictix Ürün Yelpazesi Planlama'da bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Predictix Ürün YelpazesiNe Planlama ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Predictix Ürün Yelpazesi Planlamasını](#configure-predictix-assortment-planning-single-sign-on)** uygulama tarafında tek oturum açmayı yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir Predictix Ürün Yelpazesi Planlama testi kullanıcısı oluşturun.](#create-a-predictix-assortment-planning-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Predictix Ürün Yelpazesi Planlamaile Azure AD oturum açma işlemlerini yapılandırmak için şu adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **Predictix Ürün Yelpazesine Planlama** uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları tamamlayın.

    ![Temel SAML Yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:

       | |
        |--|
        | `https://<sub-domain>.ap.predictix.com/sso/request`|
        | `https://<sub-domain>.dev.ap.predictix.com/`|
        | |

    1. Tanımlayıcı **(Entity ID)** kutusuna, bu desene bir URL girin:

        | |
        |--|
        | `https://<sub-domain>.ap.predictix.com`|
        | `https://<sub-domain>.dev.ap.predictix.com`|
        | |

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'sini ve tanımlayıcıyı kullanmanız gerekir. Değerleri almak için [Predictix Ürün Yelpazesi Planlama destek ekibine](https://www.infor.com/support) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Sertifika **(Base64)** yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/certificatebase64.png)

6. **Predictix Ürün Çeşitlendirme Planlamasını Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-predictix-assortment-planning-single-sign-on"></a>Predictix Ürün Yelpazesi Planlamasını Tek Oturum Açma'yı Yapılandır

Predictix Ürün Çeşitliliği Planlama tarafında tek oturum açma işlemi yapmak için, indirdiğiniz sertifikayı ve Azure portalından kopyaladığınız URL'leri [Predictix Ürün Çeşitliliği Planlama destek ekibine](https://www.infor.com/support)göndermeniz gerekir. Bu ekip, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanınmasını sağlar.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'a Predictix Ürün Yelpazesi Planlamasına erişimini sağlayarak Azure AD oturumunu tek oturum açma olanağı sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **Predictix Ürün Yelpazesi Planlama'yı**seçin.

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Predictix Ürün Yelpazesi Planlamasını**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-predictix-assortment-planning-test-user"></a>Predictix Ürün YelpazesiNe Planlama test kullanıcısı oluşturma

Ardından, Predictix Ürün Yelpazesi Planlama'da Britta Simon adında bir kullanıcı oluşturmanız gerekir. Kullanıcıları eklemek için [Predictix Ürün Yelpazesi Planlama destek ekibiyle](https://www.infor.com/support) birlikte çalışın. Tek oturum açmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

> [!NOTE]
> Azure AD hesap sahibi bir e-posta alır ve hesabı etkin hale gelmeden önce onaylamak için bir bağlantı seçer.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki Predictix Ürün Yelpazesi Planlama döşemesini seçtiğinizde, SSO'yu kurduğunuz Predictix Ürün Yelpazesi Planlama örneğinde otomatik olarak oturum açmalısınız. Daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)