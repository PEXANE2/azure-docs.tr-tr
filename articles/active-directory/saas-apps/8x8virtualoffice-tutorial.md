---
title: 'Öğretici: 8x8 ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve 8x8 arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/20/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c81e63a073882dea3a7aea32d5e9f4d3d5c48f7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87018661"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-8x8"></a>Öğretici: 8x8 ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile 8x8 ' i tümleştirme hakkında bilgi edineceksiniz. 8x8 ' i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de 8x8 ' e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla 8x8 ' e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* 8x8 aboneliği.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* 8x8 **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

* 8x8 ' i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-8x8-from-the-gallery"></a>Galeriden 8x8 ekleniyor

8x8 ' in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden 8x8 ' i yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **8x8** yazın.
1. Sonuçlar panelinden **8x8** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-8x8"></a>8x8 için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak 8x8 Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve 8x8 ' deki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu 8x8 ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[8x8 SSO yapılandırma](#configure-8x8-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. 8x8 **[test kullanıcısı oluşturun](#create-8x8-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan 8x8 ' de B. Simon 'a karşılık gelen bir.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **8x8** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://sso.8x8.com/saml2`

    b. **Yanıt URL** 'si metin kutusuna bir URL yazın:`https://sso.8x8.com/saml2`

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin. Sertifikayı daha sonra öğreticide **yapılandırma 8x8 SSO** bölümünde kullanacaksınız.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **8x8** ' i ayarla bölümünde, URL 'leri kopyalayın ve bu URL değerlerini öğreticide daha sonra kullanacaksınız.

    ![Yapılandırma URL 'Lerini Kopyala](./media/8x8virtualoffice-tutorial/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, 8x8 ' e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **8x8**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-8x8-sso"></a>8x8 SSO yapılandırma

Öğreticinin sonraki bölümü, 8x8 ile sahip olduğunuz abonelik türüne bağlıdır.

* Yönetim için Configuration Manager kullanan 8x8 sürümleri ve X serisi müşterileri için, " [8x8 Configuration Manager yapılandırma](#configure-8x8-configuration-manager)konusuna bakın.
* Yönetim için hesap yöneticisi 'ni kullanan sanal Office müşterileri için, [bkz. 8x8 Hesap Yöneticisi 'Ni yapılandırma](#configure-8x8-account-manager).

### <a name="configure-8x8-configuration-manager"></a>8x8 ' i yapılandırma Configuration Manager

1. 8x8 [Configuration Manager](https://vo-cm.8x8.com/)oturum açın.

1. Giriş sayfasından **kimlik yönetimi**' ne tıklayın.

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure1.png)

1. **Çoklu oturum açmayı (SSO)** denetleyip **Microsoft Azure AD**seçin.

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure2.png)

1. Azure AD 'de **SAML Ile çoklu oturum açma ayarla** sayfasından üç URL 'yi ve imzalama sertifikasını 8x8 CONFIGURATION Manager **Microsoft Azure AD SAML ayarları** bölümüne kopyalayın.

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure3.png)

    a. **IDP oturum açma URL**'Sine **oturum açma URL** 'sini kopyalayın.

    b. **Azure ad tanımlayıcısını** **ıDP veren URL/urn**'ye kopyalayın.

    c. **Oturum kapatma URL 'Sini** **ıDP oturum kapatma URL**'sine kopyalayın.

    d. **Sertifikayı indirin (base64)** ve **sertifikayı**karşıya yükleyin.

    e. **Kaydet**’e tıklayın.

### <a name="configure-8x8-account-manager"></a>8x8 hesap yöneticisini Yapılandır

1. 8x8 sanal Office kiracınızda yönetici olarak oturum açın.

1. Uygulama panelinde **sanal Office hesabı MGR** ' i seçin.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

1. Yönetilecek **iş** hesabı ' nı seçin ve **oturum aç** düğmesine tıklayın.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

1. Menü listesinde **hesaplar** sekmesi ' ne tıklayın.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

1. Hesap listesinde **Çoklu oturum açma** ' ya tıklayın.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

1. Kimlik doğrulama yöntemlerinde **Çoklu oturum açma** ' yı seçin ve **SAML**' ye tıklayın.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

1. **SAML çoklu oturum açma** bölümünde aşağıdaki adımları uygulayın:

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. **Oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    b. **Oturumu Kapat URL** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.

    c. **Veren URL** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    d. Azure portal 'den indirdiğiniz sertifikayı karşıya yüklemek için, **Araştır** düğmesine tıklayın.

    e. **Kaydet** düğmesine tıklayın.

### <a name="create-8x8-test-user"></a>8x8 test kullanıcısı oluştur

Bu bölümde, 8x8 ' de Britta Simon adlı bir Kullanıcı oluşturacaksınız. 8x8 platformda kullanıcıları eklemek için [8x8 destek ekibi](https://www.8x8.com/about-us/contact-us) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde 8x8 kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız 8x8 ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile 8x8 deneme](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle 8x8 ' i koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)