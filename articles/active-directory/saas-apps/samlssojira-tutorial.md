---
title: 'Öğretici: Resolution GmbH tarafından Jira için SAML SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAML SSO arasında Jira için GmbH çözünürlüğüne göre tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 957fee48397bc0b23737157dec0e74cf6505fab5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160137"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Öğretici: Çözüm GmbH tarafından Jira için SAML SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Azure Active Directory (Azure AD) ile GmbH çözünürlüğüne göre Jira için SAML SSO'yu nasıl ayarlayabileceğinizi öğreneceksiniz.
Çözüm GmbH ile Jira için SAML SSO entegre Azure AD size aşağıdaki avantajları sağlar:

* Azure AD'de, GmbH çözünürlüğü ile SAML SSO eklentisi ile Jira'da oturum açabilenleri kontrol edebilirsiniz.
* GmbH (Tek Oturum Açma) çözünürlüğü ile Jira için SAML SSO'yu kullanarak kullanıcılarınızın Azure REKLAM hesaplarıyla Jira ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ve Jira için SAML SSO'yu GmbH çözünürlüğüne göre yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Çözüm GmbH tek oturum açma özellikli abonelik tarafından Jira için SAML SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Karar GmbH tarafından Jira için SAML SSO **SP** destekler ve **IDP** SSO başlatılan

## <a name="adding-an-enterprise-application-for-single-sign-on"></a>Tek oturum açma için kurumsal uygulama ekleme

Azure AD'de tek oturum açma ayarlamak için yeni bir kurumsal uygulama eklemeniz gerekir. Galeride, bunun için önceden yapılandırılmış bir uygulama vardır, **Çözüm GmbH tarafından Jira için SAML SSO.**

**Galeriden GmbH çözünürlüğüne göre Jira için SAML SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamalar'ı**tıklatın.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Çözüm GmbH tarafından Jira için SAML SSO**yazın, sonuç panelinden **çözünürlük GmbH tarafından Jira için SAML SSO** seçin ve sonra uygulama eklemek için **Ekle** düğmesini tıklatın. Kurumsal uygulamanın adını da değiştirebilirsiniz.

     ![Sonuçlar listesinde çözünürlük GmbH tarafından Jira için SAML SSO](common/search-new-app.png)

## <a name="configure-and-test-single-sign-on-with-the-saml-sso-plugin-and-azure-ad"></a>SAML SSO eklentisi ve Azure AD ile tek oturum açmayı yapılandırma ve test

Bu bölümde, bir Azure AD kullanıcısı için Jira'ya tek oturum açma işlemlerini sınayacak ve yapılandıracaksınız. Bu **Britta Simon**adlı bir test kullanıcısı için yapılacaktır.
Tek oturum açma nın işe yaraması için, Bir Azure AD kullanıcısı ile Jira için SAML SSO'daki ilgili kullanıcı arasında GmbH çözünürlüğüne göre bir bağlantı ilişkisinin kurulması gerekir.

Tek oturum açma yı yapılandırmak ve test etmek için aşağıdaki adımları tamamlamanız gerekir:

