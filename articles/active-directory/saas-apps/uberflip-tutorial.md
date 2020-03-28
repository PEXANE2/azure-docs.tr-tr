---
title: 'Öğretici: Uberflip ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Uberflip arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 754b1f5b-6694-4fd6-9e1e-9fad769c64db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: d20c05e6ec5a413b81ede9cb4906de2595967115
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048465"
---
# <a name="tutorial-azure-active-directory-integration-with-uberflip"></a>Öğretici: Uberflip ile Azure Active Directory entegrasyonu

Bu eğitimde, Uberflip'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Uberflip'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Uberflip erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Uberflip'te otomatik olarak oturum açmalarını (tek oturum açma) etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla [bilgi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)için bkz.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Uberflip ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* Tek oturum açma özelliğine sahip bir Uberflip aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

Uberflip aşağıdaki özellikleri destekler:

* SP tarafından başlatılan ve IDP tarafından başlatılan tek oturum açma (SSO).
* Tam zamanında kullanıcı sağlama.

## <a name="add-uberflip-from-the-azure-marketplace"></a>Azure Marketinden Uberflip ekleme

Uberflip'in Azure AD'ye entegrasyonunu yapılandırmak için, Azure Marketi'nden Yönetilen SaaS uygulamaları listenize Uberflip eklemeniz gerekir:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

   ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

   ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **+ Yeni uygulama** seçeneğini belirleyin.

   ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Uberflip'i**girin. Arama sonuçlarında **Uberflip'i**seçin ve ardından uygulamayı eklemek için **Ekle'yi** seçin.

   ![Sonuç listesinde Uberflip](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemlerini B **Simon**adlı bir test kullanıcısına göre Uberflip ile yapılandırıp test esiniz. Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Uberflip'teki ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturumlarını Uberflip ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)**
1. **[Uberflip'i](#configure-uberflip-single-sign-on)** uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için tek oturum açma yı yapılandırın.
1. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uberflip'te B. Simon adında bir kullanıcının B. Simon adlı Azure AD kullanıcısıyla bağlantılı olması için **[bir Uberflip test kullanıcısı oluşturun.](#create-an-uberflip-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Uberflip ile yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **Uberflip** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesinde, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde, yapılandırmak istediğiniz SSO moduna bağlı olarak aşağıdaki adımlardan birini yapın:

   * Uygulamayı IDP tarafından başlatılan SSO modunda yapılandırmak için Yanıt URL'si **(Tüketici Hizmeti URL'si)** kutusuna aşağıdaki deseni kullanarak bir URL girin:

     `https://app.uberflip.com/sso/saml2/<IDPID>/<ACCOUNTID>`

     ![Uberflip etki alanı ve URL'ler tek oturum açma bilgileri](common/both-replyurl.png)

     > [!NOTE]
     > Bu değer gerçek değil. Bu değeri gerçek yanıt URL'si ile güncelleştirin. Gerçek değeri almak için [Uberflip destek ekibine](mailto:support@uberflip.com)başvurun. Azure portalındaki **Temel SAML Yapılandırma** bölmesinde gösterilen desenlere de başvurabilirsiniz.

   * Uygulamayı SP tarafından başlatılan SSO modunda yapılandırmak **için, ek URL'ler ayarla'yı**ve **Oturum Açma URL** kutusuna şu URL'yi girin:

     `https://app.uberflip.com/users/login`

     ![Uberflip etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Federasyon Metadata XML indirme seçeneği](common/metadataxml.png)

1. **Uberflip'i Ayarla** bölmesinde, gereksinim duyduğunuz URL'yi veya URL'leri kopyalayın:

   * **Giriş URL'si**
   * **Azure AD Tanımlayıcısı**
   * **Giriş URL'si**

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-uberflip-single-sign-on"></a>Uberflip tek oturum açma yı yapılandırma

Uberflip tarafında tek oturum açma yapılandırmak için, indirilen Federasyon Metadata XML'ini ve azure portalından uygun kopyalanan URL'leri [Uberflip destek ekibine](mailto:support@uberflip.com)göndermeniz gerekir. Uberflip ekibi SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlandıklarından emin olacaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B. Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

    ![Kullanıcılar ve "Tüm Kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında + **Yeni kullanıcı**seçin.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde aşağıdaki adımları yapın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. **Ad** kutusuna **BSimon**girin.
  
    1. Kullanıcı **adı** kutusuna **BSimon\@\<şirketinizin etki\< alanı> girin. uzantısı>**. Örneğin, **BSimon\@contoso.com.**

    1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın Uberflip'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları** > **Uberflip'i**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **Uberflip'i**seçin.

    ![Uberflip uygulama listesinde](common/all-applications.png)

1. Sol bölmede, **YÖNET**altında, **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcılar** listesinde **B Simon'ı** seçin ve ardından bölmenin en altında **Seç'i** seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, rolü **seç** bölmesinde listeden kullanıcı için uygun rolü seçin. Bölmenin alt kısmında **Seç'i**seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-an-uberflip-test-user"></a>Uberflip test kullanıcısı oluşturma

B. Simon adında bir kullanıcı artık Uberflip'te oluşturuldu. Bu kullanıcıyı oluşturmak için hiçbir şey yapmanız adabına gerek yoktur. Uberflip, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. B. Simon adlı bir kullanıcı Uberflip'te zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Uberflip destek ekibine](mailto:support@uberflip.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Uygulamalarım portalında **Uberflip'i** seçtiğinizde, tek oturum açma ayarladığınız Uberflip aboneliğinde otomatik olarak oturum açmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi [için, Windows portalımdaki Access'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın ve uygulamaları kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
