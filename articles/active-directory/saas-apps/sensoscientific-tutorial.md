---
title: 'Öğretici: Sensobilimsel kablosuz sıcaklık Izleme sistemiyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Sensobilimsel kablosuz sıcaklık Izleme sistemi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea2f5e33859852388357526052c39fa432471efb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67091263"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Öğretici: Sensobilimsel kablosuz sıcaklık Izleme sistemiyle Azure Active Directory tümleştirme

Bu öğreticide, Sensobilimsel kablosuz sıcaklık Izleme sistemini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Sensobilimsel kablosuz sıcaklık Izleme sistemini Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Sensobilimsel kablosuz sıcaklık Izleme sistemine erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Sensobilimsel kablosuz sıcaklık Izleme sistemine (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sensobilimsel kablosuz sıcaklık Izleme sistemiyle yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Sensobilimsel kablosuz sıcaklık Izleme sistemi çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Sensobilimsel kablosuz sıcaklık Izleme sistemi **IDP** tarafından başlatılan SSO 'yu destekliyor

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Galeriden Sensobilimsel kablosuz sıcaklık Izleme sistemi ekleme

Sensobilimsel kablosuz sıcaklık Izleme sisteminin tümleştirmesini Azure AD ile yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Sensobilimsel kablosuz sıcaklık Izleme sistemi eklemeniz gerekir.

**Galeriden Sensobilimsel kablosuz sıcaklık Izleme sistemi eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Sensobilimsel kablosuz sıcaklık Izleme sistemi**yazın, sonuç panelinden **Sensobilimsel kablosuz sıcaklık izleme sistemi** ' ni seçin ve ardından uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

    ![Sonuç listesinde Sensobilimsel kablosuz sıcaklık Izleme sistemi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayanarak Sensobilimsel kablosuz sıcaklık Izleme SISTEMIYLE Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Sensobilimsel kablosuz sıcaklık Izleme sistemindeki ilgili Kullanıcı arasındaki bağlantı ilişkisi kurulmalıdır.

Azure AD çoklu oturum açmayı, Sensobilimsel kablosuz sıcaklık Izleme sistemiyle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Sensobilimsel kablosuz sıcaklık Izleme sistemi çoklu oturum açmayı yapılandırın](#configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Sensobilimsel kablosuz sıcaklık Izleme sisteminde Britta Simon 'ın bir karşılığı olacak şekilde **[Sensobilimsel kablosuz sıcaklık Izleme sistemi test kullanıcısı oluşturun](#create-sensoscientific-wireless-temperature-monitoring-system-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Sensobilimsel kablosuz sıcaklık Izleme sistemiyle yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Sensobilimsel kablosuz sıcaklık izleme sistemi** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

    ![Sensobilimsel kablosuz sıcaklık Izleme sistem etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/preintegrated.png)

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Sensobilimsel kablosuz sıcaklık Izleme sistemini ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on"></a>Sensobilimsel kablosuz sıcaklık Izleme sistemi çoklu oturum açmayı yapılandırma

1. Sensobilimsel kablosuz sıcaklık Izleme sistemi uygulamanızda yönetici olarak oturum açın.

1. Üstteki gezinti menüsünde, **yapılandırma** ' ya tıklayın ve çoklu **oturum açma** altında **Yapılandır** ' a tıklayarak çoklu oturum açma ayarlarını açın ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png)

    a. **Verenin adını** Azure ad olarak seçin.

    b. **Veren URL** metin kutusuna, Azure Portal KOPYALADıĞıNıZ **Azure ad tanımlayıcısını** yapıştırın.

    c. **Çoklu oturum açma hizmeti URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL** 'sini yapıştırın.

    d. **Çoklu oturum kapatma hizmeti URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL** 'sini yapıştırın.

    e. Azure portal indirdiğiniz sertifikaya gözatıp buradan karşıya yükleyin.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Sensobilimsel kablosuz sıcaklık Izleme sistemine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Sensobilimsel kablosuz sıcaklık izleme sistemi**' ni seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Sensobilimsel kablosuz sıcaklık Izleme sistemi**' ni seçin.

    ![Uygulamalar listesinde Sensobilimsel kablosuz sıcaklık Izleme sistemi bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Sensobilimsel kablosuz sıcaklık Izleme sistem testi kullanıcısı oluşturma

Azure AD kullanıcılarının Sensobilimsel kablosuz sıcaklık Izleme sisteminde oturum açmasını sağlamak için, Sensobilimsel kablosuz sıcaklık Izleme sistemine sağlanması gerekir. Duysobilimsel kablosuz sıcaklık izleme sistemi platformunda kullanıcıları eklemek için [sensobilimsel kablosuz sıcaklık izleme sistemi destek ekibi](https://www.sensoscientific.com/contact-us/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Sensobilimsel kablosuz sıcaklık Izleme sistemi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Sensobilimsel kablosuz sıcaklık Izleme sisteminde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

