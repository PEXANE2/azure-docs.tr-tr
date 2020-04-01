---
title: 'Öğretici: Çalışılabilir ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizini ile Uygulanabilir arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c025cdeb-786c-4aab-abd1-132907007f7e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fc1f6434bea8f75c7bcc7859c6f90d7abdd031b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71091031"
---
# <a name="tutorial-azure-active-directory-integration-with-workable"></a>Öğretici: Çalışılabilir ile Azure Active Directory entegrasyonu

Bu eğitimde, İşlenebilir'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Azure AD ile Uygulanabilir'i tümleştirmek size aşağıdaki avantajları sağlar:

* Çalışılabilir'e erişimi olan Azure AD'da denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak Çalışılabilir (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini İşlenebilir ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Uygulanabilir tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Uygulanabilir destekler **SP ve IDP** SSO başlatılan
* Uygulanabilir destekler **Just In Time** kullanıcı sağlama

## <a name="adding-workable-from-the-gallery"></a>Galeriden İşlenebilir Ekleme

İşlenebilir'in Azure AD'ye tümleştirilmesi ni yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize İşlenebilir'i eklemeniz gerekir.

**Galeriden İşlenebilir'i eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama **kutusunda, İşlenebilir**yazın , sonuç panelinden **İşlenebilir'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde uygulanabilir](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre İşlenebilir ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure AD kullanıcısı ile Uygulanabilir'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumaçmayı İşlenebilir ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Uygulanabilir Tek Oturum Açma'yı yapılandırır](#configure-workable-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[İşlenebilir test kullanıcısı oluşturun](#create-workable-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olarak Uygulanabilir'de Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini İşlenebilir ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Uygulanabilir** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Uygulanabilir Etki Alanı ve URL'ler tek oturum açma bilgileri](common/both-replyurl.png)

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.workable.com/auth/saml/<SUBDOMAIN>/callback`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarlayın** ve aşağıdaki adımı gerçekleştirin:

    ![Uygulanabilir Etki Alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://www.workable.com/sso/signin`

    > [!NOTE]
    > YanıtURL değeri gerçek değil. Yanıtla URL değerini gerçek YanıtURL'i ile güncelleştirin. Değeri almak için [Uygulanabilir İstemci destek ekibine](mailto:support@workable.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Ayarlanabilir'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-workable-single-sign-on"></a>İşlenebilir Tek İşaret-On'u Yapılandır

SSO'yu Uygulanabilir'de etkinleştirmek için, ilgili İşlenebilir hesap yöneticinize başvurun ve onlara aşağıdaki öğeleri sağlayın.

1. Oturum Açma URL’si

2. Sertifika dosyası

3. Giriş URL'si

Tek Oturum Açma etkinleştirildikten sonra, İşlenebilir hesap yöneticiniz size haber verecektir ve İşlenebilir hesap alt etki alanınızı kullanarak oturum açabilmek için [Workable'ın SSO sayfasını](https://help.workable.com/hc/en-us/articles/360000067753-Single-Sign-on-SSO-Overview-Pro) kullanabilirsiniz.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon\@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com.

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Çalışılabilir'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Uygulanabilir'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, İşlenebilir'i**seçin.

    ![Uygulamalar listesindeki Uygulanabilir bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-workable-test-user"></a>İşlenebilir test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Workable oluşturulur. Uygulanabilir, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Çalışılabilir'de zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki İşlenebilir döşemeyi tıklattığınızda, SSO'yu kurduğunuz İşlenebilir'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
