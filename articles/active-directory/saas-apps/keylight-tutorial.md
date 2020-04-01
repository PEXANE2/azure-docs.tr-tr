---
title: 'Öğretici: LockPath Keylight ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LockPath Keylight arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 148c2c46a911088d01ab83fe2d16e8ca81d272ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67098783"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Öğretici: LockPath Keylight ile Azure Active Directory entegrasyonu

Bu eğitimde, LockPath Keylight'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
LockPath Keylight'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* LockPath Keylight erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla LockPath Keylight (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini LockPath Keylight ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* LockPath Keylight tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* LockPath Keylight **SP** başlatılan SSO destekler
* LockPath Keylight **Just In Time** kullanıcı sağlama destekler

## <a name="adding-lockpath-keylight-from-the-gallery"></a>Galeriden LockPath Keylight ekleme

LockPath Keylight'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize LockPath Keylight eklemeniz gerekir.

**Galeriden LockPath Keylight eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **LockPath Keylight**yazın, sonuç panelinden **LockPath Keylight'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuçlar listesinde LockPath Keylight](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini LockPath Keylight ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile LockPath Keylight'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini LockPath Keylight ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[LockPath Keylight Tek İşaret-On](#configure-lockpath-keylight-single-sign-on)** 'u yapıla - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[LockPath Keylight test kullanıcısını oluşturun](#create-lockpath-keylight-test-user)** - Kullanıcının Azure AD gösterimine bağlı LockPath Keylight'ta Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini LockPath Keylight ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Kilit Yolu Tuş Işığı** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![LockPath Keylight Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.keylightgrc.com/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.keylightgrc.com`

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.keylightgrc.com/Login.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL'de gerçek Oturum Aç, Tanımlayıcı ve YanıtURL'si ile güncelleştirin. Bu değerleri almak için [LockPath Keylight İstemci destek ekibine](https://www.lockpath.com/contact/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Ham)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. **LockPath Keylight'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-lockpath-keylight-single-sign-on"></a>LockPath Keylight Tek İşaretli Yapıla

1. SSO'yu LockPath Keylight'ta etkinleştirmek için aşağıdaki adımları gerçekleştirin:

    a. LockPath Keylight hesabınızda yönetici olarak oturum açın.

    b. Üstteki menüde **Kişi'yi**tıklatın ve **Keylight Kurulumu'nu**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/keylight-tutorial/401.png)

    c. Soldaki ağaç görünümünde **SAML'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/keylight-tutorial/402.png)

    d. **SAML Ayarları** iletişim **kutusunda, Edit'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/keylight-tutorial/404.png)

1. **SAML Ayarlarını Edit** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/keylight-tutorial/405.png)

    a. **SAML kimlik doğrulamasını** **Active**olarak ayarlayın.

    b. Kimlik **Sağlayıcı Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. Kimlik **Sağlayıcı Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. İndirilen LockPath Keylight sertifikanızı seçmek için **Dosyayı Seçin'i** tıklatın ve ardından sertifikayı yüklemek için **Aç'ı** tıklatın.

    e. **SAML Kullanıcı Kimliği konumunu** **konu deyiminin NameIdentifier öğesi olarak**ayarlayın.

    f. **Keylight Servis Sağlayıcısı'nı** aşağıdaki `https://<CompanyName>.keylightgrc.com`deseni kullanarak sağlayın: .

    g. **Otomatik sağlama kullanıcılarını** **Etkin**olarak ayarlayın.

    h. **Otomatik sağlama hesap türünü** Tam **Kullanıcı**olarak ayarlayın.

    i. **Otomatik sağlama güvenlik rolünü**ayarlayın, **SAML ile Standart Kullanıcı'yı**seçin.

    j. **Otomatik sağlama güvenlik config'ini**ayarlayın, Standart Kullanıcı **Yapılandırması'nı**seçin.

    k. **E-posta özniteliği** textbox'ına `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    l. Ad **özniteliği** textbox'ına `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`, yazın.

    m. Soyadı **özniteliği** textbox,. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    n. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın LockPath Keylight'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **LockPath Keylight'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **LockPath Keylight'ı**seçin.

    ![Uygulamalar listesindeki LockPath Keylight bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-lockpath-keylight-test-user"></a>LockPath Keylight test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı LockPath Keylight oluşturulur. LockPath Keylight, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı LockPath Keylight'ta zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur. Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [LockPath Keylight İstemci destek ekibine](https://www.lockpath.com/contact/)başvurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LockPath Keylight döşemesini tıklattığınızda, SSO'yu kurduğunuz LockPath Keylight'ta otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)