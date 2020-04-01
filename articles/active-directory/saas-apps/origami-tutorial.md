---
title: 'Öğretici: Origami ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Origami arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: fd347f4eb5f77dacc3c9fd61d0e885e9b3ee7959
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67095644"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Öğretici: Origami ile Azure Active Directory entegrasyonu

Bu eğitimde, Origami'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Origami'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Origami erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Origami'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Origami ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Origami tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Origami **SP** başlatılan SSO destekler

## <a name="adding-origami-from-the-gallery"></a>Galeriden Origami ekleme

Origami'nin Azure AD'ye entegrasyonunu yapılandırmak için, Origami'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Origami eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Origami**yazın, sonuç panelinden **Origami'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Origami sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Origami ile Azure AD tek oturum açma işlemini yapılandırın ve test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Origami'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi Origami ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Origami Tek Oturum Açma](#configure-origami-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Origami test kullanıcıoluşturun](#create-origami-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Origami Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Origami ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Origami** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Origami Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Origami İstemci destek ekibine](https://wordpress.org/support/theme/origami) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Origami'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-origami-single-sign-on"></a>Yapılandırılan Origami Tek İşaret-On

1. Yönetici haklarıyla Origami hesabına giriş yapın.

2. Üstteki menüde **Yönetici'yi**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_51.png)

3. Tek İşaret Kurulum da iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_531.png)

    a. **Tek İşaret i etkinleştir'i**seçin.

    b. Kimlik **Sağlayıcısının Oturum Açma Sayfası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin**değerini yapıştırın.

    c. Kimlik **Sağlayıcısının Oturum Açma Sayfası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    d. Azure portalından indirdiğiniz sertifikayı yüklemek için **Gözat'ı** tıklatın.

    e. **Değişiklikleri Kaydet**’e tıklayın.

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

Bu bölümde, Britta Simon'ın Origami'ye erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Origami'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Origami'yi**seçin.

    ![Uygulamalar listesindeki Origami bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-origami-test-user"></a>Origami test kullanıcısı oluşturma

Bu bölümde, Origami Britta Simon adlı bir kullanıcı oluşturun. 

1. Yönetici haklarıyla Origami hesabına giriş yapın.

2. Üstteki menüde **Yönetici'yi**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_51.png)

3. Kullanıcılar **ve Güvenlik** iletişim kutusunda, **Kullanıcılar'ı**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_54.png)

4. **Yeni Kullanıcı Ekle'yi**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_55.png)

5. Yeni Kullanıcı Ekle iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_56.png)

    a. Kullanıcı **Adı** metin **kutusuna\@brittasimon contoso.com**gibi kullanıcının e-postasını girin.

    b. **Parola** metin kutusuna bir parola yazın.

    c. **Parolayı Onayla** metin kutusuna parolayı yeniden yazın.

    d. Ad **metin** **kutusuna, Britta**gibi kullanıcının ilk adını girin.

    e. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    f. **Kaydet**'e tıklayın.
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_57.png)

6. Kullanıcı **Rolleri** ve **İstemci Erişimi'ni** kullanıcıya atayın. 
   
    ![Tek İşaret-On'u Yapılandır](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Origami karosu tıklattığınızda, SSO'yu kurduğunuz Origami'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

