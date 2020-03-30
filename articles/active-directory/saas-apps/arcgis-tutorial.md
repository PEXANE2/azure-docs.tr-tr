---
title: 'Öğretici: ArcGIS Online ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ArcGIS Online arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74cf0c1e055570df7702465df79dcdfbd8ea9e9f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74232086"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: ArcGIS Online ile Azure Active Directory entegrasyonu

Bu eğitimde, ArcGIS Online'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
ArcGIS Online'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* ArcGIS Online erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla ArcGIS Online'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ArcGIS Online ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* ArcGIS Online tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* ArcGIS **Online, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-arcgis-online-from-the-gallery"></a>Galeriden ArcGIS Online ekleme

ArcGIS Online'ın Azure AD'ye entegrasyonunu yapılandırmak için, arcgis online'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden ArcGIS Online eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **ArcGIS Online**yazın, sonuç panelinden **ArcGIS Online'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![ArcGIS Online sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini ArcGIS Online ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile ArcGIS Online'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumlarını ArcGIS Online ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[ArcGIS Online Tek Oturum Açma'yı uygulama](#configure-arcgis-online-single-sign-on)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[ArcGIS Online test kullanıcısını oluşturun](#create-arcgis-online-test-user)** - ArcGIS Online'da Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini ArcGIS Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **ArcGIS Online** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![ArcGIS Online Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.maps.arcgis.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<companyname>.maps.arcgis.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [ArcGIS Online Client destek ekibine](https://support.esri.com/en/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **ArcGIS Online**içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![image](./media/arcgis-tutorial/install_extension.png)

7. Tarayıcıya uzantı ekledikten sonra, **ArcGIS Online kurulumuna** tıklayın ve sizi ArcGIS Online uygulamasına yönlendirecektir. Buradan, ArcGIS Online'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandıracak ve **ArcGIS Online Tek İşaretle' yi yapılandırın**bölümündeki adımları otomatikleştirecektir.

### <a name="configure-arcgis-online-single-sign-on"></a>ArcGIS Online Tek Oturum Aç'ı Yapılandır

1. ArcGIS Online'ı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve ArcGIS şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

2. **AYARLARI ED'E**tıklayın.

    ![Ayarları Düzenle](./media/arcgis-tutorial/ic784742.png "Ayarları Düzenle")

3. **Güvenlik'i**tıklatın.

    ![Güvenlik](./media/arcgis-tutorial/ic784743.png "Güvenlik")

4. **Kurumsal Girişler**altında, **SET KİmLİk SAGLAYICI'nı**tıklatın.

    ![Kurumsal Girişler](./media/arcgis-tutorial/ic784744.png "Kurumsal Girişler")

5. Kimlik **Sağlayıcısını Ayarla** yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Kimlik Sağlayıcı Yı Ayarla](./media/arcgis-tutorial/ic784745.png "Kimlik Sağlayıcı Yı Ayarla")

    a. **Ad** metin kutusuna kuruluşunuzun adını yazın.

    b. Kurumsal Kimlik Sağlayıcısı için Meta veriler için , **Bir Dosya**seçin kullanılarak **sağlanacaktır.**

    c. İndirilen meta veri dosyanızı yüklemek için **dosyayı seç'i**tıklatın.

    d. **Kimlik SağlayıcıYı AYARLA'yı**tıklatın.

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

Bu bölümde, Britta Simon'ın ArcGIS Online'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **ArcGIS Online'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ArcGIS Online**yazın ve seçin.

    ![Uygulamalar listesindearcgis online bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-arcgis-online-test-user"></a>ArcGIS Online test kullanıcısı oluşturma

Azure AD kullanıcılarının ArcGIS Online'a giriş yapabilmeleri için ArcGIS Online'da oturum açmaları gerekir.  
ArcGIS Online durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **ArcGIS** kiracınıza giriş yapın.

2. **ÜYE DAVET ET'I**tıklatın.
   
    ![Üye Davet](./media/arcgis-tutorial/ic784747.png "Üye Davet")

3. **E-posta göndermeden üye ekle'yi**seçin ve ardından **NEXT'i**tıklatın.
   
    ![Otomatik Olarak Üye Ekle](./media/arcgis-tutorial/ic784748.png "Otomatik Olarak Üye Ekle")

4. **Üyeler** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
     ![Ekle ve gözden geçirin](./media/arcgis-tutorial/ic784749.png "Ekle ve gözden geçirin")
    
     a. Sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **E-postasını,** **Adını**ve **Soyadını** girin.
  
     b. **EKLE ve İnceLE'yi**tıklatın.
5. Girdiğiniz verileri gözden geçirin ve ardından **ÜYE EKLE'yi**tıklatın.
   
    ![Üye ekle](./media/arcgis-tutorial/ic784750.png "Üye ekle")
        
    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki ArcGIS Online döşemesini tıklattığınızda, SSO'yu kurduğunuz ArcGIS Online'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

