---
title: 'Öğretici: TigerText Secure Messenger ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve TigerText Secure Messenger arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: ea3bda1dd51a7c3a2e5e3f8b669d7138898f1595
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67088669"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Öğretici: TigerText Secure Messenger ile Azure Active Directory entegrasyonu

Bu eğitimde, TigerText Secure Messenger'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

TigerText Secure Messenger'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* TigerText Secure Messenger erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla TigerText Secure Messenger'da (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla [bilgi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)için bkz.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini TigerText Secure Messenger ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* Tek oturum açma özelliğine sahip TigerText Secure Messenger aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayın ve TigerText Secure Messenger'ı Azure AD ile tümleştirirsiniz.

TigerText Secure Messenger, SP tarafından başlatılan tek oturum açma (SSO) destekler.

## <a name="add-tigertext-secure-messenger-from-the-azure-marketplace"></a>Azure Marketinden TigerText Secure Messenger ekle

TigerText Secure Messenger'ın Azure AD'ye entegrasyonunu yapılandırmak için, Azure Marketi'nden TigerText Secure Messenger'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir:

1. [Azure portalında](https://portal.azure.com?azure-portal=true)oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **+ Yeni uygulama** seçeneğini belirleyin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **TigerText Secure Messenger**girin. Arama sonuçlarında **TigerText Secure Messenger'ı**seçin ve ardından uygulamayı eklemek için **Ekle'yi** seçin.

    ![TigerText Secure Messenger sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre TigerText Secure Messenger ile Azure AD tek oturum açma işlemini yapılandırın ve test edeyimsiniz. Tek oturum açmanın işe yaraması için, TigerText Secure Messenger'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturumaçmaişlemlerini TigerText Secure Messenger ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[TigerText Secure Messenger'ı tek oturum](#configure-tigertext-secure-messenger-single-sign-on)** açmasını yapılandırın.
1. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. TigerText Secure Messenger'da Britta Simon adında bir kullanıcının Britta Simon adlı Azure AD kullanıcısıyla bağlantılı olması için **[bir TigerText Secure Messenger test kullanıcısı oluşturun.](#create-a-tigertext-secure-messenger-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

TigerText Secure Messenger ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları izleyin:

1. **TigerText Secure Messenger** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesini seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde aşağıdaki adımları izleyin:

    ![TigerText Secure Messenger etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna bir URL girin:

       `https://home.tigertext.com`

    1. Tanımlayıcı **(Entity ID)** kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

       `https://saml-lb.tigertext.me/v1/organization/<instance ID>`

    > [!NOTE]
    > Tanımlayıcı **(Entity ID)** değeri gerçek değil. Bu değeri gerçek tanımlayıcıyla güncelleştirin. Değeri almak için [TigerText Secure Messenger destek ekibine](mailto:prosupport@tigertext.com)başvurun. Azure portalındaki **Temel SAML Yapılandırma** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Federasyon Metadata XML indirme seçeneği](common/metadataxml.png)

1. **TigerText Secure Messenger'ı ayarla** bölümünde, gereksinim duyduğunuz URL'yi veya URL'leri kopyalayın:

   * **Giriş URL'si**
   * **Azure AD Tanımlayıcısı**
   * **Giriş URL'si**

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-tigertext-secure-messenger-single-sign-on"></a>TigerText Secure Messenger'ı tek oturum aç'ı yapılandır

TigerText Secure Messenger tarafında tek oturum açma yapılandırmak için, indirilen Federasyon Metadata XML'i ve azure portalından uygun kopyalanan URL'leri [TigerText Secure Messenger destek ekibine](mailto:prosupport@tigertext.com)göndermeniz gerekir. TigerText Secure Messenger ekibi SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlandıklarından emin olacaktır.

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

Bu bölümde, Britta Simon'ın TigerText Secure Messenger'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > **TigerText Secure Messenger.**

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **TigerText Secure Messenger'ı**seçin.

    ![TigerText Secure Messenger uygulama listesinde](common/all-applications.png)

1. Sol bölmede, **YÖNET**altında, **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcılar** listesinde **Britta Simon'ı** seçin ve ardından bölmenin en altında **Seç'i** seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, rolü **seç** bölmesinde listeden kullanıcı için uygun rolü seçin. Bölmenin alt kısmında **Seç'i**seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-tigertext-secure-messenger-test-user"></a>TigerText Secure Messenger test kullanıcısı oluşturma

Bu bölümde, TigerText Secure Messenger'da Britta Simon adında bir kullanıcı oluşturursunuz. [TigerText Secure Messenger'da](mailto:prosupport@tigertext.com) Britta Simon'ı kullanıcı olarak eklemek için TigerText Secure Messenger destek ekibiyle birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Uygulamalarım portalında **TigerText Secure Messenger'ı** seçtiğinizde, tek oturum açma ayarladığınız TigerText Secure Messenger aboneliğinde otomatik olarak oturum açmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi [için, Windows portalımdaki Access'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın ve uygulamaları kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
