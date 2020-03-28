---
title: 'Öğretici: Microsoft tarafından JIRA SAML SSO ile Azure Active Directory entegrasyonu (V5.2) | Microsoft Dokümanlar'
description: Azure Active Directory ve JIRA SAML SSO arasında Microsoft (V5.2) tarafından tek oturum açma işlemlerini nasıl yapılandırılaceksiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d0c00408-f9b8-4a79-bccc-c346a7331845
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3b304bb35ea69906fc9576f45733134387be1b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67099508"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft-v52"></a>Öğretici: Microsoft tarafından JIRA SAML SSO ile Azure Active Directory entegrasyonu (V5.2)

Bu eğitimde, Microsoft'un JIRA SAML SSO'yu (V5.2) Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Microsoft tarafından JIRA SAML SSO'yu (V5.2) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Microsoft tarafından JIRA SAML SSO erişimi olan Azure AD'de (V5.2) denetleyebilirsiniz.
* Kullanıcılarınızın Microsoft (V5.2) (Tek Oturum Açma) tarafından Azure REKLAM hesaplarıyla JIRA SAML SSO ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="description"></a>Açıklama

Tek oturum açmayı etkinleştirmek için Atlassian JIRA sunucusundaki Microsoft Azure Active Directory hesabınızı kullanın. Bu şekilde tüm kuruluş kullanıcılarınız, JIRA uygulamasında oturum açtırmak için Azure REKLAM kimlik bilgilerini kullanabilir. Bu eklenti federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Ön koşullar

Microsoft (V5.2) tarafından JIRA SAML SSO ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- JIRA Core ve Software 5.2 Windows 64-bit sürümünde yüklü ve yapılandırılmış olmalıdır
- JIRA sunucusu HTTPS etkin
- JIRA Plugin için desteklenen sürümleri aşağıdaki bölümde belirtilmiştir unutmayın.
- JIRA sunucusuna internetüzerinden özellikle azure AD Giriş sayfasına kimlik doğrulama için ulaşılabilir ve belirteci Azure AD'den alabilmeli
- Yönetici kimlik bilgileri JIRA'da ayarlanır
- WebSudo JIRA devre dışı bırakılır
- JIRA sunucu uygulamasında oluşturulan test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için JIRA'nın üretim ortamını kullanmanızı önermiyoruz. Önce uygulamanın geliştirme veya evreleme ortamında tümleştirmeyi test edin ve ardından üretim ortamını kullanın.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, burada bir aylık deneme sürümü alabilirsiniz: [Deneme teklifi.](https://azure.microsoft.com/pricing/free-trial/)

## <a name="supported-versions-of-jira"></a>JIRA'nın desteklenen sürümleri

* JIRA Çekirdek ve Yazılım: 5.2
* JIRA da 6.0-7.12 destekler. Daha fazla bilgi için [Microsoft tarafından JIRA SAML SSO'yu](jiramicrosoft-tutorial.md) tıklatın

> [!NOTE]
> JiRA Eklentimizin Ubuntu Sürüm 16.04'te de çalıştığını lütfen unutmayın.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Microsoft tarafından JIRA SAML SSO (V5.2) **SP** başlatılan SSO destekler

## <a name="adding-jira-saml-sso-by-microsoft-v52-from-the-gallery"></a>Microsoft tarafından JIRA SAML SSO ekleme (V5.2) galeriden

JIRA SAML SSO'nun Microsoft (V5.2) tarafından Azure AD'ye entegrasyonunu yapılandırmak için, Microsoft tarafından jira SAML SSO'yu (V5.2) galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Microsoft tarafından jira SAML SSO eklemek için (V5.2) galeriden aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Microsoft tarafından JIRA SAML SSO yazın (V5.2)**, microsoft **tarafından JIRA SAML SSO seçin (V5.2)** sonuç panelinden sonra uygulama eklemek için **ekle** düğmesini tıklatın.

    ![Microsoft (V5.2) tarafından jira SAML SSO sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Microsoft (V5.2) tarafından JIRA SAML SSO ile Azure AD tek oturum açma işlemini yapılandırın ve test esiniz.
Tek oturum açma nın işe yaraması için, Microsoft (V5.2) tarafından bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisinin oluşturulması gerekir.

Microsoft (V5.2) tarafından JIRA SAML SSO ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Jira SAML SSO'yu Microsoft (V5.2) Tek Oturum Açma tarafından yapılandırın](#configure-jira-saml-sso-by-microsoft-v52-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Microsoft (V5.2) test kullanıcısı tarafından JIRA SAML SSO oluşturun](#create-jira-saml-sso-by-microsoft-v52-test-user)** - Microsoft (V5.2) tarafından jira SAML SSO'da Britta Simon'ın bir örneğine sahip olmak için kullanıcının Azure AD gösterimine bağlı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Microsoft (V5.2) tarafından JIRA SAML SSO ile Azure AD tek oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Microsoft (V5.2)** uygulama tümleştirme sayfasındaki JIRA SAML SSO'daki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Microsoft tarafından JIRA SAML SSO (V5.2) Etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. Bağlantı noktası, adlandırılmış bir URL olması durumunda isteğe bağlıdır. Bu değerler, daha sonra öğreticide açıklanan Jira eklentisinin konfigürasyonu sırasında alınır.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-jira-saml-sso-by-microsoft-v52-single-sign-on"></a>Microsoft (V5.2) Tek Oturum Açma tarafından JIRA SAML SSO yapılandırın

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak JIRA örneğiniz için oturum açın.

2. Dişli üzerinde hover ve **Eklentiler**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/jira52microsoft-tutorial/addon1.png)

3. Eklentiler sekmesi altında **eklentileri yönet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/jira52microsoft-tutorial/addon7.png)

