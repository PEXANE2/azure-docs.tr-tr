---
title: 'Öğretici: Katılım Yönetimi Hizmetleri ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Katılım Yönetimi Hizmetleri arasında tek oturum açma yı nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7143d0afce7a3644286703a9eba0da1ee45305f2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67106552"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Öğretici: Katılım Yönetimi Hizmetleri ile Azure Active Directory entegrasyonu

Bu eğitimde, Katılım Yönetimi Hizmetlerini Azure Etkin Dizin (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Katılım Yönetimi Hizmetlerini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Katılım Yönetimi Hizmetlerine erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Katılım Yönetimi Hizmetleri'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Katılım Yönetimi Hizmetleri ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Katılım Yönetim Hizmetleri tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Katılım Yönetim Hizmetleri **SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-attendance-management-services-from-the-gallery"></a>Galeriden Katılım Yönetimi Hizmetleri Ekleme

Katılım Yönetimi Hizmetlerinin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Katılım Yönetimi Hizmetleri eklemeniz gerekir.

**Galeriden Katılım Yönetimi Hizmetleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Katılım Yönetimi Hizmetleri**yazın, sonuç panelinden Katılım Yönetimi **Hizmetleri'ni** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Katılım Yönetim Hizmetleri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırır ve test esunarsınız.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Katılım Yönetimi Hizmetleri'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Katılım Yönetimi Hizmetleri ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Katılım Yönetimi Hizmetlerini Tek Oturum Açma](#configure-attendance-management-services-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Katılım Yönetimi Hizmetleri test kullanıcısını oluşturun](#create-attendance-management-services-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Katılım Yönetimi Hizmetleri'nde Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Katılım Yönetimi Hizmetleri ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Katılım Yönetimi Hizmetleri** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Katılım Yönetimi Hizmetleri Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://id.obc.jp/<tenant information >/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Katılım Yönetimi Hizmetleri Müşteri destek ekibine](https://www.obcnet.jp/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Katılım **Yönetimi Hizmetleri Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-attendance-management-services-single-sign-on"></a>Katılım Yönetimi Hizmetlerini Yapılandır

1. Farklı bir tarayıcı penceresinde, Katılım Yönetimi Hizmetleri şirket sitenize yönetici olarak oturum açın.

1. **Güvenlik yönetimi bölümü**altında **SAML kimlik doğrulaması** tıklayın.

    ![Katılım Yönetimi Hizmetleri Yapılandırması](./media/attendancemanagementservices-tutorial/user1.png)

1. Aşağıdaki adımları uygulayın:

    ![Katılım Yönetimi Hizmetleri Yapılandırması](./media/attendancemanagementservices-tutorial/user2.png)

    a. **SAML kimlik doğrulamasını kullan'ı**seçin.

    b. **Tanımlayıcı** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerinin değerini yapıştırın.

    c. Kimlik **Doğrulama bitiş noktası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerinin değerini yapıştırın.

    d. Azure AD'den indirdiğiniz sertifikayı yüklemek için dosya yı **seçin'i** tıklatın.

    e. **Parola kimlik doğrulamasını devre dışı bırakma'yı**seçin.

    f. **Kayıt'ı** tıklatın

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

Bu bölümde, Katılım Yönetimi Hizmetlerine erişim sağlayarak Britta Simon'ın Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından Katılım **Yönetimi Hizmetleri'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **Katılım Yönetim Hizmetleri'ni**seçin.

    ![Başvuru listesinde katılım yönetimi hizmetleri bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-attendance-management-services-test-user"></a>Katılım Yönetimi Hizmetleri test kullanıcısı oluşturma

Azure AD kullanıcılarının Katılım Yönetimi Hizmetlerinde oturum açmalarını sağlamak için Katılım Yönetimi Hizmetleri'nde bunların sağlanması gerekir. Katılım Yönetimi Hizmetleri söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Katılım Yönetim Hizmetleri şirket sitenizde yönetici olarak oturum açın.

1. **Güvenlik yönetimi bölümünde**Kullanıcı **yönetimi'ne** tıklayın.

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user5.png)

1. **Yeni kurallar giriş'i**tıklatın.

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user3.png)

1. **OBCiD bilgi** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user4.png)

    a. **OBCiD textbox'ta,** kullanıcının e-postasını şöyle `BrittaSimon\@contoso.com`yazın.

    b. **Parola** metin kutusuna, kullanıcının parolasını yazın.

    c. **Kayıt'ı** tıklatın

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Katılım Yönetimi Hizmetleri döşemesini tıklattığınızda, SSO'yu kurduğunuz Katılım Yönetimi Hizmetleri'nde otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)