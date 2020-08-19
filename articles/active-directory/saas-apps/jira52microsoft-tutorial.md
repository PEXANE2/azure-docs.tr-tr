---
title: "Öğretici: Microsoft tarafından JıRA SAML SSO 'SU ile tümleştirme Azure Active Directory (V 5.2) | Microsoft Docs"
description: Azure Active Directory ve JIRA SAML SSO arasında Microsoft (V 5.2) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/22/2019
ms.author: jeedes
ms.openlocfilehash: e0198fdcfea1656e3aec5179358e69fb6fb55723
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88547591"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft-v52"></a>Öğretici: Microsoft tarafından JıRA SAML SSO 'SU ile tümleştirme Azure Active Directory (V 5.2)

Bu öğreticide, JIRA SAML SSO 'yu Microsoft (V 5.2) ile Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Azure AD ile JIRA SAML SSO 'SU ile Microsoft (V 5.2) tümleştirmesi aşağıdaki avantajları sağlar:

* Azure AD 'de, Microsoft (V 5.2) ile JIRA SAML SSO 'ya erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Microsoft (V 5.2) (çoklu oturum açma) ile JIRA SAML SSO 'SU için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="description"></a>Açıklama

Çoklu oturum açmayı etkinleştirmek için Microsoft Azure Active Directory hesabınızı Atlaseli JIRA sunucusu ile kullanın. Bu sayede, tüm kuruluşunuz kullanıcıları JIRA uygulamasında oturum açmak için Azure AD kimlik bilgilerini kullanabilir. Bu eklenti, Federasyon için SAML 2,0 kullanır.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Microsoft (V 5.2) ile JIRA SAML SSO 'SU ile yapılandırmak için aşağıdaki öğeler gereklidir:

- Azure AD aboneliği
- JIRA Core ve Software 5,2 Windows 64 bit sürümüne yüklenip yapılandırılmalıdır
- JıRA sunucusu HTTPS etkin
- Aşağıdaki bölümde, JIRA eklentisinin desteklenen sürümlerinin bahsedildiğini aklınızda bulabilirsiniz.
- JıRA sunucusuna, kimlik doğrulaması için özellikle Azure AD oturum açma sayfasında internet 'ten erişilebilir ve bu belirteci Azure AD 'den alabilmelidir
- Yönetim kimlik bilgileri JIRA 'da ayarlanır
- WebSudo, JIRA 'da devre dışı
- JIRA sunucu uygulamasında oluşturulan test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için JIRA üretim ortamının kullanılmasını önermiyoruz. Uygulamanın geliştirme veya hazırlama ortamında önce tümleştirmeyi test edin ve ardından üretim ortamını kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadığı takdirde üretim ortamınızı kullanmayın.
- Bir Azure AD deneme ortamınız yoksa, buradan bir aylık deneme sürümü edinebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>Desteklenen JIRA sürümleri

* JIRA Core ve Software: 5,2
* JIRA Ayrıca 6,0 7,12 ' i destekler. Daha fazla ayrıntı için, [Microsoft tarafından JIRA SAML SSO 'su](jiramicrosoft-tutorial.md) ' ne tıklayın

> [!NOTE]
> JıRA eklentisinin Ubuntu sürüm 16,04 ' de çalıştığından emin olun

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Microsoft (V 5.2) tarafından desteklenen JIRA SAML SSO, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-jira-saml-sso-by-microsoft-v52-from-the-gallery"></a>Galeriden Microsoft (V 5.2) ile JıRA SAML SSO 'SU ekleme

JıRA SAML SSO 'yu Microsoft (V 5.2) ile Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Microsoft (V 5.2) ile JıRA SAML SSO 'SU eklemeniz gerekir.

**Galeriden Microsoft (V 5.2) ile JıRA SAML SSO 'SU eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Microsoft (v 5.2) Ile JıRA SAML SSO**yazın, sonuç panelinden **JıRA SAML SSO 'Su Microsoft (v 5.2)** seçeneğini belirleyin ve ardından uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde Microsoft (V 5.2) ile JıRA SAML SSO 'SU](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Microsoft (v 5.2) Ile bırlıkte Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Microsoft (V 5.2) ile ilgili bir SAML SSO 'SU içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Microsoft (V 5.2) ile JIRA SAML SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, **[JıRA SAML SSO 'Yu Microsoft (v 5.2) Ile çoklu oturum açma 'Yı yapılandırın](#configure-jira-saml-sso-by-microsoft-v52-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Microsoft (v 5.2) ile Jira SAML SSO 'SU için Microsoft **[(v 5.2) Ile JıRA SAML SSO 'Su oluşturun](#create-jira-saml-sso-by-microsoft-v52-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Microsoft (V 5.2) ile JIRA SAML SSO 'SU ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **JIRA SAML SSO 'su Ile Microsoft (v 5.2)** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![JIRA SAML SSO 'SU Microsoft (V 5.2) etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<domain:port>/`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Adlandırılmış bir URL olması durumunda bağlantı noktası isteğe bağlıdır. Bu değerler, Öğreticinin ilerleyen kısımlarında açıklanan Jira eklentisinin yapılandırması sırasında alınır.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-jira-saml-sso-by-microsoft-v52-single-sign-on"></a>JIRA SAML SSO 'yu Microsoft (V 5.2) ile çoklu oturum açma ile yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, JIRA örneğiniz için yönetici olarak oturum açın.

2. Dişli üzerine gelin ve **eklentilere**tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/jira52microsoft-tutorial/addon1.png)

