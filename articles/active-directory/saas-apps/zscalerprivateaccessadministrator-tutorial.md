---
title: 'Öğretici: Zscaler Private Access Administrator ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Private Access Administrator arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7f0bc04bb8e7358f5bdb2e4dfc3db198e24303f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67085637"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Öğretici: Zscaler Private Access Administrator ile Azure Active Directory entegrasyonu

Bu eğitimde, Zscaler Özel Erişim Yöneticisi'ni Azure Etkin Dizin (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zscaler Private Access Administrator'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zscaler Private Access Administrator erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler Özel Erişim Yöneticisi 'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zscaler Private Access Administrator ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Zscaler Private Access Administrator tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zscaler Özel Erişim Yöneticisi **SP** ve **IDP** sso başlatılan destekler

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Galeriden Zscaler Özel Erişim Yöneticisi ekleme

Zscaler Private Access Administrator'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler Private Access Administrator'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zscaler Private Access Administrator eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Zscaler Private Access Administrator**yazın, sonuç panelinden **Zscaler Private Access Administrator'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Zscaler Özel Erişim Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Zscaler Private Access Administrator ile Azure AD oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile Zscaler Private Access Administrator'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Zscaler Private Access Administrator ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zscaler Private Access Administrator Tek Oturum Açma](#configure-zscaler-private-access-administrator-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zscaler Private Access Administrator test kullanıcısını oluşturun](#create-zscaler-private-access-administrator-test-user)** - Zscaler Private Access Administrator'da Britta Simon'ın, kullanıcının Azure AD temsiline bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Zscaler Private Access Administrator ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Zscaler Private Access Administrator** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Zscaler Özel Erişim Yöneticisi Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.private.zscaler.com/auth/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.private.zscaler.com/auth/sso`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna bir URL yazın:`idpadminsso`

5.  Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:

    ![Zscaler Özel Erişim Yöneticisi Etki Alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.private.zscaler.com/auth/sso`   

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Zscaler Private Access Administrator Client destek ekibine](https://help.zscaler.com/zpa-submit-ticket) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Zscaler Private Access Administrator'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zscaler-private-access-administrator-single-sign-on"></a>Zscaler Özel Erişim Yöneticisi Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Zscaler Private Access Administrator'a imzalayın.

2. Üstte **İdare'yi** tıklatın ve **Kimlik Doğrulama** bölümüne gidin **IdP Yapılandırması'nı**tıklatın.

    ![Zscaler Özel Erişim Yöneticisi admin](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Sağ üst **köşede, IdP Yapılandırması Ekle'yi**tıklatın. 

    ![Zscaler Özel Erişim Yöneticisi addidp](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

4. **IdP Yapılandırma ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler Özel Erişim Yöneticisi idpselect](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. **IdP Meta data File Upload** alanında Azure AD'den indirilen Meta veri dosyasını yüklemek için **Dosyayı Seç'i** tıklatın.

    b. Azure AD'deki **IdP meta verilerini** okur ve aşağıda gösterildiği gibi tüm alan bilgilerini doldurur.

    ![Zscaler Özel Erişim Yöneticisi idpconfig](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. **Yönetici**olarak **Tek Oturum Açma'yı** seçin.

    d. **Etki Alanları** alanından etki alanınızı seçin.
    
    e. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Zscaler Private Access Administrator'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Zscaler Private Access Administrator'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Private Access Administrator'ı**seçin.

    ![Uygulamalar listesindeki Zscaler Özel Erişim Yöneticisi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-zscaler-private-access-administrator-test-user"></a>Zscaler Özel Erişim Yöneticisi test kullanıcısı oluşturma

Azure AD kullanıcılarının Zscaler Private Access Administrator'da oturum açabilmeleri için Zscaler Private Access Administrator'a dahil edilmeleri gerekir. Zscaler Private Access Administrator durumunda, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Zscaler Private Access Administrator şirket sitenizde yönetici olarak oturum açın.

2. Üstte **İdare'yi** tıklatın ve **Kimlik Doğrulama** bölümüne gidin **IdP Yapılandırması'nı**tıklatın.

    ![Zscaler Özel Erişim Yöneticisi admin](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Menünün sol tarafından **Yöneticiler'i** tıklatın.

    ![Zscaler Özel Erişim Yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Sağ üst köşede **Yönetici Ekle'yi**tıklatın:

    ![Zscaler Özel Erişim Yöneticisi yönetici eklemek](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. Yönetici **Ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Zscaler Özel Erişim Yöneticisi kullanıcı yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. Kullanıcı **adı** metin kutusuna, kullanıcının BrittaSimon@contoso.come-postasını girin.

    b. **Parola** metin kutusuna Parola'yı yazın.

    c. **Parolayı Onayla** metin kutusuna Parola'yı yazın.

    d. **Zscaler Özel Erişim Yöneticisi** **Rolü'nü** seçin.

    e. **E-posta** metin kutusuna, kullanıcının BrittaSimon@contoso.come-postasını girin.

    f. **Telefon** metin kutusuna Telefon numarasını yazın.

    g. Saat **Dilimi** metin kutusunda, Saat Dilimi'ni seçin.

    h. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Zscaler Private Access Administrator döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler Private Access Administrator'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

