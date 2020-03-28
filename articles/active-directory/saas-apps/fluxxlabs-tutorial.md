---
title: 'Öğretici: Fluxx Labs ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Fluxx Labs arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e624520a9d1f39bc8115ac72e9df0398065928f1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67102385"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Öğretici: Fluxx Labs ile Azure Active Directory entegrasyonu

Bu eğitimde, Fluxx Labs'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Fluxx Labs'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Fluxx Labs erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Fluxx Labs'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Fluxx Labs ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Fluxx Labs tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Fluxx Labs **IDP** başlatılan SSO destekler

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Fluxx Labs ekleme

Fluxx Labs'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Fluxx Labs'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Fluxx Labs eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Fluxx Labs**yazın, sonuç panelinden **Fluxx Labs'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Fluxx Labs sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Fluxx Labs ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Fluxx Labs'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Fluxx Labs ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Fluxx Labs Tek Oturum Açma 'yı **[yapılandırın](#configure-fluxx-labs-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Fluxx Labs test kullanıcıoluşturun](#create-fluxx-labs-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Fluxx Labs Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Fluxx Labs ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Fluxx Labs** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | Ortam | URL Deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Ön üretim | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | Ortam | URL Deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Ön üretim | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Fluxx Labs Müşteri destek ekibine](mailto:travis@fluxxlabs.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Fluxx Labs'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-fluxx-labs-single-sign-on"></a>Fluxx Labs Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Fluxx Labs şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar** bölümünün altındaki **Yönetici'yi** seçin.

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config1.png)

3. Yönetici Paneli'nde **Eklenti Tümleştirmeleri'ni** > **Integrations** seçin ve ardından **SAML SSO-(Devre Dışı) seçeneğini belirleyin**

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config2.png)

4. Öznitelik bölümünde aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config3.png)

    a. **SAML SSO** onay kutusunu seçin.

    b. İstek **Yolu** metin kutusunda **/auth/saml**yazın.

    c. Geri **Arama Yolu** metin kutusunda **/auth/saml/callback**yazın.

    d. İddia **Tüketici Hizmeti Url'si (Tek Oturum Açma URL'si)** textbox'ına, Azure portalına girdiğiniz **YanıtURL** değerini girin.

    e. Hedef **Kitle (SP Entity ID)** textbox'ına Azure portalına girdiğiniz **Tanımlayıcı** değerini girin.

    f. Kimlik **Sağlayıcısı SSO Target URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    g. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Kimlik Sağlayıcı Sertifikası** metin kutusuna yapıştırın.

    h. **Ad tanımlayıcı biçimli** metin kutusuna, değeri `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`girin.

    i. **Kaydet**'e tıklayın.

    > [!NOTE]
    > İçerik kaydedildikten sonra, alan güvenlik için boş görünür, ancak değer yapılandırmada kaydedilir.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Fluxx Labs'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Fluxx Labs'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Fluxx Labs'ı**seçin.

    ![Uygulamalar listesinde Fluxx Labs bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-fluxx-labs-test-user"></a>Fluxx Labs test kullanıcıoluşturun

Azure AD kullanıcılarının Fluxx Labs'da oturum açabilmeleri için Fluxx Labs'da oturum açmaları gerekir. Fluxx Labs durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Fluxx Labs şirket sitenizde yönetici olarak oturum açın.

2. Aşağıdaki görüntülenen **simgeye**tıklayın.

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config6.png)

3. Panoda, **Yeni KİşİLER** kartını açmak için aşağıdaki görüntülenen simgeye tıklayın.

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config4.png)

4. Yenİ **KİşİLER** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs Yapılandırma](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs, SSO girişleri için benzersiz tanımlayıcı olarak e-postayı kullanır. **SSO UID** alanını, SSO ile oturum açma olarak kullandıkları e-posta adresiyle eşleşen kullanıcının e-posta adresiyle doldurun.

    b. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Fluxx Labs döşemesini tıklattığınızda, SSO'yu kurduğunuz Fluxx Labs'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

