---
title: 'Öğretici: akıllı küresel Idare ile Azure AD SSO tümleştirmesi'
description: Azure Active Directory ile akıllı küresel Idare arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c31613e-f30d-47d9-af51-001345b6db10
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c3b2f5332f3dc94f10a7822068b165db13b3d33
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84456785"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-smart-global-governance"></a>Öğretici: akıllı genel Idare ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, akıllı küresel Idare Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure AD ile akıllı Global Idare tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'yi kullanarak, akıllı Global Idare ile kimlerin erişebileceğini denetleyin.
* Kullanıcılarınızın Azure AD hesaplarıyla akıllı küresel Idare ' de otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli bir akıllı küresel Idare aboneliği.

## <a name="tutorial-description"></a>Öğretici açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edeceksiniz.

Smart Global Idare, SP tarafından başlatılan ve ıDP tarafından başlatılan SSO 'yu destekler.

Akıllı Global Idare yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve inkesini korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-smart-global-governance-from-the-gallery"></a>Galeriden akıllı küresel Idare ekleme

Akıllı küresel yönetim 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize akıllı küresel Idare eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) bir iş veya okul hesabıyla ya da kişisel bir Microsoft hesabı oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Smart Global idare** girin.
1. Sonuçlar panelinde **Smart Global idare** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-smart-global-governance"></a>Akıllı küresel Idare için Azure AD SSO 'yu yapılandırma ve test etme

Azure AD SSO 'yu, B. Simon adlı bir test kullanıcısı kullanarak akıllı Global Idare ile yapılandırıp test edersiniz. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ilgili Kullanıcı ile akıllı küresel Idare arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu akıllı küresel Idare ile yapılandırmak ve test etmek için şu üst düzey adımları uygulamanız gerekir:

1. Kullanıcılarınızın özelliğini kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Kullanıcının Azure AD çoklu oturum açma özelliğini kullanmasını sağlamak için **[Test kullanıcısına erişim Izni verin](#grant-access-to-the-test-user)** .
1. Uygulama tarafında **[Smart Global Idare SSO 'Yu yapılandırın](#configure-smart-global-governance-sso)** .
    1. Kullanıcının Azure AD gösterimine karşılık gelen bir **[akıllı küresel idare testi kullanıcısı oluşturun](#create-a-smart-global-governance-test-user)** .
1. Yapılandırmanın çalıştığını doğrulamak için **[test SSO 'su](#test-sso)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), **akıllı küresel idare** uygulama tümleştirmesi sayfasında, **Yönet** bölümünde **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem düğmesini seçin:

   ![Temel SAML yapılandırması için kalem düğmesi](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın.

    a. **Tanımlayıcı** kutusuna şu URL 'lerden birini girin:

    | | |
    |-|-|
    | `https://eu-fr-south.console.smartglobalprivacy.com/platform/authentication-saml2/metadata`|
    | `https://eu-fr-south.console.smartglobalprivacy.com/dpo/authentication-saml2/metadata`|

    b. **Yanıt URL 'si** kutusuna şu URL 'lerden birini girin:

    | | |
    |-|-|
    | `https://eu-fr-south.console.smartglobalprivacy.com/platform/authentication-saml2/acs`|
    | `https://eu-fr-south.console.smartglobalprivacy.com/dpo/authentication-saml2/acs`|

1. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL 'Ler ayarla** ' yı seçin ve aşağıdaki adımı doldurun.

   - **Oturum açma URL 'si** kutusuna şu URL 'lerden birini girin:

    | | |
    |-|-|
    | `https://eu-fr-south.console.smartglobalprivacy.com/dpo`|
    | `https://eu-fr-south.console.smartglobalprivacy.com/platform`|

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, sertifikayı Indirip bilgisayarınıza kaydetmek için sertifika **yükleme** bağlantısını **(ham)** seçin:

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

1. **Akıllı genel Idare ayarla** bölümünde, gereksinimlerinize göre uygun URL 'Yi veya URL 'leri kopyalayın:

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal B. Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmesinde **Azure Active Directory**' ı seçin. **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları uygulayın:
   1. **Ad** kutusuna **B. Simon**girin.  
   1. **Kullanıcı adı** kutusuna \<username> @ \<companydomain> .. yazın. \<extension> Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster**' i seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'u seçin.

### <a name="grant-access-to-the-test-user"></a>Test kullanıcısına erişim izni verme

Bu bölümde, bu kullanıcıya akıllı küresel Idare erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Smart Global idare**' ı seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünde, **Kullanıcılar ve gruplar**' ı seçin:

   ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin:

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **B. Simon** öğesini seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-smart-global-governance-sso"></a>Smart Global Idare SSO 'yu yapılandırma

Akıllı küresel Idare tarafında çoklu oturum açmayı yapılandırmak için, indirilen ham sertifikayı ve Azure portal 'den kopyaladığınız uygun URL 'Leri [akıllı küresel idare desteği ekibine](mailto:support.tech@smartglobal.com)göndermeniz gerekir. SAML SSO bağlantısını her iki tarafta da doğru olacak şekilde yapılandırır.

### <a name="create-a-smart-global-governance-test-user"></a>Akıllı küresel Idare testi kullanıcısı oluşturma

Smart Global Idare 'nda B. Simon adlı bir kullanıcı eklemek için [Smart Global idare destek ekibi](mailto:support.tech@smartglobal.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim paneli ' ni kullanarak Azure AD SSO yapılandırmanızı test edeceksiniz.

Erişim panelinde Akıllı küresel Idare kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız akıllı küresel Idare örneğinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneli 'Ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile akıllı küresel Idare 'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle akıllı genel Idare koruması](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)