4. Eklentiyi Microsoft [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=56521)indirin. Microsoft tarafından sağlanan eklentiyi **Upload eklentisi** menüsünü kullanarak el ile yükleyin. Eklentinin karşıdan yüklenir, [Microsoft Hizmet Sözleşmesi](https://www.microsoft.com/servicesagreement/)kapsamındadır.

    ![Tek İşaret-On'u Yapılandır](./media/jira52microsoft-tutorial/addon12.png)

5. Eklenti yüklendikten sonra **Kullanıcı Yüklü** eklentiler bölümünde görünür. Yeni **Configure** eklentiyi yapılandırmak için Yapıya'yı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/jira52microsoft-tutorial/addon13.png)

6. Yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/jira52microsoft-tutorial/addon52.png)

    > [!TIP]
    > Meta verilerin çözümünde hata olmaması için uygulamaya karşı eşlenen tek bir sertifika olduğundan emin olun. Birden çok sertifika varsa, meta verileri çözdükten sonra yönetici bir hata alır.

    a. **Metadata URL** textbox'ında, Azure portalından kopyaladığınız **Uygulama Federasyonu Metadata Url** değerini yapıştırın ve **Çözümle** düğmesini tıklatın. IdP meta veri URL'sini okur ve tüm alan bilgilerini doldurur.

    b. **Identifier, Yanıt URL'sini ve URL değerlerine** Imzala'yı kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** bölümünde **sırasıyla Identifier, YanıtURL ve URL** metin kutularında oturum aç'a yapıştırın.

    c. **Giriş Düğmesi Adı'nda** kuruluşunuzun kullanıcıların giriş ekranında görmesini istediği düğmenin adını yazın.

    d. **SAML Kullanıcı Kimliği Konumları'nda,** **Kullanıcı Kimliği'nin Konu deyiminin NameIdentifier öğesinde olduğunu** veya Kullanıcı Kimliği'nin bir **Öznitelik öğesinde olduğunu**seçin.  Bu kimlik JIRA kullanıcı kimliği olmalıdır. Kullanıcı kimliği eşleşmiyorsa, sistem kullanıcıların oturum açmasına izin vermez.

    > [!Note]
    > Varsayılan SAML Kullanıcı Kimliği konumu Ad Tanımlayıcı'dır. Bunu bir öznitelik seçeneğiolarak değiştirebilir ve uygun öznitelik adını girebilirsiniz.

    e. Kullanıcı Kimliği'ni seçerseniz **Bir Öznitelik öğesi seçeneğindeyse,** **Atnitelik adı** textbox'ta Kullanıcı Kimliğinin beklendiği özniteliğin adını yazın. 

    f. Azure AD ile federe etki alanını (ADFS vb.) kullanıyorsanız, **Ana Sayfa Alanı Bulma** seçeneğini tıklatın ve Etki Alanı **Adını**yapılandırın.

    g. **Alan** Adı'nda, ADFS tabanlı oturum açma durumunda alan adını buraya yazın.

    h. Bir kullanıcı JIRA'dan oturum açtığında Azure AD'den oturum u imzalamak istiyorsanız Tek Oturum Aç'ı **Etkinleştir'i** işaretleyin. 

    i. Ayarları kaydetmek için **Kaydet** düğmesini tıklatın.

    > [!NOTE]
    > Kurulum ve sorun giderme hakkında daha fazla bilgi için [MS JIRA SSO Bağlayıcı Yönetici Kılavuzu'nu](../ms-confluence-jira-plugin-adminguide.md) ziyaret edin ve yardımınız için [SSS](../ms-confluence-jira-plugin-faq.md) de var

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon\@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com.

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Microsoft tarafından JIRA SAML SSO'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız (V5.2).

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Microsoft'un JIRA SAML SSO'yu (V5.2)** seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, Microsoft **tarafından JIRA SAML SSO 'yu seçin (V5.2).**

    ![Microsoft tarafından JIRA SAML SSO (V5.2) Uygulamalar listesinde bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-jira-saml-sso-by-microsoft-v52-test-user"></a>Microsoft (V5.2) test kullanıcısı tarafından JIRA SAML SSO oluşturma

Azure AD kullanıcılarının JIRA şirket içi sunucusunda oturum açabilmeleri için, bunların JIRA şirket içi sunucusunda sağlanması gerekir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jira şirket içi sunucunuzda yönetici olarak oturum açın.

2. Dişli üzerinde hover ve **Kullanıcı yönetimi**tıklayın.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user1.png)

3. **Parola'yı** girmek ve **Onayla** düğmesini tıklatmak için Administrator Access sayfasına yönlendirilirsiniz.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user2.png)

4. **Kullanıcı yönetimi** sekmesi bölümünde, **kullanıcı oluştur'u**tıklatın.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user3.png) 

5. **"Yeni kullanıcı oluştur"** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user4.png)

    a. **E-posta adresi** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

    b. Tam **Ad** metin kutusuna Britta Simon gibi kullanıcının tam adını yazın.

    c. Kullanıcı **adı** metin kutusuna, kullanıcının Brittasimon@contoso.come-postasını yazın.

    d. **Parola** metin kutusuna, kullanıcının parolasını yazın.

    e. **Kullanıcı Oluştur'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Microsoft tarafından Microsoft (V5.2) döşemesi tarafından Microsoft tarafından tıkladığınızda, Otomatik olarak Microsoft (V5.2) tarafından SSO'yu kurduğunuz JIRA SAML SSO'da oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
