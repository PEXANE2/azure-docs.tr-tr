---
title: 'Öğretici: Adobe Sign ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Adobe Sign arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 788c1292b844088f171479b40fc566ff5cfc8a57
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73154032"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Öğretici: Adobe Sign ile Azure Active Directory entegrasyonu

Bu eğitimde, Adobe Sign'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Adobe Sign'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Adobe Sign erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Adobe Sign 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Adobe Sign ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Adobe Sign tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Adobe **Sign, SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-adobe-sign-from-the-gallery"></a>Galeriden Adobe Sign ekleme

Adobe Sign'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Adobe Sign eklemeniz gerekir.

**Galeriden Adobe Sign eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Adobe Sign**yazın, sonuç panelinden **Adobe Sign'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Adobe Sign sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Adobe Sign ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Adobe Sign'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumaçma işlemlerini Adobe Sign ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Adobe Sign Single Sign-On](#configure-adobe-sign-single-sign-on)** 'u yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Adobe Sign'da](#create-adobe-sign-test-user)** Britta Simon'ın kullanıcının Azure REKLAM gösterimine bağlı bir örneğine sahip olmak için Adobe Sign test kullanıcısını oluşturun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Adobe Sign ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Adobe Sign** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Adobe Sign Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.echosign.com/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.echosign.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Adobe Sign Client destek ekibine](https://helpx.adobe.com/in/contact/support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Adobe **Sign'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-adobe-sign-single-sign-on"></a>Adobe Sign Tek İşareti'ni Yapılandırma

1. Yapılandırmadan önce, alan adınızı Adobe Sign izin listesine eklemek için [Adobe Sign Client destek ekibine](https://helpx.adobe.com/in/contact/support.html) başvurun. Etki alanını şu şekilde ekleyebilirsiniz:

    a. [Adobe Sign Client destek ekibi](https://helpx.adobe.com/in/contact/support.html) size rasgele oluşturulmuş bir belirteç gönderir. Etki alanınız için belirteç aşağıdaki gibi olacaktır: **adobe-sign-verification= xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Doğrulama belirteci'ni Bir DNS metin kaydında yayımlayın ve [Adobe Sign Client destek ekibine](https://helpx.adobe.com/in/contact/support.html)bildirin.
    
    > [!NOTE]
    > Bu işlem birkaç gün veya daha uzun sürebilir. DNS yayılma gecikmelerinin, DNS'de yayınlanan bir değerin bir saat veya daha uzun süre görünemeyen anlamına geldiğini unutmayın. BT yöneticiniz, bu belirteci bir DNS metin kaydında nasıl yayımlayacağı konusunda bilgili olmalıdır.
    
    c. Adobe Sign [Client destek ekibini](https://helpx.adobe.com/in/contact/support.html) destek bileti aracılığıyla bilgilendirdiğinizde, belirteç yayımlandıktan sonra etki alanını doğrular ve hesabınıza eklerler.
    
    d. Genellikle, belirteci Bir DNS kaydında şu şekilde yayımlayabilirsiniz:

    * Etki alanı hesabınızda oturum açın
    * DNS kaydını güncelleştirmek için sayfayı bulun. Bu sayfa, DNS Yönetimi, Ad Sunucusu Yönetimi veya Gelişmiş Ayarlar olarak adlandırılabilir.
    * Etki alanınızın TXT kayıtlarını bulun.
    * Adobe tarafından sağlanan tam belirteç değeriyle birlikte bir TXT kaydı ekleyin.
    * Yaptığınız değişiklikleri kaydedin.

1. Farklı bir web tarayıcısı penceresinde, Adobe Sign şirket sitenizde yönetici olarak oturum açın.

1. SAML menüsünde **Hesap Ayarları** > **SAML Ayarlarını**seçin.
   
    ![Adobe Sign SAML Ayarları sayfasının ekran görüntüsü](./media/adobe-echosign-tutorial/ic789520.png "Hesap")

1. **SAML Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:
  
   ![SAML Ayarları ekran görüntüsü](./media/adobe-echosign-tutorial/ic789521.png "SAML Ayarları")
   
   ![SAML Ayarları ekran görüntüsü](./media/adobe-echosign-tutorial/ic789522.png "SAML Ayarları")

   a. **SAML Modu**altında, **SAML Zorunlu**seçin.
   
   b. **Echosign Hesap Yöneticilerinin Echosign Kimlik Bilgilerini kullanarak oturum açmasına izin ver'i**seçin.
   
   c. **Kullanıcı Oluşturma**altında, OTOMATIK **olarak SAML üzerinden kimlik doğrulaması kullanıcılar ekleyin**seçin.

   d. Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcısını** **Idp Entity ID** metin kutusuna yapıştırın.
    
   e. Azure portalından kopyaladığınız **Giriş URL'sini** **Idp Giriş URL** metin kutusuna yapıştırın.
   
   f. Azure portalından kopyaladığınız **Logout URL'yi** **Idp Logout URL** metin kutusuna yapıştırın.

   g. İndirilen **Sertifika(Base64)** dosyanızı Notepad'de açın. İçeriğini panonuza kopyalayın ve **Ardından IdP Sertifikası** metin kutusuna yapıştırın.

   h. **Değişiklikleri Kaydet'i**seçin.

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

Bu bölümde, Britta Simon'ın Adobe Sign'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Adobe Sign'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Adobe Sign**yazın ve seçin.

    ![Uygulamalar listesindeki Adobe Sign bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-adobe-sign-test-user"></a>Adobe Sign test kullanıcısını oluşturma

Azure AD kullanıcılarının Adobe Sign'da oturum açabilmeleri için Adobe Sign'da oturum açmaları gerekir. Bu el ile bir görevdir.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Adobe Sign tarafından sağlanan diğer Adobe Sign kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz. 

1. **Adobe Sign** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **Hesap'ı**seçin. Daha sonra, sol bölmede, **Kullanıcılar & Grupları** > **yeni bir kullanıcı oluşturun'ı**seçin.
   
    ![Hesap, Kullanıcılar &Grupları ile Adobe Sign şirket sitesinin ekran görüntüsü ve vurgulanan yeni bir kullanıcı oluşturma](./media/adobe-echosign-tutorial/ic789524.png "Hesap")
   
3. Yeni **Kullanıcı Oluştur** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Yeni Kullanıcı Oluştur bölümünün ekran görüntüsü](./media/adobe-echosign-tutorial/ic789525.png "Kullanıcı Oluştur")
   
    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **E-posta Adresini,** **Adını**ve **Soyadını** yazın.
   
    b. **Kullanıcı Oluştur'u**seçin.

>[!NOTE]
>Azure Etkin Dizin hesabı sahibi, etkin hale gelmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Adobe İşaret iüzereği'ni tıklattığınızda, SSO'yu kurduğunuz Adobe Sign'da otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

