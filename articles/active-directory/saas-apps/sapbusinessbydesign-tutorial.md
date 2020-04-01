---
title: 'Öğretici: SAP Business ByDesign ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Business ByDesign arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0b830f0760b768826b2d937b4a8b2ffbd8e9e2a2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67091684"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme

Bu eğitimde, SAP Business ByDesign'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SAP Business ByDesign'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SAP Business ByDesign erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Business ByDesign (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SAP Business ByDesign ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SAP Business ByDesign tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SAP Business **ByDesign, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Galeriden SAP Business ByDesign ekleme

SAP Business ByDesign'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden sap Business ByDesign'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SAP Business ByDesign eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda SAP **Business ByDesign**yazın, sonuç panelinden **SAP Business ByDesign'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![SAP Business ByDesign sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre SAP Business ByDesign ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile SAP Business ByDesign'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmaişlemlerini SAP Business ByDesign ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Sap Business ByDesign Tek Oturum Açma 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-sap-business-bydesign-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SAP Business ByDesign test kullanıcısını oluşturun](#create-sap-business-bydesign-test-user)** - kullanıcının Azure AD gösterimine bağlı SAP Business ByDesign'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini SAP Business ByDesign ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **SAP Business ByDesign** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<servername>.sapbydesign.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<servername>.sapbydesign.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [SAP Business ByDesign Client destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. SAP Business ByDesign uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Ad tanımlayıcı değerini**yeniden leştirmek için **Edit** simgesine tıklayın.

    ![image](media/sapbusinessbydesign-tutorial/mail-prefix1.png)

7. Kullanıcı **taleplerini yönet** bölümünde aşağıdaki adımları ![gerçekleştirin: resim](media/sapbusinessbydesign-tutorial/mail-prefix2.png)

    a. **Kaynak**Olarak **Dönüşüm'u** seçin.

    b. **Dönüşüm** açılır listesinde **ExtractMailPrefix()** seçeneğini belirleyin.

    c. **Parametre 1** açılır listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin. Örneğin, EmployeeID'yi benzersiz kullanıcı tanımlayıcısı olarak kullanmak istiyorsanız ve Öznitelik değerini ExtensionAttribute2'de depolamışsanız, user.extensionattribute2'yi seçin.

    d. **Kaydet**'e tıklayın.

8. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. SAP **Business ByDesign'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-sap-business-bydesign-single-sign-on"></a>SAP Business ByDesign Tek Oturum Açma'yı Yapılandır

1. Yönetici haklarıyla SAP Business ByDesign portalınızda oturum açın.

2. Uygulama **ve Kullanıcı Yönetimi Ortak Görevi'ne** gidin ve **Kimlik Sağlayıcısı** sekmesini tıklatın.

3. **Yeni Kimlik Sağlayıcısı'nı** tıklatın ve Azure portalından indirdiğiniz meta veri XML dosyasını seçin. Sistem, meta verileri içe aktararak gerekli imza sertifikasını ve şifreleme sertifikasını otomatik olarak yükler.

    ![Tek İşaret-On'u Yapılandır](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

4. **İstek'e İddia Tüketici Hizmeti URL'sini** eklemek için, İddia Tüketici Hizmeti URL'sini **ekleyin.**

5. **Tek Oturum Açma'yı Etkinleştir'i**tıklatın.

6. Yaptığınız değişiklikleri kaydedin.

7. Sistem **Hesabım** sekmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

8. AZURE **AD Sign On URL** metin kutusunda, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    ![Tek İşaret-On'u Yapılandır](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

9. Manuel **Kimlik Sağlayıcı Seçimi'ni**seçerek çalışanın kullanıcı kimliği ve parolayla oturum açma veya SSO arasında el ile seçim yapıp yapamayacağını belirtin.

10. **SSO URL** bölümünde, çalışan tarafından sisteme oturum açmak için kullanılması gereken URL'yi belirtin.
    Çalışan açılır listesine Gönderilen URL'de aşağıdaki seçenekler arasında seçim yapabilirsiniz:

    **SSO dışı URL**

    Sistem çalışana yalnızca normal sistem URL'sini gönderir. Çalışan SSO kullanarak oturum alamaz ve bunun yerine parola veya sertifika kullanmalıdır.

    **SSO URL**

    Sistem çalışana yalnızca SSO URL'sini gönderir. Çalışan SSO kullanarak oturum açabilir. Kimlik doğrulama isteği IdP üzerinden yönlendirilir.

    **Otomatik Seçim**

    SSO etkin değilse, sistem normal sistem URL'sini çalışana gönderir. SSO etkinse, sistem çalışanın bir parolası olup olmadığını denetler. Parola varsa, çalışana hem SSO URL'si hem de SSO olmayan URL gönderilir. Ancak, çalışanın şifresi yoksa, çalışana yalnızca SSO URL'si gönderilir.

11. Yaptığınız değişiklikleri kaydedin.

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

Bu bölümde, Britta Simon'ın SAP Business ByDesign'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından SAP **Business ByDesign'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Business ByDesign'ı**seçin.

    ![Uygulamalar listesindeki SAP Business ByDesign bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sap-business-bydesign-test-user"></a>SAP Business ByDesign test kullanıcısı oluşturma

Bu bölümde, SAP Business ByDesign'da Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları SAP Business ByDesign platformuna eklemek için lütfen [SAP Business ByDesign Client destek ekibiyle](https://www.sap.com/products/cloud-analytics.support.html) birlikte çalışın. 

> [!NOTE]
> Lütfen NameID değerinin SAP Business ByDesign platformundaki kullanıcı adı alanıyla eşleşmesi gerektiğinden emin olun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SAP Business ByDesign döşemesini tıklattığınızda, SSO'yu kurduğunuz SAP Business ByDesign'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
