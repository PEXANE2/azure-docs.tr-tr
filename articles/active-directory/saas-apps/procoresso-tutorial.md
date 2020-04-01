---
title: 'Öğretici: Procore SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Procore SSO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca6863a6b02e867afd732ce1662136051b8afec8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67093673"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Öğretici: Procore SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Procore SSO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Procore SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Procore SSO erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Procore SSO'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Procore SSO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Procore SSO tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Procore **SSO, IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-procore-sso-from-the-gallery"></a>Galeriden Procore SSO ekleme

Procore SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Procore SSO eklemeniz gerekir.

**Galeriden Procore SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Procore SSO**yazın, sonuç panelinden **Procore SSO'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Procore SSO sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Procore SSO ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Procore SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Procore SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Procore SSO Tek Oturum Açma](#configure-procore-sso-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Procore SSO test kullanıcısını oluşturun](#create-procore-sso-test-user)** - Kullanıcının Azure AD gösterimine bağlı Procore SSO'daki Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Procore SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Procore SSO** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![Procore SSO Etki Alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Procore SSO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-procore-sso-single-sign-on"></a>Procore SSO Tek İşaret-On yapıla

1. **Procore SSO** tarafında tek oturum açma yapılandırmak için, procore şirket sitenizde yönetici olarak oturum açın.

2. Araç kutusu açılır ken, SSO ayarları sayfasını açmak için **Admin'e** tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/procore_tool_admin.png)

3. Aşağıda açıklandığı gibi kutulardaki değerleri yapıştırın-

    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. Tek **İşaret Veren URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı** değerini yapıştırın.

    b. **HEDEFTE SAML İşareti URL** kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    c. Azure portalından yukarıda indirilen **Federasyon Metadata XML'i** açın ve sertifikayı **X509Certificate**adlı etikette kopyalayın. Kopyalanan değeri **x509 Sertifika** kutusuna yapıştırın.

4. **Değişiklikleri Kaydet**’e tıklayın.

5. Bu ayarlardan sonra, [Procore Destek ekibine](https://support.procore.com/) oturum açtığınız **alan adını** (örn. **contoso.com)** göndermeniz gerekir ve bu alan adı için federe SSO'yu etkinleştirirler.

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

Bu bölümde, Britta Simon'ın Procore SSO'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Procore SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Procore SSO'yu**seçin.

    ![Uygulamalar listesindeki Procore SSO bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-procore-sso-test-user"></a>Procore SSO test kullanıcısı oluşturma

Procore SSO tarafında bir Procore test kullanıcısı oluşturmak için lütfen aşağıdaki adımları izleyin.

1. Procore şirket sitenizde yönetici olarak oturum açın.    

2. Araç kutusu açılır, şirket dizin sayfasını açmak için **Dizin'e** tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Formu açmak ve aşağıdaki seçenekleri gerçekleştirmek için **Kişi Ekle** seçeneğini tıklatın -

    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/Procore_user_add.png)

    a. Ad **metin** kutusunda, **Britta**gibi kullanıcının ilk adını yazın.

    b. **Soyadı** metin kutusunda, **kullanıcının**soyadını Simon gibi yazın.

    c. **E-posta Adresi** metin kutusuna, kullanıcının BrittaSimon@contoso.come-posta adresini 'yi ' gibi yazın.

    d. **İzin Şablonu** Uygula Olarak İzin **Şablonu'nun Daha Sonra'ı**seçin.

    e. **Oluştur'u**tıklatın.

4. Yeni eklenen ilgili kişi için ayrıntıları kontrol edin ve güncelleyin.

    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/Procore_user_check.png)

5. Kullanıcı kaydını tamamlamak için **Daveti Kaydet ve Gönder'e** (posta yoluyla davet gerekiyorsa) veya **Kaydet** 'e (Doğrudan Kaydet) tıklayın.
    
    ![Tek İşaret-On'u Yapılandır](./media/procoresso-tutorial/Procore_user_save.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Procore SSO döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Procore SSO'da oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

