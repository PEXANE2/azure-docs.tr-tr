---
title: 'Öğretici: SensoScientific Kablosuz Sıcaklık İzleme Sistemi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SensoScientific Kablosuz Sıcaklık İzleme Sistemi arasında tek oturum açma yı nasıl yapılandırıştırabilirsiniz öğrenin.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67091263"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Öğretici: SensoScientific Kablosuz Sıcaklık İzleme Sistemi ile Azure Active Directory entegrasyonu

Bu eğitimde, SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni Azure AD ile entegre etmek size aşağıdaki avantajları sağlar:

* SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ne erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SensoScientific Kablosuz Sıcaklık İzleme Sistemi'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD entegrasyonunu SensoScientific Kablosuz Sıcaklık İzleme Sistemi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SensoScientific Kablosuz Sıcaklık İzleme Sistemi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SensoScientific Kablosuz Sıcaklık İzleme Sistemi **IDP** başlatılan SSO destekler

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Galeriden SensoScientific Kablosuz Sıcaklık İzleme Sistemi Ekleme

SensoScientific Kablosuz Sıcaklık İzleme Sistemi'nin Azure AD'ye entegrasyonunu yapılandırmak için, SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SensoScientific Kablosuz Sıcaklık İzleme Sistemi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **SensoScientific Kablosuz Sıcaklık İzleme Sistemi**yazın, sonuç panelinden **SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![SensoScientific Kablosuz Sıcaklık İzleme Sistemi sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini SensoScientific Wireless Temperature Monitoring System ile yapılandırıp test emziyorsunuz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

SensoScientific Kablosuz Sıcaklık İzleme Sistemi ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[SensoScientific Kablosuz Sıcaklık İzleme Sistemi Tek İşaret-On -](#configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on)** uygulama tarafında Tek Sign-On ayarlarını yapılandırmak için yapıl.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SensoScientific Kablosuz Sıcaklık İzleme Sistemi test kullanıcıoluşturun](#create-sensoscientific-wireless-temperature-monitoring-system-test-user)** - SensoScientific Kablosuz Sıcaklık İzleme Sistemi kullanıcının Azure AD gösterimi ile bağlantılı Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

SensoScientific Kablosuz Sıcaklık İzleme Sistemi ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **SensoScientific Kablosuz Sıcaklık İzleme Sistemi** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![SensoScientific Kablosuz Sıcaklık İzleme Sistemi Etki Alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on"></a>Yapılandırma SensoScientific Kablosuz Sıcaklık İzleme Sistemi Tek İşaret-On

1. SensoScientific Kablosuz Sıcaklık İzleme Sistemi uygulamanızda yönetici olarak oturum açın.

1. Üstteki gezinti menüsünde, Tek Oturum Açma Ayarlarını açmak ve aşağıdaki adımları gerçekleştirmek için **Tek Oturum Açma** altında **Yapılandırma** ve Yapılandırma'ya gidin: **Configure**

    ![Tek İşaret-On'u Yapılandır](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png)

    a. Azure AD olarak **Veren Adı'nı** seçin.

    b. Veren **URL** metin kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısını** yapıştırın.

    c. Tek **Oturum Açma Hizmeti URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    d. Tek **Oturum Açma Hizmeti URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    e. Azure portalından indirdiğiniz sertifikaya göz atın ve buraya yükleyin.

    f. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ne erişim sağlayarak Azure tek oturum açma özelliğini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **SensoScientific Kablosuz Sıcaklık İzleme Sistemi'ni**seçin.

    ![Uygulamalar listesindeki SensoScientific Kablosuz Sıcaklık İzleme Sistemi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>SensoScientific Kablosuz Sıcaklık İzleme Sistemi test kullanıcısı oluşturun

Azure AD kullanıcılarının SensoScientific Kablosuz Sıcaklık İzleme Sistemi'nde oturum açabilmeleri için, bunların SensoScientific Kablosuz Sıcaklık İzleme Sistemi'nde sağlanması gerekir.  [SensoScientific Kablosuz Sıcaklık İzleme Sistemi destek ekibi](https://www.sensoscientific.com/contact-us/) ile birlikte çalışarak Kullanıcıları SensoScientific Kablosuz Sıcaklık İzleme Sistemi platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki SensoScientific Kablosuz Sıcaklık İzleme Sistemi döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz SensoScientific Kablosuz Sıcaklık İzleme Sistemi'nde oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

