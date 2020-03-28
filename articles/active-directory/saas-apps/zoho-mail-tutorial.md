---
title: 'Öğretici: Zoho ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zoho arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b5d0e52e927a44d5bce5314e724c1c753809ce0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74231933"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Öğretici: Zoho ile Azure Active Directory entegrasyonu

Bu eğitimde, Zoho'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Zoho'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zoho erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zoho'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zoho ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Zoho tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zoho **SP** başlatılan SSO destekler

## <a name="adding-zoho-from-the-gallery"></a>Galeriden Zoho ekleme

Zoho'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Zoho'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Zoho**yazın, sonuç panelinden **Zoho'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Zoho sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Zoho ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Zoho'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Zoho ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zoho Tek Oturum Açma'yı uygulama](#configure-zoho-single-sign-on)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zoho test kullanıcıoluşturun](#create-zoho-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Zoho Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Zoho ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Zoho** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Zoho Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.zohomail.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Zoho Client destek ekibine](https://www.zoho.com/mail/contact.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Zoho'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zoho-single-sign-on"></a>Zoho Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Zoho Mail şirket sitenize yönetici olarak giriş yapın.

2. **Kontrol paneline**git.
   
    ![Denetim Masası](./media/zoho-mail-tutorial/ic789607.png "Denetim Masası")

3. **SAML Kimlik Doğrulama** sekmesini tıklatın.
   
    ![SAML Kimlik Doğrulama](./media/zoho-mail-tutorial/ic789608.png "SAML Kimlik Doğrulama")

4. **SAML Kimlik Doğrulama Ayrıntıları** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![SAML Kimlik Doğrulama Ayrıntıları](./media/zoho-mail-tutorial/ic789609.png "SAML Kimlik Doğrulama Ayrıntıları")
   
    a. Giriş **URL** metin kutusuna, Azure portalından kopyaladığınız **Giriş URL'sini** yapıştırın.
   
    b. **Logout URL** metin kutusuna, Azure portalından kopyaladığınız **Logout URL'sini** yapıştırın.
   
    c. **ParolaURL'sini Değiştir** metin kutusuna Azure portalından kopyaladığınız **Parola URL'sini değiştir'i** yapıştırın.
       
    d. Azure portalından indirilen taban-64 kodlu sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından PublicKey** metin kutusuna yapıştırın.
   
    e. **Algoritma**olarak **RSA'yı**seçin.
   
    f. **Tamam**'a tıklayın.

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

Bu bölümde, Britta Simon'ın Zoho'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Zoho'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Zoho**yazın ve seçin.

    ![Uygulamalar listesindeki Zoho bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-zoho-test-user"></a>Zoho test kullanıcıoluşturma

Azure AD kullanıcılarının Zoho Mail'e giriş yapabilmeleri için Zoho Mail'e dahil edilmeleri gerekir. Zoho Mail durumunda, sağlama manuel bir görevdir.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Zoho Mail tarafından sağlanan diğer Zoho Mail kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Yönetici olarak **Zoho Mail** şirket sitenize giriş yapın.

1. Denetim **Masası \> Posta & Dokümanlar**gidin.

1. Kullanıcı **Ayrıntılarına \> **Git Kullanıcı Ekle .
   
    ![Kullanıcı Ekle](./media/zoho-mail-tutorial/ic789611.png "Kullanıcı Ekleme")

1. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı Ekle](./media/zoho-mail-tutorial/ic789612.png "Kullanıcı Ekleme")
   
    a. Ad **metin** kutusuna, **Britta**gibi kullanıcının ilk adını yazın.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını yazın.

    c. **E-posta kimliği** textbox, **\@brittasimon contoso.com**gibi kullanıcının e-posta kimliği yazın.

    d. **Parola** metin kutusuna, kullanıcının parolasını girin.
   
    e. **Tamam**'a tıklayın.  
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bağlantı içeren bir e-posta alır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zoho döşemesini tıklattığınızda, SSO'yu kurduğunuz Zoho'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

