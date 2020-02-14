---
title: 'Öğretici: CODA ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Coda arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: acaf2012-ef2e-4ce0-8467-ceece3bae50e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/23/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74da278dbbc0ac32407c345524e224ca5f7616da
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77194761"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-coda"></a>Öğretici: CODA ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Coda 'yı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. CODA 'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Coda erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Coda 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* GDrive tümleştirmesi ile Coda çoklu oturum açma (SSO) özellikli abonelik (Kurumsal) devre dışı. Etkin durumda olan kuruluşunuz için GDrive tümleştirmesini devre dışı bırakmak üzere [Coda destek ekibine](mailto:support@coda.io) başvurun.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* CODA, **IDP** tarafından başlatılan SSO 'yu destekler

* CODA **, tam zamanında** Kullanıcı sağlamayı destekler

* CODA 'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve korunmasını koruyan oturum denetimleri uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-coda-from-the-gallery"></a>Galeriden Coda ekleme

CODA tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeriden, yönetilen SaaS uygulamaları listenize Coda eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Coda** yazın.
1. Sonuçlar panelinden **Coda** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-coda"></a>CODA için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu CODA ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Coda 'daki ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu CODA ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. CODA SSO yapılandırmasına başlamak için Coda **[SSO 'Yu kullanmaya başlayın](#begin-configuration-of-coda-sso)** .
1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
   * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
   * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. CODA SSO 'yu, Coda 'da çoklu oturum açma ayarlarının yapılandırılmasını tamamlayacak şekilde **[yapılandırın](#configure-coda-sso)** .
   * CODA, kullanıcının Azure AD gösterimine bağlı olan, Coda 'da B. Simon 'a karşılık gelen bir, **[Coda test kullanıcısı oluşturun](#create-coda-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="begin-configuration-of-coda-sso"></a>CODA SSO yapılandırmasına başla

Başlamak için Coda 'daki adımları izleyin.

1. CODA bölümünde **kuruluş ayarları** masasını açın.

   ![Kuruluş ayarlarını aç](media/coda-tutorial/org-settings.png)

1. Kuruluşunuzun GDrive tümleştirmesi kapalı olduğundan emin olun. Şu anda etkinse, GDrive 'a geçiş yapmanıza yardımcı olması için [Coda destek ekibine](mailto:support@coda.io) başvurun.

   ![GDrive devre dışı](media/coda-tutorial/gdrive-off.png)

1. **SSO (SAML) Ile kimlik doğrulaması**altında **SAML Yapılandır** seçeneğini belirleyin.

   ![SAML ayarları](media/coda-tutorial/saml-settings-link.png)

1. Sonraki adımlarda ihtiyacınız olacak **VARLıK kimliği** ve **SAML yanıtı URL 'si**değerlerini aklınızda bulabilirsiniz.

   ![Azure 'da kullanılacak varlık KIMLIĞI ve SAML yanıtı URL 'SI](media/coda-tutorial/azure-settings.png)

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Coda** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki alanlar için değerleri girin:

   a. **Tanımlayıcı** metin kutusunda, yukarıdaki "varlık kimliği" ni girin. Şu kalıbı izlemelidir: `https://coda.io/samlId/<CUSTOMID>`

   b. **Yanıt URL 'si** metin kutusunda, YUKARıDAKI "SAML yanıt URL 'si" nu girin. Şu kalıbı izlemelidir: `https://coda.io/login/sso/saml/<CUSTOMID>/consume`

   > [!NOTE]
   > Değerleriniz, Yukarıdakilerden farklı olacaktır; değerlerinizi, Coda "SAML yapılandırma" konsolunda bulabilirsiniz. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Coda ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Coda erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Coda**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-coda-sso"></a>CODA SSO 'yu yapılandırma

Kurulumu gerçekleştirmek için, Coda **SAML yapılandırma** paneline Azure Active Directory değerler girersiniz.

1. CODA bölümünde **kuruluş ayarları** masasını açın.
1. **SSO (SAML) Ile kimlik doğrulaması**altında **SAML Yapılandır** seçeneğini belirleyin.
1. **SAML sağlayıcısını** **Azure Active Directory**olarak ayarlayın.
1. **Kimlik sağlayıcısı oturum açma URL 'si**' nde, Azure konsolundan **oturum açma URL** 'sini yapıştırın.
1. **Kimlik sağlayıcısı yayımcısı**' nda Azure **ad tanımlayıcısını** Azure konsolundan yapıştırın.
1. **Kimlik sağlayıcısı genel sertifikası**' nda **sertifikayı karşıya yükle** seçeneğini seçin ve daha önce indirdiğiniz sertifika dosyasını seçin.
1. **Kaydet**’i seçin.

Bu, SAML SSO bağlantı kurulumu için gereken işi tamamlar.

### <a name="create-coda-test-user"></a>CODA test kullanıcısı oluşturma

Bu bölümde, Coda 'da Britta Simon adlı bir Kullanıcı oluşturulur. CODA, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten Coda 'da yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Coda kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Coda 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Coda 'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [, Gelişmiş görünürlük ve denetimlerle Coda koruması](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
