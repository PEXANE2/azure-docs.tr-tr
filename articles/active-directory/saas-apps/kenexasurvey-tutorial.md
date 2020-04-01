---
title: 'Öğretici: IBM Kenexa Survey Enterprise ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve IBM Kenexa Survey Enterprise arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/08/2019
ms.author: jeedes
ms.openlocfilehash: c649b966b3e210f6b026b06a9654761e0f97aea1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67099050"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Öğretici: IBM Kenexa Survey Enterprise ile Azure Active Directory entegrasyonu

Bu eğitimde, IBM Kenexa Survey Enterprise'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
IBM Kenexa Survey Enterprise'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* IBM Kenexa Survey Enterprise erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla IBM Kenexa Survey Enterprise (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini IBM Kenexa Survey Enterprise ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* IBM Kenexa Survey Enterprise tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* IBM Kenexa Survey **Enterprise, IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>Galeriden IBM Kenexa Survey Enterprise ekleme

IBM Kenexa Survey Enterprise'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden IBM Kenexa Survey Enterprise'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden IBM Kenexa Survey Enterprise eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda IBM **Kenexa Survey Enterprise**yazın, sonuç panelinden **IBM Kenexa Survey Enterprise'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![IBM Kenexa Survey Enterprise sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini IBM Kenexa Survey Enterprise ile **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile IBM Kenexa Survey Enterprise'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını IBM Kenexa Survey Enterprise ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[IBM Kenexa Survey Enterprise Tek Oturum Açma](#configure-ibm-kenexa-survey-enterprise-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[IBM Kenexa Survey Enterprise test kullanıcısını oluşturun](#create-ibm-kenexa-survey-enterprise-test-user)** - kullanıcının Azure AD gösterimine bağlı IBM Kenexa Survey Enterprise'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

AZURE AD oturumaçmaişlemlerini IBM Kenexa Survey Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **IBM Kenexa Survey Enterprise** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![IBM Kenexa Survey Enterprise Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://surveys.kenexa.com/<companycode>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [IBM Kenexa Survey Enterprise Client destek ekibine](https://www.ibm.com/support/home/?lnk=fcw) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. IBM Kenexa Survey Enterprise uygulaması, SAML belirteç özniteliklerinizin yapılandırmasına özel öznitelik eşlemeleri eklemenizi gerektiren güvenlik iddiaları Biçimlendirme Dili (SAML) iddialarını belirli bir biçimde almayı bekler. Yanıttaki kullanıcı tanımlayıcı talebinin değeri, Kenexa sisteminde yapılandırılan SSO Kimliği ile eşleşmelidir. Kuruluşunuzdaki uygun kullanıcı tanımlayıcısını SSO Internet Datagram Protokolü (IDP) olarak eşlemek için [IBM Kenexa Survey Enterprise destek ekibiyle](https://www.ibm.com/support/home/?lnk=fcw)birlikte çalışın.

    Varsayılan olarak, Azure AD kullanıcı tanımlayıcısını kullanıcı anaadı (UPN) değeri olarak ayarlar. Aşağıdaki ekran görüntüsünde gösterildiği gibi, bu değeri **Kullanıcı Öznitelikleri** sekmesinde değiştirebilirsiniz. Tümleştirme yalnızca eşlemi doğru bir şekilde tamamladıktan sonra çalışır.

    ![image](common/edit-attribute.png)

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. IBM **Kenexa Survey Enterprise'ı kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-ibm-kenexa-survey-enterprise-single-sign-on"></a>IBM Kenexa Survey Enterprise Tek Oturum Açma'yı yapılandırın

**IBM Kenexa Survey Enterprise** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve azure portalından uygun kopyalanmış URL'leri [IBM Kenexa Survey Enterprise destek ekibine](https://www.ibm.com/support/home/?lnk=fcw)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın IBM Kenexa Survey Enterprise'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **IBM Kenexa Survey Enterprise'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **IBM Kenexa Survey Enterprise'ı**seçin.

    ![Uygulamalar listesindeki IBM Kenexa Survey Enterprise bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-ibm-kenexa-survey-enterprise-test-user"></a>IBM Kenexa Survey Enterprise test kullanıcısı oluşturun

Bu bölümde, IBM Kenexa Survey Enterprise'da Britta Simon adında bir kullanıcı oluşturursunuz.

IBM Kenexa Survey Enterprise sisteminde kullanıcılar oluşturmak ve onlar için SSO kimliğini haritalamak için [IBM Kenexa Survey Enterprise destek ekibiyle](https://www.ibm.com/support/home/?lnk=fcw)çalışabilirsiniz. Bu SSO kimlik değeri, Azure AD'nin kullanıcı tanımlayıcı değeriyle de eşlenmelidir. Bu varsayılan ayarı **Öznitelik** sekmesinde değiştirebilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki IBM Kenexa Survey Enterprise döşemesini tıklattığınızda, SSO'yu kurduğunuz IBM Kenexa Survey Enterprise'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

