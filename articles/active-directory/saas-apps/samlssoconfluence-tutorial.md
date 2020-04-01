---
title: 'Öğretici: Resolution GmbH ile Birleşimi için SAML SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAML SSO arasında GmbH çözünürlüğüne göre tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9676a71940569b26d6b0b6bfef767108ae57d953
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161225"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Öğretici: Resolution GmbH ile Birleşimi için SAML SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, GmbH çözünürlüğüne göre Saml SSO'yu Azure Active Directory (Azure AD) ile nasıl birleştiracağınızı öğreneceksiniz.
Resolution GmbH ile Birleştirme için SAML SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de, GmbH çözünürlüğü ile Saml SSO'ya erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla GmbH (Tek Oturum Açma) çözünürlüğü yle Birleşimi için SAML SSO'da otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Çözüm GmbH'ye göre Birleştirme için SAML SSO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Resolution GmbH tek oturum açma özellikli abonelik ile Confluence için SAML SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SamL SSO resolution GmbH tarafından Confluence için **SP** destekler ve **IDP** SSO başlatılan

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>Galeriden çözünürlüğe göre Confluence için SAML SSO ekleme

Confluence için SAML SSO'nun entegrasyonunu GmbH'nin Azure AD'ye göre yapılandırmak için, galeriden çözüm GmbH ile Birlikte çalmak için SAML SSO'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden çözünürlüğe göre Birleştirme için SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **GmbH çözünürlüğüne göre Birleştirme için SAML SSO**yazın, sonuç panelinden **GmbH çözünürlüğüne göre Birleştirme için SAML SSO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![SamL SSO sonuç listesinde çözünürlük GmbH tarafından Confluence için](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak, Azure AD tek oturum açma işlemini SAML SSO ile GmbH çözünürlüğüne göre yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure AD kullanıcısı ile çözüm GmbH ile Birleştirme için SAML SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Azure AD oturumaçmayı, Çözüm GmbH ile Birleştirme için SAML SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak **[için Saml SSO'yu GmbH Tek Oturum Açma çözünürlüğüne göre yapılandırın](#configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on)** .
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Resolution GmbH test kullanıcısı tarafından Confluence için SAML SSO'yu oluşturun](#create-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - kullanıcının Azure AD temsiline bağlı olan GmbH çözünürlüğü ile Birleşimi için SAML SSO'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini, Çözüm GmbH'ye göre Birleştirme için SAML SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Resolution GmbH uygulama tümleştirme sayfasında **nfluence için SAML SSO'daki** [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** Başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Çözünürlük GmbH Domain ve URL'ler tek oturum açma bilgileri ile Confluence için SAML SSO](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Uygulamayı SP başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Çözünürlük GmbH Domain ve URL'ler tek oturum açma bilgileri ile Confluence için SAML SSO](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak [için Resolution GmbH Client destek ekibi ile Confluence için SAML SSO](https://www.resolution.de/go/support) ile iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on"></a>SamL SSO'yu GmbH Tek İşaretleme çözünürlüğüne göre Birleştirme için yapılandırın

1. Farklı bir web tarayıcısı penceresinde, bir yönetici olarak **çözünürlük GmbH yönetici portalı tarafından Confluence için SAML SSO** oturum açın.

2. Dişli üzerinde hover ve **Eklentiler**tıklayın.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon1.png)

3. Administrator Access sayfasına yönlendirilirsiniz. Parolayı girin ve **Onayla** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon2.png)

4. **ATLASSIAN MARKETPLACE** sekmesi altında, **yeni eklentileri bul'u**tıklatın. 

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon.png)

5. **Confluence için SAML Tek İşaret Açma 'yı (SSO)** arayın ve yeni SAML eklentisini yüklemek için **Yükle** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon7.png)

6. Eklenti yüklemesi başlayacaktır. **Kapat'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon8.png)

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon9.png)

7.  **Yönet**'e tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon10.png)
    
8. Yeni **Configure** eklentiyi yapılandırmak için Yapıya'yı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon11.png)

