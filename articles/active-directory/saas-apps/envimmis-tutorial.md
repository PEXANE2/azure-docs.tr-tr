---
title: 'Öğretici: Envi MMIS ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Envi MMIS arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1689517042713e9a3ce75c6ada822cb3d6ff138
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158261"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Öğretici: Envi MMIS ile Azure Active Directory entegrasyonu

Bu eğitimde, Envi MMIS'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Envi MMIS'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Envi MMIS erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Envi MMIS (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Envi MMIS ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Envi MMIS tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Envi **MMIS, SP** ve **IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-envi-mmis-from-the-gallery"></a>Galeriden Envi MMIS ekleme

Envi MMIS'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Envi MMIS'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Envi MMIS eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Envi MMIS**yazın, sonuç panelinden **Envi MMIS'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Envi MMIS](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Envi MMIS ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Envi MMIS'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Envi MMIS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Envi MMIS Tek Oturum Açma](#configure-envi-mmis-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Envi MMIS test kullanıcısını oluşturun](#create-envi-mmis-test-user)** - Envi MMIS'te Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumlarını Envi MMIS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Envi MMIS** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Kurulum Tek Oturum** Açma'da **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Envi MMIS Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Envi MMIS Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.<CUSTOMER DOMAIN>.com/Account`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Envi MMIS İstemci destek ekibine](mailto:support@ioscorp.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML** ile Kurulum Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'i** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Envi MMIS'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-envi-mmis-single-sign-on"></a>Envi MMIS Tek İşaretli Yapıla

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Envi MMIS sitenizde oturum açın.

2. Etki **Alanım** sekmesine tıklayın.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure1.png)

3. **Düzenle**’ye tıklayın.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure2.png)

4. **Uzaktan kimlik doğrulama** onay kutusunu kullan'ı seçin ve ardından Kimlik **Doğrulama Türü** açılır sayfasından HTTP **Yeniden Yönlendirme'yi** seçin.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure3.png)

5. **Kaynaklar** sekmesini seçin ve ardından **Meta Verilerini Yükle'yi**tıklatın.

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure4.png)

6. Meta **verilerini yükle** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure5.png)

    a. Açılan dan **Yükle'den** **Dosya** seçeneğini seçin.

    b. İndirilen meta veri dosyasını **Select dosya simgesini**seçerek Azure portalından yükleyin.

    c. **Tamam**'a tıklayın.

7. İndirilen meta veri dosyasını yükledikten sonra, alanlar otomatik olarak doldurulur. **Güncelle'yi** tıklatın

    ![Tek Oturum Açma Kaydet düğmesini yapılandır](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanında **brittasimon\@yourcompanydomain.extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Envi MMIS'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Envi MMIS'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Envi MMIS'i**seçin.

    ![Uygulamalar listesindeki Envi MMIS bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında daha sonra **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-envi-mmis-test-user"></a>Envi MMIS test kullanıcısını oluşturun

Azure AD kullanıcılarının Envi MMIS'te oturum açabilmeleri için, bunların Envi MMIS'de sağlanması gerekir. Envi MMIS durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Envi MMIS şirket sitenizde oturum açın.

2. Kullanıcı **Listesi** sekmesine tıklayın.

    ![Çalışan Ekle](./media/envimmis-tutorial/user1.png)

3. **Kullanıcı Ekle** düğmesini tıklatın.

    ![Çalışan Ekle](./media/envimmis-tutorial/user2.png)

4. Kullanıcı **Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/envimmis-tutorial/user3.png)

    a. Kullanıcı **Adı** metin kutusuna Brittasimon **\@contoso.com**gibi Britta Simon hesabının kullanıcı adını yazın.
    
    b. **Ad** metin kutusuna **BrittaGibi**BrittaSimon'Un ilk adını yazın.

    c. **Soyadı** metin kutusuna, **Simon**gibi BrittaSimon soyadını yazın.

    d. Metin kutusunun **başlığına** kullanıcının başlığını girin.
    
    e. **E-posta Adresi** metin kutusuna **Brittasimon\@contoso.com**gibi Britta Simon hesabının e-posta adresini yazın.

    f. **SSO Kullanıcı Adı** metin kutusuna Brittasimon **\@contoso.com**gibi Britta Simon hesabının kullanıcı adını yazın.

    g. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Envi MMIS karosu tıklattığınızda, SSO'yu kurduğunuz Envi MMIS'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

