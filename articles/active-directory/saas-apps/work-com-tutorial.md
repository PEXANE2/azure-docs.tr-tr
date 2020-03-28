---
title: 'Öğretici: Work.com ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Work.com arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7a6dc16eef1bb36a5bd6cbf0502a83481230bc0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67087089"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Öğretici: Work.com ile Azure Active Directory tümleştirmesi

Bu eğitimde, azure active directory (Azure AD) ile Work.com nasıl entegre acağınızı öğrenirsiniz.
Azure AD ile Work.com tümleştirmek size aşağıdaki avantajları sağlar:

* Work.com erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla otomatik olarak oturum açma ve Work.com (Tek Oturum Açma) olmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Work.com yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Work.com oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Work.com **SP** başlatılan SSO destekler

## <a name="adding-workcom-from-the-gallery"></a>Galeriden Work.com ekleme

Work.com Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Work.com eklemeniz gerekir.

**Galeriden Work.com eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Work.com**yazın, sonuç panelinden **Work.com** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![sonuç listesinde Work.com](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Work.com yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Work.com'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Work.com yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Work.com Tek Oturum Açma'yı **[yapılandırın.](#configure-workcom-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Work.com **[test kullanıcısı oluşturun](#create-workcom-test-user)** - Work.com'da Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir örneğine sahip olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

>[!NOTE]
>Tek oturum açma yapılandırmak için henüz özel bir Work.com etki alanı adı ayarlamanız gerekir. En az bir etki alanı adı tanımlamanız, alan adınızı sınamanız ve tüm kuruluşunuza dağıtmanız gerekir.

Azure AD oturum açma işlemlerini Work.com yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Work.com** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Work.com Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`http://<companyname>.my.salesforce.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Work.com İstemci destek ekibine](https://help.salesforce.com/articleView?id=000159855&type=3) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Work.com **ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-workcom-single-sign-on"></a>Tek Oturum Açma Work.com yapılandır

1. Yönetici olarak Work.com kiracınızda oturum açın.

2. **Kurulum'a**gidin.
   
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulum")

3. Soldaki gezinti bölmesinde, **Yönet** bölümünde, ilgili bölümü genişletmek için **Etki Alanı Yönetimi'ni** tıklatın ve ardından Etki Alanım sayfasını açmak için **Etki Alanımı** tıklatın. **My Domain** 
   
    ![Etki Alanım](./media/work-com-tutorial/ic767825.png "Etki Alanım")

4. Etki alanınızın doğru şekilde ayarlandığını doğrulamak için, "**Kullanıcılara Dağıtılan Adım 4**" de olduğundan emin olun ve "**Etki Alanı Ayarlarım**" i gözden geçirin.
   
    ![Kullanıcıya Dağıtılan Etki Alanı](./media/work-com-tutorial/ic784377.png "Kullanıcıya Dağıtılan Etki Alanı")

5. Work.com kiracınızla oturum açın.

6. **Kurulum'a**gidin.
    
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulum")

7. Güvenlik **Denetimleri** menüsünü genişletin ve ardından **Tek Oturum Açma Ayarları'nı**tıklatın.
    
    ![Tek Oturum Açma Ayarları](./media/work-com-tutorial/ic794113.png "Tek Oturum Açma Ayarları")

8. Tek **Oturum Açma Ayarları** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![SAML Etkin](./media/work-com-tutorial/ic781026.png "SAML Etkin")
    
    a. **SAML Etkin'i**seçin.
    
    b. **Yeni'yi**tıklatın.

9. **SAML Tek Oturum Açma Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:
    
    ![SAML Tek Oturum Açma Ayarı](./media/work-com-tutorial/ic794114.png "SAML Tek Oturum Açma Ayarı")
    
    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın.  
       
    > [!NOTE]
    > **Ad** için bir değer **sağlamak, API Adı** metin kutusunu otomatik olarak doldurmaz.
    
    b. **Veren** metin kutusunda, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın** değerini yapıştırın.
    
    c. İndirilen sertifikayı Azure portalından yüklemek için **Gözat'ı**tıklatın.
    
    d. Varlık **Kimliği** metin kutusuna, `https://salesforce-work.com`yazın.
    
    e. **SAML Identity Type**olarak, **Assertion'ı seçin, Kullanıcı nesnesinden Federasyon Kimliği içerir.**
    
    f. **SAML Identity Location**olarak, select **Identity Konu deyiminin NameIdentfier öğesindedir.**
    
    g. **Kimlik Sağlayıcı Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.

    h. **Kimlik Sağlayıcı Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.
    
    i. **Hizmet Sağlayıcı Başlatılan İstek Bağlama**olarak, HTTP **Post**seçin.
    
    j. **Kaydet**'e tıklayın.

10. Work.com klasik portalınızda, sol daki gezinti bölmesinde, ilgili bölümü genişletmek için **Etki Alanı Yönetimi'ni** tıklatın ve ardından **Etki Alanım** sayfasını açmak için Etki **Alanımı** tıklatın. 
    
    ![Etki Alanım](./media/work-com-tutorial/ic794115.png "Etki Alanım")

11. Etki **Alanım** sayfasında, **Giriş Sayfası Markalama** **bölümünde, Edit'i**tıklatın.
    
    ![Giriş Sayfası Markalama](./media/work-com-tutorial/ic767826.png "Giriş Sayfası Markalama")

12. Giriş **Sayfası Markalama** sayfasında, **Kimlik Doğrulama Hizmeti** bölümünde **SAML SSO Ayarlarınızın** adı görüntülenir. Seçin ve sonra **Kaydet'i**tıklatın.
    
    ![Giriş Sayfası Markalama](./media/work-com-tutorial/ic784366.png "Giriş Sayfası Markalama")

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

Bu bölümde, Britta Simon'ın Work.com erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Work.com**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Work.com'** yi seçin.

    ![Uygulamalar listesindeki Work.com bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-workcom-test-user"></a>Work.com test kullanıcısı oluşturma

Azure Active Directory kullanıcılarının oturum açabilmesi için Work.com sağlanmalıdır. Work.com durumunda, sağlama manuel bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Work.com şirket sitenizde yönetici olarak oturum açın.

2. **Kurulum'a**gidin.
   
    ![Kurulum](./media/work-com-tutorial/IC794108.png "Kurulum")

3. ** \> Kullanıcıları Yönetme'ye**gidin.
   
    ![Kullanıcıları Yönet](./media/work-com-tutorial/IC784369.png "Kullanıcıları Yönet")

4. **Yeni Kullanıcı'yı**tıklatın.
   
    ![Tüm Kullanıcılar](./media/work-com-tutorial/IC794117.png "Tüm Kullanıcılar")

5. Kullanıcı Edit bölümünde, ilgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının özniteliklerinde aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı Edit](./media/work-com-tutorial/ic794118.png "Kullanıcı Edit")
   
    a. Ad **metin** kutusuna, kullanıcı **Britta'nın** **ilk adını** yazın.
    
    b. **Soyadı** metin kutusuna, kullanıcı **Simon'ın** **soyadını** yazın.
    
    c. Diğer **Ad** metin kutusuna, kullanıcı **BrittaS'ın** **adını** yazın.
    
    d. **E-posta** metin kutusuna, kullanıcının Brittasimon@contoso.com **e-posta adresini** yazın.
    
    e. Kullanıcı **Adı** metin kutusuna, kullanıcının kullanıcı Brittasimon@contoso.comadını şöyle yazın.
    
    f. Nick **Adı** metin kutusuna, kullanıcı **Simon'ın** **nick adını** yazın.
    
    g. **Rol,** **Kullanıcı Lisansı**ve **Profil'i**seçin.
    
    h. **Kaydet**'e tıklayın.  
      
    > [!NOTE]
    > Azure AD hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bir bağlantı içeren bir e-posta alır.
    > 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Work.com döşemesini tıklattığınızda, SSO'yu kurduğunuz Work.com otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

