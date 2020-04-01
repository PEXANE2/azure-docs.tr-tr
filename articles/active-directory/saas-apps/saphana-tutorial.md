---
title: 'Öğretici: SAP HANA ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP HANA arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44a34fe5637e895ea69b6fc4c277b7722b306c97
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161190"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Öğretici: SAP HANA ile Azure Active Directory entegrasyonu

Bu eğitimde, SAP HANA'yı Azure Etkin Dizin (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SAP HANA'yı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SAP HANA erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP HANA'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SAP HANA ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- Tek oturum açma (SSO) etkin bir SAP HANA aboneliği
- Azure'daki herkese açık IaaS, şirket içi, Azure VM veya SAP büyük örneklerinde çalışan bir HANA örneği
- XSA İdaresi web arayüzü yanı sıra HANA Studio HANA örneğin yüklü

> [!NOTE]
> Bu öğreticideki adımları test etmek için SAP HANA'nın üretim ortamını kullanmanızı önermiyoruz. Önce uygulamanın geliştirme veya evreleme ortamında tümleştirmeyi test edin ve ardından üretim ortamını kullanın.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izleyin:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* SAP HANA tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SAP **HANA, IDP'nin** başlattığı SSO'ya destek verdi
* SAP HANA **tam zamanında** kullanıcı sağlama yı destekler

## <a name="adding-sap-hana-from-the-gallery"></a>Galeriden SAP HANA ekleme

