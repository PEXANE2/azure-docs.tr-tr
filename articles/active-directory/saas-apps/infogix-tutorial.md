---
title: 'Öğretici: Infogix Data3Sixty Govern ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Infogix Data3Sixty Govern arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: a0b8dbb3992808bdcb94291d876192431c72f6c2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67100355"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Öğretici: Infogix Data3Sixty Govern ile Azure Active Directory entegrasyonu

Bu eğitimde, Infogix Data3Sixty Govern'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Infogix Data3Sixty Govern'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Infogix Data3Sixty Govern erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Infogix Data3Sixty Govern (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Infogix Data3Sixty Govern ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Infogix Data3Sixty Tek oturum açma özellikli aboneliği yönetin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Infogix Data3Sixty Govern **SP ve IDP** SSO başlatılan destekler
* Infogix Data3Sixty Govern **Just In Time** kullanıcı sağlama destekler

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Galeriden Infogix Data3Sixty Govern ekleme

Infogix Data3Sixty Govern'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Infogix Data3Sixty Govern eklemeniz gerekir.

**Galeriden Infogix Data3Sixty Govern eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Infogix Data3Sixty Govern**yazın, sonuç panelinden **Infogix Data3Sixty Govern'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Infogix Data3Sixty Sonuç listesinde yönet](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Infogix Data3Sixty Govern ile Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Infogix Data3Sixty Govern'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Infogix Data3Sixty Govern ile Azure AD oturumaçmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Infogix Data3Sixty Govern Tek Oturum Açma](#configure-infogix-data3sixty-govern-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Infogix Data3Sixty Govern test kullanıcısını oluşturun](#create-infogix-data3sixty-govern-test-user)** - Infogix Data3Sixty Govern'da Britta Simon'ın bir muadili kullanıcının Azure AD gösterimine bağlı olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Infogix Data3Sixty Govern ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Infogix Data3Sixty Govern** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Infogix Data3Sixty Yöneten Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://data3sixty.com/ui`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.data3sixty.com/sso/acs`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Infogix Data3Sixty Yöneten Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.data3sixty.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Infogix Data3Sixty Govern Client destek ekibine](mailto:data3sixtysupport@infogix.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. Infogix Data3Sixty Govern uygulaması SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

7. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, **Edit simgesini** kullanarak talepleri düzenleme veya yukarıdaki resimde gösterildiği gibi SAML belirteç özniteliğini yapılandırmak için **yeni talep ekle** kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği|
    | -----------| -------------- |
    | Ad  | user.givenname |
    | Soyadı   | user.surname |
    | kullanıcı adı   | kullanıcı.posta    |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

8. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Ham)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

9. **Infogix Data3Sixty Govern'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-infogix-data3sixty-govern-single-sign-on"></a>Infogix Data3Sixty Yönet Tek Oturum Açma'yı Yapılandır

**Infogix Data3Sixty Govern** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Raw)** ve uygun kopyalanmış URL'leri Azure portalından [Infogix Data3Sixty Govern destek ekibine](mailto:data3sixtysupport@infogix.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın Infogix Data3Sixty Govern'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Infogix Data3Sixty Govern'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Infogix Data3Sixty Govern'ı**seçin.

    ![Uygulamalar listesindeInfogix Data3Sixty Govern bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-infogix-data3sixty-govern-test-user"></a>Infogix Data3Sixty Govern test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Infogix Data3Sixty Govern oluşturulur. Infogix Data3Sixty Govern, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Infogix Data3Sixty Govern'da zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Infogix Data3Sixty Govern destek ekibine](mailto:data3sixtysupport@infogix.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Infogix Data3Sixty Govern döşemesini tıklattığınızda, SSO'yu kurduğunuz Infogix Data3Sixty Govern'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

