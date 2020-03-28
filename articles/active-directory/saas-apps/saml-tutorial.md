---
title: 'Öğretici: SAML 1.1 Belirteci ile Azure Active Directory entegrasyonu LOB Uygulaması etkin | Microsoft Dokümanlar'
description: Azure Active Directory ve SAML 1.1 Belirteç etkin LOB Uygulaması arasında tek oturum açma yı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6de8906f15b170511c5548eeaf13173bd39a2734
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160067"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Öğretici: SAML 1.1 Belirteci ile Azure Active Directory entegrasyonu LOB Uygulaması etkin

Bu eğitimde, SAML 1.1 Belirteken etkin lob uygulamasını Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SAML 1.1 Belirteç etkin LOB Uygulamasını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SAML 1.1 Belirteç özellikli LOB Uygulamasına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAML 1.1 Belirteç özellikli LOB Uygulaması (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SAML 1.1 Belirteci etkin LOB Uygulaması ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* SAML 1.1 Belirteç etkin LOB App tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SAML 1.1 Token etkin LOB Uygulaması **SP** başlatılan SSO destekler

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Galeriden SAML 1.1 Belirteç etkin LOB Uygulaması ekleme

SAML 1.1 Token özellikli LOB Uygulamasının Azure AD'ye entegrasyonunu yapılandırmak için, galeriden SAML 1.1 Token özellikli LOB Uygulamasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SAML 1.1 Token etkin LOB Uygulaması eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **SAML 1.1 Token etkin LOB Uygulaması**yazın, sonuç panelinden **SAML 1.1 Token etkin LOB Uygulamasını** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![SAML 1.1 Belirteci, sonuç listesinde LOB Uygulamasını etkinleştirildi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini SAML 1.1 Token etkin leştirilmiş LOB Uygulaması ile **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test emzebilirsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile SAML 1.1 Belirteken'deki ilgili kullanıcı arasında lob uygulamasının kurulması gerekir.

Azure AD oturum açma özelliğini SAML 1.1 Token özellikli LOB Uygulamasıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Yapılandırılan SAML 1.1 Belirteken,](#configure-saml-11-token-enabled-lob-app-single-sign-on)** uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için LOB Uygulaması Tek Oturum Açma özelliğini etkinleştirin.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SAML 1.1 Token etkin LOB App test kullanıcıoluşturun](#create-saml-11-token-enabled-lob-app-test-user)** - SAML 1.1 Token etkin LOB Uygulaması kullanıcının Azure AD gösterimine bağlı Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma özelliğini SAML 1.1 Token özellikli LOB Uygulaması ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **SAML 1.1 Belirteci etkin LOB Uygulaması** uygulama tümleştirme sayfasındaki [Azure portalında,](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML 1.1 Belirteci, LOB App Domain ve URL'lerin tek oturum açma bilgilerini etkinleştirildi](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://your-app-url`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://your-app-url`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için SAML 1.1 Belirteci etkin LOB App Client destek ekibine başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **SAML 1.1 Token etkin LOB Uygulaması** bölümünde, gereksiniminize göre uygun URL(ler) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-saml-11-token-enabled-lob-app-single-sign-on"></a>Yapılandırılan SAML 1.1 Belirteci etkin LOB Uygulaması Tek Oturum Açma

**SAML 1.1 Token özellikli LOB Uygulaması** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından SAML 1.1 Token etkin LOB App destek ekibine göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SamL 1.1 Belirteç özellikli LOB Uygulamasına erişim sağlayarak Britta Simon'ın Azure tek oturum açma özelliğini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **SAML 1.1 Token özellikli LOB Uygulamasını**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **SAML 1.1 Belirteci etkin LOB Uygulaması**yazın ve seçin.

    ![SamL 1.1 Belirteci, Uygulamalar listesinde LOB App bağlantısını etkinleştirildi](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-saml-11-token-enabled-lob-app-test-user"></a>SAML 1.1 Belirteç etkin LOB App test kullanıcı oluşturma

Bu bölümde, SAML 1.1 Token etkin LOB Uygulamasında Britta Simon adında bir kullanıcı oluşturursunuz. SAML 1.1 Token ile çalışmak, LOB App destek ekibinin kullanıcıları SAML 1.1 Token özellikli LOB App platformuna eklemesine olanak sağladı. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki SAML 1.1 Token etkin LOB Uygulaması döşemesini tıklattığınızda, SSO'yu kurduğunuz SAML 1.1 Token etkin LOB Uygulaması'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