SAP HANA'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden SAP HANA'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SAP HANA eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda SAP **HANA**yazın, sonuç panelinden **SAP HANA'yı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![SAP HANA sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre SAP HANA ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile SAP HANA'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı SAP HANA ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Sap HANA Tek Oturum Açma 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-sap-hana-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SAP HANA test kullanıcısını oluşturun](#create-sap-hana-test-user)** - kullanıcının Azure AD gösterimine bağlı SAP HANA'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini SAP HANA ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. AZURE [portalında,](https://portal.azure.com/) **SAP HANA** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![SAP HANA Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna aşağıdakileri yazın:`HA100`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [SAP HANA İstemci destek ekibine](https://cloudplatform.sap.com/contact.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. SAP HANA uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri & Talepler** iletişim kutusundaki Kullanıcı **öznitelikleri** bölümünde aşağıdaki adımları gerçekleştirin:
 
    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için **Edit simgesini** tıklatın.

    ![image](./media/saphana-tutorial/tutorial_usermail.png)

    ![image](./media/saphana-tutorial/tutorial_usermailedit.png)

    b. **Dönüşüm** listesinden **ExtractMailPrefix()** seçeneğini belirleyin.

    c. **Parametre 1** listesinden **user.mail'i**seçin.

    d. **Kaydet**'e tıklayın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-sap-hana-single-sign-on"></a>SAP HANA Tek Oturum Açma'yı Yapılandır

1. SAP HANA tarafında tek oturum açma yapılandırmak için, ilgili HTTPS bitiş noktasına giderek **HANA XSA Web Konsolunuzda** oturum açın.

    > [!NOTE]
    > Varsayılan yapılandırmada, URL isteği kimlik doğrulaması sap HANA veritabanı kullanıcısının kimlik bilgilerini gerektiren oturum açma ekranına yönlendirir. İmzalayan kullanıcının SAML yönetim görevlerini gerçekleştirmek için izinleri olmalıdır.

2. XSA Web Arabiriminde **SAML Identity Provider**adresine gidin. Buradan, Kimlik **+** **Sağlayıcı Bilgileri Ekle** bölmesini görüntülemek için ekranın altındaki düğmeyi seçin. Ardından aşağıdaki adımları izleyin:

    ![Kimlik Sağlayıcı Ekle](./media/saphana-tutorial/sap1.png)

    a. Kimlik **Sağlayıcı Ekle** bölmesi'nde, Metadata XML'nin (Azure portalından indirdiğiniz) içeriğini **Meta veri** kutusuna yapıştırın.

    ![Kimlik Sağlayıcı ayarları ekleme](./media/saphana-tutorial/sap2.png)

    b. XML belgesinin içeriği geçerliyse, ayrıştırma işlemi **Genel veri** ekranı alanındaki **Konu, Varlık Kimliği ve Veren** alanları için gerekli olan bilgileri ayıklar. Ayrıca, **Hedef** ekran alanındaki URL alanları için gerekli olan bilgileri (örneğin, **Temel URL ve SingleSignOn URL (*)** alanları ayıklar.

    ![Kimlik Sağlayıcı ayarları ekleme](./media/saphana-tutorial/sap3.png)

    c. **Genel Veri** ekran alanının **Ad** kutusuna, yeni SAML SSO kimlik sağlayıcısının adını girin.

    > [!NOTE]
    > SAML IDP'nin adı zorunludur ve benzersiz olmalıdır. Kullanılacak SAP HANA XS uygulamalarıiçin kimlik doğrulama yöntemi olarak SAML'i seçtiğinizde görüntülenen kullanılabilir SAML IDP'leri listesinde görünür. Örneğin, bunu XS Yapı Yönetimi aracının **Kimlik Doğrulama** ekran alanında yapabilirsiniz.

3. SAML kimlik sağlayıcısının ayrıntılarını kaydetmek ve yeni SAML IDP'sini bilinen SAML IDP'leri listesine eklemek için **Kaydet'i** seçin.

    ![Kaydet düğmesi](./media/saphana-tutorial/sap4.png)

4. HANA Studio'da, **Yapılandırma** sekmesinin sistem özellikleri içinde, **ayarları saml'e**göre filtreleyin. Daha sonra **assertion_timeout** **10 sn ile 120 sn** arasında ayarlayın. **120 sec**

    ![assertion_timeout ayarı](./media/saphana-tutorial/sap7.png)

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

Bu bölümde, Britta Simon'ın SAP HANA'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **SAP HANA'yı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP HANA**yazın ve seçin.

    ![Uygulamalar listesindeki SAP HANA bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sap-hana-test-user"></a>SAP HANA test kullanıcısı oluşturma

Azure AD kullanıcılarının SAP HANA'da oturum açmasını sağlamak için bunları SAP HANA'da sağlamanız gerekir.
SAP HANA, varsayılan olarak etkinleştirilen **tam zamanında sağlamayı**destekler.

Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

>[!NOTE]
>Kullanıcının kullandığı dış kimlik doğrulamasını değiştirebilirsiniz. Onlar Kerberos gibi harici bir sistem ile kimlik doğrulaması yapabilirsiniz. Dış kimlikler hakkında ayrıntılı bilgi için [etki alanı yöneticinize](https://cloudplatform.sap.com/contact.html)başvurun.

1. SAP [HANA Studio'yu](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) yönetici olarak açın ve saml SSO için DB-User'ı etkinleştirin.

    ![Kullanıcı oluştur](./media/saphana-tutorial/sap5.png)

2. **SAML'nin**solundaki görünmez onay kutusunu seçin ve sonra **Yapıla** bağlantısını seçin.

3. SAML IDP eklemek için **Ekle'yi** seçin.  Uygun SAML IDP'yi seçin ve ardından **Tamam'ı**seçin.

4. Dış **Kimlik** ekleyin (bu durumda, BrittaSimon) veya **Herhangi**birini seçin. Sonra **Tamam**’ı seçin.

   > [!Note]
   > **Herhangi** bir onay kutusu seçili değilse, HANA'daki kullanıcı adının etki alanı sonekinden önce UPN'deki kullanıcının adıyla tam olarak eşleşmesi gerekir. (Örneğin, BrittaSimon@contoso.com HANA BrittaSimon olur.)

5. Sınama amacıyla, tüm **XS** rollerini kullanıcıya atayın.

    ![Roller atama](./media/saphana-tutorial/sap6.png)

    > [!TIP]
    > Yalnızca kullanım durumlarınıza uygun izinler vermelisiniz.

6. Kullanıcıyı kaydedin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki SAP HANA döşemesini tıklattığınızda, SSO'yu kurduğunuz SAP HANA'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

