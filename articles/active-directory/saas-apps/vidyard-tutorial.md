---
title: 'Öğretici: Vidyard ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Vidyard arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: a55ec7afc94440ea8b6a48ed1507476d362df6c0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67087431"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Öğretici: Vidyard ile Azure Active Directory entegrasyonu

Bu eğitimde, Vidyard'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Vidyard'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Vidyard erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Vidyard'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Vidyard ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Vidyard tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Vidyard **SP** ve **IDP'nin** SSO'ya başlattığı destekler

* Vidyard **Just In Time** kullanıcı sağlama destekler

## <a name="adding-vidyard-from-the-gallery"></a>Galeriden Vidyard ekleme

Vidyard'ın Azure AD'ye entegrasyonunu yapılandırmak için, Vidyard'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Vidyard eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Vidyard**yazın, sonuç panelinden **Vidyard'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Vidyard sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Vidyard ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Vidyard'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi Vidyard ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Vidyard Tek Oturum Açma 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-vidyard-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Vidyard test kullanıcısını oluşturun](#create-vidyard-test-user)** - Britta Simon'ın Vidyard'daki bir örneğine sahip olmak için kullanıcının Azure AD gösterimine bağlı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Vidyard ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Vidyard** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Vidyard Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/consume`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Vidyard Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek Tanımlayıcı, Yanıt URL'si ve Oturum Açma URL'si ile güncelleştireceksiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Vidyard'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-vidyard-single-sign-on"></a>Vidyard Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Vidyard Software şirket sitenizde yönetici olarak oturum açın.

2. Vidyard panosundan **Grup** > **Güvenliği'ni** seçin

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure1.png)

3. **Yeni Profil** sekmesini tıklatın.

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure2.png)

4. **SAML Yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure3.png)

    a. Lütfen **Profil Adı** metin kutusuna genel profil adını girin.

    b. **SSO Kullanıcı Girişi Sayfası** değerini kopyalayın ve Azure **portalındaki** **Temel SAML Yapılandırma** bölümünde URL textbox'a yapıştırın.

    c. **ACS URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırmabölümündeyanıt** **URL** textbox'ına yapıştırın.

    d. **Veren/Metadata URL** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırma** bölümündeki **Tanımlayıcı** textbox'a yapıştırın.

    e. İndirilen sertifika dosyanızı Notepad'deki Azure portalından açın ve **ardından X.509 Sertifikası** metin kutusuna yapıştırın.

    f. **SAML Endpoint URL** textbox'ına Azure portalından kopyalanan **Giriş URL'sinin** değerini yapıştırın.

    g. **Onayla**'ya tıklayın.

5. Tek Oturum Açma sekmesinden, varolan bir profilin yanına **Atla'yı** seçin

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Bir SSO profili oluşturduktan sonra, bu profili kullanıcıların Azure üzerinden erişim emmesi gereken herhangi bir gruba atayın. Kullanıcı atandığı grup içinde yoksa, Vidyard otomatik olarak bir kullanıcı hesabı oluşturur ve rollerini gerçek zamanlı olarak atar.

6. **Atamak için Kullanılabilir Gruplar'da**görünen organizasyon grubunuzu seçin.

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure5.png)

7. **Atanan Grupların**Altında Atanan Grupları Görebilirsiniz. Kuruluşunuza göre grup için bir rol seçin ve **Onayla'yı**tıklatın.

    ![Vidyard Yapılandırma](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Daha fazla bilgi için [bu dokümana](https://knowledge.vidyard.com/hc/articles/360009990033-SAML-based-Single-Sign-On-SSO-in-Vidyard)bakın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Vidyard'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Vidyard'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Vidyard'ı**seçin.

    ![Uygulamalar listesindeki Vidyard bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-vidyard-test-user"></a>Vidyard test kullanıcısı oluşturma

Bu bölümde Vidyard'da Britta Simon adında bir kullanıcı oluşturulmuştur. Vidyard, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Vidyard'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanım oluşturmanız gerekiyorsa, [Vidyard destek ekibine](mailto:support@vidyard.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Vidyard karosuna tıkladığınızda, Otomatik olarak SSO'yu kurduğunuz Vidyard'da oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

