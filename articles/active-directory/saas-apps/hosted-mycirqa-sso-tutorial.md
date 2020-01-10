---
title: "Öğretici Azure Active Directory: barındırılan MyCirqa SSO 'SU ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs"
description: Azure Active Directory ile barındırılan MyCirqa SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4337e343-180d-475d-83ac-6f5b5f133dfe
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: adc6cd36190ca3847fdc9e3f6935aed5c78e0942
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75662342"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hosted-mycirqa-sso"></a>Öğretici: barındırılan MyCirqa SSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, barındırılan MyCirqa SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Barındırılan MyCirqa SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de barındırılan MyCirqa SSO 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla barındırılan MyCirqa SSO 'SU için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Barındırılan MyCirqa SSO çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Barındırılan MyCirqa SSO 'SU **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-hosted-mycirqa-sso-from-the-gallery"></a>Galeriden barındırılan MyCirqa SSO 'SU ekleme

Barındırılan MyCirqa SSO 'SU ile Azure AD arasında tümleştirmeyi yapılandırmak için, Galeriden barındırılan MyCirqa SSO 'SU ile yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **MYCIRQA SSO adlı** öğesini yazın.
1. Sonuçlar panelinden **barındırılan MyCirqa SSO** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-hosted-mycirqa-sso"></a>Barındırılan MyCirqa SSO 'SU için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu barındırılan MYCIRQA SSO 'su ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve barındırılan MyCirqa SSO 'SU içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu barındırılan MyCirqa SSO 'SU ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Barındırılan MyCirqa SSO 'Yu yapılandırma](#configure-hosted-mycirqa-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Barındırılan **[MYCIRQA SSO test kullanıcısı oluşturun](#create-hosted-mycirqa-sso-test-user)** -kullanıcının Azure AD gösterimine bağlı olan, barındırılan MYCIRQA SSO 'Su içinde B. Simon 'un bir karşılığı olacak şekilde.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) **barındırılan MYCIRQA SSO** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<SUBDOMAIN>.cirqahosting.com/CirqaIdentity/external?`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://isoxford.com/<CUSTOMID>/cirqaidentity/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [barındırılan MyCirqa SSO istemci destek ekibine](not sure) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, barındırılan MyCirqa SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **barındırılan MyCirqa SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-hosted-mycirqa-sso"></a>Barındırılan MyCirqa SSO 'yu yapılandırma

**Barındırılan MyCirqa SSO** tarafında çoklu oturum açmayı yapılandırmak Için, [barındırılan mycirqa SSO destek ekibine](mailto:support@isoxford.com) **uygulama Federasyon meta veri URL 'sini** göndermeniz gerekir. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

## <a name="create-hosted-mycirqa-sso-test-user"></a>Barındırılan MyCirqa SSO test kullanıcısı oluştur

Bu bölümde, barındırılan MyCirqa SSO 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. Barındırılan mycirqa SSO platformunda kullanıcıları eklemek için [barındırılan MyCirqa SSO destek ekibi](mailto:support@isoxford.com) ile çalışın. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde barındırılan MyCirqa SSO kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız barındırılan MyCirqa SSO 'SU için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile barındırılan MyCirqa SSO 'yu deneyin](https://aad.portal.azure.com/)