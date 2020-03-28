---
title: 'Öğretici: Mozy Enterprise ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Mozy Enterprise arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 7d3b7f8982a24856818cbb6ce86060beac969fc2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233504"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Öğretici: Mozy Enterprise ile Azure Active Directory entegrasyonu

Bu eğitimde, Mozy Enterprise'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Mozy Enterprise'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Mozy Enterprise erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Mozy Enterprise'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Mozy Enterprise ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Mozy Enterprise tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Mozy Enterprise **SP** başlatılan SSO destekler

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Galeriden Mozy Enterprise ekleme

Mozy Enterprise'ın Azure AD'ye entegrasyonunu yapılandırmak için Galeriden Mozy Enterprise'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Mozy Enterprise eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Mozy Enterprise**yazın, sonuç panelinden **Mozy Enterprise'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesinde Mozy Enterprise](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırır ve test esunarsınız.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Mozy Enterprise'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Mozy Enterprise ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Mozy Enterprise Tek Oturum](#configure-mozy-enterprise-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Mozy Enterprise test kullanıcısını oluşturun](#create-mozy-enterprise-test-user)** - Kullanıcının Azure AD gösterimine bağlı Mozy Enterprise'da Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Mozy Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Mozy Enterprise** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Mozy Enterprise Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.Mozyenterprise.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Mozy Enterprise Client destek ekibine](https://support.mozy.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Mozy Enterprise'ı Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-mozy-enterprise-single-sign-on"></a>Yapılandırılan Mozy Enterprise Tek Sign-On

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Mozy Enterprise şirket sitenize giriş yapın.

2. **Yapılandırma** bölümünde Kimlik **Doğrulama İlkesi'ni**tıklatın.
   
    ![Kimlik doğrulama ilkesi](./media/mozy-enterprise-tutorial/ic777314.png "Kimlik doğrulama ilkesi")

3. Kimlik **Doğrulama İlkesi** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama ilkesi](./media/mozy-enterprise-tutorial/ic777315.png "Kimlik doğrulama ilkesi")
   
    a. **Sağlayıcı**Olarak **Dizin Hizmeti'ni** seçin.
   
    b. **LDAP Push'u kullan'ı**seçin.
   
    c. **SAML Kimlik Doğrulama** sekmesini tıklatın.
   
    d. Azure portalından kopyaladığınız **Giriş URL'sini**Kimlik **Doğrulama URL** metin kutusuna yapıştırın.
   
    e. Azure portalından KOPYALADIĞINIZ **Azure AD Tanımlayıcısını** **SAML Endpoint** textbox'a yapıştırın.
   
    f. İndirilen base-64 kodlu sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından tüm Sertifikayı **SAML Certificate** textbox'a yapıştırın.
   
    g. **Yöneticilerin ağ kimlik bilgileriyle oturum açması için SSO'ya etkin**olun'u seçin.
   
    h. **Değişiklikleri Kaydet**’e tıklayın.

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

Bu bölümde, Britta Simon'ın Mozy Enterprise'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Mozy Enterprise'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mozy Enterprise'ı**seçin.

    ![Uygulamalar listesindeki Mozy Enterprise bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-mozy-enterprise-test-user"></a>Mozy Enterprise test kullanıcısı oluşturma

Azure AD kullanıcılarının Mozy Enterprise'a giriş yapabilmeleri için, bu kullanıcıların Mozy Enterprise'a dahil edilmesi gerekir. Mozy Enterprise durumunda, sağlama manuel bir görevdir.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Mozy Enterprise tarafından sağlanan diğer Mozy Enterprise kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Mozy Enterprise** kiracınıza giriş yapın.

2. **Kullanıcılar'ı**tıklatın ve ardından **Yeni Kullanıcı Ekle'yi**tıklatın.
   
    ![Kullanıcılar](./media/mozy-enterprise-tutorial/ic777317.png "Kullanıcılar")
   
    >[!NOTE]
    >**Yeni Kullanıcı Ekle** seçeneği yalnızca **Mozy** Kimlik **Doğrulama ilkesi**altında sağlayıcı olarak seçilirse görüntülenir. SAML Kimlik Doğrulaması yapılandırılırsa, kullanıcılar tek oturum açma yoluyla ilk girişlerine otomatik olarak eklenir.
    
3. Yeni kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı Ekleme](./media/mozy-enterprise-tutorial/ic777318.png "Kullanıcı Ekle")
   
    a. **Grup** seç listesinden bir grup seçin.
   
    b. Hangi kullanıcı türü **listesinden** bir tür seçin.
   
    c. Kullanıcı **adı** metin kutusuna Azure AD kullanıcısının adını yazın.
   
    d. **E-posta** metin kutusuna Azure AD kullanıcısının e-posta adresini yazın.
   
    e. **Kullanıcı talimatı e-postası gönder'i**seçin.
   
    f. **Kullanıcı Ekle'yi**tıklatın.

     >[!NOTE]
     > Kullanıcıyı oluşturduktan sonra, azure REKLAM kullanıcısına, hesabı etkin hale gelmeden önce onaylamak için bir bağlantı içeren bir e-posta gönderilir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Mozy Enterprise döşemesini tıklattığınızda, SSO'yu kurduğunuz Mozy Enterprise'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

