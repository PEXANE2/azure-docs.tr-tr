---
title: 'Öğretici: Sonsuz Kampüs ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Sonsuz Kampüs arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: d54769c1f3265e2cee619520044313fca46855a5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67100364"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Öğretici: Sonsuz Kampüs ile Azure Active Directory entegrasyonu

Bu eğitimde, Sonsuz Kampüs'ü Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Sonsuz Kampüs'e Azure AD ile entegre olmak size aşağıdaki avantajları sağlar:

* Sonsuz Kampüs'e erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Sonsuz Kampüs'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sonsuz Kampüs ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Sonsuz Kampüs tek oturum açma özellikli abonelik
* En azından, yapılandırmayı tamamlamak için Azure Etkin Dizin yöneticisi olmanız ve "Öğrenci Bilgi Sistemi (SIS)" Kampüs Ürün Güvenliği Rolüne sahip olmanız gerekir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Sonsuz Kampüs **SP** başlatılan SSO destekler

## <a name="adding-infinite-campus-from-the-gallery"></a>Galeriden Sonsuz Kampüs Ekleme

Sonsuz Kampüs'ün Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Sonsuz Kampüs eklemeniz gerekir.

**Galeriden Sonsuz Kampüs eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Sonsuz Kampüs**yazın, sonuç panelinden **Sonsuz Kampüs'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Sonsuz Kampüs](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Infinite Campus ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Sonsuz Kampüs'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Infinite Campus ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Sonsuz Kampüs Tek Oturum Açma](#configure-infinite-campus-single-sign-on)** 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Sonsuz Kampüs test kullanıcıoluşturun](#create-infinite-campus-test-user)** - Kullanıcının Azure AD gösterimine bağlı Sonsuz Kampüs'te Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Sonsuz Kampüs ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Sonsuz Kampüs** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel SAML Yapılandırma sıbölümünde aşağıdaki adımları gerçekleştirin (etki alanının Barındırma Modeline göre değişeceğini unutmayın, ancak **TAM KALİTELİ-DOMAIN** değerinin Sonsuz Kampüs yüklemenize uyması gerekir):

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![Sonsuz Kampüs Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-infinite-campus-single-sign-on"></a>Yapılandırılan Sonsuz Kampüs Tek İşaret-On

1. Farklı bir web tarayıcısı penceresinde, Güvenlik Yöneticisi olarak Sonsuz Kampüs'te oturum açın.

2. Menünün sol tarafında **Sistem Yönetimi'ni**tıklatın.

    ![Yönetici](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

3. Kullanıcı **Güvenliği** > **SAML Yönetimi** > **SSO Servis Sağlayıcı Yapılandırmasına**gidin.

    ![Saml](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

4. **SSO Hizmet Sağlayıcı Yapılandırma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Sso](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. **SAML Tek İşaretini Etkinleştir'i**seçin.

    b. Adı içerecek **şekilde İsteğe Bağlı Öznitelik Adını** **edin**

    c. Kimlik Sağlayıcısı (IDP) sunucu veri seçimi **alma seçeneğini seçeneÄ** inde, **Metaveri URL'** sini seçin , kutudaki Azure portalından kopyaladığınız **App Federation Metadata Url** değini süründen sürerek **Senkronize**edin.

    d. **Eşitle'yi** tıklattıktan sonra değerler **SSO Servis Sağlayıcı Yapılandırma** sı sayfasında otomatik olarak doldurulur. Bu değerler, yukarıda Adım 4'te görülen değerlerle eşleşecek şekilde doğrulanabilir.

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
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com.

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

> [!NOTE]
> Tüm Azure kullanıcılarınızın Sonsuz Kampüs'e tek oturum açma erişimiolmasını ve erişimi denetlemek için Sonsuz Kampüs dahili izin sistemine güvenerek olmasını istiyorsanız, uygulamanın **Kullanıcı Ataması Gerekli** özelliğini Hayır olarak ayarlayabilir ve aşağıdaki adımları atlayabilirsiniz.

Bu bölümde, Britta Simon'ın Sonsuz Kampüs'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Sonsuz Kampüs'ü**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Sonsuz Kampüs'ün**

    ![Uygulamalar listesindeki Sonsuz Kampüs bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-infinite-campus-test-user"></a>Sonsuz Kampüs test kullanıcısı oluşturma

Infinite Campus demografik merkezli bir mimariye sahiptir. Kullanıcıları Sonsuz Kampüs platformuna eklemek için lütfen [Infinite Campus destek ekibiyle](mailto:sales@infinitecampus.com) iletişime geçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Sonsuz Kampüs döşemesini tıklattığınızda, SSO'yu kurduğunuz Sonsuz Kampüs'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
