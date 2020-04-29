---
title: 'Öğretici: Officesi yazılımıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Officesi yazılımı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 80712c7f59845287006c1699524573c6094498b3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75561723"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-officespace-software"></a>Öğretici: Officesi yazılımıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Officesi yazılımını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure AD ile Officesi yazılımını tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, Officesi yazılımına erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Officesi yazılımına otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Officesi yazılımı çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Officesi yazılımı **SP** tarafından başlatılan SSO 'yu destekler


* Officesi yazılımı **, tam zamanında** Kullanıcı sağlamasını destekler


## <a name="adding-officespace-software-from-the-gallery"></a>Galeriden Officesi yazılımı ekleme

Officesi yazılımının Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden Officesi yazılımını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Officesi yazılımı** yazın.
1. Sonuçlar panelinden **Officesi yazılımı** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-officespace-software"></a>Officesi yazılımı için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Officesi yazılımıyla yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Officesi yazılımındaki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Officesi yazılımıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Officesi yazılım SSO 'Yu yapılandırın](#configure-officespace-software-sso)** .
    1. Officesi **[yazılım test kullanıcısı oluşturun](#create-officespace-software-test-user)** -Kullanıcı Azure AD gösterimine bağlı olan Officesi yazılımında B. Simon 'a karşılık gelen bir.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Officesi yazılım** uygulaması tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<company name>.officespacesoftware.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Officesi yazılım istemci destek ekibine](mailto:support@officespacesoftware.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Officesi yazılım uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir. Officesi yazılım uygulaması, **NameIdentifier** 'ın **User. Mail**ile eşlenmesini bekliyor, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

1. Daha fazlasına ek olarak, Officesi yazılım uygulaması aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | e-posta | Kullanıcı. Mail |
    | ad | User. DisplayName |
    | first_name | Kullanıcı. |
    | last_name | User. soyadı |

1. **SAML Imzalama sertifikası** bölümünde, **SAML imzalama sertifikası** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![SAML Imzalama sertifikasını Düzenle](common/edit-certificate.png)

1. **SAML Imzalama sertifikası** bölümünde, **parmak izi değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini Kopyala](common/copy-thumbprint.png)

1. **Officesi yazılımını ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak Officesi yazılımına erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Officesi yazılımı**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-officespace-software-sso"></a>Officesi yazılım SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Officesi yazılım kiracınızda yönetici olarak oturum açın.

2. **Ayarlar** ' a gidin ve **Bağlayıcılar**' a tıklayın.

    ![Uygulama tarafında çoklu oturum açmayı yapılandırma](./media/officespace-tutorial/tutorial_officespace_002.png)

3. **SAML kimlik doğrulaması**' na tıklayın.

    ![Uygulama tarafında çoklu oturum açmayı yapılandırma](./media/officespace-tutorial/tutorial_officespace_003.png)

4. **SAML kimlik doğrulaması** bölümünde aşağıdaki adımları uygulayın:

    ![Uygulama tarafında çoklu oturum açmayı yapılandırma](./media/officespace-tutorial/tutorial_officespace_004.png)

    a. **Logout Provider URL** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.

    b. **İstemci IDP hedef URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. Azure portal kopyaladığınız **parmak izi** değerini **istemci IDP sertifikası parmak izi** metin kutusuna yapıştırın. 

    d. **Ayarları Kaydet**' e tıklayın.

### <a name="create-officespace-software-test-user"></a>Officesi yazılım test kullanıcısı oluşturma

Bu bölümde, Officesi yazılımında B. Simon adlı bir Kullanıcı oluşturulur. Officesi yazılımı, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı, Officesi yazılımında zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Officesi yazılım destek ekibine](mailto:support@officespacesoftware.com)başvurmanız gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Officesi yazılımı kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız officesi yazılımında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Officesi yazılımlarını deneyin](https://aad.portal.azure.com/)

