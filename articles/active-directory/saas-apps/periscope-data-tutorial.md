---
title: 'Öğretici: Periscope Verileri ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Periscope Data arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3f378edb-9ac9-494d-a84a-03357b923ee1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: ffa0176c4dde6334e70b9a56b066a677d1610b48
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160203"
---
# <a name="tutorial-azure-active-directory-integration-with-periscope-data"></a>Öğretici: Periscope Verileri ile Azure Active Directory entegrasyonu

Bu eğitimde, Periscope Verilerini Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Periscope Verilerini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Periskop Verilerine erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Periscope Verileri'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Periscope Verileri ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Periscope Data tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Periscope **Data, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-periscope-data-from-the-gallery"></a>Galeriden Periscope Veriekleme

Periscope Verilerinin Azure AD'ye entegrasyonunu yapılandırmak için, galerideki Periscope Verilerini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Periscope Verileri eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Periscope Verileri**yazın, sonuç panelinden **Periscope Verileri'ni** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesindeki Periscope Verileri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Periscope Verileri ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Periscope Verileri'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Periscope Verileri ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Periscope Veri Tek Oturum](#configure-periscope-data-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Periscope Veri test kullanıcısını oluşturun](#create-periscope-data-test-user)** - Kullanıcının Azure AD gösterimine bağlı Periscope Verileri'nde Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Periscope Verileri ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Periscope Data** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Periscope Veri Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna URL'lerden herhangi birini yazın:
    
    | |
    |--|
    | `https://app.periscopedata.com/` |
    | `https://app.periscopedata.com/app/<SITENAME>` |

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.periscopedata.com/<SITENAME>/sso`

    > [!NOTE]
    > URL değerindeki Oturum gerçek değildir. URL'de gerçek Oturum İşareti ile değerleri güncelleştirin. Bu değeri ve daha sonra öğreticide açıklanan **Yapılandırma Periscope Veri Tek Oturum Açma** bölümünden alacağınız Tanımlayıcı değerini almak için [Periscope Veri İstemci destek ekibine](mailto:support@periscopedata.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-periscope-data-single-sign-on"></a>Periskop Verilerini Yapılandırma Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Periscope Data'da oturum açın.

2. Sol alttaki vites menüsünü açın ve **Fatura Güvenliği** > **menüsünü** açın ve aşağıdaki adımları gerçekleştirin. Yalnızca yöneticiler bu ayarlara erişebilir.

    ![Periskop Veri Yapılandırma bilgileri](./media/periscope-data-tutorial/configure01.png)

    a. **Saml İmzaLama Sertifikası** adım#5 App **Federation Metadata URL'sini** kopyalayın ve bir tarayıcıda açın. Bu bir XML belgesi açılır.

    b. Tek **Oturum Açma** metin kutusunda **Azure Etkin Dizini'ni**seçin.

    c. **SingleSignOnService** etiketini bulun ve **SSO URL** metin kutusuna **Konum** değerini yapıştırın.

    d. **SingleLogoutService** etiketini bulun ve Konum **değerini** **SLO URL** metin kutusuna yapıştırın.

    e. Örneğiniz için **Tanımlayıcı** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümünün **Tanımlayıcı (Entity ID)** textbox'ına yapıştırın.

    f. XML dosyasının ilk etiketini bulun, **entityID** değerini kopyalayın ve **Veren** metin kutusuna yapıştırın.

    g. SAML protokolü ile **IDPSSODescriptor** etiketini bulun. Bu bölümde, **use=signing**ile **AnahtarTanımlayıcı** etiketini bulun. **X509Certificate** değerini kopyalayın ve **Sertifika** metin kutusuna yapıştırın.

    h. Birden çok boşalana sahip siteler **Varsayılan Alan** açılır tarihinden varsayılan alanı seçebilir. Bu alan, yeni kullanıcıların Periscope Data'da ilk kez oturum açtıklarında ve Active Directory Single Sign On aracılığıyla sağlandığında eklenecekleri alan olacaktır.

    i. Son olarak, **Kaydet'i** tıklatın **ve** **Logout**yazarak SSO ayarlarını değiştirin.

    ![Periskop Veri Yapılandırma bilgileri](./media/periscope-data-tutorial/configure02.png)

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

Bu bölümde, Britta Simon'ın Periscope Verilerine erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Periscope Verileri'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Periscope Verileri'ni**seçin.

    ![Uygulamalar listesindeki Periscope Veri bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-periscope-data-test-user"></a>Periscope Veri test kullanıcıoluşturma

Azure AD kullanıcılarının Periscope Verileri'nde oturum açabilmeleri için Periscope Verileri'nde bunların sağlanması gerekir. Periskop Verilerinde, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Periscope Data'da oturum açın.

2. Menünün sol alt kısmındaki **Ayarlar** simgesine tıklayın ve **İzinler'e**gidin.

    ![Periskop Veri Yapılandırma bilgileri](./media/periscope-data-tutorial/configure03.png)

3. **ADD USER'a** tıklayın ve aşağıdaki adımları gerçekleştirin:

      ![Periskop Veri Yapılandırma bilgileri](./media/periscope-data-tutorial/configure04.png)

    a. **Ad metin** kutusuna, **Britta**gibi kullanıcının ilk adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    c. **E-posta** metin kutusuna, **\@brittasimon contoso.com**gibi kullanıcının e-posta girin.

    d. **EKLE'yi**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Periscope Veri döşemesini tıklattığınızda, SSO'yu kurduğunuz Periscope Verileri'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

