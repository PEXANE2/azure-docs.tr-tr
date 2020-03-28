---
title: 'Öğretici: PageDNA ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve PageDNA arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c8765864-45f4-48c2-9d86-986a4aa431e4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c7f8c8efcad0a07a3d3a56925866b10d94f82ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68227473"
---
# <a name="tutorial-azure-active-directory-integration-with-pagedna"></a>Öğretici: PageDNA ile Azure Active Directory entegrasyonu

Bu eğitimde, PageDNA'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

PageDNA'yı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de PageDNA'ya kimlerin erişebileceğini kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla PageDNA'da (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla [bilgi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)için bkz.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini PageDNA ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* Tek oturum açma özelliği etkin bir PageDNA aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, bir test ortamında Azure AD tek oturum açma işlemlerini yapılandırıp sınayın ve PageDNA'yı Azure AD ile tümleştirirsiniz.

PageDNA aşağıdaki özellikleri destekler:

* SP tarafından başlatılan tek oturum açma (SSO).

* Tam zamanında kullanıcı sağlama.

## <a name="add-pagedna-from-the-azure-marketplace"></a>Azure Marketinden PageDNA ekleme

PageDNA'nın Azure AD'ye entegrasyonunu yapılandırmak için, Azure Marketi'nden PageDNA'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir:

1. [Azure portalında](https://portal.azure.com?azure-portal=true)oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **+ Yeni uygulama** seçeneğini belirleyin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **PageDNA**girin. Arama sonuçlarında **PageDNA'yı**seçin ve ardından uygulamayı eklemek için **Ekle'yi** seçin.

    ![Sonuç listesinde PageDNA](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre PageDNA ile yapılandırıp test esinizsiniz. Tek oturum açmanın işe yaraması için, PageDNA'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturumaçmayı PageDNA ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[PageDNA tek oturum](#configure-pagedna-single-sign-on)** açma yı yapılandırın.
1. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. PageDNA'da Britta Simon adında bir kullanıcının, Azure AD kullanıcısı na mıla bağlı olduğu için **[bir PageDNA test kullanıcısı oluşturun.](#create-a-pagedna-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini PageDNA ile yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **PageDNA** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesinde, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde aşağıdaki adımları izleyin:

    ![PageDNA etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, aşağıdaki desenlerden birini kullanarak bir URL girin:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>` |
        | `https://<your domain>` |
        | `https://<your domain>/<your site>` |
        | `https://www.nationsprint.com/<your site>` |
        | |

    1. Tanımlayıcı **(Entity ID)** kutusuna, aşağıdaki desenlerden birini kullanarak bir URL girin:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>/saml2ep.cgi` |
        | `https://www.nationsprint.com/<your site>/saml2ep.cgi` |
        | |

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek oturum açma URL'si ve tanımlayıcısıyla güncelleştirin. Bu değerleri almak için [PageDNA destek ekibine](mailto:success@pagedna.com)başvurun. Azure portalındaki **Temel SAML Yapılandırma** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. **SAML bölmeli Tek Oturum Açma** bölümünde, **SAML İmza Sertifikası** bölümünde, verilen seçeneklerden **Sertifikayı (Raw)** indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika (Ham) indirme seçeneği](common/certificateraw.png)

1. **PageDNA'yı Ayarla** bölümünde, gereksinim duyduğunuz URL'yi veya URL'leri kopyalayın:

   * **Giriş URL'si**
   * **Azure AD Tanımlayıcısı**
   * **Giriş URL'si**

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-pagedna-single-sign-on"></a>PageDNA tek oturum açma yı yapılandırma

PageDNA tarafında tek oturum açma yapılandırmak için, indirilen Sertifikayı (Raw) ve uygun kopyalanan URL'leri Azure portalından [PageDNA destek ekibine](mailto:success@pagedna.com)gönderin. PageDNA ekibi SAML SSO bağlantısının her iki tarafta da düzgün ayarlandıklarından emin olacaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizin**   > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

    ![Kullanıcılar ve "Tüm Kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında + **Yeni kullanıcı**seçin.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde aşağıdaki adımları yapın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** **kutusuna BrittaSimon\@\<şirketinizin etki\< alanı> girin. uzantısı>**. Örneğin, **BrittaSimon\@contoso.com.**

    1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, kullanıcıya PageDNA erişimi vererek Kullanıcı Britta Simon'ın Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > **PageDNA**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **PageDNA'yı**seçin.

    ![Uygulamalar listesinde PageDNA](common/all-applications.png)

1. Sol bölmede, **YÖNET**altında, **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcılar** listesinde **Britta Simon'ı** seçin ve ardından bölmenin en altında **Seç'i** seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, rolü **seç** bölmesinde listeden kullanıcı için uygun rolü seçin. Bölmenin alt kısmında **Seç'i**seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-pagedna-test-user"></a>PageDNA test kullanıcısı oluşturma

Britta Simon adında bir kullanıcı artık PageDNA'da oluşturuldu. Bu kullanıcıyı oluşturmak için hiçbir şey yapmanız adabına gerek yoktur. PageDNA, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Britta Simon adında bir kullanıcı PageDNA'da zaten yoksa, kimlik doğrulamaişleminden sonra yeni bir kullanıcı oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Uygulamalarım portalında **PageDNA'yı** seçtiğinizde, tek oturum açma ayarladığınız PageDNA aboneliğinde otomatik olarak oturum açmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi [için, Windows portalımdaki Access'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın ve uygulamaları kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

