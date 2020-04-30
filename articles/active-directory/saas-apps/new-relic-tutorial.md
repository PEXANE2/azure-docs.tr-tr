---
title: 'Öğretici: hesaba göre yeni relik ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Hesaba göre Azure Active Directory ve yeni yeniden oturum açma arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: jeedes
ms.openlocfilehash: 9fdcec5b55f52b7b6b824bf2ba25c14541b2a3c6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82186575"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic-by-account"></a>Öğretici: hesaba göre yeni relik ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile yeni bir relik hesabını nasıl tümleştirileceğini öğreneceksiniz. Yeni relik hesabını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de hesaba göre yeni relik erişimine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla hesaba göre otomatik olarak oturum açmaya olanak sağlar.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Hesap çoklu oturum açma (SSO) etkin abonelik tarafından yeni relik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Yeni relik hesabı, **SP** tarafından başlatılan SSO 'yu destekler

* Yeni relik hesabını hesap bazında yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimleri uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-new-relic-by-account-from-the-gallery"></a>Galeriden hesap bazında yeni relik ekleme

Yeni relik hesabı ile Azure AD arasında tümleştirmeyi yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize yeni relik hesabı ekleyerek hesap eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Yeni bir relik hesabı** yazın.
1. Sonuçlar panelinden **Hesap bazında yeni relik** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-new-relic-by-account"></a>Yeni hesap için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu yeni relik hesabıyla yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında yeni relik hesabı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu yeni relik hesabıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Hesap SSO 'su Ile yeni relik ayarlarını yapılandırma](#configure-new-relic-by-account-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * **[Hesap testi kullanıcısına göre yeni bir relik oluşturun](#create-new-relic-by-account-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan hesaba göre yeni relik 'e göre B. Simon 'ya sahip olacak şekilde
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Yeni hesap** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)
1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login`-Hesaba göre yeni relik hesap KIMLIĞINIZ `acc_id` ile değiştirdiğinizden emin olun.

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna bir URL yazın:`rpm.newrelic.com`

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Hesaba göre yeni relik ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

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

Bu bölümde, yeni hesaba göre yeni relik erişimine izin vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Hesap bazında yeni relik**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-new-relic-by-account-sso"></a>Hesap SSO 'SU ile yeni relik yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, hesap şirket sitesinde yönetici olarak **Yeni relik** ortamınızda oturum açın.

2. Üstteki menüde **Hesap ayarları**' na tıklayın.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797036.png "Hesap ayarları")

3. **Güvenlik ve kimlik doğrulama** sekmesine tıklayın ve ardından **Çoklu oturum açma** sekmesine tıklayın.
   
    ![Çoklu oturum açma](./media/new-relic-tutorial/ic797037.png "Çoklu Oturum Açma")

4. SAML iletişim sayfasında, aşağıdaki adımları uygulayın:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. İndirilen Azure Active Directory sertifikanızı karşıya yüklemek için **Dosya Seç** ' e tıklayın.

    b. **Uzaktan oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.
   
    c. **Oturum kapatma giriş URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL 'si**değerini yapıştırın.

    d. **Değişikliklerimi kaydet**' e tıklayın.

### <a name="create-new-relic-by-account-test-user"></a>Hesap testi kullanıcısına göre yeni relik oluştur

1. Hesap şirket sitesinde yönetici olarak **Yeni relik** ortamınızda oturum açın.

2. Üstteki menüde **Hesap ayarları**' na tıklayın.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797040.png "Hesap ayarları")

3. Sol taraftaki **Hesap** bölmesinde, **Özet**' e ve ardından **Kullanıcı Ekle**' ye tıklayın.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797041.png "Hesap ayarları")

4. **Etkin kullanıcılar** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Etkin Kullanıcılar](./media/new-relic-tutorial/ic797042.png "Etkin Kullanıcılar")
   
    a. **E-posta** metin kutusuna, sağlamak istediğiniz geçerli bir Azure Active Directory kullanıcısının e-posta adresini yazın.

    b. **Rol** olarak **Kullanıcı**' yı seçin.

    c. **Bu kullanıcıyı Ekle**' ye tıklayın.

> [!NOTE]
> Hesap Kullanıcı hesabı oluşturma araçları veya Azure AD Kullanıcı hesaplarını sağlamak için yeni relik hesabı tarafından sunulan API 'Leri kullanarak başka bir yeni relik kullanmayı kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde yeni relik ölçütü hesap kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız hesaba göre otomatik olarak yeni yeniden oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile yeni bir relik hesabı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)