---
title: 'Öğretici: LaunchDarkly ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LaunchDarkly arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3f0671bc-f93f-496e-b465-b9ce8c6633fa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e32f8c3ea300960893163264e99bd6c51138c7c5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159647"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Öğretici: LaunchDarkly ile Azure Active Directory entegrasyonu

Bu eğitimde, LaunchDarkly'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
LaunchDarkly'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* LaunchDarkly erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla LaunchDarkly (Tek Oturum Açma) için otomatik olarak oturum açmalarını etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini LaunchDarkly ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* LaunchDarkly tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* LaunchDarkly **SP ve IDP** başlatılan SSO destekler
* LaunchDarkly **Just In Time** kullanıcı sağlama destekler

## <a name="adding-launchdarkly-from-the-gallery"></a>Galeriden LaunchDarkly ekleme

LaunchDarkly'nin Azure AD'ye entegrasyonunu yapılandırmak için, LaunchDarkly'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden LaunchDarkly'yi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **LaunchDarkly**yazın , sonuç panelinden **LaunchDarkly'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![LaunchDarkly sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre [Uygulama adı] ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile [Uygulama adı]'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[LaunchDarkly Single Sign-On](#configure-launchdarkly-single-sign-on)** 'u yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[LaunchDarkly test kullanıcı oluşturun](#create-launchdarkly-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı LaunchDarkly Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini [Uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **LaunchDarkly** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![LaunchDarkly Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`app.launchdarkly.com`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`

    > [!NOTE]
    > YanıtURL değeri gerçek değil. Değeri, öğreticide daha sonra açıklanan gerçek Yanıt URL'si ile güncelleştireceksiniz. Uygulamayı **IDP** modunda kullanmayı düşünüyorsanız, URL alanında **Oturum'u** boş bırakmanız gerekir, aksi takdirde **IDP'den**giriş başlatamazsınız. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://app.launchdarkly.com`

    ![LaunchDarkly Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **LaunchDarkly'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-launchdarkly-single-sign-on"></a>LaunchDarkly Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, LaunchDarkly şirket sitenize yönetici olarak giriş yapın.

2. Sol daki gezinti panelinden **Hesap Ayarları'nı** seçin.

    ![LaunchDarkly Yapılandırma](./media/launchdarkly-tutorial/configure1.png)

3. **Güvenlik** sekmesini tıklatın.

    ![LaunchDarkly Yapılandırma](./media/launchdarkly-tutorial/configure2.png)

4. **SSO'ya ETKIN'i** ve ardından **SAML YAPıLANDıRMASıNı EDIT'i**tıklatın.

    ![LaunchDarkly Yapılandırma](./media/launchdarkly-tutorial/configure3.png)

5. **SAML yapılandırma bölümünüzü düzenleme de** aşağıdaki adımları gerçekleştirin:

    ![LaunchDarkly Yapılandırma](./media/launchdarkly-tutorial/configure4.png)

    a. Örneğiniz için **SAML tüketici hizmeti URL'sini** kopyalayın ve Azure portalındaki **LaunchDarkly Etki Alanı ve URL'ler** bölümündeki YanıtLA URL metin kutusuna yapıştırın.

    b. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. Azure portalından indirilen sertifikayı Notepad'e açın, içeriği kopyalayın ve **Ardından X.509 sertifika** kutusuna yapıştırın veya **yükleme sertifikasını**tıklatarak sertifikayı doğrudan yükleyebilirsiniz.

    d. **Kaydet'i** tıklatın

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

Bu bölümde, Britta Simon'ın LaunchDarkly'ye erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **LaunchDarkly'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **LaunchDarkly'yi**seçin.

    ![Uygulamalar listesinde LaunchDarkly bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-launchdarkly-test-user"></a>LaunchDarkly test kullanıcı oluşturun

Bu bölümün amacı LaunchDarkly Britta Simon adlı bir kullanıcı oluşturmaktır. LaunchDarkly, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Henüz yoksa LaunchDarkly'ye erişme girişimi sırasında yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa [LaunchDarkly Client destek ekibine](mailto:support@launchdarkly.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LaunchDarkly döşemesini tıklattığınızda, SSO'yu kurduğunuz LaunchDarkly'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
