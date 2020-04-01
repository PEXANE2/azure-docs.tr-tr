---
title: 'Öğretici: LearnUpon ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LearnUpon arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecdcd8c6024b3cacb422b556718bbbdbb5d601c2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67098231"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Öğretici: LearnUpon ile Azure Active Directory entegrasyonu

Bu eğitimde, LearnUpon'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
LearnUpon'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* LearnUpon erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla LearnUpon (Tek Oturum Açma) için otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini LearnUpon ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* LearnUpon tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.


* LearnUpon, **IDP'nin** başlattığı SSO'ya destek veriyor

* LearnUpon **Just In Time** kullanıcı sağlama destekler


## <a name="adding-learnupon-from-the-gallery"></a>Galeriden LearnUpon ekleme

LearnUpon'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize LearnUpon eklemeniz gerekir.

**Galeriden LearnUpon eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **LearnUpon**yazın , sonuç panelinden **LearnUpon'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde LearnUpon](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayanarak Azure AD tek oturum açma işlemini LearnUpon ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile LearnUpon'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmaişlemlerini LearnUpon ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[LearnUpon Tek Oturum Açma](#configure-learnupon-single-sign-on)** 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[LearnUpon test kullanıcısını oluşturun](#create-learnupon-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı LearnUpon'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini LearnUpon ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **LearnUpon** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![LearnUpon Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-reply.png)

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Yanıt URL'si ile güncelleştirin. Değeri almak için [LearnUpon Client destek ekibine](https://www.learnupon.com/features/support/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML** ile Tek Oturum Açma'da **THUMBPRINT'i** bulun - Bu, LearnUpon SAML Ayarlarınıza eklenecektir.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. **LearnUpon'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-learnupon-single-sign-on"></a>LearnUpon Tek Oturum Açma'yı Yapılandır

1. Başka bir tarayıcı örneği açın ve bir yönetici hesabıyla LearnUpon'da oturum açın.

1. **Ayarlar** sekmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. **Tek Oturum Açma - SAML'yi**tıklatın ve ardından SAML ayarlarını yapılandırmak için Genel **Ayarlar'ı** tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. Genel **Ayarlar** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. **Etkin**'i seçin.

    b. **Sürüm'u** **2.0**olarak seçin.

    c. **Atlama koşullarını** **Hayır**olarak seçin.

    d. **SAML Token Post param adı** textbox'ında, istek posta parametresinin adını yukarıda belirtilen ve doğrulanacak ve doğrulanacak SAML İddiasını içeren SAML tüketici URL'sine yazın - örneğin **SAMLResponse**.

    e. Ad **Tanımlayıcı Biçim** metin kutusuna, SAML İddianızda kullanıcıların tanımlayıcısının (E-posta adresi) nerede bulunduğunu gösteren değeri `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`yazın - örneğin.
  
    f. Sağlayıcı **konumunu belirleyin** metin kutusuna, Azure portal giriş ekranınızdan yüklenen simgenizi tıkladıklarında kullanıcıların nereye gönderildiğini gösteren değeri yazın.
  
    g. **URL'yi Oturumu Aç** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    h. **Parmak izlerini yönet'i**tıklatın ve ardından indirdiğiniz sertifikanın parmak baskısını yükleyin.

1. **Kullanıcı Ayarları'nı**tıklatın ve ardından aşağıdaki adımları gerçekleştirin:

     ![Tek İşaret-On'u Yapılandır](./media/learnupon-tutorial/tutorial_learnupon_11.png)  

    a. Ad **Tanımlayıcı Biçim** metin kutusunda, SAML İddianızda kullanıcıların ilk adının nerede bulunduğunu söyleyen değeri yazın - `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`örneğin: .
  
    b. Soyadı **Tanımlayıcı Biçim** metin kutusunda, SAML İddianızda kullanıcıların soyadının nerede olduğunu söyleyen değeri yazın - örneğin: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

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

Bu bölümde, Britta Simon'ın LearnUpon'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **LearnUpon'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **LearnUpon'u**seçin.

    ![Uygulamalar listesindeki LearnUpon bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-learnupon-test-user"></a>LearnUpon test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı LearnUpon oluşturulur. LearnUpon, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. LearnUpon'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur. Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [LearnUpon destek ekibine](https://www.learnupon.com/features/support/)başvurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LearnUpon döşemesini tıklattığınızda, SSO'yu kurduğunuz LearnUpon'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)