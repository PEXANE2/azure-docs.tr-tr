---
title: 'Öğretici: Predictix Fiyat Raporlaması ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve Predictix Fiyat Raporlaması arasında tek oturum açma işlemlerini nasıl yapılandırabileceğinizi öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 808b2d964bb39af6b410a84563717102ebece454
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094102"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Öğretici: Predictix Fiyat Raporlaması ile Azure Active Directory entegrasyonu

Bu eğitimde, Predictix Fiyat Raporlama'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Bu tümleştirme şu avantajları sağlar:

* Predictix Fiyat Raporlaması'na kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Predictix Fiyat Raporlaması'nda (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD entegrasyonunu Predictix Fiyat Raporlaması ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği için kaydolabilirsiniz.
* Tek oturum açma özelliği etkin olan Predictix Fiyat Raporlama aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* Predictix Fiyat Raporlama, SP tarafından başlatılan SSO'yı destekler.

## <a name="adding-predictix-price-reporting-from-the-gallery"></a>Galeriden Predictix Fiyat Raporlaması Ekleme

Predictix Fiyat Raporlamasının Azure AD'ye entegrasyonunu ayarlamak için, galeriden yönetilen SaaS uygulamaları listenize Predictix Fiyat Raporlaması eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **Predictix Fiyat Raporlaması'nı**girin. Arama sonuçlarında **Predictix Fiyat** Raporlaması'nı seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD oturum açma işlemini Predictix Fiyat Raporlaması ile yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için Predictix Fiyat Raporlama'da bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Predictix Fiyat Raporlaması ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Predictix Fiyat Raporlamasını](#configure-predictix-price-reporting-single-sign-on)** uygulama tarafında tek oturum açma yı yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir Predictix Fiyat Raporlama test kullanıcısı oluşturun.](#create-a-predictix-price-reporting-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Predictix Fiyat Raporlaması ile Azure AD oturum açma işlemlerini yapılandırmak için şu adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **Predictix Fiyat Raporlama** uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları tamamlayın.

    ![Temel SAML Yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:

       `https://<companyname-pricing>.predictix.com/sso/request`

    1. Tanımlayıcı **(Entity ID)** kutusuna, bu desene bir URL girin:

        | |
        |--|
        | `https://<companyname-pricing>.predictix.com` |
        | `https://<companyname-pricing>.dev.predictix.com` |
        | |

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'sini ve tanımlayıcıyı kullanmanız gerekir. Değerleri almak için [Predictix Fiyat Raporlama destek ekibine](https://www.infor.com/company/customer-center/) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Sertifika **(Base64)** yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/certificatebase64.png)

6. **Predictix Fiyat Raporlaması Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-predictix-price-reporting-single-sign-on"></a>Predictix Fiyat Raporlamasını tek oturum açma yapılandırma

Predictix Fiyat Raporlama tarafında tek oturum açma işlemi yapmak için, indirdiğiniz sertifikayı ve Azure portalından kopyaladığınız URL'leri [Predictix Fiyat Raporlama destek ekibine](https://www.infor.com/company/customer-center/)göndermeniz gerekir. Bu ekip, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanınmasını sağlar.

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

Bu bölümde, Britta Simon'a Predictix Fiyat Raporlaması'na erişimini sağlayarak Azure AD oturumunu tek oturum açma olanağı sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **Predictix Fiyat**Raporlaması'nı seçin.

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Predictix Fiyat**Raporlaması'nı seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-predictix-price-reporting-test-user"></a>Predictix Fiyat Raporlama test kullanıcısı oluşturma

Ardından, Predictix Fiyat Raporlama'da Britta Simon adında bir kullanıcı oluşturmanız gerekir. Kullanıcıları eklemek için [Predictix Fiyat Raporlama destek ekibiyle](https://www.infor.com/company/customer-center/) birlikte çalışın. Tek oturum açmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki Predictix Fiyat Raporlama döşemesini seçtiğinizde, SSO'yu kurduğunuz Predictix Fiyat Raporlama örneğinde otomatik olarak oturum açmış olmalısınız. Daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)