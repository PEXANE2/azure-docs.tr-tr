---
title: "Öğretici Azure Active Directory: Orgvıtitesi SSO 'SU ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs"
description: Azure Active Directory ve Orgvıtitesi SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/29/2020
ms.author: jeedes
ms.openlocfilehash: 4cb46487904f1f909686119d074dff5dcfad40c9
ms.sourcegitcommit: ffa7a269177ea3c9dcefd1dea18ccb6a87c03b70
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91604650"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-orgvitality-sso"></a>Öğretici: Orgvititesi SSO 'SU ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Orgvıtitesi SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Orgvıtitesi SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Orgvititesi SSO 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte etkinliği SSO 'SU için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Orgvıtitesi SSO çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Orgvıtitesi SSO, **IDP** tarafından başlatılan SSO 'yu destekliyor

## <a name="adding-orgvitality-sso-from-the-gallery"></a>Galeriden Orgvıtitesi SSO 'SU ekleme

Orgvıtitesi SSO 'yu Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Orgvititesi SSO 'SU eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Orgvıtitesi SSO** yazın.
1. Sonuçlar panelinden **Orgvıtitesi SSO** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-orgvitality-sso"></a>Orgvititesi SSO 'SU için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Orgvititesi SSO 'su ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı ve Orgvıtitesi SSO 'SU arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Orgvıtitesi SSO 'SU ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Orgvıtitesi SSO SSO 'Yu yapılandırın](#configure-orgvitality-sso-sso)** .
    1. Kullanıcı Azure AD gösterimi ile bağlantılı olan Orgvititesi SSO 'SU içindeki B. Simon 'a sahip olmak için **[Orgvıtitesi SSO test kullanıcısı oluşturun](#create-orgvitality-sso-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Orgvıtitesi SSO** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://rpt.orgvitality.com/<COMPANY_NAME>/`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://rpt.orgvitality.com/<COMPANY_NAME>Auth/default.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Orgvıtitesi SSO destek ekibine](https://orgvitality.com/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Orgvıtitesi SSO uygulamanız, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir. Orgvıtitesi SSO uygulaması, **NameIdentifier** 'ın **User. EmployeeID**ile eşlenmesini bekliyor, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/default-attributes.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Orgvıtitesi SSO ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Orgvıtitesi SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Orgvıtitesi SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-orgvitality-sso-sso"></a>Orgvıtitesi SSO SSO 'yu yapılandırma

**ORGVITITESI SSO** tarafında çoklu oturum açma 'yı yapılandırmak için, indirilen **sertifikayı (Base64)** ve Azure Portal ' den [orgvıtitesi SSO destek ekibine](https://orgvitality.com/contact-us/)uygun şekilde kopyalanmış URL 'leri göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-orgvitality-sso-test-user"></a>Orgvıtitesi SSO test kullanıcısı oluşturma

Bu bölümde, Orgvıtitesi SSO 'SU içinde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Orgvıtitesi SSO platformunda kullanıcıları eklemek için [ORGVITITESI SSO destek ekibi](https://orgvitality.com/contact-us/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

1. Azure portal bu uygulamayı test et ' e tıklayın ve SSO 'yu ayarladığınız Orgvıtitesi SSO 'SU için otomatik olarak oturum açmış olmanız gerekir

1. Microsoft Access panel ' i kullanabilirsiniz. Erişim panelinde Orgvıtitesi SSO kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Orgvıtitesi SSO 'SU için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki adımlar

Orgvititesi SSO 'yu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).
