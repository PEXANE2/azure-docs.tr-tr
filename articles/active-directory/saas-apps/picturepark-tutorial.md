---
title: 'Öğretici: Picturepark ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Picturepark arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 08c5bd8da0dda74156b2d44c8106ed345ef749dd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73177006"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Öğretici: Picturepark ile Azure Active Directory entegrasyonu

Bu eğitimde Picturepark'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Picturepark'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Picturepark'a erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Picturepark'ta (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Picturepark ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Picturepark tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Picturepark **SP** başlatılan SSO destekler

## <a name="adding-picturepark-from-the-gallery"></a>Galeriden Picturepark Ekleme

Picturepark'ın Azure AD'ye entegrasyonunu yapılandırmak için, Picturepark'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Picturepark eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Picturepark**yazın, sonuç panelinden **Picturepark'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Picturepark](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp sınayın.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile Picturepark'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Picturepark ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Picturepark Tek Oturum Açma 'yı **[yapılandırın](#configure-picturepark-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Picturepark test kullanıcısını oluşturun](#create-picturepark-test-user)** - Picturepark'ta Kullanıcının Azure AD gösterimine bağlı Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Picturepark ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Picturepark** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Picturepark Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.picturepark.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Picturepark İstemci destek ekibine](https://picturepark.com/company/picturepark-customer-support) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

6. **SAML İmza Sertifikası** **bölümünde, Thumbprint'i** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

7. **Picturepark'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın. **Giriş URL'si için**değeri aşağıdaki desenle kullanın:`https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id,_ Azure AD aboneliğinin kiracı kimliğidir.

    ![Yapılandırma URL'lerini kopyalama](./media/picturepark-tutorial/configurls.png)

    a. Azure AD Tanımlayıcısı

    b. Giriş URL'si

### <a name="configure-picturepark-single-sign-on"></a>Picturepark'ı Yapılandırtek İşaretle

1. Farklı bir web tarayıcısı penceresinde, Picturepark şirket sitenizde yönetici olarak oturum açın.

2. Üstteki araç çubuğunda **Yönetim araçlarını**tıklatın ve ardından **Yönetim Konsolu'nu**tıklatın.
   
    ![Yönetim Konsolu](./media/picturepark-tutorial/ic795062.png "Yönetim Konsolu")

3. **Kimlik Doğrulama'yı**tıklatın ve ardından **Kimlik sağlayıcılarını**tıklatın.
   
    ![Kimlik doğrulaması](./media/picturepark-tutorial/ic795063.png "Kimlik doğrulaması")

4. Kimlik **sağlayıcısı yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcı yapılandırması](./media/picturepark-tutorial/ic795064.png "Kimlik sağlayıcı yapılandırması")
   
    a. **Ekle**’ye tıklayın.
  
    b. Yapılandırmanız için bir ad yazın.
   
    c. **Varsayılan olarak Ayarla'yı**seçin.
   
    d. **İhraççı URI** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.
   
    e. **Güvenilir Veren Thumb Print** textbox'ta, **SAML İmzaLama Sertifikası** bölümünden kopyalamış olduğunuz **Thumbprint** değerini yapıştırın. 

5. **JoinDefaultUsersGroup'u**tıklatın.

6. **E-posta adresi** özniteliğini **Talep** metin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` kutusunda ayarlamak için **Kaydet**yazın ve tıklatın.

      ![Yapılandırma](./media/picturepark-tutorial/ic795065.png "Yapılandırma")

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

Bu bölümde, Britta Simon'ın Picturepark'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Picturepark'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Picturepark'ı**seçin.

    ![Uygulamalar listesindeki Picturepark bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-picturepark-test-user"></a>Picturepark test kullanıcısı oluşturma

Azure AD kullanıcılarının Picturepark'ta oturum açabilmeleri için Picturepark'a dahil edilmeleri gerekir. Picturepark durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Picturepark** kiracınızla oturum açın.

1. Üstteki araç çubuğunda **Yönetim araçlarını**tıklatın ve ardından **Kullanıcılar'ı**tıklatın.
   
    ![Kullanıcılar](./media/picturepark-tutorial/ic795067.png "Kullanıcılar")

1. **Kullanıcılara genel bakış** sekmesinde **Yeni'yi**tıklatın.
   
    ![Kullanıcı yönetimi](./media/picturepark-tutorial/ic795068.png "Kullanıcı yönetimi")

1. Kullanıcı **Oluştur** iletişim kutusunda, sağlamak istediğiniz geçerli bir Azure Etkin Dizin Kullanıcısının aşağıdaki adımlarını gerçekleştirin:
   
    ![Kullanıcı Oluştur](./media/picturepark-tutorial/ic795069.png "Kullanıcı Oluştur")
   
    a. **E-posta Adresi** metin kutusuna, kullanıcının `BrittaSimon@contoso.com` **e-posta adresini** yazın.  
   
    b. **Parola** ve **Şifreyi Onayla** metin kutularına BrittaSimon'Un **parolasını** yazın. 
   
    c. Ad **metin** kutusuna, kullanıcı **Britta'nın** **Adını** yazın. 
   
    d. **Soyadı** metin kutusuna, **kullanıcıNın** **Soyadı** Simon yazın.
   
    e. **Şirket** metin kutusuna, kullanıcının **Şirket adını** yazın. 
   
    f. **Ülke** metin kutusunda, kullanıcının **Ülke/Bölgesi'ni** seçin.
  
    g. **Posta** metin kutusuna şehrin **posta kodunu** yazın.
   
    h. **Şehir** metin kutusuna, kullanıcının **Şehir adını** yazın.

    i. Bir **Dil**seçin.
   
    j. **Oluştur'u**tıklatın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Picturepark tarafından sağlanan diğer Picturepark kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.
>

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Picturepark döşemesini tıklattığınızda, SSO'yu kurduğunuz Picturepark'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

