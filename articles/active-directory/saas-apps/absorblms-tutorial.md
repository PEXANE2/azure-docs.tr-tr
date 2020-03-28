---
title: 'Öğretici: Absorb LMS ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Absorb LMS arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 936de76d1117c56f5a9dec48b51f33b9afa15351
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67107511"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: Absorb LMS ile Azure Active Directory entegrasyonu

Bu eğitimde, Absorb LMS'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Absorb LMS'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Emer LMS erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla LMS'yi (Tek Oturum Açma) Absorbe etmek için otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Absorb LMS ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* LMS tek oturum açma özellikli aboneliği emer

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Absorb LMS **IDP'nin** başlattığı SSO'ya destek veriyor

## <a name="adding-absorb-lms-from-the-gallery"></a>Galeriden Absorb LMS ekleme

Absorb LMS'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Absorb LMS eklemeniz gerekir.

**Galeriden Absorb LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **LmS'i Absorbe**yazın, sonuç panelinden **LMS'yi emer'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesindeLMS'yi absorbe edin](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Absorb LMS ile yapılandırıp test emebilirsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Absorb LMS'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Absorb LMS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[LMS Tek Oturum](#configure-absorb-lms-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Absorb LMS test kullanıcısını oluşturun](#create-absorb-lms-test-user)** - Kullanıcının Azure AD gösterimine bağlı Absorb LMS'deki Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Absorb LMS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Emer LMS** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** düğmesini tıklatın.

    ![LMS Etki Alanı ve URL'leri tek oturum açma bilgilerini emer](common/idp-intiated.png)

    Absorb 5 **kullanıyorsanız - UI** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.myabsorb.com/account/saml`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.myabsorb.com/account/saml`

    Absorb 5 **kullanıyorsanız - Yeni Öğrenci Deneyimi** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.myabsorb.com/api/rest/v2/authentication/saml`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.myabsorb.com/api/rest/v2/authentication/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Absorb LMS İstemci destek ekibine](https://support.absorblms.com/hc/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi.

    ![image](common/edit-attribute.png)

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **LMS'yi Hazm'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-absorb-lms-single-sign-on"></a>Configure Absorb LMS Tek İşaret-On

1. Yeni bir web tarayıcısı penceresinde, yönetici olarak Absorb LMS şirket sitenizde oturum açın.

2. Sağ üstteki **Hesap** düğmesini seçin.

    ![Hesap düğmesi](./media/absorblms-tutorial/1.png)

3. Hesap bölmesinde Portal **Ayarları'nı**seçin.

    ![Portal Ayarları bağlantısı](./media/absorblms-tutorial/2.png)

4. **SSO Ayarlarını Yönet** sekmesini seçin.

    ![Kullanıcılar sekmesi](./media/absorblms-tutorial/managesso.png)

5. Tek **Oturum Açma Ayarlarını Yönet** sayfasında aşağıdakileri yapın:

    ![Tek oturum açma yapılandırma sayfası](./media/absorblms-tutorial/settings.png)

    a. **Ad** metin kutusuna Azure AD Marketplace SSO gibi adı girin.

    b. **Yöntem**olarak **SAML'yi** seçin.

    c. Not Defteri'nde, Azure portalından indirdiğiniz sertifikayı açın. **---BEGIN SERTIFIKAsI---** ve **---SON BELGEsI---** etiketlerini kaldırın. Ardından, **Anahtar** kutusuna kalan içeriği yapıştırın.

    d. **Mod** kutusunda, **Başlatılan Kimlik Sağlayıcı'yı**seçin.

    e. Id **Özelliği** kutusunda, Azure AD'de kullanıcı tanımlayıcısı olarak yapılandırdığınız özniteliği seçin. Örneğin, Azure AD'de *nameidentifier seçiliyse,* **Kullanıcı Adı'nı**seçin.

    f. **İmza Türü**olarak **Sha256'yı** seçin.

    g. Giriş **URL** kutusuna, Uygulamanın **Azure** portalının Özellikler sayfasından Kullanıcı **Erişimi URL'sini** yapıştırın.

    h. Oturum **Açma**URL'sinde, Azure portalının **Yapılandırma oturum** açma penceresinden kopyaladığınız Oturum **Açma URL** değerini yapıştırın.

    i. Otomatik **Olarak** **Açık'a**Yönlendirin.

6. **Kaydet'i seçin.**

    ![Tek İzin SSO Giriş geçiş](./media/absorblms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon\@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Em'l-EkSS'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **LMS'yi Emer'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **LMS'yi Absorbe**yazın ve seçin.

    ![Uygulamalar listesindeki Absorb LMS bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-absorb-lms-test-user"></a>Absorb LMS test kullanıcısını oluşturun

Azure AD kullanıcılarının LmS'i Absorbe'de oturum açabilmesi için, EmIle LMS'de ayarlanması gerekir. Absorb LMS durumunda, sağlama manuel bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Absorb LMS şirket sitenizde oturum açın.

2. **Kullanıcılar** **bölmesinde, Kullanıcılar'ı**seçin.

    ![Kullanıcılar bağlantısı](./media/absorblms-tutorial/absorblms_userssub.png)

3. **Kullanıcı** sekmesini seçin.

    ![Yeni açılır liste ekle](./media/absorblms-tutorial/absorblms_createuser.png)

4. Kullanıcı **Ekle** sayfasında aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/absorblms-tutorial/user.png)

    a. Ad **kutusuna,** **Britta**gibi ilk adı yazın.

    b. **Soyadı** kutusuna, **Simon**gibi soyadını yazın.

    c. Kullanıcı **adı** **kutusuna Britta Simon**gibi tam bir ad yazın.

    d. **Parola** kutusuna kullanıcı parolasını yazın.

    e. **Parolayı Onayla** kutusuna, parolayı yeniden yazın.

    f. **Etkin** geçişini **Active**olarak ayarlayın.

5. **Kaydet'i seçin.**

    ![Tek İzin SSO Giriş geçiş](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > Varsayılan olarak, SSO'da Kullanıcı Sağlama etkinleştirmez. Müşteri bu özelliği etkinleştirmek istiyorsa, [bu](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) belgede belirtildiği gibi ayarlamaları gerekir. Ayrıca, Kullanıcı Provisioing'in yalnızca **Absorb 5 - ACS** URL ile Yeni Öğrenci Deneyimi-`https://company.myabsorb.com/api/rest/v2/authentication/saml`

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Emer LMS döşemesini tıklattığınızda, SSO'yu kurduğunuz Absorb LMS'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
