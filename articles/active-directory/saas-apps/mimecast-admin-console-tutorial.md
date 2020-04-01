---
title: 'Öğretici: Mimecast Yönetici Konsolu ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Mimecast Yönetici Konsolu arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9edadd6462052f82f92c05c1678f845ece856cfb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160658"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Öğretici: Mimecast Yönetici Konsolu ile Azure Active Directory entegrasyonu

Bu eğitimde, Mimecast Yönetici Konsolu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Mimecast Yönetici Konsolu'nu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Mimecast Yönetici Konsolu'na erişimi olan Azure AD'da kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Mimecast Yönetici Konsolu'nda (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Mimecast Admin Console tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Mimecast Admin Console **SP** başlatılan SSO destekler

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Galeriden Mimecast Yönetici Konsolu Ekleme

Mimecast Yönetici Konsolu'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Mimecast Yönetici Konsolu eklemeniz gerekir.

**Galeriden Mimecast Yönetici Konsolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Mimecast Yönetici Konsolu**yazın, sonuç panelinden **Mimecast Yönetici Konsolu'nu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesinde Mimecast Yönetici Konsolu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Mimecast Yönetici Konsolu ile Azure AD tek oturumunu yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Mimecast Yönetici Konsolu'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumunu Mimecast Yönetici Konsolu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Mimecast Yönetici Konsolu Tek Oturum Açma](#configure-mimecast-admin-console-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Mimecast Yönetici Konsolu test kullanıcısını oluşturun](#create-mimecast-admin-console-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Mimecast Yönetici Konsolu'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Mimecast Yönetici Konsolu ile Azure AD oturumunu yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Mimecast Yönetici Konsolu** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Mimecast Admin Console Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna URL'yi yazın:
    
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > URL'deki işaret bölgeye özgüdür.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Mimecast Yönetici Konsolu'nu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-mimecast-admin-console-single-sign-on"></a>Yapıla, Mimecast Yönetici Konsolu Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak Mimecast Yönetici Konsolunuza giriş yapın.

2. ** \> Hizmetler Uygulamasına**gidin.

    ![Hizmetler](./media/mimecast-admin-console-tutorial/ic794998.png "Hizmetler")

3. **Kimlik Doğrulama Profilleri'ni**tıklatın.

    ![Kimlik Doğrulama Profilleri](./media/mimecast-admin-console-tutorial/ic794999.png "Kimlik Doğrulama Profilleri")
    
4. **Yeni Kimlik Doğrulama Profili'ni**tıklatın.

    ![Yeni Kimlik Doğrulama Profilleri](./media/mimecast-admin-console-tutorial/ic795000.png "Yeni Kimlik Doğrulama Profilleri")

5. Kimlik **Doğrulama Profili** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kimlik Doğrulama Profili](./media/mimecast-admin-console-tutorial/ic795015.png "Kimlik Doğrulama Profili")
    
    a. **Açıklama** metin kutusunda, yapılandırmanız için bir ad yazın.
    
    b. **Mimecast Yönetici Konsolu için SAML Kimlik Doğrulamasını Uygula'yı**seçin.
    
    c. **Sağlayıcı**olarak Azure **Etkin Dizini'ni**seçin.
    
    d. Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcısını** **Veren URL** metin kutusuna yapıştırın.
    
    e. Azure portalından kopyaladığınız **Giriş URL'sini**Giriş **URL'sine** yapıştırın.

    f. Azure portalından kopyaladığınız **Giriş URL'sini** **Logout URL'sine** yapıştırın.
    
    >[!NOTE]
    >Giriş URL değeri ve Giriş URL değeri Mimecast Yönetici Konsolu için aynıdır.
    
    g. Not defterinde Azure portalından indirilen taban-64 sertifikanızı açın,*--* ilk satırı ("*--*") ve son satırı (" ") kaldırın, kalan içeriğini panonuza kopyalayın ve ardından **Kimlik Sağlayıcı Sertifikası (MetaData)** metin kutusuna yapıştırın.
    
    h. **Tek Oturum Aç'ı**seçin.
    
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

Bu bölümde, Britta Simon'ın Mimecast Yönetici Konsolu'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Mimecast Yönetici Konsolu'nu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mimecast Yönetici Konsolu**yazın ve seçin.

    ![Uygulamalar listesindeki Mimecast Yönetici Konsolu bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-mimecast-admin-console-test-user"></a>Mimecast Yönetici Konsolu test kullanıcıoluşturma

Azure AD kullanıcılarının Mimecast Yönetici Konsolu'na giriş yapabilmeleri için Mimecast Yönetici Konsolu'na dahil edilmeleri gerekir. Mimecast Yönetici Konsolu durumunda, sağlama manuel bir görevdir.

* Kullanıcı oluşturmadan önce bir etki alanı kaydetmeniz gerekir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak **Mimecast Yönetici Konsolunuzda** oturum açın.

2. **Dizinler \> Dahili**gidin.
   
    ![Dizinler](./media/mimecast-admin-console-tutorial/ic795003.png "Dizinler")

3. **Yeni Etki Alanını Kaydedin'i**tıklatın.
   
    ![Yeni Etki Alanı Kaydol](./media/mimecast-admin-console-tutorial/ic795004.png "Yeni Etki Alanı Kaydol")

4. Yeni etki alanınız oluşturulduktan sonra **Yeni Adres'i**tıklatın.
   
    ![Yeni Adres](./media/mimecast-admin-console-tutorial/ic795005.png "Yeni Adres")

5. Yeni adres iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kaydet](./media/mimecast-admin-console-tutorial/ic795006.png "Kaydet")
   
    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **E-posta Adresi,** **Genel Adı,** **Parola**ve Parola özniteliklerini **onaylayın** yazın.

    b. **Kaydet**'e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Mimecast Yönetici Konsolu tarafından sağlanan diğer Mimecast Yönetici Konsolu kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Mimecast Yönetici Konsolu döşemesini tıklattığınızda, SSO'yu kurduğunuz Mimecast Yönetici Konsolu'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

