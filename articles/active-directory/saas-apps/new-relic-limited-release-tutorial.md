---
title: 'Öğretici: yeni relik ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve yeni relik arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.openlocfilehash: 4cf3f9d0ae23bab4d2412b47e5841d6b8a56b65a
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96327075"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic"></a>Öğretici: yeni relik ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile yeni bir relik tümleştirmeyi nasıl tümleştirileceğini öğreneceksiniz. Yeni bir depoyu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de yeni relik erişimine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla yeni bir depoda otomatik olarak oturum açmalarına olanak sağlar.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Yeni relik çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Yeni relik **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler.
* Yeni bir relik yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verilere yönelik olarak korunmasını sağlayan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).

## <a name="add-new-relic-application-from-the-gallery"></a>Galeriden yeni relik uygulaması ekleme

Yeni relik 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize **Yeni relik (kuruluşa göre)** eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. **Azure Active Directory** hizmeti seçin.
1. **Kurumsal uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Azure AD galerisine gözatamıyorum** sayfasında, arama kutusuna **Yeni relik (kuruluşa göre)** yazın.
1. Sonuçlar panelinden **Yeni relik (kuruluşa göre)** öğesini seçin ve ardından **Oluştur**' u seçin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-new-relic"></a>Yeni relik için Azure AD SSO 'yu yapılandırın ve test edin

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu yeni relik ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında yeni relik bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu yeni relik ile yapılandırmak ve test etmek için aşağıdaki adımları izleyin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
   1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
   1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Yeni relik, yeni relik tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Yeni RELIK SSO 'Yu yapılandırın](#configure-new-relic-sso)** .
   1. **[Yeni bir relik test kullanıcısı oluşturun](#create-a-new-relic-test-user)** ve bu, Azure AD kullanıcısına bağlı yeni relik 'de B. Simon için bir karşılığına sahip olur.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **yeni kuruluşa göre** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.

1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.

1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde **tanımlayıcı** ve **yanıt URL 'si** değerlerini girin.

   * Bu değerler, yeni Relü **kuruluş uygulamamı** kullanılarak alınamam. Bu uygulamayı kullanmak için şu adımları uygulayın:
      1. Yeni relik ['de oturum açın](https://login.newrelic.com/) .
      1. Üstteki menüden **uygulamalar**' ı seçin.
      1. **Uygulamalarınız** bölümünde **Kuruluşum**' u seçin.
      1. **Kimlik doğrulama etki alanları**' na tıklayın.
      1. Azure AD SSO 'nun bağlanmasını istediğiniz kimlik doğrulama etki alanını seçin (birden fazla kimlik doğrulama etki alanınız varsa). Çoğu şirketin **varsayılan** olarak yalnızca bir kimlik doğrulama etki alanı vardır. Yalnızca bir kimlik doğrulama etki alanı ile seçim yapmanız gerekmez.
      1. **Kimlik doğrulama** bölümünde **onaylama tüketici URL 'SI** , **yanıt URL**'si için kullanılacak değeri içerir.
      1. **Kimlik doğrulama** bölümünde, **varlık kimliğiniz** **tanımlayıcı** için kullanılacak değeri içerir.

1. **Kullanıcı öznitelikleri & talepler** bölümünde, **benzersiz kullanıcı tanımlayıcısının** yeni bir relik sırasında kullanılan e-posta adresini içeren bir alanla eşlendiğinden emin olun.

   * Varsayılan alan olan **User. UserPrincipalName** değeri, yeni relik e-posta adresleriyle aynı ise, sizin için çalışacaktır.
   * User. **userPrincipalName** yeni relik e-posta adresi değilse, **Kullanıcı. Mail** alanı sizin için daha iyi çalışabilir.

1. **SAML Imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalayın ve daha sonra kullanmak üzere bu değeri kaydedin.

1. **Kuruluşa göre yeni relik ayarla** bölümünde, **oturum açma URL 'sini** kopyalayın ve daha sonra kullanmak üzere bu değeri kaydedin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal **Azure Active Directory** hizmet ' i seçin.
1. **Kullanıcılar**’ı seçin.
1. Yeni bir kullanıcı eklemek için ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Yeni Kullanıcı** sayfasında, aşağıdaki adımları izleyin:
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `b.simon@contoso.com`. Bu, yeni relik tarafında kullanacağınız e-posta adresiyle eşleşmelidir.
   1. **Ad** alanına `B.Simon` girin.  
   1. **Parolayı göster** onay kutusunu işaretleyin ve ardından **ilk parola** alanında görüntülenen değeri kaydedin.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, **kuruluş uygulamasına göre yeni relik** erişimine Izin vererek Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Azure Active Directory** hizmet ' i seçin.
1. **Kurumsal uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **kuruluşa göre yeni relik**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** (veya plan düzeyinize bağlı olarak **Kullanıcılar** ) seçeneğini belirleyin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** (veya **Kullanıcılar**) iletişim kutusunda, kullanıcılar listesinden **B. Simon** öğesini seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-new-relic-sso"></a>Yeni relik SSO 'yu yapılandırma

Yeni relik 'de SSO 'yu yapılandırmak için bu adımları izleyin.

1. Yeni relik ['de oturum açın](https://login.newrelic.com/) .

1. Üstteki menüden **uygulamalar**' ı seçin.

1. **Uygulamalarınız** bölümünde **Kuruluşum**' u seçin.

1. **Kimlik doğrulama etki alanları**' na tıklayın.

1. Azure AD SSO 'nun bağlanmasını istediğiniz kimlik doğrulama etki alanını seçin (birden fazla kimlik doğrulama etki alanınız varsa). Çoğu şirketin **varsayılan** olarak yalnızca bir kimlik doğrulama etki alanı vardır. Yalnızca bir kimlik doğrulama etki alanı ile seçim yapmanız gerekmez.

1. **Kimlik doğrulaması** bölümünde, **Yapılandır**' a tıklayın.

   1. **SAML meta veri kaynağı** alanında, daha önce Azure AD tarafı **uygulaması Federasyon meta verileri URL 'si** alanından kaydettiğiniz değeri girin.

   1. **SSO hedef URL 'si** alanında, daha önce Azure AD tarafı **oturum açma URL 'si** alanından kaydettiğiniz değeri girin.

   1. Ayarları doğruladıktan sonra **Kaydet** ' e TıKLAYARAK Azure AD ve yeni relik tarafları üzerinde her ikisi de iyi görünür. Her iki taraf düzgün yapılandırılmamışsa, kullanıcılarınız yeni bir depoda oturum açamaz.

### <a name="create-a-new-relic-test-user"></a>Yeni bir relik test kullanıcısı oluşturma

Bu bölümde, yeni relik 'de B. Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcı oluşturmak için bu adımları izleyin.

1. Yeni relik ['de oturum açın](https://login.newrelic.com/) .

1. Üstteki menüden **uygulamalar**' ı seçin.

1. **Uygulamalarınız** bölümünde **Kullanıcı yönetimi**' ni seçin.

1. **Kullanıcı Ekle** düğmesine tıklayın.

   1. **Ad** alanına **B. Simon** girin.
   
   1. **E-posta** ALANıNA Azure AD SSO tarafından gönderilecek değeri girin.
   
   1. Kullanıcı için bir Kullanıcı **türü** ve Kullanıcı **grubu** seçin. Bir test kullanıcısı için, türü ve **Kullanıcı** grubu Için **temel Kullanıcı** makul seçimlerdir.
   
   1. Kullanıcıyı kaydetmek için **Kullanıcı Ekle** ' ye tıklayın.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde **kuruluşa göre yeni relik** kutucuğuna tıkladığınızda, otomatik olarak yeni relik oturumu açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile yeni bir relik deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](/cloud-app-security/proxy-intro-aad)
