---
title: 'Öğretici: JIRA için Kantega SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: JIRA için Azure Active Directory ve Kantega SSO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e97a66a1b21b46649a1206eb522c180e84a0a0d4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67099165"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Öğretici: JIRA için Kantega SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Jira için Kantega SSO'yu Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Jira için Kantega SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* JiRA için Kantega SSO erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla JIRA (Tek Oturum Açma) için Kantega SSO'da otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini JIRA için Kantega SSO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* JIRA tek oturum açma özellikli abonelik için Kantega SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* JiRA için Kantega SSO **SP ve IDP** SSO başlatılan destekler

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Galeriden JIRA için Kantega SSO ekleme

JIRA için Kantega SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, GALERIden JIRA için Kantega SSO'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden JIRA için Kantega SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **JIRA için Kantega SSO**yazın, sonuç panelinden **JIRA için Kantega SSO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Kantega SSO jira için sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayanarak, JiRA için Kantega SSO ile Azure AD tek oturum açma işlemini yapılandırın ve test esinizsiniz.
Tek oturum açma nın işe yaraması için, Jira için Bir Azure AD kullanıcısı ile Kantega SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Azure AD oturumlarını JIRA için Kantega SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Jira Tek Oturum Açma için Kantega SSO'yu yapılandırın](#configure-kantega-sso-for-jira-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[JIRA test kullanıcısı için Kantega SSO oluşturun](#create-kantega-sso-for-jira-test-user)** - kullanıcının Azure REKLAM gösterimine bağlı jira için Kantega SSO'da Britta Simon'ın bir muadili olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

JiRA için Kantega SSO ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/)JIRA uygulama tümleştirme sayfası **için Kantega SSO'da** **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![JIRA Etki Alanı ve URL'ler için Kantega SSO tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![JIRA Etki Alanı ve URL'ler için Kantega SSO tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. Bu değerler, daha sonra öğreticide açıklanan Jira eklentisinin konfigürasyonu sırasında alınır.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. JIRA için **Kantega SSO'yu ayarlayın,** gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-kantega-sso-for-jira-single-sign-on"></a>JIRA Tek Oturum Açma için Kantega SSO'yu yapılandırın

1. Farklı bir web tarayıcısı penceresinde, jira şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerinde hover ve **Eklentiler**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon1.png)

1. Eklentiler sekmesi nin altında, **yeni eklentileri bul'u**tıklatın. **JIRA (SAML & Kerberos) için Kantega SSO'yu** arayın ve yeni SAML eklentisini yüklemek için **Yükle** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon2.png)

1. Eklenti yüklemesi başlar.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon3.png)

1. Yükleme tamamlandıktan sonra. **Kapat'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon33.png)

1.  **Yönet**'e tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon34.png)
    
1. Yeni eklenti **ENTEGRASYONLAR**altında listelenir. Yeni **Configure** eklentiyi yapılandırmak için Yapıya'yı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon35.png)

1. **SAML** bölümünde. Kimlik ekle **sağlayıcısı** açılır tarafından **Azure Etkin Dizin 'i (Azure AD) seçeneğini** belirleyin.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon4.png)

1. **Temel**olarak abonelik düzeyini seçin.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon5.png)       

1. Uygulama **özellikleri** bölümünde aşağıdaki adımları gerçekleştirin: 

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon6.png)

    a. App **ID URI** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırmabölümünde** **Tanımlayıcı, Yanıt URL'si ve Oturum Açma URL'si** olarak kullanın.

    b. **İleri**'ye tıklayın.

1. Meta **veri alma** bölümünde aşağıdaki adımları gerçekleştirin: 

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon7.png)

    a. **Bilgisayarımda Meta Veri dosyasını**seçin ve Azure portalından indirdiğiniz meta veri dosyasını yükleyin.

    b. **İleri**'ye tıklayın.

1. Ad **ve SSO konum** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon8.png)

    a. **Kimlik sağlayıcı ad** metin kutusuna Kimlik Sağlayıcının Adını ekleyin (örn. Azure AD).

    b. **İleri**'ye tıklayın.

1. İmzasertifikasını doğrulayın ve **İleri'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon9.png)

1. **JIRA kullanıcı hesapları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon10.png)

    a. **Gerekirse JIRA'nın iç Dizininde Kullanıcıları Oluştur'u** seçin ve kullanıcılar için grubun uygun adını girin (birden çok hayır olabilir. virgülle ayrılmış grupların).

    b. **İleri**'ye tıklayın.

1. **Son**'a tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon11.png)

1. Azure **AD için bilinen etki alanlarında** aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforjira-tutorial/addon12.png)

    a. Sayfanın sol panelinden **Bilinen etki alanlarını** seçin.

    b. **Bilinen etki alanları** textbox'a etki alanı adı girin.

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
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın JIRA için Kantega SSO'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **jira için Kantega SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, JIRA için Kantega SSO'yu**seçin.

    ![Uygulamalar listesinde JIRA bağlantısı için Kantega SSO](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-kantega-sso-for-jira-test-user"></a>JIRA test kullanıcısı için Kantega SSO oluşturun

Azure AD kullanıcılarının JIRA'da oturum açabilmeleri için JIRA'da oturum açmaları gerekir. JIRA için Kantega SSO'da, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jira şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerinde hover ve **Kullanıcı yönetimi**tıklayın.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user1.png) 

1. **Kullanıcı yönetimi** sekmesi bölümünde, **kullanıcı oluştur'u**tıklatın.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user2.png) 

1. **"Yeni kullanıcı oluştur"** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user3.png) 

    a. **E-posta adresi** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

    b. Tam **Ad** metin kutusuna Britta Simon gibi kullanıcının tam adını yazın.

    c. Kullanıcı **adı** metin kutusuna, kullanıcının Brittasimon@contoso.come-postasını yazın.

    d. **Parola** metin kutusuna, kullanıcının parolasını yazın.

    e. **Kullanıcı Oluştur'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki JIRA karosu için Kantega SSO'yu tıklattığınızda, SSO'yu kurduğunuz JIRA için Kantega SSO'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
