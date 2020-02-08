---
title: 'Öğretici: yeni relik (sınırlı sürüm) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve yeni relik (sınırlı sürüm) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ffbcf4a-b6b4-45ac-922f-358abe4c05e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad229e7964b6e7ad0e3f0d58cfc09669ee2fb301
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77088203"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic-limited-release"></a>Öğretici: yeni relik (sınırlı sürüm) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, yeni relik (sınırlı sürüm) Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Yeni relik (sınırlı sürüm) ile Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de yeni relik (sınırlı sürüm) erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla yeni bir relik (sınırlı sürüm) ile otomatik olarak oturum açmaları için izin sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Yeni relik (sınırlı sürüm) çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Yeni relik (sınırlı sürüm) **IDP** tarafından başlatılan SSO 'yu destekler

* Yeni yeniden oluşturmayı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimlerini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-new-relic-limited-release-from-the-gallery"></a>Galeriden yeni relik (sınırlı sürüm) ekleme

Yeni relik (sınırlı sürüm) ile Azure AD arasında tümleştirmeyi yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize yeni relik (sınırlı sürüm) eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Yeni relik (sınırlı sürüm)** yazın.
1. Sonuçlar panelinden **Yeni relik (sınırlı yayın)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-new-relic-limited-release"></a>Yeni relik için Azure AD çoklu oturum açmayı yapılandırma ve test etme (sınırlı sürüm)

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu yeni relik (sınırlı sürüm) ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında yeni bir relik (sınırlı yayın) arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu yeni relik (sınırlı yayın) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Yeni relik (sınırlı sürüm) SSO 'Yu yapılandırma](#configure-new-relic-limited-release-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Kullanıcının Azure AD gösterimine bağlı yeni relik (sınırlı sürüm) ile B. Simon ' a sahip olmak için **[Yeni relik (sınırlı yayın) bir test kullanıcısı oluşturun](#create-new-relic-limited-release-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Yeni relik (sınırlı sürüm)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız**varsa, aşağıdaki adımları uygulayın:

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![image](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL** değerleri temel SAML yapılandırması bölümünde otomatik olarak doldurulur:

    ![image](common/idp-intiated.png)

    > [!Note]
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak almıyorsanız, değerleri gereksinimlerinize göre el ile girin.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, yeni relik (sınırlı yayın) erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Yeni relik (sınırlı yayın)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-new-relic-limited-release-sso"></a>Yeni relik (sınırlı sürüm) SSO yapılandırma

**Yeni relik (sınırlı yayın** ) tarafında çoklu oturum açmayı yapılandırmak Için, **uygulama Federasyon meta verileri URL 'Sini** [Yeni relik (sınırlı sürüm) destek ekibine](https://support.newrelic.com/)göndermeniz gerekir. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.


### <a name="create-new-relic-limited-release-test-user"></a>Yeni relik (sınırlı yayın) oluştur test kullanıcısı

Bu bölümde, yeni relik (sınırlı sürüm) olarak B. Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları yeni relik (sınırlı yayın) platformunda eklemek için [Yeni relik (sınırlı sürüm) destek ekibi](https://support.newrelic.com/) . Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde yeni relik (sınırlı yayın) kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız yeni relik (sınırlı sürüm) ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile yeni relik (sınırlı sürüm) deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)