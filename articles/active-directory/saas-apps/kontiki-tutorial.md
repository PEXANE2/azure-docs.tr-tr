---
title: 'Öğretici: Kontiki ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Kontiki arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfb154bce34b2ceda99b82c7ca3534b8a8ee0a1f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67098500"
---
# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Öğretici: Kontiki ile tümleştirme Azure Active Directory

Bu öğreticide, Kontiki 'yı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

Kontiki 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'yi, Kontiki 'a kimlerin erişebileceğini denetlemek için kullanabilirsiniz.
* Kullanıcılar, Azure AD hesapları (çoklu oturum açma) ile Konşimento ' de otomatik olarak oturum açabilirler.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Kontiki ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Çoklu oturum açma özelliği etkin olan bir Kontiki aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edin ve Kontiki 'yı Azure AD ile tümleştirin.

Kontiki aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **Tam zamanında Kullanıcı hazırlama**

## <a name="add-kontiki-in-the-azure-portal"></a>Azure portal Kontiki ekleme

Kontiki 'yı Azure AD ile bütünleştirmek için, yönetilen SaaS uygulamaları listenize Kontiki eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüden **Azure Active Directory**' yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için **Yeni uygulama**' yı seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Kontiki**girin. Arama sonuçlarında **konşimento**' i seçin ve ardından **Ekle**' yi seçin.

    ![Sonuç listesinde Kontiki](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Kontiki ile yapılandırıp test edersiniz. Çoklu oturum açma için, bir Azure AD kullanıcısı ve Kontiki 'da ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

Azure AD çoklu oturum açma 'yı Kontiki ile yapılandırmak ve test etmek için, aşağıdaki yapı taşlarını doldurmanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasına olanak sağlar. |
| **[Kontiki çoklu oturum açmayı yapılandırma](#configure-kontiki-single-sign-on)** | Uygulamadaki çoklu oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı Kullanıcı için Azure AD çoklu oturum açmayı sınar. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açma kullanımı için Britta Simon 'U sağlar. |
| **[Kontiki test kullanıcısı oluşturma](#create-a-kontiki-test-user)** | Kullanıcının Azure AD gösterimine bağlı olan Kontiki 'da Britta Simon 'un bir karşılığı oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde Azure AD çoklu oturum açma 'yı Azure portal Kontiki ile yapılandırırsınız.

1. [Azure Portal](https://portal.azure.com/), **Kontiki** uygulama tümleştirme bölmesinde **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML** veya **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** ' yi (kurşun kalem simgesi) seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölmesinde, **oturum açma URL 'si** metin kutusuna AŞAĞıDAKI düzene sahip bir URL girin:`https://<companyname>.mc.eval.kontiki.com`

    ![Kontiki etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    > [!NOTE]
    > Kullanılacak doğru değeri almak için [Kontiki istemci destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML**' nün yanındaki **İndir** ' i seçin. Gereksinimlerinize göre bir indirme seçeneği belirleyin. Sertifikayı bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML sertifikası indirme seçeneği](common/metadataxml.png)

1. **Kontiki ayarla** bölümünde, gereksinimlerinize göre aşağıdaki URL 'leri kopyalayın:

    * Oturum Açma URL’si
    * Azure AD tanımlayıcısı
    * Oturum kapatma URL 'SI

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-kontiki-single-sign-on"></a>Kontiki çoklu oturum açmayı yapılandırma

Kontiki tarafında çoklu oturum açmayı yapılandırmak için, indirilen Federasyon meta verileri XML dosyasını ve Azure portal kopyaladığınız ilgili URL 'Leri [Kontiki destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html)gönderin. Kontiki destek ekibi, SAML çoklu oturum açma bağlantısının her iki tarafında da doğru şekilde ayarlandığından emin olmak için, bunları göndereceğiniz bilgileri kullanır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal,**Kullanıcılar** >  **Azure Active Directory** > **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. **Yeni Kullanıcı**' yı seçin.

    ![Yeni Kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde, aşağıdaki adımları izleyin:

    1. **Ad** kutusuna **Brittasıon**yazın.
  
    1. **Kullanıcı adı** kutusuna, **Şirket içi etki alanı> brittasıon\@\<yazın.\< Uzantı>**. Örneğin, **brittasıon\@contoso.com**.

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma 'yı kullanabilmesi için Kontiki 'a Britta Simon erişimi verirsiniz.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar** > **konşimento**' i seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **Kontiki**' ı seçin.

    ![Uygulamalar listesinde Kontiki](common/all-applications.png)

1. Menüsünde **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin. Sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar**' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** bölmesinde, Kullanıcı listesinde **Britta Simon** ' u seçin. **Seç**’i seçin.

1. SAML assertion 'da bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, kullanıcının listeden ilgili rolünü seçin. **Seç**’i seçin.

1. **Atama Ekle** bölmesinde **ata**' yı seçin.

### <a name="create-a-kontiki-test-user"></a>Kontiki test kullanıcısı oluşturma

Kontiki 'da Kullanıcı sağlamayı yapılandırabilmeniz için herhangi bir eylem öğesi yoktur. Atanan bir Kullanıcı uygulamalarım portalı 'nı kullanarak Kontiki 'da oturum açmaya çalıştığında, Kontiki kullanıcının var olup olmadığını denetler. Kullanıcı hesabı bulunmazsa, Kontiki Kullanıcı hesabını otomatik olarak oluşturur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Çoklu oturum açmayı ayarladıktan sonra, My Apps Portal 'da **konşimento** ' i seçtiğinizde, otomatik olarak Kontiki oturumu açtınız. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [Apps Portalındaki uygulamalara erişme ve bunları kullanma](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri gözden geçirin:

- [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
