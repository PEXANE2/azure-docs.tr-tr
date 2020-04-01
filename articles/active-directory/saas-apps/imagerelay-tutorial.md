---
title: 'Öğretici: Görüntü Rölesi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Image Relay arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4fd0637a632b277eae019ac4aebfbc7cdb87e8e2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158979"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Öğretici: Görüntü Rölesi ile Azure Active Directory entegrasyonu

Bu eğitimde, Görüntü Rölesini Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Görüntü Rölesini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Görüntü Rölesi'ne erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak Görüntü Rölesi (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Görüntü Rölesi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Görüntü Rölesi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Image **Relay, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-image-relay-from-the-gallery"></a>Galeriden Görüntü Rölesi Ekleme

Image Relay'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Görüntü Rölesi'ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Resim Rölesi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Resim Röle**yazın, sonuç panelinden **Görüntü Rölesi'ni** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde görüntü rölesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Görüntü Rölesi ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile Görüntü Rölesi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Görüntü Rölesi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Görüntü Rölesi Tek Oturum Açma'yı yapılandırın.](#configure-image-relay-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Görüntü Rölesi testi kullanıcısı oluşturun](#create-image-relay-test-user)** - Kullanıcının Azure AD gösterimine bağlı Görüntü Rölesi'nde Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Görüntü Rölesi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Resim Rölesi** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Görüntü Röle Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.imagerelay.com/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Image Relay Client destek ekibine](http://support.imagerelay.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Resim **Rölesi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-image-relay-single-sign-on"></a>Görsel Röle Tek İşaret-On yapıla

1. Başka bir tarayıcı penceresinde, Yönetici olarak Image Relay şirket sitenizde oturum açın.

2. Üstteki araç çubuğunda, **İzinler & Kullanıcılar** iş yükünü tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

3. **Yeni İzin Oluştur'u**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

4. Tek **Oturum Açma Ayarları** iş yükünde, **Bu Grup'u** seçin yalnızca Tek Oturum Açma onay kutusu yla oturum açabilir ve sonra **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

5. Hesap **Ayarları'na**gidin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

6. **Tek İşaret Ayarları İş** yüküne gidin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

7. **SAML Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)

    a. **Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    b. **Logout URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    c. **Ad Kimliği Formatında**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.

    d. **Servis Sağlayıcıdan Gelen İstekler için Bağlama Seçenekleri (Resim Rölesi)** olarak **POST Binding'i**seçin.

    e. **x.509 Sertifikası**altında, **Sertifikayı Güncelleştir'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. İndirilen sertifikayı not defterinde açın, içeriği kopyalayın ve **ardından x.509 Sertifika** metin kutusuna yapıştırın.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. **Tam Zamanında Kullanıcı Sağlama** bölümünde, Tam Zamanında Kullanıcı **Sağlama**etkinleştir'i seçin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Yalnızca tek oturum açma yoluyla oturum açmasına izin verilen izin grubunu (örneğin, **SSO Basic)** seçin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Görüntü Rölesi'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Görüntü Rölesi'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Görüntü Rölesi'ni**seçin.

    ![Uygulamalar listesindeki Görüntü Rölesi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-image-relay-test-user"></a>Görüntü Rölesi testi kullanıcısı oluşturma

Bu bölümün amacı Görüntü Röle Britta Simon adlı bir kullanıcı oluşturmaktır.

**Görüntü Rölesi'nde Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Image Relay şirket sitenize oturum açın.

2. **İzinler & Kullanıcılara** gidin ve **SSO Kullanıcısı Oluştur'u**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Yalnızca tek oturum açma yoluyla oturum açabilen grup olan izin grubunu (örneğin, SSO Basic) sağlamak istediğiniz kullanıcının **E-postasını,** **Adını,** **Soyadını**ve **Şirketini** girin.

    ![Tek İşaret-On'u Yapılandır](./media/imagerelay-tutorial/tutorial_imagerelay_22.png)

4. **Oluştur'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Görüntü Röle döşemesini tıklattığınızda, SSO'yu kurduğunuz Resim Rölesi'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)