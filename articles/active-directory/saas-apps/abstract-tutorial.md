---
title: 'Öğretici: abstract ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve soyut arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.openlocfilehash: 3f19733f01cf601c1145d5e2bb5deb58db828dc4
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88538340"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Öğretici: Özet Azure Active Directory ile tümleştirin

Bu öğreticide, özetin Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Özet 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de soyut erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla soyut olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Soyut çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Abstract **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-abstract-from-the-gallery"></a>Galeriden Özet ekleme

Özetin Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Özet eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Özet** yazın.
1. Sonuçlar panelinden **Özet** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu abstract ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında Özet olarak bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Özet ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. **[Soyut SSO 'Yu yapılandırma](#configure-abstract-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan abstract 'te Britta Simon 'ın bir karşılığı olacak şekilde **[soyut test kullanıcısı oluşturun](#create-abstract-test-user)** .
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **soyut** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL 'ler Azure ile önceden doldurulmuştur. Kullanıcının **Kaydet** düğmesine tıklayarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://app.abstract.com/signin`

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-abstract-sso"></a>Soyut SSO 'yu yapılandırma

' `App Federation Metadata Url` I ve ' `Azure AD Identifier` yi, soyut üzerinde SSO 'yu yapılandırmak için gerekli olacak şekilde Azure Portal aldığınızdan emin olun.

Bu bilgileri, **SAML Ile çoklu oturum açma ayarlama** sayfasında bulabilirsiniz:

* , `App Federation Metadata Url` **SAML imzalama sertifikası** bölümünde bulunur.
* , `Azure AD Identifier` **Soyut ayarla** bölümünde bulunur.


Artık, soyut olarak SSO 'yu yapılandırmaya hazırsınız:

>[!Note]
>Soyut olan SSO ayarlarına erişmek için bir kuruluş yöneticisi hesabıyla kimlik doğrulaması yapmanız gerekir.

1. [Soyut Web uygulamasını](https://app.abstract.com/)açın.
2. Sol taraftaki çubukta **izinler** sayfasına gidin.
3. **SSO yapılandırma** bölümünde, **meta veri URL 'SINI** ve **varlık kimliğinizi**girin.
4. Sahip olabileceğiniz herhangi bir el ile özel durumu girin. El ile özel durumlar bölümünde listelenen e-postalar, SSO 'yu atlar ve e-posta ve parolayla oturum açabiliyor. 
5. **Değişiklikleri Kaydet**’e tıklayın.

>[!Note] 
>El ile özel durumlar listesindeki birincil e-posta adreslerini kullanmanız gerekir. Listenizde bir kullanıcının ikincil e-postası varsa, bu e-posta, SSO etkinleştirmesi başarısız olur. Bu durumda, başarısız olan hesabın birincil e-postasına sahip bir hata iletisi görürsünüz. Kullanıcıyı öğrendiklerinizi doğruladıktan sonra bu birincil e-postayı el ile özel durumlar listesine ekleyin.

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

Bu bölümde, abstract 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **soyut**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-abstract-test-user"></a>Soyut test kullanıcısı oluştur

Özet üzerindeki SSO 'yu test etmek için:

1. [Soyut Web uygulamasını](https://app.abstract.com/)açın.
2. Sol taraftaki çubukta **izinler** sayfasına gidin.
3. **Hesabım Ile test et**' e tıklayın. Test başarısız olursa, lütfen [destek ekibimize başvurun](https://www.abstract.com/help/contact/).

>[!Note]
>Soyut olan SSO ayarlarına erişmek için bir kuruluş yöneticisi hesabıyla kimlik doğrulaması yapmanız gerekir.
Bu kuruluş yöneticisi hesabının Azure portal soyut olarak atanması gerekir.

### <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Özet kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız özetin otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

