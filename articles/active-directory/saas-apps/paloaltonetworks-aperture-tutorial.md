---
title: 'Öğretici: Palo Alto Networks ile Azure Active Directory entegrasyonu - Diyafram | Microsoft Dokümanlar'
description: Azure Active Directory ve Palo Alto Networks - Diyafram arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: fd498dc1c37ed6e9518fcefbdb237153504b5e98
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67095052"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Öğretici: Palo Alto Networks ile Azure Active Directory entegrasyonu - Diyafram

Bu eğitimde, Palo Alto Ağları - Diyafram ile Azure Active Directory (Azure AD) entegre etmeyi öğrenirsiniz.
Palo Alto Ağlarını Tümleştirmek - Azure AD ile diyafram açıklığı size aşağıdaki avantajları sağlar:

* Palo Alto Networks - Diyafram açıklığına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Palo Alto Networks - Diyafram açıklığı (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Palo Alto Networks - Diyafram ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Palo Alto Networks - Diyafram tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Palo Alto Networks - Diyafram **SP** ve **IDP** SSO başlatılan destekler

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>Palo Alto Networks ekleme - Galeriden Diyafram

Palo Alto Networks -Diyafram ı Azure AD'ye dönüştürmek için galeriden Palo Alto Networks - Diyafram açıklığını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Palo Alto Networks - Galeriden Diyafram eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Palo Alto Networks - Diyafram,** **Palo Alto Ağları seçin -** Sonuç panelinden Diyafram, uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Palo Alto Networks - Sonuç listesinde diyafram](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı Olarak Palo Alto Networks - Diyafram açıklığı ile Azure AD tek oturum açma işlemini yapılandırın ve test esinizsiniz.
Tek oturum açmanın işe yaraması için, Palo Alto Networks - Diyafram açıklığı alanında bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturum açma işlemlerini Palo Alto Networks - Diyafram ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Palo Alto Ağlarını yapılandırır - Diyafram Tek İşaret-On](#configure-palo-alto-networks---aperture-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Palo Alto Networks'te](#create-palo-alto-networks---aperture-test-user)** Britta Simon'ın bir muadili olan Palo Alto Ağları - Diyafram açıklığı oluşturma- Kullanıcının Azure AD gösterimine bağlı diyafram açıklığı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Palo Alto Networks - Diyafram ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Palo Alto Networks - Diyafram** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto Networks - Diyafram Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Palo Alto Networks - Diyafram Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Palo Alto Networks - Diyafram İstemci destek ekibiyle](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Palo Alto Networks - Diyafram açıklığı** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-palo-alto-networks---aperture-single-sign-on"></a>Palo Alto Ağlarını Yapılandır - Diyafram Tek İşaret-On

1. Farklı bir web tarayıcısı penceresinde, Palo Alto Networks - Aperture as a Administrator'a giriş yapın.

2. Üst menü çubuğunda **AYARLAR'ı**tıklatın.

    ![Ayarlar sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

3. **UYGULAMA** bölümüne gidin, menüden kimlik **doğrulama** formuna tıklayın.

    ![Auth sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
4. Kimlik **Doğrulama** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Kimlik doğrulama sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Tek **Oturum Açma'yı Etkinleştir'i (Desteklenen SSP Sağlayıcıları Okta, Tek oturum açma)** **Tek Oturum Açma** alanından kontrol edin.

    b. Kimlik **Sağlayıcı Kimliği** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    c. Kimlik **Sağlayıcı Sertifikası** alanına Azure AD'den indirilen Sertifikayı yüklemek için **Dosyayı Seç'i** tıklatın.

    d. Kimlik **Sağlayıcısı SSO URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    e. Diyafram Bilgileri bölümünden IdP bilgilerini gözden **geçirin** ve sertifikayı Diyafram Anahtarı alanından **indirin.**

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
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Palo Alto Networks - Diyafram açıklığına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Palo Alto Networks - Diyafram**açıklığını seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Palo Alto Networks - Diyafram açıklığını**seçin.

    ![Palo Alto Networks - Uygulamalar listesindediyafram bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-palo-alto-networks---aperture-test-user"></a>Palo Alto Networks oluşturma - Diyafram testi kullanıcısı

Bu bölümde, Palo Alto Networks - Diyafram Britta Simon adlı bir kullanıcı oluşturun. [Palo Alto Networks - Diyafram Destek ekibi](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) ile çalışarak kullanıcıları Palo Alto Networks - Diyafram platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki Palo Alto Networks - Diyafram karosu'nu tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Palo Alto Networks - Diyafram açıklığına oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

