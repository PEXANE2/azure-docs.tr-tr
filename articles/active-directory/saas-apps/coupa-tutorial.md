---
title: 'Öğretici: Coupa ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Coupa arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 709f3a5b66db660ade482660cd9b3930ff0b7141
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227641"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Öğretici: Coupa ile Azure Active Directory entegrasyonu

Bu eğitimde, Coupa'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Coupa'yı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Coupa erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Coupa'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Coupa ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Coupa tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Coupa **SP** başlatılan SSO destekler

## <a name="adding-coupa-from-the-gallery"></a>Galeriden Coupa ekleme

Coupa'nın Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Coupa eklemeniz gerekir.

**Galeriden Coupa eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda Coupa yazın, sonuç panelinden **Coupa'yı**seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın. **Coupa**

     ![Sonuç listesinde Coupa](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Coupa'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Coupa ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Coupa Single Sign-On'u yapılandırın.](#configure-coupa-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Coupa test kullanıcıoluşturun](#create-coupa-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Coupa Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Coupa ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Coupa** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Coupa Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.coupahost.com`

    > [!NOTE]
    > Oturum açma URL değeri gerçek değildir. Bu değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değeri almak için [Coupa Client destek ekibine](https://success.coupa.com/Support/Contact_Us?) başvurun.

    b. **Tanımlayıcı** kutusuna bir URL yazın:

    | Ortam  | URL'si |
    |:-------------|----|
    | Korumalı alan | `sso-stg1.coupahost.com`|
    | Üretim | `sso-prd1.coupahost.com`|
    | | |

    c. **Yanıtla URL** metin kutusuna bir URL yazın:

    | Ortam | URL'si |
    |------------- |----|
    | Korumalı alan | `https://sso-stg1.coupahost.com/sp/ACS.saml2`|
    | Üretim | `https://sso-prd1.coupahost.com/sp/ACS.saml2`|
    | | |

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Coupa'yı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-coupa-single-sign-on"></a>Coupa Tek İşaret-On yapıla

1. Coupa şirket sitenizde yönetici olarak oturum açın.

2. **Kurulum \> Güvenlik Denetimi'ne**gidin.

    ![Güvenlik Denetimleri](./media/coupa-tutorial/ic791900.png "Güvenlik Denetimleri")

3. **Coupa kimlik bilgilerini kullanarak giriş** yapın bölümünde aşağıdaki adımları gerçekleştirin:

    ![Coupa SP meta verileri](./media/coupa-tutorial/ic791901.png "Coupa SP meta verileri")

    a. **SAML kullanarak Giriş Yap'ı**seçin.

    b. Azure portalından indirilen meta verileri yüklemek için **Gözat'ı** tıklatın.

    c. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Coupa'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Coupa'yı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Coupa'yı**seçin.

    ![Uygulamalar listesindecoupa bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-coupa-test-user"></a>Coupa test kullanıcısı oluşturma

Azure AD kullanıcılarının Coupa'ya giriş yapabilmeleri için Coupa'ya dahil edilmeleri gerekir.  

* Coupa durumunda, sağlama manuel bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **Coupa** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **Kurulum'u**tıklatın ve ardından **Kullanıcılar'ı**tıklatın.

    ![Kullanıcılar](./media/coupa-tutorial/ic791908.png "Kullanıcılar")

3. **Oluştur'u**tıklatın.

    ![Kullanıcı Oluşturma](./media/coupa-tutorial/ic791909.png "Kullanıcı Oluşturma")

4. Kullanıcı **Oluştur** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Detayları](./media/coupa-tutorial/ic791910.png "Kullanıcı Detayları")

    a. **İlgili**textbox'lara sağlamak istediğiniz geçerli bir Azure Active Directory hesabının **Giriş, Ad,** **Soyadı,** **Tek Oturum Açma Kimliği,** **E-posta** özniteliklerini yazın.

    b. **Oluştur'u**tıklatın.

    >[!NOTE]
    >Azure Active Directory hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bağlantı içeren bir e-posta alır.
    >

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Coupa tarafından sağlanan diğer Coupa kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Coupa karosu tıklattığınızda, SSO'yu kurduğunuz Coupa'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