9. Bu yeni eklenti, **KULLANICIlar & GÜVENLİk** sekmesi altında da bulunabilir.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon3.png)
    
10. **SAML SingleSignOn Eklenti Yapılandırma** sayfasında, Kimlik Sağlayıcı ayarlarını yapılandırmak için **yeni IdP ekle** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon4.png)

11. **SAML Kimlik Sağlayıcısı sayfanızı seçin,** aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. **Azure AD'yi** IdP türü olarak ayarlayın.
    
    b. Kimlik Sağlayıcısının **Adını** Ekleyin (örn. Azure AD).
    
    c. Kimlik Sağlayıcısının **Açıklamasını** Ekleyin (örn. Azure AD).
    
    d. **İleri**'ye tıklayın.
    
12. **Kimlik sağlayıcısı yapılandırma** sayfasında **İleri** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon5b.png)

13. **SAML IdP Meta veri** verisi alma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon5c.png)

    a. **Dosyayı Yükle** düğmesini tıklatın ve Adım 5'te indirdiğiniz Metadata XML dosyasını seçin.

    b. **İçe Aktar** düğmesini tıklatın.
    
    c. Alma başarılı olana kadar kısa bir süre bekleyin.
    
    d. **İleri** düğmesini tıklatın.
    
14. **Kullanıcı Kimliği özniteliği ve dönüşüm** sayfasında **İleri** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon5d.png)
    
15. **Kullanıcı oluşturma ve güncelleme** sayfasında, ayarları kaydetmek için & **Kaydet'i** tıklatın.   
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon6a.png)
    
16. **Ayarlar sayfanızı test edin,** şimdilik kullanıcı testini atlamak için **el ile yapılandır& testat'ı** tıklatın. Bu, bir sonraki bölümde gerçekleştirilir ve Azure portalında bazı ayarlar gerektirir. 
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon6b.png)
    
17. Görünen iletişim **okumasında test atlama anlamına gelir ...**, **Tamam'ı**tıklatın.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssoconfluence-tutorial/addon6c.png)

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

Bu bölümde, Britta Simon'ın GmbH çözünürlüğü ile Birleşimi için SAML SSO'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında Kurumsal **Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **GmbH çözünürlüğüne göre Birleştirme için SAML SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **GmbH çözünürlüğüne göre Birleşimi için SAML SSO**yazın ve seçin.

    ![Uygulamalar listesinde çözünürlüğe GmbH bağlantısı ile Confluence için SAML SSO](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>Resolution GmbH test kullanıcısı tarafından Confluence için SAML SSO oluşturun

Azure AD kullanıcılarının GmbH çözünürlüğü ile Birleşimi için SAML SSO'da oturum açabilmeleri için, GmbH çözünürlüğü ile Birleştirme için SAML SSO'ya dahil edilmeleri gerekir.  
Karar GmbH tarafından Confluence için SAML SSO'da, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bir yönetici olarak çözünürlük GmbH şirket sitesi tarafından Confluence için SAML SSO giriş yapın.

2. Dişli üzerinde hover ve **Kullanıcı yönetimi**tıklayın.

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user1.png) 

3. Kullanıcılar bölümünün **altında, kullanıcı ekle** sekmesini tıklatın. **"Kullanıcı Ekle"** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user2.png) 

    a. Kullanıcı **adı** metin kutusuna Britta Simon gibi kullanıcının e-postasını yazın.

    b. Tam **Ad** metin kutusuna Britta Simon gibi kullanıcının tam adını yazın.

    c. **E-posta** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

    d. **Parola** metin kutusuna Britta Simon'ın parolasını yazın.

    e. **Parolayı Onayla'yı** tıklatın.
    
    f. **Ekle** düğmesini tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki GmbH çözünürlük teki GmbH döşemesi ile Birlikte şilep için SAML SSO'yu tıklattığınızda, SSO'yu kurduğunuz GmbH çözünürlüğü ile otomatik olarak SAML SSO'ya oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

