---
title: 'Öğretici: Resolution GmbH tarafından Bitbucket için SAML SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SamL SSO arasında Bitbucket için gmbH çözünürlüğüne göre tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57ae83aee563d4893f767331fff2289a4595cc60
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73157652"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Öğretici: Resolution GmbH ile Bitbucket için SAML SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Bitbucket için SAML SSO'nun GmbH çözünürlüğüne göre Azure Active Directory (Azure AD) ile nasıl entegre ediliş ediliş ini öğrenirsiniz.
Çözüm GmbH ile Bitbucket için SAML SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Bitbucket için SAML SSO erişimi olan Azure AD'de GmbH çözünürlüğü ile kontrol edebilirsiniz.
* Kullanıcılarınızın, Azure REKLAM hesaplarıyla GmbH (Tek Oturum Açma) çözünürlüğü yle Bitbucket için SAML SSO'da otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Bitbucket için SAML SSO ile GmbH çözünürlüğüne göre yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Çözünürlük GmbH tek oturum açma özellikli abonelik ile Bitbucket için SAML SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Karar GmbH tarafından Bitbucket için SAML SSO **SP destekler ve IDP** SSO başlatılan
* Çözünürlüğe göre Bitbucket için SAML SSO GmbH **Just In Time** kullanıcı sağlama destekler


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Galeriden Çözünürlüğe GmbH ile Bitbucket için SAML SSO ekleme

Bitbucket için SAML SSO'nun Entegrasyonunu, GmbH'nin Azure AD'ye göre tümleştirilmesi için, Galeriden Çözüm GmbH ile BitBucket için SAML SSO'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Çözünürlüğe Göre Bitbucket için SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Karar GmbH tarafından Bitbucket için SAML SSO**yazın, sonuç panelinden **Çözünürlük GmbH tarafından Bitbucket için SAML SSO** seçin sonra uygulama eklemek için **ekle** düğmesini tıklatın.

     ![Sonuç listesinde çözünürlük GmbH tarafından Bitbucket için SAML SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak, Azure AD'nin Bitbucket için SAML SSO ile tek oturum açma işlemini GmbH çözünürlüğüne göre yapılandırıp test emzebilirsiniz.
Tek oturum açma nın işe yaraması için, Bir Azure AD kullanıcısı ile Bitbucket için SAML SSO'daki ilgili kullanıcı arasında GmbH çözünürlüğüne göre bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Bitbucket için SAML SSO ile GmbH çözünürlüğüne göre yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak **[için Parça Kova için SAML SSO'yu GmbH Tek Oturum Açma çözünürlüğüne göre yapılandırın](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)** .
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Resolution GmbH test kullanıcısı tarafından Bitbucket için SAML SSO oluşturun](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)** - kullanıcının Azure AD temsiline bağlı olan GmbH çözünürlüğü ile SAML SSO'da Britta Simon'ın bir örneğine sahip olun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Bitbucket için SAML SSO ile GmbH çözünürlüğüne göre yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Çözünürlük GmbH uygulama tümleştirme sayfası **ile Bitbucket için SAML SSO'daki** [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Çözünürlük GmbH Etki Alanı ve URL'ler tek oturum açma bilgileri ile Bitbucket için SAML SSO](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Çözünürlük GmbH Etki Alanı ve URL'ler tek oturum açma bilgileri ile Bitbucket için SAML SSO](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak [için Resolution GmbH Client destek ekibi tarafından Bitbucket için SAML SSO](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) ile iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>Parça kova için SAML SSO'nun GmbH Tek Oturum Açma çözünürlüğüne göre yapılandırın

1. Çözüm GmbH şirket sitesi tarafından Bitbucket için SAML SSO'nuza yönetici olarak oturum açın.

2. Ana araç çubuğunun sağ tarafında **Ayarlar'ı**tıklatın.

3. HESAPLAR bölümüne gidin, Menü çubuğundaki **SAML SingleSignOn'a** tıklayın.

    ![The Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. **SAML SIngleSignOn Eklenti Yapılandırma sayfasında,** **idp ekle'yi**tıklatın. 

    ![idp Ekle](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. **SAML Kimlik Sağlayıcı Sayfanızı Seçin'de** aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. AZURE AD olarak **Idp Türü'nü** seçin. **AZURE AD**

    b. **Ad** metin kutusuna, adı yazın.

    c. **Açıklama** metin kutusuna açıklamayı yazın.

    d. **İleri**'ye tıklayın.

6. Kimlik **sağlayıcı yapılandırma sayfasında** **İleri'yi**tıklatın.

    ![Kimlik config](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  **SamL Idp Meta Veri** Sayfasını İçe Aktar'da, Azure portalından indirdiğiniz **METADATA XML** dosyasını yüklemek için **Dosyayı Yükle'yi** tıklatın.

    ![Idpmeta data](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. **İleri**'ye tıklayın.

9. **Ayarları kaydet**’e tıklayın.

    ![Kaydet](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

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

Bu bölümde, Britta Simon'ın, GmbH çözünürlüğü yle Bitbucket için SAML SSO'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında, **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **GmbH çözünürlüğüne göre Bitbucket için SAML SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Karar GmbH tarafından Bitbucket için SAML SSO**yazın ve seçin.

    ![Uygulamalar listesinde çözünürlük GmbH bağlantısı ile Bitbucket için SAML SSO](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Resolution GmbH test kullanıcısı tarafından Bitbucket için SAML SSO oluşturun

Bu bölümün amacı, Çözüm GmbH tarafından Bitbucket için SAML SSO Britta Simon adlı bir kullanıcı oluşturmaktır. Resolution GmbH tarafından Bitbucket için SAML SSO tam zamanında sağlanması destekler ve aynı zamanda kullanıcılar el ile oluşturulabilir, sizin gereksiniminize göre [çözünürlük GmbH Müşteri destek ekibi tarafından Bitbucket için SAML SSO](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki GmbH çözünürlükteki GmbH döşemesi ile Bitbucket için SAML SSO'yu tıklattığınızda, SSO'yu kurduğunuz GmbH çözünürlüğü ile Otomatik olarak Bitbucket için SAML SSO'ya oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

