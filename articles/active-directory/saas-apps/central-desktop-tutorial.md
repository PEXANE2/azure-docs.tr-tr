---
title: 'Öğretici: Merkezi Masaüstü ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Merkezi Masaüstü arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fda8e928b530001faeae34c364dfed91d7620f0a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73157519"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Merkezi Masaüstü ile Azure Active Directory entegrasyonu

Bu eğitimde, Merkezi Masaüstü'nü Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Merkezi Masaüstünü Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Merkezi Masaüstü'ne erişimi olan Azure AD'da denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Merkezi Masaüstünde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Merkezi Masaüstü ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Merkezi Masaüstü tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Merkezi Masaüstü **SP** başlatılan SSO destekler

## <a name="adding-central-desktop-from-the-gallery"></a>Galeriden Merkezi Masaüstü Ekleme

Merkezi Masaüstü'nün Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Central Desktop eklemeniz gerekir.

**Galeriden Merkezi Masaüstü eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama **kutusunda, Merkezi Masaüstü**yazın, sonuç panelinden **Merkezi Masaüstü'nü** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesinde Merkezi Masaüstü](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırır ve test emzebilirsiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure AD kullanıcısı ile Central Desktop'daki ilgili kullanıcı arasında bir bağlantı ilişkisikurulması gerekir.

Azure AD oturum açma işlemlerini Central Desktop ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Merkezi Masaüstü Tek Oturum Açma'yı **[yapılandırın.](#configure-central-desktop-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Merkezi Masaüstü test kullanıcısını oluşturun](#create-central-desktop-test-user)** - Kullanıcının Azure AD gösterimine bağlı Merkezi Masaüstü'nde Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Merkezi Masaüstü ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Merkezi Masaüstü** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Merkezi Masaüstü Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.centraldesktop.com`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|
    | |

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.centraldesktop.com/saml2-assertion.php`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Merkezi Masaüstü İstemci destek ekibine](https://imeetcentral.com/contact-us) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Ham)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. Merkezi **Masaüstü'nü Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-central-desktop-single-sign-on"></a>Yapılandırın Merkezi Masaüstü Tek Oturum Açma

1. **Merkezi Masaüstü** kiracınızda oturum açın.

2. **Ayarlar'a**gidin. **Gelişmiş'i**seçin ve ardından **Tek İşaret Açma'yı**seçin.

    ![Kurulum - Gelişmiş](./media/central-desktop-tutorial/ic769563.png "Kurulum - Gelişmiş")

3. Tek **Oturum Açma Ayarları** sayfasında aşağıdaki adımları izleyin:

    ![Tek oturum açma ayarları](./media/central-desktop-tutorial/ic769564.png "Tek İşaret Ayarları")

    a. **SAML v2 Tek İşaretini Etkinleştir'i**seçin.

    b. **SSO URL** kutusuna, Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcı** değerini yapıştırın.

    c. **SSO Giriş URL** kutusuna Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. **SSO Giriş URL** kutusuna, Azure portalından kopyaladığınız **Logout URL** değerini yapıştırın.

4. İleti **İmza Doğrulama Yöntemi** bölümünde aşağıdaki adımları izleyin:

    ![İleti imzası doğrulama yöntemi](./media/central-desktop-tutorial/ic769565.png "İleti İmza Doğrulama Yöntemi")
    
    a. **Sertifika**’yı seçin.

    b. **SSO Sertifika** listesinde **RSH SHA256'yı**seçin.

    c. İndirilen sertifikanızı Notepad'de açın. Ardından sertifikanın içeriğini kopyalayın ve **SSO Sertifika** alanına yapıştırın.

    d. **SAMLv2 giriş sayfanıza bir bağlantı görüntüle'yi**seçin.

    e. **Güncelleştir** seçeneğini belirleyin.

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

Bu bölümde, Britta Simon'ın Merkezi Masaüstü'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Merkezi Masaüstü'nü**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Merkezi Masaüstü'nü**seçin.

    ![Uygulamalar listesindeki Merkezi Masaüstü bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-central-desktop-test-user"></a>Merkezi Masaüstü test kullanıcıoluşturma

Azure AD kullanıcılarının oturum açabilmesi için, bunların Merkezi Masaüstü uygulamasında sağlanması gerekir. Bu bölümde, Merkezi Masaüstü'nde Azure AD kullanıcı hesaplarının nasıl oluşturulutamamolduğu açıklanmaktadır.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için, Merkezi Masaüstü tarafından sağlanan diğer Merkezi Masaüstü kullanıcı hesabı oluşturma araçlarını veya API'leri kullanabilirsiniz.

**Kullanıcı hesaplarını Merkezi Masaüstü'ne sağlamak için:**

1. Merkezi Masaüstü kiracınızda oturum açın.

2. **Kişiler'i** seçin ve ardından **Dahili Üye Ekle'yi**seçin.

    ![People](./media/central-desktop-tutorial/ic781051.png "People")

3. Yeni **Üyelerin E-posta Adresi** kutusuna, sağlamak istediğiniz bir Azure REKLAM hesabı yazın ve ardından **İleri'yi**seçin.

    ![Yeni üyelerin e-posta adresleri](./media/central-desktop-tutorial/ic781052.png "Yeni üyelerin e-posta adresleri")

4. **Dahili üye(ler) ekle'yi**seçin.

    ![Dahili üye ekleme](./media/central-desktop-tutorial/ic781053.png "Dahili üye ekleme")
  
   > [!NOTE]
   > Eklediğiniz kullanıcılar, hesaplarını etkinleştirme için onay bağlantısı içeren bir e-posta alır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Merkezi Masaüstü döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Merkezi Masaüstünde oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
