---
title: 'Öğretici: Kudos ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Kudos arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 1fb1a1bc7bfd8b3cc9d7758bf8e80d8759f9357e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227475"
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a>Öğretici: Kudos ile Azure Active Directory entegrasyonu

Bu eğitimde, Kudos'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Kudos'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Kudos'a erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Kudos'ta (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Kudos ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Kudos tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Kudos **SP** başlatılan SSO destekler

## <a name="adding-kudos-from-the-gallery"></a>Galeriden Kudos ekleme

Kudos'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Kudos eklemeniz gerekir.

**Galeriden Kudos eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Kudos**yazın, sonuç panelinden **Kudos'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde kudos](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Kudos'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Kudos ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Tek Oturum Açma ayarlarını uygulama tarafında yapılandırmak için **[Kudos Tek Oturum Açma'yı yapılandırın.](#configure-kudos-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Kudos test kullanıcıoluşturun](#create-kudos-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Kudos Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Kudos ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Kudos** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kudos Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<company>.kudosnow.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Kudos İstemci destek ekibine](http://success.kudosnow.com/home) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Kudos'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-kudos-single-sign-on"></a>Kudos Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Bir yönetici olarak Kudos şirket sitenizde oturum açın.

1. Üstteki menüde Ayarlar **simgesini**tıklatın.

    ![Ayarlar](./media/kudos-tutorial/ic787806.png "Ayarlar")

1. **SSO> Tümleştirmeleri** tıklatın ve aşağıdaki adımları gerçekleştirin:

    ![SSO](./media/kudos-tutorial/ic787807.png "SSO")

    a. **URL textbox'ta Oturum** Aç'ta, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    b. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından X.509 sertifika** metin kutusuna yapıştırın

    c. **Giriş Url** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. **Kudos URL metin kutunuza** şirket adınızı yazın.

    e. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Kudos'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Kudos'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Kudos'u**seçin.

    ![Uygulamalar listesindeki Kudos bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-kudos-test-user"></a>Kudos test kullanıcısı oluşturma

Azure AD kullanıcılarının Kudos'ta oturum açabilmeleri için Kudos'a dahil edilmeleri gerekir. Kudos durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Kudos** şirket sitenizde yönetici olarak oturum açın.

1. Üstteki menüde Ayarlar **simgesini**tıklatın.

   ![Ayarlar](./media/kudos-tutorial/ic787806.png "Ayarlar")

1. **Kullanıcı Yöneticisi'ni**tıklatın.

1. **Kullanıcılar** sekmesini tıklatın ve ardından **Kullanıcı Ekle'yi**tıklatın.

   ![Kullanıcı Yöneticisi](./media/kudos-tutorial/ic787809.png "Kullanıcı Yöneticisi")

1. Kullanıcı **Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Ekleme](./media/kudos-tutorial/ic787810.png "Kullanıcı Ekleme")

    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure Active Directory hesabının **Adı,** **Soyadı,** **E-postave** diğer ayrıntılarını yazın.

    b. **Kullanıcı Oluştur'u**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Kudos tarafından sağlanan diğer Kudos kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Kudos karosu tıklattığınızda, SSO'yu kurduğunuz Kudos'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
