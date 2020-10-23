---
title: 'Öğretici: Hoxhunt ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Hoxhunt arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: jeedes
ms.openlocfilehash: e18cd80c0232c4552c0e3dc57a23477d410c5395
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92442584"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hoxhunt"></a>Öğretici: Hoxhunt ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Hoxhunt 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Hoxhunt 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Hoxhunt 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Hoxhunt 'ye otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Hoxhunt çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Hoxhunt, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-hoxhunt-from-the-gallery"></a>Galeriden Hoxhunt ekleme

Hoxhunt 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Hoxhunt eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Hoxhunt** yazın.
1. Sonuçlar panelinden **Hoxhunt** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-hoxhunt"></a>Hoxhunt için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Hoxhunt Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Hoxhunt içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Hoxhunt ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Hoxhunt SSO 'Yu yapılandırın](#configure-hoxhunt-sso)** .
    1. Kullanıcının Azure AD gösterimine bağlı olan Hoxhunt 'de B. Simon 'a sahip olmak için **[hoxhunt test kullanıcısı oluşturun](#create-hoxhunt-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Hoxhunt** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusuna URL 'yi yazın:`https://app.hoxhunt.com/`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://app.hoxhunt.com/saml/consume/<ID>`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://app.hoxhunt.com/saml/consume/<ID>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek yanıt URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Hoxhunt istemci destek ekibine](mailto:support@hoxhunt.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Hoxhunt ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Hoxhunt 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Hoxhunt**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-hoxhunt-sso"></a>Hoxhunt SSO 'yu yapılandırma

**Hoxhunt** tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (Base64)** ve Azure Portal ' den [hoxhunt destek ekibine](mailto:support@hoxhunt.com)uygun şekilde kopyalanmış URL 'leri göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-hoxhunt-test-user"></a>Hoxhunt test kullanıcısı oluşturma

Bu bölümde, Hoxhunt 'de Britta Simon adlı bir Kullanıcı oluşturacaksınız. Barındırxhunt platformunda kullanıcıları eklemek için [hoxhunt destek ekibi](mailto:support@hoxhunt.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

1. Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Hoxhunt oturum açma URL 'sine yeniden yönlendirilir. 

2. Doğrudan Hoxhunt oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

3. Microsoft Access panel ' i kullanabilirsiniz. Erişim panelinde Hoxhunt kutucuğuna tıkladığınızda bu, Hoxhunt oturum açma URL 'sine yeniden yönlendirilir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Hoxhunt 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve infilini koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).