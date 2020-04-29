---
title: 'Öğretici: Saba TalentSpace ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Saba TalentSpace arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/20/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76c13a649e1c1888a11e02b83d969255615cdc67
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77561398"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-saba-talentspace"></a>Öğretici: Saba TalentSpace ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Saba TalentSpace 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Saba TalentSpace 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Saba TalentSpace erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Saba TalentSpace 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Saba TalentSpace çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Saba TalentSpace **SP** tarafından başlatılan SSO 'yu destekler
* Saba TalentSpace 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve korunmasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-saba-talentspace-from-the-gallery"></a>Galeriden Saba TalentSpace ekleme

Saba TalentSpace 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden Saba TalentSpace 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Saba TalentSpace** yazın.
1. Sonuçlar panelinden **Saba TalentSpace** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-saba-talentspace"></a>Saba TalentSpace için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Saba TalentSpace Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Saba TalentSpace 'teki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Saba TalentSpace ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Saba TalentSpace SSO 'Yu yapılandırma](#configure-saba-talentspace-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Saba **[talentspace test kullanıcısı oluşturma](#create-saba-talentspace-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Saba talentspace 'te B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Saba TalentSpace** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/login`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/metadata`

    c. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/SSO`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Saba TalentSpace istemci destek ekibine](https://support.saba.com/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Saba TalentSpace ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Saba TalentSpace erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Saba TalentSpace**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-saba-talentspace-sso"></a>Saba TalentSpace SSO 'yu yapılandırma

1. Farklı bir tarayıcı penceresinde, **Saba TalentSpace** uygulamanızda yönetici olarak oturum açın.

2. **Seçenekler** sekmesine tıklayın.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-12.png)

3. Sol gezinti bölmesinde **SAML yapılandırması**' na tıklayın.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-13.png)

4. **SAML yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-14.png)

    a. **Benzersiz tanımlayıcı**olarak, **NameID**' yi seçin.

    b. **Benzersiz tanımlayıcı Ile eşleniyorsa** **Kullanıcı adı**' nı seçin.
  
    c. İndirilen meta veri dosyanızı karşıya yüklemek için, dosya ' ya ve ardından **dosyayı karşıya yüklemek**Için, **Araştır** ' a tıklayın.

    d. Yapılandırmayı test etmek için **Test Çalıştır**' a tıklayın.

    > [!NOTE]
    > "SAML testi tamamlanmıştır" iletisini beklemeniz gerekir *. Lütfen bu pencereyi kapatın*. Sonra, açılan tarayıcı penceresini kapatın. **SAML etkinleştir** onay kutusu yalnızca test tamamlandıysa etkinleştirilir.

    e. **SAML etkinleştir**' i seçin.

    f. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-saba-talentspace-test-user"></a>Saba TalentSpace test kullanıcısı oluştur

Bu bölümün amacı, Saba TalentSpace 'te Britta Simon adlı bir Kullanıcı oluşturmaktır.

**Saba TalentSpace 'te Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. **Saba TalentSpace** uygulamanızda yönetici olarak oturum açın.

2. **Kullanıcı Merkezi** sekmesine tıklayın ve ardından **Kullanıcı oluştur**' a tıklayın.

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-300.png)  

3. **Yeni Kullanıcı** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-301.png)

    a. **Ilk ad** metin kutusuna **B**gibi kullanıcının adını yazın.

    b. **Soyadı** metin kutusunda, **Simon**adlı kullanıcının soyadını yazın.

    c. **Kullanıcı adı metin kutusuna** **B. simon**yazın ve Azure Portal.

    d. **Parola** metin kutusuna B. Simon için bir parola yazın.

    e. **Kaydet**’e tıklayın.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Saba TalentSpace kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Saba TalentSpace 'te otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Saba TalentSpace 'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)