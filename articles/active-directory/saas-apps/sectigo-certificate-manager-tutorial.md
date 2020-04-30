---
title: 'Öğretici: Sectigo sertifika yöneticisiyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Sectıgo Sertifika Yöneticisi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 62cd6987-3373-4b58-b1ff-589f4a3d70a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0447a8dd464363ae7e076dde2520565005d7c0a5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67588245"
---
# <a name="tutorial-azure-active-directory-integration-with-sectigo-certificate-manager"></a>Öğretici: Sectigo sertifika yöneticisiyle Azure Active Directory tümleştirme

Bu öğreticide, Sectigo sertifika yöneticisini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

Sectigo Sertifika Yöneticisi 'Ni Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'yi, Sectigo Sertifika Yöneticisi 'ne kimlerin erişebileceğini denetlemek için kullanabilirsiniz.
* Kullanıcılar, Azure AD hesaplarıyla (çoklu oturum açma) Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturum açabilirler.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sectıgo Sertifika Yöneticisi ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Çoklu oturum açma özellikli sectigo Sertifika Yöneticisi aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edin ve Sectigo Sertifika Yöneticisi 'Ni Azure AD ile tümleştirin.

Sectıgo Sertifika Yöneticisi aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **IDP-başlatılan çoklu oturum açma**

## <a name="add-sectigo-certificate-manager-in-the-azure-portal"></a>Azure portal Sectıgo Sertifika Yöneticisi ekleme

