---
title: 'Öğretici: Çözünürlük GmbH tarafından Bambu için SAML SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAML SSO arasında Bamboo için tek oturum açma yı çözünürlük GmbH ile nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 044a2f9a9ecd0ccceb99ce7999af7e2c8578950d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67106529"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: Resolution GmbH ile Bambu için SAML SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, GmbH çözünürlüğüne göre SAML SSO'nun Bamboo için Azure Active Directory (Azure AD) ile nasıl entegre ediliş ediliş ini öğrenirsiniz.
SamL SSO'yu Bamboo için Resolution GmbH ile Azure AD ile entegre etmek size aşağıdaki avantajları sağlar:

* Resolution GmbH ile Bambu için SAML SSO erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla GmbH (Tek Oturum Açma) çözünürlüğü yle Kullanıcılarınızın Bambu için SAML SSO'da otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Çözüm GmbH'ye göre SamL SSO ile Yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SamL SSO Bambu için çözünürlük GmbH tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SamL SSO Bambu için çözünürlük GmbH **SP destekler ve IDP** SSO başlatılan
* Çözünürlük GmbH tarafından Bambu için SAML SSO **Just In Time** kullanıcı sağlama destekler

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Galeriden çözünürlük GmbH tarafından Bambu için SAML SSO ekleme

Saml SSO'nun Bamboo için entegrasyonunu GmbH'nin Azure AD'a göre yapılandırarak yapılandırmak için, galeriden Çözüm GmbH ile SamL SSO'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden çözünürlüğe Göre Bambu için SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **çözünürlük GmbH tarafından Bambu için SAML SSO**yazın, sonuç panelinden **çözünürlük GmbH tarafından Bambu için SAML SSO** seçin sonra uygulama eklemek için **ekle** düğmesini tıklatın.

    ![SamL SSO bambu için çözünürlük GmbH tarafından sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak, Azure AD'nin Saml SSO ile Bambu için tek oturum açma işlemini GmbH çözünürlüğü ile yapılandırıp test emzebilirsiniz.
Tek oturum açma nın işe yaraması için, Bir Azure AD kullanıcısı ile Çözüm GmbH'nin çözüme göre Bambu için SAML SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumaçmayı, Bamboo için SAML SSO ile gmbH çözünürlüğü yle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[SamL SSO'yu Bambu için GmbH Tek Oturum Açma çözünürlüğüne göre yapılandırın](#configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Resolution GmbH test kullanıcısı tarafından Bambu için SAML SSO oluşturun](#create-saml-sso-for-bamboo-by-resolution-gmbh-test-user)** - kullanıcının Azure AD temsiline bağlı olan GmbH çözünürlüğü ile SAML SSO'daki Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı, Bamboo için SAML SSO ile GmbH çözünürlüğüne göre yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/)Resolution GmbH uygulama tümleştirme **sayfasından Bambu için SAML SSO'da** **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Çözünürlük GmbH Domain ve URL'ler tek oturum açma bilgileri ile Bambu için SAML SSO](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Çözünürlük GmbH Domain ve URL'ler tek oturum açma bilgileri ile Bambu için SAML SSO](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak [için Resolution GmbH Client destek ekibi tarafından Bambu için SAML SSO](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) ile iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Çözünürlük GmbH bölümü ne kadar **SamL SSO'yu Bambu olarak ayarlayın,** gereksiniminize göre uygun URL'yi(ler) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-saml-sso-for-bamboo-by-resolution-gmbh-single-sign-on"></a>Çözünürlüğe göre Bambu için SAML SSO'su GmbH Tek İşaret-On çözünürlüğe göre yapılandırın

1. SamL SSO'nuza Bambu için çözüm gmbH şirket sitesini yönetici olarak kaydedin.

1. Ana araç çubuğunun sağ tarafında, **Ayarlar** > **Eklentileri'ni**tıklatın.

    ![Ayarlar](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. GÜVENLİk bölümüne gidin, Menü çubuğundaki **SAML SingleSignOn'a** tıklayın.

    ![The Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. **SAML SIngleSignOn Eklenti Yapılandırma sayfasında,** **idp ekle'yi**tıklatın.

    ![idp Ekle](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. **SAML Kimlik Sağlayıcı Sayfanızı Seçin'de** aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. AZURE AD olarak **Idp Türü'nü** seçin. **AZURE AD**

    b. **Ad** metin kutusuna, adı yazın.

    c. **Açıklama** metin kutusuna açıklamayı yazın.

    d. **İleri**'ye tıklayın.

1. Kimlik **sağlayıcısı yapılandırma** sayfasında **İleri'yi**tıklatın.

    ![Kimlik config](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1. **SamL Idp Meta Veri** Sayfasını İçe Aktar'da, Azure portalından indirdiğiniz **METADATA XML** dosyasını yüklemek için **Dosyayı Yükle'yi** tıklatın.

    ![Idpmeta data](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. **İleri**'ye tıklayın.

1. **Ayarları kaydet**’e tıklayın.

    ![Kaydet](./media/bamboo-tutorial/tutorial_bamboo_save.png)

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

Bu bölümde, Britta Simon'ın GmbH çözünürlüğü yle Saml SSO'ya Bambu için erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında, **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **GmbH çözünürlüğüne göre Bambu için SAML SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, resolution **GmbH tarafından Bambu için SAML SSO'yu**seçin.

    ![Uygulamalar listesinde çözünürlük GmbH bağlantısı ile Bambu için SAML SSO](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Resolution GmbH test kullanıcısı tarafından Bambu için SAML SSO oluşturun

Bu bölümün amacı, Çözünürlüğü GmbH tarafından Bambu için SAML SSO Britta Simon adlı bir kullanıcı oluşturmaktır. Çözünürlük GmbH tarafından Bambu için SAML SSO sadece zamanında sağlama destekler ve aynı zamanda kullanıcılar el ile oluşturulabilir, [çözüm GmbH Müşteri destek ekibi tarafından Bambu için SAML SSO](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) başvurun gereksiniminize göre.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki GmbH karosu yla Bambu için SAML SSO'yu tıklattığınızda, SSO'yu kurduğunuz GmbH çözünürlüğü ile otomatik olarak SAML SSO'ya oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
