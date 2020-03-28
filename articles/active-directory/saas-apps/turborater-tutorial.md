---
title: 'Öğretici: TurboRater ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve TurboRater arasında tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: abb116b8-8024-4cc6-bc81-f32ef490ea17
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 3/8/2019
ms.author: jeedes
ms.openlocfilehash: 3777cf09ec669fe3df6bca13f6960f53c689767c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67088283"
---
# <a name="tutorial-azure-active-directory-integration-with-turborater"></a>Öğretici: TurboRater ile Azure Active Directory entegrasyonu

Bu eğitimde, TurboRater'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

TurboRater'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* TurboRater erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak TurboRater'da (tek oturum açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla [bilgi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)için bkz.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini TurboRater ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* Tek oturum açma özellikli bir TurboRater aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

TurboRater, IDP tarafından başlatılan tek oturum açmayı (SSO) destekler.

## <a name="add-turborater-from-the-azure-marketplace"></a>Azure Marketinden TurboRater Ekle

TurboRater'in Azure AD'ye entegrasyonunu yapılandırmak için, Azure Marketi'nden TurboRater'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir:

1. [Azure portalında](https://portal.azure.com?azure-portal=true)oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

    ![Kurumsal uygulamalar seçeneği](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **+ Yeni uygulama** seçeneğini belirleyin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **TurboRater**girin. Arama sonuçlarında **TurboRater'ı**seçin ve uygulamayı eklemek için **Ekle'yi** seçin.

    ![Sonuç listesinde ki TurboRater](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD oturum açma işlemlerini TurboRater ile **B Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz. Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile TurboRater'deki ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturum açma işlemlerini TurboRater ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)**
1. **[TurboRater'ı uygulama](#configure-turborater-single-sign-on)** tarafındaki tek oturum açma ayarlarını yapılandırmak için tek oturum açma yı yapılandırın.
1. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. TurboRater'da B. Simon adında bir kullanıcının B. Simon adlı Azure AD kullanıcısıyla bağlantılı olması için **[bir TurboRater test kullanıcısı oluşturun.](#create-a-turborater-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini TurboRater ile yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **TurboRater** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesini seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde aşağıdaki adımları yapın:

    ![TurboRater etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    1. Tanımlayıcı **(Entity ID)** kutusuna bir URL girin:

       `https://www.itcdataservices.com`

    1. **YanıtLA URL'si (Tüketici Hizmeti URL'sini İddia Et)** kutusuna aşağıdaki deseni kullanarak bir URL girin:

       | Ortam | URL'si |
       | ---------------| --------------- |
       | Test  | `https://ratingqa.itcdataservices.com/webservices/imp/saml/login` |
       | Canlı  | `https://www.itcratingservices.com/webservices/imp/saml/login` |

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek tanımlayıcı ve yanıt URL'si ile güncelleştirin. Bu değerleri almak için [TurboRater destek ekibine](https://www.getitc.com/support)başvurun. Azure portalındaki **Temel SAML Yapılandırma** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Federasyon Metadata XML indirme seçeneği](common/metadataxml.png)

1. **TurboRater'ı ayarla** bölümünde, ihtiyacınız olan URL'yi veya URL'leri kopyalayın:

   * **Giriş URL'si**
   * **Azure AD Tanımlayıcısı**
   * **Giriş URL'si**

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-turborater-single-sign-on"></a>TurboRater'ı tek oturum aç'ı yapılandır

TurboRater tarafında tek oturum açma yapılandırmak için, indirilen Federasyon Metadata XML'i ve uygun kopyalanmış URL'leri Azure portalından [TurboRater destek ekibine](https://www.getitc.com/support)göndermeniz gerekir. TurboRater ekibi SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlandıklarından emin olacaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizin**   > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

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

Bu bölümde, B. Simon'ın TurboRater'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **TurboRater.**

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulama listesinde **TurboRater'ı**seçin.

    ![Uygulamalar listesinde TurboRater](common/all-applications.png)

1. Sol bölmede, **YÖNET**altında, **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcılar** listesinde **B. Simon'ı** seçin ve ardından bölmenin en altında **Seç'i** seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, rolü **seç** bölmesinde listeden kullanıcı için uygun rolü seçin. Bölmenin alt kısmında **Seç'i**seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-turborater-test-user"></a>TurboRater test kullanıcısı oluşturma

Bu bölümde, TurboRater'da B. Simon adında bir kullanıcı oluşturursunuz. [TurboRater'da](https://www.getitc.com/support) b. Simon'ı kullanıcı olarak eklemek için TurboRater destek ekibiyle birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Uygulamalarım portalında **TurboRater'ı** seçtiğinizde, tek oturum açma ayarladığınız TurboRater aboneliğinde otomatik olarak oturum açmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi [için, Windows portalımdaki Access'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın ve uygulamaları kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