Sectigo sertifika yöneticisini Azure AD ile tümleştirmek için, yönetilen SaaS uygulamaları listenize Sectıgo Sertifika Yöneticisi eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüden **Azure Active Directory**' yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için **Yeni uygulama**' yı seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Sectıgo Sertifika Yöneticisi**girin. Arama sonuçlarında, **Sectigo Sertifika Yöneticisi**' ni seçin ve ardından **Ekle**' yi seçin.

    ![Sonuçlar listesinde sectıgo Sertifika Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına bağlı olarak Sectigo Sertifika Yöneticisi Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz. Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve Sectıgo Sertifika Yöneticisi 'nde ilgili Kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD çoklu oturum açma 'yı Sectigo Sertifika Yöneticisi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamalısınız:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasına olanak sağlar. |
| **[Sectigo Sertifika Yöneticisi çoklu oturum açmayı yapılandırma](#configure-sectigo-certificate-manager-single-sign-on)** | Uygulamadaki çoklu oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı Kullanıcı için Azure AD çoklu oturum açmayı sınar. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açma kullanımı için Britta Simon 'U sağlar. |
| **[Sectıgo Sertifika Yöneticisi test kullanıcısı oluşturma](#create-a-sectigo-certificate-manager-test-user)** | Kullanıcının Azure AD gösterimine bağlı olan Sectigo Sertifika Yöneticisi 'nde Britta Simon 'ın bir karşılığı oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde Azure AD çoklu oturum açma 'yı Azure portal Sectıgo Sertifika Yöneticisi ile yapılandırırsınız.

1. [Azure Portal](https://portal.azure.com/), **Sectıgo Sertifika Yöneticisi** uygulama tümleştirme bölmesinde **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML** veya **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** ' yi (kurşun kalem simgesi) seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölmesinde, *IDP tarafından başlatılan modu*yapılandırmak için aşağıdaki adımları izleyin:

    1. **Tanımlayıcı** kutusuna şu URL 'lerden birini girin:
       * https:\//CERT-Manager.com/Shibboleth
       * https:\//Hard.CERT-Manager.com/Shibboleth

    1. **Yanıt URL 'si** kutusuna şu URL 'lerden birini girin:
        * https:\//CERT-Manager.com/Shibboleth.SSO/SAML2/Post
        * https:\//Hard.CERT-Manager.com/Shibboleth.SSO/SAML2/Post

    1. **Ek URL 'Ler ayarla**' yı seçin.

    1. **Geçiş durumu** kutusunda şu URL 'lerden birini girin:
       * https:\//CERT-Manager.com/customer/SSLSupport/idp
       * https:\//Hard.CERT-Manager.com/customer/SSLSupport/idp

    ![Sectıgo sertifika yöneticisi etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-relay.png)

1.  Uygulamayı *SP tarafından başlatılan modda*yapılandırmak için aşağıdaki adımları izleyin:

    * **Oturum açma URL 'si** kutusuna şu URL 'lerden birini girin:
      * https:\//CERT-Manager.com/Shibboleth.SSO/Login
      * https:\//Hard.CERT-Manager.com/Shibboleth.SSO/Login

      ![Sectıgo sertifika yöneticisi etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/both-signonurl.png)

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** yanındaki **İndir** ' i seçin. Gereksinimlerinize göre bir indirme seçeneği belirleyin. Sertifikayı bilgisayarınıza kaydedin.

    ![Sertifika (base64) indirme seçeneği](common/certificatebase64.png)

1. **Sectıgo sertifika yöneticisini ayarla** bölümünde, gereksinimlerinize göre aşağıdaki URL 'leri kopyalayın:

    * Oturum Açma URL’si
    * Azure AD tanımlayıcısı
    * Oturum kapatma URL 'SI

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-sectigo-certificate-manager-single-sign-on"></a>Sectigo Sertifika Yöneticisi çoklu oturum açmayı yapılandırma

Sectigo Sertifika Yöneticisi tarafında çoklu oturum açmayı yapılandırmak için, indirilen sertifika (base64) dosyasını ve Azure portal kopyaladığınız ilgili URL 'Leri [Sectıgo Sertifika Yöneticisi destek ekibine](https://sectigo.com/support)gönderin. Sectıgo Sertifika Yöneticisi destek ekibi, SAML çoklu oturum açma bağlantısının her iki tarafında da doğru şekilde ayarlandığından emin olmak için, bunları göndereceğiniz bilgileri kullanır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal,**Kullanıcılar** >  **Azure Active Directory** > **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. **Yeni Kullanıcı**' yı seçin.

    ![Yeni Kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde, aşağıdaki adımları izleyin:

    1. **Ad** kutusuna **Brittasıon**yazın.
  
    1. **Kullanıcı adı** kutusuna, **Şirket içi etki alanı> brittasıon\@\<yazın.\< uzantı\>**. Örneğin, **brittasıon\@contoso.com**.

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma 'yı kullanabilmesi için, Sectıgo Sertifika Yöneticisi 'ne Britta Simon erişimi verirsiniz.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar** > **sectıgo Sertifika Yöneticisi**' ni seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **Sectigo Sertifika Yöneticisi**' ni seçin.

    ![Uygulamalar listesinde sectıgo Sertifika Yöneticisi](common/all-applications.png)

1. Menüsünde **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin. Sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar**' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** bölmesinde, Kullanıcı listesinde **Britta Simon** ' u seçin. **Seç**’i seçin.

1. SAML assertion 'da bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, kullanıcının listeden ilgili rolünü seçin. **Seç**’i seçin.

1. **Atama Ekle** bölmesinde **ata**' yı seçin.

### <a name="create-a-sectigo-certificate-manager-test-user"></a>Sectıgo Sertifika Yöneticisi test kullanıcısı oluşturma

Bu bölümde, Sectigo Sertifika Yöneticisi 'nde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Sectigo Sertifika Yöneticisi platformuna eklemek için [Sectigo Sertifika Yöneticisi destek](https://sectigo.com/support) ekibiyle birlikte çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Çoklu oturum açmayı ayarladıktan sonra, Uygulamalarım portalındaki **Sectıgo Sertifika Yöneticisi** ' ni seçtiğinizde, Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturumunuz açılır. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [Apps Portalındaki uygulamalara erişme ve bunları kullanma](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri gözden geçirin:

- [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


