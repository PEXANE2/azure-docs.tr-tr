---
title: 'Öğretici: Mimecast kişisel portalı ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Mimecast kişisel portalı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d28835472198a1fddc5f7ed0fe5f0037b602f039
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848496"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mimecast-personal-portal"></a>Öğretici: Mimecast kişisel portalı ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Mimecast kişisel portalını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Mimecast kişisel portalını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Mimecast kişisel portalına erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla, Mimecast kişisel Portalı ' na otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Mimecast kişisel portalı çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Mimecast kişisel portalı **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Mimecast kişisel portalını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin bir kısmını gerçek zamanlı olarak koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Galeriden Mimecast kişisel portalı ekleme

Mimecast kişisel portalının tümleştirmesini Azure AD 'ye yapılandırmak için, Galeriden Mimecast kişisel portalı 'nı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Mimecast kişisel portalı** yazın.
1. Sonuçlar panelinden **Mimecast kişisel portalı** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-mimecast-personal-portal"></a>Mimecast kişisel portalı için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Mimecast kişisel portalı Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Mimecast kişisel portalındaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Mimecast kişisel portalı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Mimecast Personal Portal SSO 'Yu yapılandırma](#configure-mimecast-personal-portal-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Mimecast kişisel **[portalı test kullanıcısı oluşturma](#create-mimecast-personal-portal-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Mimecast kişisel portalı 'nda B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Mimecast kişisel portalı** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    | Bölge  |  Değer | 
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/sso/<accountcode>`|
    | Güney Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Avustralya       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Yurtdışında        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    > [!NOTE]
    > Bu `accountcode` değeri, **Hesap**  >  **ayarları**  >  **Hesap kodu**altında Mimecast kişisel portalında bulabilirsiniz. `accountcode`Tanımlayıcısını tanımlayıcıya ekleyin.

    b. **Yanıt URL** 'si metin kutusuna bir URL yazın:

    | Bölge  |  Değer |
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:

    | Bölge  |  Değer |
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

1. **Kaydet**’e tıklayın.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Mimecast kişisel portalına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Mimecast kişisel portalı**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-mimecast-personal-portal-sso"></a>Mimecast kişisel portalı SSO 'SU yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Mimecast yönetim konsolunda oturum açın.

1. **Yönetim**  >  **Hizmetleri**  >  **uygulamalarına**gidin.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/services.png)

1. **Kimlik doğrulama profilleri** sekmesi ' ne tıklayın.
    
    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/authentication-profiles.png)

1. **Yeni kimlik doğrulama profili** sekmesi ' ne tıklayın.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/new-authenticatio-profile.png)

1. **Açıklama** metin kutusuna geçerli bir açıklama girin ve **Mimecast kIşIsel portalı Için SAML kimlik doğrulamasını zorla** onay kutusunu seçin.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/selecting-personal-portal.png)

1. **Mimecast kişisel portalı Için SAML yapılandırması** sayfasında, aşağıdaki adımları uygulayın:

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/sso-settings.png)

    a. **Sağlayıcı**Için, açılan listeden **Azure Active Directory** seçin.

    b. **Meta veri URL 'si** metin kutusunda, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'si** değerini yapıştırın.

    c. **İçeri Aktar**’a tıklayın. Meta veri URL 'sini içeri aktardıktan sonra alanlar otomatik olarak doldurulur ve bu alanlar üzerinde herhangi bir işlem gerçekleştirmeye gerek kalmaz.

    d. **Parola korumalı bağlam kullan** ' ın işaretini kaldırın ve **Tümleşik kimlik doğrulaması bağlam** onay kutularını kullanın.

    e. **Kaydet**’e tıklayın.

### <a name="create-mimecast-personal-portal-test-user"></a>Mimecast kişisel portalı test kullanıcısı oluştur

1. Farklı bir Web tarayıcısı penceresinde, Mimecast yönetim konsolunda oturum açın.

1. **Yönetim**  >  **dizinleri**  >  **iç dizinleri**' ne gidin.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/internal-directories.png)

1. Etki alanı aşağıda bahsedildiğinde, etki alanınızı seçin, aksi takdirde lütfen **yeni etki alanına**tıklayarak yeni bir etki alanı oluşturun.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/domain-name.png)

1. **Yeni adres** sekmesi ' ne tıklayın.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/new-address.png)

1. Aşağıdaki sayfada gerekli Kullanıcı bilgilerini sağlayın:

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/user-information.png)

    a. **E-posta adresi** metin kutusuna, gibi kullanıcının e-posta adresini girin `B.Simon@yourdomainname.com` .

    b. **Genel ad** metin kutusuna kullanıcının **tam adını** girin.

    c. **Parola** ve **Parolayı Onayla** metin kutularına kullanıcının parolasını girin.

    d. **Oturum açmada zorla Değiştir** onay kutusunu seçin.

    e. **Kaydet**’e tıklayın.

    f. Kullanıcılara roller atamak için **rol Düzenle** ' ye tıklayın ve gerekli rolü, kuruluşunuzun gereksinimlerine göre kullanıcıya atayın.

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/assign-role.png)


## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Mimecast kişisel portalı kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Mimecast kişisel portalında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Mimecast kişisel portalı 'nı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Mimecast kişisel portalını gelişmiş görünürlük ve denetimlerle koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
