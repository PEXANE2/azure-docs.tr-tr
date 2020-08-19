---
title: 'Öğretici: Floxx Labs ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Floxx Labs arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/21/2020
ms.author: jeedes
ms.openlocfilehash: 756bbaab1cbf359def01e371e4370607bee67ce3
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554872"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fluxx-labs"></a>Öğretici: Floxx Labs ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Floxx laboratuvarlarını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Floxx laboratuvarlarını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Floxx Labs 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Floxx Labs 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Floxx Labs çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Floxx Labs **IDP** tarafından başlatılan SSO 'yu destekler
* Floxx laboratuvarlarını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Floxx Labs ekleme

Floxx Labs 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize Floxx Labs eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **floxx Labs** yazın.
1. Sonuçlar panelinden **Floxx Labs** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-fluxx-labs"></a>Floxx Labs için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısını kullanarak Azure AD SSO 'Yu Floxx Labs ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Floxx laboratuvardaki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu akışkan xx Labs ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Akışkan xx Labs SSO](#configure-fluxx-labs-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcının Azure AD gösterimine bağlı olan Floxx laboratuvarlarında B. Simon 'a sahip olmak için **[Floxx Labs test kullanıcısı oluşturun](#create-fluxx-labs-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **floxx Labs** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki adımları uygulayın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    | Ortam | URL kalıbı|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    | Ortam | URL kalıbı|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Floxx Labs istemci destek ekibine](https://fluxx.zendesk.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Akışkan xx laboratuvarlarını ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

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

Bu bölümde, Floxx Labs 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Floxx Labs**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-fluxx-labs-sso"></a>Floxx Labs SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Floxx Labs şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar** bölümünün altındaki **yönetici** ' yi seçin.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config1.png)

3. Yönetim panelinde, **Eklentiler**  >  **tümleştirme** ' i seçin ve ardından **SAML SSO-(devre dışı)** seçeneğini belirleyin.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config2.png)

4. Öznitelik bölümünde aşağıdaki adımları uygulayın:

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config3.png)

    a. **SAML SSO** onay kutusunu seçin.

    b. **Istek yolu** metin kutusuna **/AUTH/SAML**yazın.

    c. **Geri çağırma yolu** metin kutusuna **/AUTH/SAML/callback**yazın.

    d. **Onaylama tüketici hizmeti URL 'si (çoklu oturum açma URL 'si)** metin kutusunda, Azure Portal girdiğiniz **yanıt URL 'si** değerini girin.

    e. **Hedef kitle (SP VARLıK kimliği)** metin kutusuna, Azure Portal girdiğiniz **tanımlayıcı** değerini girin.

    f. **Kimlik sağlayıcısı SSO hedef URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    örneğin: Base-64 kodlu sertifikanızı Not defteri 'nde açın, içeriğini panonuza kopyalayın ve ardından **kimlik sağlayıcısı sertifikası** metin kutusuna yapıştırın.

    h. **Ad tanımlayıcı biçimi** metin kutusuna değeri girin `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress` .

    i. **Kaydet**’e tıklayın.

    > [!NOTE]
    > İçerik kaydedildikten sonra, bu alan güvenlik için boş görünür, ancak değer yapılandırmada kaydedilir.

### <a name="create-fluxx-labs-test-user"></a>Floxx Labs test kullanıcısı oluşturma

Azure AD kullanıcılarının akıcı xx Labs 'de oturum açmasını sağlamak için, Floxx Labs 'e sağlanması gerekir. Akıcı xx Labs durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Floxx Labs şirket sitenizde yönetici olarak oturum açın.

2. Aşağıdaki görüntülenmiş **simgeye**tıklayın.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config6.png)

3. Panoda, **yenı kişiler** kartını açmak için aşağıdaki görüntülenmiş simgeye tıklayın.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config4.png)

4. **Yenı kişiler** bölümünde aşağıdaki adımları uygulayın:

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config5.png)

    a. Floxx Labs, SSO oturum açmaları için benzersiz tanımlayıcı olarak e-postayı kullanır. **SSO UID** ALANıNı, SSO ile oturum açma olarak kullandıkları e-posta adresiyle eşleşen kullanıcının e-posta adresiyle doldurun.

    b. **Kaydet**’e tıklayın.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Floxx Labs kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Floxx laboratuvarlarında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Floxx Labs 'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Floxx laboratuvarlarını koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)