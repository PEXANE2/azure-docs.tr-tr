---
title: 'Öğretici: Algı Amerika Birleşik Devletleri (Non-UltiPro) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Perception Amerika Birleşik Devletleri (Non-UltiPro) arasında tek oturum açma işlemlerini nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: e9ba42f780c93486409077383750d0635637e99b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094847"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Öğretici: Algı Amerika Birleşik Devletleri ile Azure Active Directory entegrasyonu (Non-UltiPro)

Bu eğitimde, Algı Amerika Birleşik Devletleri'ni (UltiPro Olmayan) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Algı Amerika Birleşik Devletleri'ni (UltiPro Olmayan) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Algı Amerika Birleşik Devletleri 'ne (Non-UltiPro) erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Algı Amerika Birleşik Devletleri'nde (Non-UltiPro) (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Algı Amerika Birleşik Devletleri (Non-UltiPro) ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Algı Amerika Birleşik Devletleri (Non-UltiPro) tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Algı Amerika Birleşik Devletleri (Non-UltiPro) **IDP** sso başlatılan destekler

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Galeriden Algı Amerika Birleşik Devletleri (Non-UltiPro) ekleme

Algılama Amerika Birleşik Devletleri'nin (Non-UltiPro) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Algılama Amerika Birleşik Devletleri 'ni (Non-UltiPro) eklemeniz gerekir.

**Galeriden Perception United States (Non-UltiPro) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Algı Amerika Birleşik Devletleri (Non-UltiPro)** yazın, sonuç panelinden Perception United **(Non-UltiPro)** seçeneğini seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Algı Amerika Birleşik Devletleri (Non-UltiPro) sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Azure AD tek oturum açma işlemini Algı Amerika Birleşik Devletleri (Non-UltiPro) ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Perception United States'teki (Non-UltiPro) ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi Algılama Amerika Birleşik Devletleri (Non-UltiPro) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Yapılandırılan Algı Amerika Birleşik Devletleri (Non-UltiPro) Tek Oturum Açma](#configure-perception-united-states-non-ultipro-single-sign-on)** - uygulama tarafında Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Algı Amerika Birleşik Devletleri (Non-UltiPro) test kullanıcısı oluşturun](#create-perception-united-states-non-ultipro-test-user)** - Kullanıcının Azure AD gösterimine bağlı Algı Amerika Birleşik Devletleri'nde (Non-UltiPro) Britta Simon'ın bir muadili olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Algı Amerika Birleşik Devletleri (Non-UltiPro) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Algı Amerika Birleşik Devletleri (Non-UltiPro)** uygulama tümleştirme **sayfasında, Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![Algı Amerika Birleşik Devletleri (Non-UltiPro) Etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://perception.kanjoya.com/sp`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://perception.kanjoya.com/sso?idp=<entity_id>`

    c. **Algı Amerika Birleşik Devletleri (Non-UltiPro)** uygulaması, <entity_id> olarak Azure AD **Tanımlayıcı** değerini gerektirir ve bu değer, Algı Amerika Birleşik **Devletleri (Non-UltiPro)** bölümünden uri kodlanması nı alır. Uri kodlanmış değeri elde etmek için aşağıdaki **http://www.url-encode-decode.com/** bağlantıyı kullanın: .

    d. Uri kodlanmış değeri aldıktan sonra aşağıda belirtildiği gibi **Yanıt URL** ile birleştirin-

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    e. Yukarıdaki değeri **Yanıtla URL** metin kutusuna yapıştırın.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Algı **Amerika Birleşik Devletleri (Non-UltiPro)** bölümünde, gereksiniminize göre uygun URL(ler) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Yapılandırılan Algı Amerika Birleşik Devletleri (Non-UltiPro) Tek Sign-On

1. Başka bir tarayıcı penceresinde, bir yönetici olarak Algı Amerika Birleşik Devletleri (Non-UltiPro) şirket sitenize oturum açın.

2. Ana araç çubuğunda **Hesap Ayarları'nı**tıklatın.

    ![Algı Amerika Birleşik Devletleri (Non-UltiPro) kullanıcı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. Hesap **Ayarları** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Algı Amerika Birleşik Devletleri (Non-UltiPro) kullanıcı](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. Şirket **Adı** metin kutusuna, **Şirketin**adını yazın.
    
    b. Hesap **Adı** metin kutusuna, **Hesabın**adını yazın.

    c. **Varsayılan Yanıt-E-posta metin** kutusuna, geçerli **E-posta**yazın.

    d. **SAML 2.0**olarak **SSO Kimlik Sağlayıcısı'nı** seçin.

4. **SSO Yapılandırma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Algı Amerika Birleşik Devletleri (Non-UltiPro) SSOConfig](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. **E-POSTA**olarak **SAML NameID Türünü** seçin.

    b. **SSO Configuration Name** textbox'ına **Yapılandırmanızın**adını yazın.
    
    c. **Kimlik Sağlayıcı Adı** metin kutusuna, Azure portalından kopyaladığınız Azure AD **Tanımlayıcısı'nın**değerini yapıştırın. 

    d. **SAML Domain textbox,** gibi @contoso.cometki alanı girin .

    e. **Metadata XML** dosyasını yüklemek için **Yeniden Yükle'yi** tıklatın.

    f. **Güncelleştir**’e tıklayın.

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

Bu bölümde, Britta Simon'ın Algı Amerika Birleşik Devletleri'ne (Non-UltiPro) erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında Kurumsal **Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından Algı Amerika Birleşik **Devletleri'ni (Non-UltiPro)** seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, Algı **Amerika Birleşik Devletleri (Non-UltiPro)** seçin.

    ![Uygulamalar listesinde Algı Amerika Birleşik Devletleri (Non-UltiPro) bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Algı Amerika Birleşik Devletleri (Non-UltiPro) test kullanıcısı oluşturun

Bu bölümde, Algı Amerika Birleşik Devletleri (Non-UltiPro) Britta Simon adlı bir kullanıcı oluşturun. Perception [Amerika Birleşik Devletleri (Non-UltiPro) destek ekibi](https://www.ultimatesoftware.com/Contact/ContactUs) ile perception amerika birleşik devletleri (Non-UltiPro) platformunda kullanıcıları eklemek için çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Algı Amerika Birleşik Devletleri (Non-UltiPro) döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Algı Amerika Birleşik Devletleri'nde (Non-UltiPro) oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