1. **[Azure AD kurumsal uygulamasını tek oturum açma için yapılandırın](#configure-the-azure-ad-enterprise-application-for-single-sign-on)** - Azure AD kurumsal uygulamasını tek oturum açma için yapılandırın
2. **[Jira örneğinizin SAML SSO eklentisini yapılandırın](#configure-the-saml-sso-plugin-of-your-jira-instance)** - Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırın.
3. **[Bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** - Azure AD'de bir test kullanıcısı oluşturun.
1. **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** - Test kullanıcısının Azure tarafında tek oturum açma yı kullanmasını etkinleştirme.
1. **[Jira'da test kullanıcısını oluşturun](#create-the-test-user-also-in-jira)** - Azure AD test kullanıcısı için Jira'da bir karşı test kullanıcısı oluşturun.
1. **[Tek oturum açma -](#test-single-sign-on)** Yapılandırmanın çalışıp çalışmadığını doğrulayın.

### <a name="configure-the-azure-ad-enterprise-application-for-single-sign-on"></a>Azure AD kurumsal uygulamasını tek oturum açma için yapılandırın

Bu bölümde, Azure portalında tek oturum açma'yı ayarlarsınız.

Çözüm GmbH tarafından Jira için SAML SSO ile tek oturum yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/)Çözüm GmbH kurumsal uygulaması tarafından Jira için yeni oluşturulan **SAML SSO'da** sol panelde **Tek oturum açma'yı** seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. **Tek oturum açma yöntemini seçin,** tek oturum açmayı etkinleştirmek için **SAML** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. Daha sonra, **Temel SAML Yapılandırmailetişim** kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Çözüm GmbH Etki Alanı ve URL'ler tek oturum açma bilgileri ile Jira için SAML SSO](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarlayın** ve aşağıdaki adımı gerçekleştirin:

    ![Çözüm GmbH Etki Alanı ve URL'ler tek oturum açma bilgileri ile Jira için SAML SSO](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si için, Jira örneğinizin temel URL'si ile ** \<sunucu tabanı-url>** değiştirin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz. Eğer bir sorun varsa, [çözüm GmbH Müşteri destek ekibi tarafından Jira için SAML SSO](https://www.resolution.de/go/support)bize ulaşın.

4. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, **Federasyon Metadata XML'i** indirin ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-the-saml-sso-plugin-of-your-jira-instance"></a>Jira örneğinizin SAML SSO eklentisini yapılandırın 

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak Jira örneğiniz için oturum açın.

2. Sağ taraftaki çarkın üzerine taşırınız ve **Uygulamaları Yönet'e**tıklayın.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon1.png)

3. Yönetici Erişimi sayfasına yönlendirilirseniz, **Parola'yı** girin ve **Onayla** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon2.png)

4. Jira normalde sizi Atlassian pazarına yönlendirir. Değilse, sol panelde **yeni uygulamaları bul'a** tıklayın. **JIRA için SAML Tek İşaret Açma (SSO)** araması yapın ve SAML eklentisini yüklemek için **Yükle** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/store.png)

5. Eklenti yüklemesi başlayacaktır. Bittiğinde, **Kapat** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/store-2.png)

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/store-3.png)

6. Ardından, **Yönet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/store-4.png)
    
8. Daha sonra, yalnızca yüklenen eklentiyi yapılandırmak için Yapılandır'ı tıklatın. **Configure**

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/store-5.png)

9. **SAML SingleSignOn Eklenti** Yapılandırmasihirbazı'nda, Azure AD'yi yeni bir Kimlik Sağlayıcısı olarak yapılandırmak için **yeni IdP ekle'yi** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon4.png) 

10. **SAML Kimlik Sağlayıcınızı Seçin** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon5a.png)
 
    a. **Azure AD'yi** IdP türü olarak ayarlayın.
    
    b. Kimlik Sağlayıcısının **Adını** ekleyin (örneğin Azure AD).
    
    c. Kimlik Sağlayıcısının (isteğe bağlı) **açıklamasını** ekleyin (örneğin Azure AD).
    
    d. **İleri**'ye tıklayın.
    
11. Kimlik **sağlayıcısı yapılandırma** sayfasında **İleri'yi**tıklatın.
 
    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon5b.png)

12. **SAML IdP Meta veri** verisi alma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon5c.png)

    a. **Metaveri XML Dosyasını Seç** düğmesini tıklatın ve daha önce indirdiğiniz **Federasyon Metadata XML** dosyasını seçin.

    b. **İçe Aktar** düğmesini tıklatın.
     
    c. Alma işlemi başarılı olana kadar kısa bir süre bekleyin.  
     
    d. **İleri** düğmesini tıklatın.
    
13. **Kullanıcı Kimliği özniteliği ve dönüşüm** **sayfasında, İleri** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon5d.png)
    
14. Kullanıcı **oluşturma ve güncelleme** sayfasında, ayarları kaydetmek için **& Kaydet'i** tıklatın.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon6a.png)
    
15. Ayarlar **sayfanızı test edin,** şimdilik kullanıcı testini atlamak için **test & el ile yapılandırmayı** tıklatın. Bu, bir sonraki bölümde gerçekleştirilir ve Azure portalında bazı ayarlar gerektirir.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon6b.png)
    