3. Eklentiler sekmesi bölümünde eklentileri **Yönet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/jira52microsoft-tutorial/addon7.png)

4. Eklentiyi [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=56521)' nden indirin. **Yükleme** eklentisi menüsünü kullanarak Microsoft tarafından sunulan eklentiyi el ile karşıya yükleyin. Eklenti indirmesi, [Microsoft hizmet sözleşmesi](https://www.microsoft.com/servicesagreement/)kapsamında ele alınmıştır.

    ![Çoklu oturum açmayı yapılandırma](./media/jira52microsoft-tutorial/addon12.png)

5. Eklenti yüklendikten sonra, **Kullanıcı tarafından yüklenen** eklentiler bölümünde görünür. Yeni eklentiyi yapılandırmak için **Yapılandır** ' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/jira52microsoft-tutorial/addon13.png)

6. Yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/jira52microsoft-tutorial/addon52.png)

    > [!TIP]
    > Meta verileri çözümlemede bir hata olmadığından, uygulamaya yönelik yalnızca bir sertifika eşlendiğinden emin olun. Birden çok sertifika varsa, meta veriler çözümlendikten sonra yönetici bir hata alır.

    a. **Meta veri URL 'si** metin kutusunda, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'si** değerini yapıştırın ve **Çözümle** düğmesine tıklayın. IDP meta veri URL 'sini okur ve tüm alan bilgilerini doldurur.

    b. Azure portal ' daki **temel SAML yapılandırması** bölümünde tanımlayıcıyı kopyalayın, URL 'yi **yanıtlayın ve URL** değerlerini **tanımlayıcı, Yanıtla URL 'si ve URL** metin kutularına yapıştırın.

    c. **Oturum açma düğmesi adı** ' nda, kuruluşunuzun oturum açma ekranında görmesini istediği düğmenin adını yazın.

    d. **SAML Kullanıcı kimliği konumlarında** , **Konu bildiriminin NameIdentifier öğesinde kullanıcı kimliği '** ni veya **Kullanıcı kimliği ' ni bir öznitelik öğesinde**seçin.  Bu KIMLIK, JIRA Kullanıcı KIMLIĞI olmalıdır. Kullanıcı KIMLIĞI eşleşmiyorsa, sistem kullanıcıların oturum açmalarına izin vermez.

    > [!Note]
    > Varsayılan SAML Kullanıcı KIMLIĞI konumu ad tanımlayıcısıdır. Bunu bir öznitelik seçeneği olarak değiştirebilir ve uygun öznitelik adını girebilirsiniz.

    e. **Bir öznitelik öğesi seçeneğinde Kullanıcı kimliği** ' ni seçerseniz, **öznitelik adı** metin kutusuna kullanıcı kimliğinin beklenildiği özniteliğin adını yazın. 

    f. Federasyon etki alanını (örneğin, ADFS vb.) Azure AD ile kullanıyorsanız, **giriş bölgesi bulmayı etkinleştir** seçeneğini tıklayın ve **etki alanı adını**yapılandırın.

    örneğin: **Etki alanı adı** ' nda, ADFS tabanlı oturum açma durumunda etki alanı adını buraya yazın.

    h. Kullanıcı JIRA oturumunu kapattığında Azure AD 'den oturumu kapatmak istiyorsanız **Çoklu oturum açmayı etkinleştir** ' i işaretleyin. 

    i. Ayarları kaydetmek için **Kaydet** düğmesine tıklayın.

    > [!NOTE]
    > Yükleme ve sorun giderme hakkında daha fazla bilgi için, [MS JIRA SSO bağlayıcı yönetici kılavuzunu](../ms-confluence-jira-plugin-adminguide.md) ziyaret edin ve yardım Için de [SSS](../ms-confluence-jira-plugin-faq.md) vardır

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon\@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com.

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Microsoft (V 5.2) ile JıRA SAML SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Microsoft (v 5.2) ile JIRA SAML SSO 'su**' nu seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **JIRA SAML SSO 'Su Microsoft (v 5.2)** öğesini seçin.

    ![Uygulamalar listesindeki Microsoft (V 5.2) ile JIRA SAML SSO bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-jira-saml-sso-by-microsoft-v52-test-user"></a>Microsoft (V 5.2) test kullanıcısı tarafından JıRA SAML SSO 'SU oluşturma

Azure AD kullanıcılarının JIRA şirket içi sunucusunda oturum açmasını sağlamak için, bu kullanıcıların JIRA on-premises Server 'a sağlanması gerekir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuzda yönetici olarak oturum açın.

2. Dişli 'ye gelin ve **Kullanıcı yönetimine**tıklayın.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user1.png)

3. **Parola** girmek Için yönetici erişimi sayfasına yönlendirilirsiniz ve **Onayla** düğmesine tıklayın.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user2.png)

4. **Kullanıcı yönetimi** sekmesi bölümünde **Kullanıcı oluştur**' a tıklayın.

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user3.png) 

5. **"Yeni Kullanıcı Oluştur"** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/jira52microsoft-tutorial/user4.png)

    a. **E-posta adresi** metin kutusuna, gibi kullanıcının e-posta adresini yazın Brittasimon@contoso.com .

    b. **Tam ad** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. Kullanıcı **adı** metin kutusuna, gibi kullanıcının e-postasını yazın Brittasimon@contoso.com .

    d. **Parola** metin kutusuna kullanıcının parolasını yazın.

    e. **Kullanıcı oluştur**' a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde JIRA SAML SSO 'SU Microsoft (V 5.2) kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Microsoft (V 5.2) ile JıRA SAML SSO 'SU için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
