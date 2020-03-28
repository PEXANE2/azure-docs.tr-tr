---
title: 'Öğretici: Kintone ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Kintone arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 6786b44aca9ceed3cec5daf0f858a51e2dd12833
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227573"
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Öğretici: Kintone ile Azure Active Directory entegrasyonu

Bu eğitimde, Kintone'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Kintone'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Kintone erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Kintone'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Kintone ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Kintone tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Kintone **SP** başlatılan SSO destekler

## <a name="adding-kintone-from-the-gallery"></a>Galeriden Kintone ekleme

Kintone'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Kintone'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Kintone eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Kintone**yazın, sonuç panelinden **Kintone'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Kintone](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD'nin Kintone ile **britta Simon**adlı bir test kullanıcısına göre tek oturum açma işlemini yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Kintone'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Kintone ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Kintone Tek Oturum](#configure-kintone-single-sign-on)** Açma 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Kintone test kullanıcısını oluşturun](#create-kintone-test-user)** - Kintone'daki Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumlarını Kintone ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Kintone** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kintone Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.kintone.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://<companyname>.cybozu.com` |
    | `https://<companyname>.kintone.com` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Kintone İstemci destek ekibine](https://www.kintone.com/contact/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Kintone'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-kintone-single-sign-on"></a>Kintone Tek İşaretle Yapıla

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak **Kintone** şirket sitenizde oturum açın.

1. **Ayarlar simgesine**tıklayın.

    ![Ayarlar](./media/kintone-tutorial/ic785879.png "Ayarlar")

1. **Sistem Yönetimi & Kullanıcıları**tıklatın.

    ![Sistem Yönetimi & Kullanıcılar](./media/kintone-tutorial/ic785880.png "Sistem Yönetimi & Kullanıcılar")

1. **Sistem İdaresi \> Güvenlik** altında **Giriş**'i tıklatın.

    ![Oturum açma](./media/kintone-tutorial/ic785881.png "Oturum Aç")

1. **SAML kimlik doğrulamasını etkinleştir'i**tıklatın.

    ![SAML Kimlik Doğrulama](./media/kintone-tutorial/ic785882.png "SAML Kimlik Doğrulama")

1. SAML Kimlik Doğrulama bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Kimlik Doğrulama](./media/kintone-tutorial/ic785883.png "SAML Kimlik Doğrulama")

    a. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    b. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    c. İndirilen sertifika dosyanızı Azure portalından yüklemek için **Gözat'ı** tıklatın.

    d. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Kintone'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Kintone'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Kintone'u**seçin.

    ![Uygulamalar listesindeki Kintone bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-kintone-test-user"></a>Kintone test kullanıcısı oluşturma

Azure AD kullanıcılarının Kintone'da oturum açabilmeleri için Kintone'de oturum açmaları gerekir. Kintone durumunda, sağlama manuel bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Yönetici olarak **Kintone** şirket sitenizde oturum açın.

1. **Ayarlar simgesine**tıklayın.

    ![Ayarlar](./media/kintone-tutorial/ic785879.png "Ayarlar")

1. **Sistem Yönetimi & Kullanıcıları**tıklatın.

    ![Kullanıcı & Sistem Yönetimi](./media/kintone-tutorial/ic785880.png "Kullanıcı & Sistem Yönetimi")

1. **Kullanıcı Yönetimi**altında, & **Kullanıcılar Bölümleri**tıklatın.

    ![Bölüm & Kullanıcıları](./media/kintone-tutorial/ic785888.png "Bölüm & Kullanıcıları")

1. **Yeni Kullanıcı'yı**tıklatın.

    ![Yeni Kullanıcılar](./media/kintone-tutorial/ic785889.png "Yeni Kullanıcılar")

1. Yeni **Kullanıcı** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcılar](./media/kintone-tutorial/ic785890.png "Yeni Kullanıcılar")

    a. Bir **Görüntü Adı,** **Giriş Adı,** **Yeni Parola,** **Parolayı Onayla,** **E-posta Adresi**ve ilgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının diğer ayrıntılarını yazın.

    b. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Kintone tarafından sağlanan diğer Kintone kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Kintone döşemesini tıklattığınızda, SSO'yu kurduğunuz Kintone'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
