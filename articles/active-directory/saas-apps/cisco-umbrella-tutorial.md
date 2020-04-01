---
title: 'Öğretici: Cisco Umbrella ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Cisco Umbrella arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 675dca98-f119-4463-8350-d6a45d5601e3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee91ca53e32bfdc387dc20054493d02d506a75da
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158633"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-umbrella"></a>Öğretici: Cisco Umbrella ile Azure Active Directory entegrasyonu

Bu eğitimde, Cisco Umbrella'yı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Cisco Umbrella'yı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Cisco Umbrella erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Cisco Umbrella'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Cisco Umbrella ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Cisco Umbrella tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Cisco **Umbrella, SP ve IDP'nin** başlattığı SSO'yu destekliyor

## <a name="adding-cisco-umbrella-from-the-gallery"></a>Galeriden Cisco Umbrella ekleme

Cisco Umbrella'nın Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Cisco Umbrella eklemeniz gerekir.

**Galeriden Cisco Umbrella eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cisco Umbrella**yazın, sonuç panelinden **Cisco Umbrella'yı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Cisco Umbrella](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre [Uygulama adı] ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile [Uygulama adı]'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Cisco Umbrella Tek Oturum Açma 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-cisco-umbrella-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Cisco Umbrella test kullanıcısını oluşturun](#create-cisco-umbrella-test-user)** - Cisco Umbrella'da Britta Simon'ın kullanıcının Azure AD temsiline bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Cisco Umbrella** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![Cisco Umbrella Domain ve URL'ler tek oturum açma bilgileri](common/both-preintegrated-signon.png)

    a. Uygulamayı **SP** intiated modunda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    b. **Ek URL'ler ayarla'yı**tıklatın.

    c. Oturum **Açma URL** metin kutusuna bir URL yazın:`https://login.umbrella.com/sso`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, **metadata XML'yi** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Cisco **Umbrella'yı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-cisco-umbrella-single-sign-on"></a>Cisco Umbrella Tek İşaretli Yapıl

1. Farklı bir tarayıcı penceresinde, Cisco Umbrella şirket sitenize yönetici olarak oturum açın.

2. Menünün sol **tarafından, Admin'i** tıklatın ve **Kimlik Doğrulama'ya** gidin ve ardından **SAML'yi**tıklatın.

    ![Yönetici](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_admin.png)

3. **Diğer'i** seçin ve **NEXT'e**tıklayın.

    ![Diğer](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_other.png)

4. Cisco **Umbrella Metadata,** sayfasında **NEXT'i**tıklatın.

    ![Meta veriler](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_metadata.png)

5. Meta **Veri Yükle** sekmesinde, önceden yapılandırılmış SAML'niz varsa, bunları değiştirmek için **burayı tıklatın'ı** seçin ve aşağıdaki adımları izleyin.

    ![Sonraki](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_next.png)

6. Seçenek **A: Upload XML dosyasında,** Azure portalından indirdiğiniz **Federasyon Metadata XML** dosyasını yükleyin ve meta verileri yükledikten sonra aşağıdaki değerler otomatik olarak doldurulur ve **NEXT'i**tıklatın.

    ![Selectfile](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_choosefile.png)

7. **SAML Yapılandırmayı Doğrula** bölümünde, **SAML YAPıLANDıRMANIZI TEST ED'i**tıklatın.

    ![The Test](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_test.png)

8. **KAYDET'i**tıklatın.

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

Bu bölümde, Britta Simon'ın Cisco Umbrella'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Cisco Umbrella'yı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cisco Umbrella**yazın ve seçin.

    ![Uygulamalar listesindeki Cisco Umbrella bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-cisco-umbrella-test-user"></a>Cisco Umbrella test kullanıcısı oluşturma

Azure AD kullanıcılarının Cisco Umbrella'da oturum açabilmeleri için Cisco Umbrella'da oturum açmaları gerekir.  
Cisco Umbrella söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir tarayıcı penceresinde, Cisco Umbrella şirket sitenize yönetici olarak oturum açın.

2. Menünün sol tarafından **Yönetici'yi** tıklatın ve **Hesaplar'a**gidin.

    ![Hesap](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_account.png)

3. **Hesaplar** sayfasında, sayfanın sağ üst tarafındaki **Ekle'yi** tıklatın ve aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_createuser.png)

    a. Ad **alanı** nın ilk **adı, Britta**gibi ilk adı girin.

    b. **Soyadı** alanına, **simon**gibi soyadını girin.

    c. Temsilci Seç **Yönetici Rolü'nden,** rolünüzü seçin.
  
    d. **E-posta Adresi** alanına **\@brittasimon contoso.com**gibi kullanıcının e-posta adresini girin.

    e. **Parola** alanına parolanızı girin.

    f. **Parolayı Onayla** alanına, parolanızı yeniden girin.

    g. **OLUŞTUR'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Cisco Umbrella döşemesini tıklattığınızda, SSO'yu kurduğunuz Cisco Umbrella'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
