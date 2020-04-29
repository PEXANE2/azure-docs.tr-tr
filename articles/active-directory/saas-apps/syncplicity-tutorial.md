---
title: 'Öğretici: syncplicity ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve syncplicity arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: 663958ae367162eaeb336c819d1d219dc74a2cbe
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74233278"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Öğretici: syncplicity ile tümleştirin Azure Active Directory

Bu öğreticide, syncplicity 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Syncplicity 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de syncplicity 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla syncplicity otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü edinebilirsiniz.
* Syncplicity çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Syncplicity, **SP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-syncplicity-from-the-gallery"></a>Galeriden syncplicity ekleme

Syncplicity tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden syncplicity yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **syncplicity** yazın.
1. Sonuçlar panelinden **syncplicity** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu syncplicity ile yapılandırın ve test edin. SSO 'nun çalışması için, syncplicity içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu syncplicity ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[SYNCPLICITY SSO 'Yu yapılandırın](#configure-syncplicity-sso)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan syncplicity 'de B. Simon 'ya karşılık gelen bir **[syncplicity test kullanıcısı oluşturun](#create-syncplicity-test-user)** .
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **syncplicity** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.syncplicity.com`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [syncplicity istemci destek ekibine](https://www.syncplicity.com/contact-us) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Syncplicity ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso"></a>Syncplicity SSO 'yu yapılandırma

1. **Syncplicity** kiracınızda oturum açın.

1. Üstteki menüde, **yönetici**' ye tıklayın, **Ayarlar**' ı seçin ve ardından **özel etki alanı ve çoklu oturum açma**' ya tıklayın.

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. **Çoklu oturum açma (SSO)** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma \(SSO 'su\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    a. **Özel etki alanı** metin kutusuna etki alanı adını yazın.
  
    b. **Çoklu oturum açma durumu**olarak **etkin** ' i seçin.

    c. **Varlık kimliği** metin kutusunda, Azure Portal **temel SAML yapılandırmasında** KULLANDıĞıNıZ **tanımlayıcı (varlık kimliği)** değerini yapıştırın.

    d. **Oturum açma sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL** 'sini yapıştırın.

    e. **Oturum kapatma sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL** 'sini yapıştırın.

    f. **Kimlik sağlayıcısı sertifikası**' nda **Dosya Seç**' e tıklayın ve ardından Azure Portal indirdiğiniz sertifikayı karşıya yükleyin.

    g. **Değişiklikleri Kaydet**' e tıklayın.

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

Bu bölümde, syncplicity 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **syncplicity**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-syncplicity-test-user"></a>Syncplicity test kullanıcısı oluştur

Azure AD kullanıcılarının oturum açabilmeleri için syncplicity uygulamasına sağlanması gerekir. Bu bölümde, syncplicity içinde Azure AD Kullanıcı hesaplarının nasıl oluşturulacağı açıklanmaktadır.

**Syncplicity 'e bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Syncplicity** kiracınızda oturum açın (örneğin: `https://company.Syncplicity.com`).

1. **Yönetici** ' ye tıklayın ve **Kullanıcı hesapları** ' nı seçtikten sonra **Kullanıcı Ekle**' ye tıklayın.

    ![Kullanıcıları yönetme](./media/syncplicity-tutorial/ic769764.png "Kullanıcıları Yönet")

1. Sağlamak istediğiniz bir Azure AD hesabının **e-posta adreslerini** yazın, **rol**olarak **Kullanıcı** ' yı seçin ve ardından **İleri**' ye tıklayın.

    ![Hesap bilgileri](./media/syncplicity-tutorial/ic769765.png "Hesap Bilgileri")

    > [!NOTE]
    > Azure AD hesap sahibi, hesabı onaylamak ve etkinleştirmek için bir bağlantı içeren bir e-posta alır.

1. Şirketinizde yeni Kullanıcı üyesi olması gereken bir grup seçin ve ardından **İleri**' ye tıklayın.

    ![Grup üyeliği](./media/syncplicity-tutorial/ic769772.png "Grup üyeliği")

    > [!NOTE]
    > Listede Grup yoksa **İleri**' ye tıklayın.

1. Kullanıcının bilgisayarında syncplicity 'in denetimi altına yerleştirmek istediğiniz klasörleri seçin ve ardından **İleri**' ye tıklayın.

    ![Syncplicity klasörleri](./media/syncplicity-tutorial/ic769773.png "Syncplicity klasörleri")

> [!NOTE]
> Azure AD Kullanıcı hesaplarını sağlamak için syncplicity tarafından sunulan diğer tüm syncplicity Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde syncplicity kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız syncplicity için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)