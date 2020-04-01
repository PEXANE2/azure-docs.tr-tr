---
title: 'Öğretici: OneTrust Gizlilik Yönetimi Yazılımı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve OneTrust Gizlilik Yönetimi Yazılımı arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 71c2b6d0-3d28-4130-a2c8-1e72ab3d5814
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: jeedes
ms.openlocfilehash: 5fbec8b6a1a21826896f8e2499b1e8b7237d0ff9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67095889"
---
# <a name="tutorial-azure-active-directory-integration-with-onetrust-privacy-management-software"></a>Öğretici: OneTrust Gizlilik Yönetimi Yazılımı ile Azure Active Directory entegrasyonu

Bu eğitimde, OneTrust Gizlilik Yönetimi Yazılımlarını Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
OneTrust Gizlilik Yönetimi Yazılımını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* OneTrust Gizlilik Yönetimi Yazılımına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla OneTrust Gizlilik Yönetimi Yazılımında (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini OneTrust Gizlilik Yönetimi Yazılımı ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* OneTrust Gizlilik Yönetimi Yazılımı tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* OneTrust Gizlilik Yönetimi Yazılımı **SP** ve **IDP** sso başlatılan destekler

* OneTrust Gizlilik Yönetimi Yazılımı **Just In Time** kullanıcı sağlama destekler

## <a name="adding-onetrust-privacy-management-software-from-the-gallery"></a>Galeriden OneTrust Gizlilik Yönetimi Yazılımı Ekleme

OneTrust Gizlilik Yönetimi Yazılımının Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize OneTrust Gizlilik Yönetimi Yazılımı eklemeniz gerekir.

**Galeriden OneTrust Gizlilik Yönetimi Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **OneTrust Gizlilik Yönetimi Yazılımı**yazın, sonuç panelinden **OneTrust Gizlilik Yönetimi Yazılımı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde OneTrust Gizlilik Yönetimi Yazılımı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak OneTrust Gizlilik Yönetimi Yazılımı ile Azure AD tek oturum açma işlemini yapılandırın ve test edersiniz.
Tek oturum açma nın işe yaraması için, OneTrust Gizlilik Yönetimi Yazılımı'ndaki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

OneTrust Gizlilik Yönetimi Yazılımı ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. OneTrust Gizlilik Yönetimi Yazılımı Tek Oturum Açma -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-onetrust-privacy-management-software-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[OneTrust Gizlilik Yönetimi Yazılımı test kullanıcısını oluşturun](#create-onetrust-privacy-management-software-test-user)** - kullanıcının Azure AD temsiline bağlı OneTrust Gizlilik Yönetimi Yazılımı'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

OneTrust Gizlilik Yönetimi Yazılımı ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **OneTrust Gizlilik Yönetimi Yazılımı** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![OneTrust Gizlilik Yönetimi Yazılım Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://www.onetrust.com/saml2`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.onetrust.com/auth/consumerservice`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![OneTrust Gizlilik Yönetimi Yazılım Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.onetrust.com/auth/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [OneTrust Gizlilik Yönetimi Yazılımı İstemci destek ekibine](mailto:support@onetrust.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **OneTrust Gizlilik Yönetimi Yazılımı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-onetrust-privacy-management-software-single-sign-on"></a>OneTrust Gizlilik Yönetimi Yazılımı Tek Oturum Aç'ı Yapılandır

**OneTrust Gizlilik Yönetimi Yazılımı** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'ini** ve uygun kopyalanmış URL'leri Azure portalından [OneTrust Gizlilik Yönetimi Yazılımı destek ekibine](mailto:support@onetrust.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın OneTrust Gizlilik Yönetimi Yazılımına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **OneTrust Gizlilik Yönetimi Yazılımı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **OneTrust Gizlilik Yönetimi Yazılımı'nı**seçin.

    ![Uygulamalar listesindeki OneTrust Gizlilik Yönetimi Yazılımı bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-onetrust-privacy-management-software-test-user"></a>OneTrust Gizlilik Yönetimi Yazılımı test kullanıcısı oluşturun

Bu bölümde, Britta Simon adlı bir kullanıcı OneTrust Gizlilik Yönetimi Yazılımı oluşturulur. OneTrust Gizlilik Yönetimi Yazılımı, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. OneTrust Gizlilik Yönetimi Yazılımı'nda bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [OneTrust Gizlilik Yönetimi Yazılımı destek ekibine](mailto:support@onetrust.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki OneTrust Gizlilik Yönetimi Yazılımı döşemesini tıklattığınızda, SSO'yu kurduğunuz OneTrust Gizlilik Yönetimi Yazılımı'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

