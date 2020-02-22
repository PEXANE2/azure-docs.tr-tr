---
title: 'Öğretici: Splashtop ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Splashtop arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c05f63c2-4170-49ce-a967-be1cb1dbcd06
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6ecb03130e26d432f0bd10980c7c3553ce9f8b0
ms.sourcegitcommit: 163be411e7cd9c79da3a3b38ac3e0af48d551182
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77539789"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-splashtop"></a>Öğretici: Splashtop ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Karşılamaüst 'i tümleştirmeyi öğreneceksiniz. Azure AD ile Splashtop tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, Splashtop 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak en iyi şekilde oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Splashtop çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Splashtop, **SP** tarafından başlatılan SSO 'yu destekler

* Splashtop 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimleri uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-splashtop-from-the-gallery"></a>Galeriden bir Splashtop ekleme

Splashtop 'ın Azure AD ile tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize bir Splashtop eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Splashtop** yazın.
1. Sonuçlar panelinden **Splashtop** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-splashtop"></a>Splashtop için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Splashtop ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı ile Splashtop arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu, Splashtop ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Splashtop SSO 'Yu yapılandırma](#configure-splashtop-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Kullanıcının Azure AD gösterimine bağlı olan, Splashtop 'da B. Simon 'ın bir karşılığı olacak şekilde, **[Splashtop test kullanıcısı oluşturun](#create-splashtop-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Splashtop** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarla** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın: `https://my.splashtop.com/login/sso`

1. Splashtop uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir, ancak **NameIdentifier** **User. UserPrincipalName**ile eşlenir. Bilet Yöneticisi uygulaması, **NameIdentifier** 'ın **User. Mail**ile eşlenmesini bekliyor, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML ile çoklu oturum açmayı ayarla** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Ayarlama Splashtop** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Splashtop 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Splashtop**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-splashtop-sso"></a>Splashtop SSO 'yu yapılandırma

Bu bölümde, [Splashtop Web portalından](https://my.splashtop.com/login)yenı bir SSO yöntemi uygulamanız gerekir.
1. Splashtop web portalında **hesap bilgileri** / **Takım** sekmesi ' ne gidin, **Çoklu oturum açma** bölümünü bulmak için aşağı kaydırın. Ardından **yenı SSO yöntemi Için Uygula**' ya tıklayın.

    ![image](media/splashtop-tutorial/apply-for-new-SSO-method.png)

1. Uygulanan penceresinde bir **SSO adı**verin. Örneğin, yeni Azure, ıDP türü olarak **Azure** ' ı seçin ve Azure Portal ' de Azure AD tanımlayıcısı ' nı ve ' de Splashtop uygulamasından kopyalanmış **Azure ad tanımlayıcısını** **ekleyin.**

    ![image](media/splashtop-tutorial/azure-sso-1.png)

1. Sertifika bilgileri için, Azure portal üzerindeki Splashtop uygulamasından indirilen CERT dosyasına sağ tıklayın, Not defteri ile düzenleyin, sonra içeriği kopyalayın ve **sertifikayı indirin (base64)** alanına yapıştırın.

    ![görüntü](media/splashtop-tutorial/cert-1.png) ![görüntü](media/splashtop-tutorial/cert-2.png) ![görüntü](media/splashtop-tutorial/azure-sso-2.png)

1. İşte bu kadar! **Kaydet** ve SPLASHTOP SSO doğrulama ekibi, doğrulama bilgileri için sizinle iletişim kuracaktır, sonra SSO yöntemini etkinleştirir.

### <a name="create-splashtop-test-user"></a>Splashtop test kullanıcısı oluşturma

1. SSO yöntemi etkinleştirildikten sonra, **Çoklu oturum açma** bölümünde etkinleştirmek için lütfen yenı oluşturulan SSO metodunu denetleyin.

    ![image](media/splashtop-tutorial/enable.png)

1. Yeni oluşturulan SSO yöntemiyle, örneğin `B.Simon@contoso.com` test kullanıcısını, Splashtop ekibinize davet edin.

    ![image](media/splashtop-tutorial/invite.png)

1. Ayrıca, var olan bir Splashtop hesabını bir SSO hesabı olarak değiştirebilirsiniz, bkz. [yönergeler](https://support-splashtopbusiness.splashtop.com/hc/en-us/articles/360038685691-How-to-associate-SSO-method-to-existing-team-admin-member-).

1. İşte bu kadar! Bu SSO hesabını kullanarak, Splashtop web portalında veya Splashtop Business uygulamasında oturum açabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Splashtop kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Splashtop üzerinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Splashtop 'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Splashtop koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)