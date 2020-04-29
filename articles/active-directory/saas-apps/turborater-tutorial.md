---
title: 'Öğretici: TurboRater ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve TurboRater arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67088283"
---
# <a name="tutorial-azure-active-directory-integration-with-turborater"></a>Öğretici: TurboRater ile tümleştirme Azure Active Directory

Bu öğreticide, TurboRater 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

TurboRater 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* TurboRater 'a erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla TurboRater (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Türborater ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .
* Çoklu oturum açma özellikli bir TurboRater aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

TurboRater, ıDP tarafından başlatılan çoklu oturum açmayı (SSO) destekler.

## <a name="add-turborater-from-the-azure-marketplace"></a>Azure Marketi 'nden TurboRater ekleme

TurboRater 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Azure Marketi 'nden yönetilen SaaS uygulamaları listenize TurboRater eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com?azure-portal=true) oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar seçeneği](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **+ Yeni uygulama** ' yı seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Turborater**girin. Arama sonuçlarında, **Turborater**' ı seçin ve ardından uygulamayı eklemek için **Ekle** ' yi seçin.

    ![Sonuçlar listesinde TurboRater](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **B Simon**adlı bir test kullanıcısına göre TurboRater Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz. Çoklu oturum açma için, TurboRater 'da bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD çoklu oturum açma 'yı Türborater ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[TurboRater çoklu oturum açmayı yapılandırın](#configure-turborater-single-sign-on)** .
1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. B. Simon adlı Azure AD kullanıcısına bağlı olan TurboRater 'da B. Simon adlı bir Kullanıcı **[oluşturmak için bir turborater test kullanıcısı oluşturun](#create-a-turborater-test-user)** .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı test](#test-single-sign-on)** edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı TurboRater ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Turborater** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** ' yi (kurşun kalem simgesi) seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölmesinde, aşağıdaki adımları uygulayın:

    ![TurboRater etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    1. **Tanımlayıcı (VARLıK kimliği)** kutusuna bir URL girin:

       `https://www.itcdataservices.com`

    1. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** kutusunda, aşağıdaki kalıbı kullanarak bir URL girin:

       | Ortam | URL'si |
       | ---------------| --------------- |
       | Test etme  | `https://ratingqa.itcdataservices.com/webservices/imp/saml/login` |
       | Canlı  | `https://www.itcratingservices.com/webservices/imp/saml/login` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için, [Turborater destek ekibine](https://www.getitc.com/support)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Federasyon meta verileri XML indirme seçeneği](common/metadataxml.png)

1. **Türborater ayarla** bölümünde, gerek duyduğunuz URL veya URL 'leri kopyalayın:

   * **Oturum açma URL 'SI**
   * **Azure AD tanımlayıcısı**
   * **Oturum kapatma URL 'SI**

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-turborater-single-sign-on"></a>Türborater çoklu oturum açmayı yapılandırma

TurboRater tarafında çoklu oturum açmayı yapılandırmak için, indirilen Federasyon meta veri XML 'sini ve Azure portal kopyalanmış URL 'Leri [türborater destek ekibine](https://www.getitc.com/support)göndermeniz gerekir. TurboRater ekibi, SAML SSO bağlantısının her iki tarafında da düzgün şekilde ayarlanmış olmasını sağlayacaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmedeki **Azure Active Directory**   > **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve "tüm kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında **+ Yeni Kullanıcı**' yı seçin.

    ![Yeni Kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde, aşağıdaki adımları uygulayın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. **Ad** kutusuna **bsıon**yazın.
  
    1. **Kullanıcı adı** kutusuna **bsıon\@\<yourcompanydomain> yazın.\< Uzantı>**. Örneğin, **Bsıon\@contoso.com**.

    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, TurboRater 'a erişimleri vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar** > **türborater**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **Turborater**' ı seçin.

    ![Uygulamalar listesinde TurboRater](common/all-applications.png)

1. Sol bölmedeki **Yönet**altında **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** bölmesinde, **Kullanıcılar** listesinde **B. Simon** öğesini seçin ve ardından bölmenin en altında bulunan **Seç** ' i seçin.

1. SAML onaylama 'da bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, Kullanıcı için listeden uygun rolü seçin. Bölmenin en altında bulunan **Seç**' i seçin.

1. **Atama Ekle** bölmesinde **ata**' yı seçin.

### <a name="create-a-turborater-test-user"></a>Bir Türborater test kullanıcısı oluşturma

Bu bölümde, TurboRater 'da B. Simon adlı bir Kullanıcı oluşturacaksınız. Bir kullanıcı olarak TurboRater 'da B. Simon eklemek için [turborater destek ekibi](https://www.getitc.com/support) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Uygulamalarım portalındaki **Türborater** ' ı seçtiğinizde, çoklu oturum açmayı ayarladığınız türborater aboneliğinde otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [My Apps Portalındaki uygulamaları erişme ve kullanma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
