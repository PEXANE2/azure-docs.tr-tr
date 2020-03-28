---
title: 'Öğretici: Bonusly ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Bonusly arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea9c88f8eb8ac7b72f11ff286d2294df8cb70860
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74232054"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Öğretici: Bonusly ile Azure Active Directory entegrasyonu

Bu eğitimde, Azure Etkin Dizini (Azure AD) ile Bonus olarak nasıl entegre acağınızı öğrenirsiniz.
Azure AD ile Bonus'u tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Bonus'a erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla otomatik olarak Bonusly (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Bonusly ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bonus olarak tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **İkA'nın** başlattığı SSO'ya bonus destek

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden Bonusly ekleme

Bonus'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Bonus olarak eklemeniz gerekir.

**Galeriden Bonus ly eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Bonusly**yazın , sonuç panelinden **Bonusly'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde bonus olarak](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Bonusly ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Bonusly'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi Bonusly ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Bonusly Single Sign-On](#configure-bonusly-single-sign-on)** 'u yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Bonus test kullanıcısı oluşturun](#create-bonusly-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Bonusly'de Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma özelliğini Bonusly ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Bonusly** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Bonusly Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-reply.png)

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Yanıt URL'si ile güncelleştirin. Değeri almak için [Bonusly Client destek ekibine](https://bonus.ly/contact) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

6. **SAML İmza Sertifikası** **bölümünde, THUMBPRINT'i** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

7. **Bonus'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-bonusly-single-sign-on"></a>Bonus Ly Tek Oturum Açma'yı Yapılandır

1. Farklı bir tarayıcı penceresinde, **Bonusly** kiracınızla oturum açın.

1. Üstteki araç çubuğunda **Ayarlar'ı** tıklatın ve ardından **Tümleştirmeler ve uygulamalar'ı**seçin.

    ![Bonusly Sosyal Bölüm](./media/bonus-tutorial/ic773686.png "Bonusly")
1. **Tek Oturum Açma**altında **SAML'yi**seçin.

1. **SAML** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Bonusly Saml İletişim sayfası](./media/bonus-tutorial/ic773687.png "Bonusly")

    a. **IdP SSO hedef URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    b. **IdP Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. **IdP Veren** metin kutusunda, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.
    
    d. Azure portalından kopyalanan **Parmak İzi** değerini **Cert Parmak İzi** metin kutusuna yapıştırın.
    
    e. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Bonusly'ye erişim izni vererek Azure tek oturum açma özelliğini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Bonusly'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **Bonusly'i**seçin.

    ![Uygulamalar listesindeki Bonus bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-bonusly-test-user"></a>Bonus test kullanıcısı oluşturun

Azure AD kullanıcılarının Bonusly'de oturum açabilmeleri için Bonusly'de oturum açmaları gerekir. Bonusly durumunda, sağlama manuel bir görevdir.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Bonustarafından sağlanan diğer Bonusly kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz. 

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde, Bonusly kiracınızla oturum açın.

1. **Ayarlar**'a tıklayın.

    ![Ayarlar](./media/bonus-tutorial/ic781041.png "Ayarlar")

1. Kullanıcılar **ve bonuslar** sekmesini tıklatın.

    ![Kullanıcılar ve bonuslar](./media/bonus-tutorial/ic781042.png "Kullanıcılar ve bonuslar")

1. **Kullanıcıları Yönet'i**tıklatın.

    ![Kullanıcıları Yönet](./media/bonus-tutorial/ic781043.png "Kullanıcıları Yönet")

1. **Kullanıcı Ekle**'ye tıklayın.

    ![Kullanıcı Ekle](./media/bonus-tutorial/ic781044.png "Kullanıcı Ekleme")

1. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Ekle](./media/bonus-tutorial/ic781045.png "Kullanıcı Ekleme")  

    a. **Ad** textbox'ına **Britta**gibi kullanıcının ilk adını girin.

    b. **Soyadı** textbox'ına, **Simon**gibi kullanıcının soyadını girin.

    c. **E-posta** metin kutusuna, kullanıcının `brittasimon\@contoso.com`e-postasını girin.

    d. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure AD hesap sahibi, hesabı etkin hale gelmeden önce onaylamak için bir bağlantı içeren bir e-posta alır.  

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Bonus kiremitini tıklattığınızda, SSO'yu kurduğunuz Bonus'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
