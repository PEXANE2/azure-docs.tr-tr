---
title: 'Öğretici: SkyDesk E-posta ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SkyDesk E-postası arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: faaa5dcc435452d6ed9e0f2c5b481df1e352dfd2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67090437"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Öğretici: SkyDesk E-posta ile Azure Active Directory entegrasyonu

Bu eğitimde, SkyDesk E-postasını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
SkyDesk E-postasını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SkyDesk E-postasına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SkyDesk E-postasında (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SkyDesk E-postasıyla yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* SkyDesk E-posta tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SkyDesk E-posta **SP** başlatılan SSO destekler

## <a name="adding-skydesk-email-from-the-gallery"></a>Galeriden SkyDesk E-postaekleme

SkyDesk E-postasının Azure AD'ye entegrasyonunu yapılandırmak için, galeriden skydesk e-postasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden SkyDesk E-postası eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **SkyDesk E-posta**yazın, sonuç panelinden **SkyDesk E-postasını** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesinde SkyDesk E-posta](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre SkyDesk E-postasıyla yapılandırıp test e-sinebilirsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile SkyDesk E-posta'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmaişlemlerini SkyDesk E-postasıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[SkyDesk E-posta Tek Oturum Açma](#configure-skydesk-email-single-sign-on)** 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SkyDesk E-posta testi kullanıcıoluşturun](#create-skydesk-email-test-user)** - SkyDesk E-posta kullanıcının Azure AD gösterimi ile bağlantılı Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini SkyDesk E-postasıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **SkyDesk E-posta** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SkyDesk E-posta Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [SkyDesk Email Client destek ekibine](https://www.skydesk.jp/apps/support/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **SkyDesk E-posta** ayarla bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-skydesk-email-single-sign-on"></a>SkyDesk E-posta Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısında, Yönetici olarak SkyDesk E-posta hesabınıza oturum açın.

1. Üstteki menüde **Kurulum'u**tıklatın ve **Org'u**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
1. Sol panelden **Etki Alanları'na** tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

1. Etki **Alanı Ekle'ye**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

1. Etki Alanı adınızı girin ve ardından Etki Alanını doğrulayın.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

1. Sol panelden **SAML Kimlik Doğrulaması'na** tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. **SAML Kimlik Doğrulama** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [!NOTE]
    > SAML tabanlı kimlik doğrulamasını kullanmak için etki alanını veya **portal URL** **kurulumunu doğrulamış** olmalısınız. Portal URL'sini benzersiz bir adla ayarlayabilirsiniz.

    ![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    b. Oturum **Açma** URL metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    c. **Parola URL'yi değiştirin** isteğe bağlıdır, bu nedenle boş bırakın.

    d. Azure **portalından** indirdiğiniz sertifikayı seçmek için Dosyadan Anahtar Al'ı tıklatın ve ardından sertifikayı yüklemek için **Aç'ı** tıklatın.

    e. **Algoritma**olarak **RSA'yı**seçin.

    f. Değişiklikleri kaydetmek için **Tamam'ı** tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SkyDesk E-postasına erişim sağlayarak Britta Simon'ın Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **SkyDesk E-postasını**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SkyDesk E-posta'yı**seçin.

    ![Uygulamalar listesindeki SkyDesk E-posta bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-skydesk-email-test-user"></a>SkyDesk E-posta test kullanıcısı oluşturun

Bu bölümde, SkyDesk E-posta britta Simon adında bir kullanıcı oluşturun.

SkyDesk E-posta'daki sol panelden **Kullanıcı Erişimi'ne** tıklayın ve ardından kullanıcı adınızı girin.

![Tek İşaret-On'u Yapılandır](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

> [!NOTE]
> Toplu kullanıcılar oluşturmanız gerekiyorsa, [SkyDesk E-posta İstemci destek ekibine](https://www.skydesk.jp/apps/support/)başvurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SkyDesk E-posta döşemesini tıklattığınızda, SSO'yu kurduğunuz SkyDesk E-postasında otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

