---
title: 'Öğretici: ScreenSteps ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Screen Steps arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 864a9243a9f737506fd4d8cbc3940d7a86711f20
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67091658"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Öğretici: ScreenSteps ile Azure Active Directory entegrasyonu

Bu eğitimde, ScreenSteps'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
ScreenSteps'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* ScreenSteps'e erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ScreenSteps 'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Screen Steps ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* ScreenSteps tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* ScreenSteps **SP** başlatılan SSO destekler

## <a name="adding-screensteps-from-the-gallery"></a>Galeriden ScreenSteps Ekleme

ScreenSteps'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize ScreenSteps eklemeniz gerekir.

**Galeriden ScreenSteps eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **ScreenSteps**yazın, sonuç panelinden **ScreenSteps'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesindeki ScreenSteps](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre ScreenSteps ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile ScreenSteps'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini ScreenSteps ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[ScreenSteps Tek Oturum Açma'yı yapılandırın.](#configure-screensteps-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[ScreenSteps test kullanıcısını oluşturun](#create-screensteps-test-user)** - Kullanıcının Azure AD gösterimine bağlı ScreenSteps'te Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Screen Steps ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **ScreenSteps** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![ScreenSteps Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.ScreenSteps.com`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri, daha sonra bu öğreticide açıklanan gerçek Oturum Açma URL'si ile güncelleştirin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **ScreenSteps'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-screensteps-single-sign-on"></a>ScreenSteps'i Yapılandır Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, ScreenSteps şirket sitenize yönetici olarak giriş yapın.

1. **Hesap Ayarları'nı**tıklatın.

    ![Hesap yönetimi](./media/screensteps-tutorial/ic778523.png "Hesap yönetimi")

1. **Tek Oturum Açma'yı**tıklatın.

    ![Uzaktan kimlik doğrulama](./media/screensteps-tutorial/ic778524.png "Uzaktan kimlik doğrulama")

1. **Tek Oturum Açma Bitiş Noktası Oluştur'u**tıklatın.

    ![Uzaktan kimlik doğrulama](./media/screensteps-tutorial/ic778525.png "Uzaktan kimlik doğrulama")

1. Tek **Oturum Açma Bitiş Noktası** Oluştur bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama bitiş noktası oluşturma](./media/screensteps-tutorial/ic778526.png "Kimlik doğrulama bitiş noktası oluşturma")

    a. **Başlık** metin kutusuna bir başlık yazın.

    b. **Mod** listesinden **SAML'yi**seçin.

    c. **Oluştur'u**tıklatın.

1. Yeni bitiş noktasını **edin.**

    ![Bitiş noktasını edin](./media/screensteps-tutorial/ic778528.png "Bitiş noktasını edin")

1. Tek Oturum Açma **Bitiş Noktasını Edit** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Uzak kimlik doğrulama bitiş noktası](./media/screensteps-tutorial/ic778527.png "Uzak kimlik doğrulama bitiş noktası")

    a. **Yeni SAML Sertifikası dosyasını yükle'yi**tıklatın ve ardından Azure portalından indirdiğiniz sertifikayı yükleyin.

    b. Azure portalından kopyaladığınız **Giriş URL** değerini Uzak **Giriş URL** metin kutusuna yapıştırın.

    c. Azure portalından kopyaladığınız **Logout URL** değerini URL **giriş** kutusuna yapıştırın.

    d. Kullanıcıları, ne zaman sağlanabileceklerine atamak için bir **Grup** seçin.

    e. **Güncelleştir**’e tıklayın.

    f. **SAML Tüketici URL'sini** panoya kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Oturum Açma URL** metin kutusuna yapıştırın.

    g. **Tek Oturum Açma Bitiş Noktasını Edin.**

    h. ScreenSteps'e giriş yapan tüm kullanıcılar için bu bitiş noktasını kullanmak **için hesap için varsayılan** ı tıklatın. Alternatif **olarak, ScreenSteps'teki**belirli siteler için bu bitiş noktasını kullanmak için **Siteye Ekle** düğmesini tıklatabilirsiniz.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın ScreenSteps'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından ScreenSteps'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ScreenSteps'i**seçin.

    ![Uygulamalar listesindeki ScreenSteps bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-screensteps-test-user"></a>ScreenSteps test kullanıcı oluşturma

Bu bölümde, ScreenSteps'te Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları ScreenSteps platformuna eklemek için [ScreenSteps İstemci destek ekibiyle](https://www.screensteps.com/contact) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki ScreenSteps döşemesini tıklattığınızda, SSO'yu kurduğunuz ScreenSteps'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)