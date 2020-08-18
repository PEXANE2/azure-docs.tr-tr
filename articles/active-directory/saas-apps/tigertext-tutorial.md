---
title: 'Öğretici: TigerText Secure Messenger ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile TigerText güvenli Messenger arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 9dff60767e923bad1322b689acd98e69eb9c2ac6
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88517006"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Öğretici: TigerText Secure Messenger ile Azure Active Directory tümleştirme

Bu öğreticide, TigerText Secure Messenger 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

TigerText güvenli Messenger 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de TigerText güvenli Messenger erişimi olan ' i denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla TigerText Secure Messenger 'da (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini TigerText Secure Messenger ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Çoklu oturum açma özellikli bir TigerText güvenli Messenger aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz ve TigerText Secure Messenger 'ı Azure AD ile tümleştirin.

TigerText güvenli Messenger, SP tarafından başlatılan çoklu oturum açmayı (SSO) destekler.

## <a name="add-tigertext-secure-messenger-from-the-azure-marketplace"></a>Azure Marketi 'nden TigerText güvenli Messenger ekleyin

TigerText Secure Messenger 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Azure Marketi 'nden yönetilen SaaS uygulamaları listenize TigerText Secure Messenger ' ı eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com?azure-portal=true) oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **+ Yeni uygulama** ' yı seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **tigertext Secure Messenger**girin. Arama sonuçlarında **tigertext Secure Messenger**' ı seçin ve ardından uygulamayı eklemek için **Ekle** ' yi seçin.

    ![Sonuçlar listesinde TigerText güvenli Messenger](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre TigerText güvenli Messenger Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz. Çoklu oturum açma için, bir Azure AD kullanıcısı ve TigerText güvenli Messenger 'daki ilgili Kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD çoklu oturum açma 'yı TigerText Secure Messenger ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[TigerText Secure Messenger çoklu oturum açmayı yapılandırın](#configure-tigertext-secure-messenger-single-sign-on)** .
1. Britta Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
1. Azure AD 'de çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirmek için **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. Britta Simon adlı Azure AD kullanıcısına bağlı olan TigerText güvenli Messenger 'da Britta Simon adlı bir Kullanıcı **[oluşturun](#create-a-tigertext-secure-messenger-test-user)** .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı test](#test-single-sign-on)** edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı TigerText Secure Messenger ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **tigertext güvenli Messenger** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** ' yi (kurşun kalem simgesi) seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölmesinde aşağıdaki adımları uygulayın:

    ![TigerText güvenli Messenger etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    1. **Oturum açma URL 'si** kutusuna bir URL girin:

       `https://home.tigertext.com`

    1. **Tanımlayıcı (VARLıK kimliği)** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

       `https://saml-lb.tigertext.me/v1/organization/<instance ID>`

    > [!NOTE]
    > **Tanımlayıcı (VARLıK kimliği)** değeri gerçek değil. Bu değeri gerçek tanımlayıcıyla güncelleştirin. Değeri almak için, [tigertext Secure Messenger destek ekibine](mailto:prosupport@tigertext.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Federasyon meta verileri XML indirme seçeneği](common/metadataxml.png)

1. **TigerText güvenli Messenger ayarla** bölümünde, gereken URL veya URL 'leri kopyalayın:

   * **Oturum açma URL 'SI**
   * **Azure AD tanımlayıcısı**
   * **Oturum kapatma URL 'SI**

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-tigertext-secure-messenger-single-sign-on"></a>TigerText güvenli Messenger çoklu oturum açmayı yapılandırma

TigerText güvenli Messenger tarafında çoklu oturum açmayı yapılandırmak için, indirilen Federasyon meta veri XML 'sini ve Azure portal kopyalanmış URL 'Leri, [tigertext Secure Messenger destek ekibine](mailto:prosupport@tigertext.com)göndermeniz gerekir. TigerText Secure Messenger ekibi, SAML SSO bağlantısının her iki tarafında da düzgün şekilde ayarlanmış olmasını sağlayacaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmedeki **Azure Active Directory**    >  **Kullanıcılar**  >  **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve "tüm kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında **+ Yeni Kullanıcı**' yı seçin.

    ![Yeni Kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde, aşağıdaki adımları uygulayın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. **Ad** kutusuna **Brittasıon**yazın.
  
    1. **Kullanıcı adı** kutusuna **Brittasıon yazın \@ \<yourcompanydomain> . \<extension> ** Örneğin, **Brittasıon \@ contoso.com**.

    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, TigerText Secure Messenger erişimine izin vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**  >  **tıgertext güvenli Messenger**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **tigertext Secure Messenger**' ı seçin.

    ![Uygulamalar listesinde TigerText güvenli Messenger](common/all-applications.png)

1. Sol bölmedeki **Yönet**altında **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" seçeneği](common/users-groups-blade.png)

1. **+ Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** bölmesinde, **Kullanıcılar** listesinde **Britta Simon** ' ı seçin ve ardından bölmenin en altında bulunan **Seç** ' i seçin.

1. SAML onaylama 'da bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, Kullanıcı için listeden uygun rolü seçin. Bölmenin en altında bulunan **Seç**' i seçin.

1. **Atama Ekle** bölmesinde **ata**' yı seçin.

### <a name="create-a-tigertext-secure-messenger-test-user"></a>Bir TigerText güvenli Messenger test kullanıcısı oluşturma

Bu bölümde, TigerText Secure Messenger 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. TigerText güvenli Messenger 'da Kullanıcı olarak Britta Simon eklemek için [tigertext güvenli Messenger destek ekibi](mailto:prosupport@tigertext.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Uygulamalarım portalında **tigertext Secure Messenger** ' ı seçtiğinizde, çoklu oturum açmayı ayarladığınız tigertext güvenli Messenger aboneliğinde otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [My Apps Portalındaki uygulamaları erişme ve kullanma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