16. Uyarıyı atlamak için **Tamam'ı** tıklatın.
    
    ![Tek İşaret-On'u Yapılandır](./media/samlssojira-tutorial/addon6c.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır. Kullanıcı ile, tek oturum açma test edecek.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı **özelliklerinde,** aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Britta Simon**girin.
  
    b. Kullanıcı **adı** alanına girin. <b>BrittaSimon@contoso.com</b>

    c. **Parolayı Göster** onay kutusunu seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde Britta Simon'ı tek oturum açma kullanmasına izin veren kurumsal uygulamaya eklersiniz.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin. 

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, bu eğitimin başında oluşturduğunuz kurumsal uygulamayı arayın. Eğer öğretici adımları takip ediyorsanız, bu **çözünürlük GmbH tarafından Jira için SAML SSO**denir. Başka bir ad verdiyseniz, bu ismi arayın.

    ![Uygulamalar listesinde çözünürlük GmbH bağlantısı ile Jira için SAML SSO](common/all-applications.png)

3. Sol panelde, **Kullanıcılar ve gruplar'ı**tıklatın.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, rolü **seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-the-test-user-also-in-jira"></a>Jira'da da test kullanıcısını oluşturun

Azure AD kullanıcılarının GmbH çözünürlüğü ile Jira için SAML SSO'da oturum açabilmeleri için, GmbH çözünürlüğü ile Jira için SAML SSO'ya dahil edilmeleri gerekir. Bu öğretici durumda, elle hükmü yapmak zorunda. Ancak, saml SSO eklentisi için çözünürlük tarafından kullanılabilir diğer sağlama modelleri de vardır, örneğin **Just In Time** sağlama. [Karar GmbH tarafından SAML SSO](https://wiki.resolution.de/doc/saml-sso/latest/all)kendi belgelerine bakın. Bu konuda bir sorunuz varsa, [çözünürlük desteğinde](https://www.resolution.de/go/support)desteğe başvurun.

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jira örneğinde yönetici olarak oturum açın.

2. Dişlinin üzerine titreyin ve **Kullanıcı yönetimini**seçin.

   ![Çalışan Ekle](./media/samlssojira-tutorial/user1.png)

3. Yönetici Erişimi sayfasına yönlendirilirseniz, **Parola'yı** girin ve **Onayla** düğmesini tıklatın.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user2.png) 

4. Kullanıcı **yönetimi** sekmesi bölümü **altında, kullanıcı oluştur'u**tıklatın.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user3-new.png) 

5. **"Yeni kullanıcı oluştur"** iletişim sayfasında aşağıdaki adımları gerçekleştirin. Kullanıcıyı Azure AD'deki gibi oluşturmanız gerekir:

    ![Çalışan Ekle](./media/samlssojira-tutorial/user4-new.png) 

    a. **E-posta adresi** metin kutusuna, kullanıcının <b>BrittaSimon@contoso.com</b>e-posta adresini yazın: .

    b. Tam **Ad** metin kutusunda, kullanıcının tam adını yazın: **Britta Simon**.

    c. Kullanıcı **adı** metin kutusuna, kullanıcının e-posta adresini yazın: <b>BrittaSimon@contoso.com</b>. 

    d. **Parola** metin kutusuna, kullanıcının parolasını girin.

    e. Kullanıcı oluşturmayı bitirmek için **kullanıcıyı oluştur'u** tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki GmbH çözünürlükteki GmbH döşemesi ile Jira için SAML SSO'yu tıklattığınızda, SSO'yu kurduğunuz GmbH çözünürlüğü ile otomatik olarak Jira için SAML SSO'ya oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

Ayrıca, [https://\<sunucu-base-url>/eklentileri/servlet/samlsso](https://\<server-base-url>/plugins/servlet/samlsso)gidin, tek oturum açma test edebilirsiniz. Jira örneğinizin temel URL'si ile ** \<sunucu-temel url>** değiştirin.


## <a name="enable-single-sign-on-redirection-for-jira"></a>Jira için tek oturum açma yeniden yönlendirmesini etkinleştirme

Daha önce bölümde belirtildiği gibi, şu anda tek oturum açmatetikiçin iki yol vardır. **Azure portalını** kullanarak veya **Jira örneğinize özel bir bağlantı**kullanarak. Çözünürlük GmbH tarafından SAML SSO eklentisi de sadece **Jira örneğini gösteren herhangi bir URL erişerek**tek oturum açma tetiklemek için izin verir.

Özünde, Jira'ya erişen tüm kullanıcılar eklentideki bir seçeneği etkinleştirdikten sonra tek oturum açmaya yönlendirilir.

SSO yönlendirmesini etkinleştirmek için **Jira örneğinde**aşağıdakileri yapın:

1. Jira'daki SAML SSO eklentisinin yapılandırma sayfasına erişin.
1. Sol panelde **Yönlendirme'ye** tıklayın.
![](./media/samlssojira-tutorial/ssore1.png)

1. SSO **Yönlendirme'yi Etkinleştir'i**Onayla.
![](./media/samlssojira-tutorial/ssore2.png) 

1. Sağ üst köşedeki **Ayarları Kaydet** düğmesine basın.

Seçeneği etkinleştirdikten sonra, [sunucu-base-url>/login.jsp?nosso'ya\<](https://\<server-base-url>/login.jsp?nosso)yönlendirilerek **nosso seçeneğini etkinleştir** seçeneği işaretlenirs https://e kullanıcı adı/parola istemine ulaşabilirsiniz. Her zaman olduğu gibi, yerine ** \<sunucu-base-url>** temel URL ile.


## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

