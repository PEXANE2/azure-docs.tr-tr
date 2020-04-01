---
title: 'Öğretici: Tableau Online ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Tableau Online arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: 187600edb599f5a5775e1b847ed1cb3a49f3b827
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76985637"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-tableau-online"></a>Öğretici: Tableau Online ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Tableau Online'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Tableau Online'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Tableau Online erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Tableau Online'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tableau Online tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Tableau **Online, SP** tarafından başlatılan SSO'yu destekliyor
* Tableau Online'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-tableau-online-from-the-gallery"></a>Galeriden Tableau Online Ekleme

Tableau Online'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Tableau Online'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama **kutusuna Tableau Online** yazın.
1. Sonuç panelinden **Tableau Online'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Tablo au Online ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Tableau Online'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO'yu Tableau Online ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Tableau Online SSO'yu yapılandırın.](#configure-tableau-online-sso)**
    1. **[Tableau Online test kullanıcısını oluşturun](#create-tableau-online-test-user)** - Tableau Online'da kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Tableau Online ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Tableau Online** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tableau Online Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna URL'yi yazın:`https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna URL'yi yazın:`https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

    > [!NOTE]
    > Bu eğitimde `<entityid>` Tablo **Online'ı Kur** bölümünden değeri alırsınız. Entity ID **değeri, Tableau Online'ı ayarla** **bölümündeazure AD tanımlayıcı** değeri olacaktır.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Tableau Online'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

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
    Örneğin, BrittaSimon\@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Tableau Online'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Tableau Online'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau Online'ı**seçin.

    ![Uygulamalar listesindeki Tablo Au Online bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

## <a name="configure-tableau-online-sso"></a>Tableau Online SSO'yu yapılandır

1. Farklı bir tarayıcı penceresinde, Tableau Online uygulamanızda oturum açın. **Ayarlar'a** ve ardından **Kimlik Doğrulama'ya**gidin.

    ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. SAML'yi etkinleştirmek için **Kimlik Doğrulama türleri** bölümü altında. **Ek kimlik doğrulama yöntemini etkinleştir'i** işaretleyin ve ardından **SAML** onay kutusunu işaretleyin.

    ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. Meta veri **dosyasını Tableau Online bölümüne aktarın.**  Azure AD'den indirdiğiniz meta veri dosyasına Gözat'ı ve içe aktar'ı tıklatın. Ardından **Uygula'yı**tıklatın.

   ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. **Eşleç iddiaları** bölümünde, **e-posta adresi,** **ad**ve **soyadı**için ilgili Kimlik Sağlayıcı sınayı adı ekleyin. Bu bilgileri Azure AD'den almak için: 
  
    a. Azure portalında **Tableau Online** uygulama tümleştirme sayfasına gidin.

    b. Kullanıcı **Öznitelikleri & Talepler** bölümünde, edit simgesine tıklayın.

   ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/attributesection.png)

    c. Bu öznitelikler için ad alanı değerini kopyalayın: verilen ad, e-posta ve soyadı aşağıdaki adımları kullanarak:

   ![Azure AD Tek Oturum Açma](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. **user.givenname** değerine tıklayın

    e. **Değeri Namespace** metin kutusundan kopyalayın.

    ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/attributesection2.png)

    f. E-posta ve soyadı için ad alanı değerlerini kopyalamak için yukarıdaki adımları yineleyin.

    g. Tableau Online uygulamasına geçin ve **ardından Kullanıcı Öznitelikleri & Talepler** bölümünü aşağıdaki gibi ayarlayın:

    * E-posta: **posta** veya **kullanıcı adı**

    * Ad soyad: **givenname**

    * Soyadı: **Soyadı**

    ![Tek İşaret-On'u Yapılandır](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="create-tableau-online-test-user"></a>Tableau Online test kullanıcıoluşturma

Bu bölümde, Tableau Online'da Britta Simon adında bir kullanıcı oluşturursunuz.

1. **Tableau Online'da** **Ayarlar** ve ardından **Kimlik Doğrulama** bölümünü tıklatın. **Kullanıcıları Yönet** bölümüne gidin. **Kullanıcı Ekle'yi** tıklatın ve ardından **E-posta Adreslerini Girin'i**tıklatın.
  
    ![Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. **(SAML) kimlik doğrulaması için kullanıcı ekle'yi seçin.** **E-posta adresini girin** textbox ekle\@britta.simon contoso.com
  
    ![Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. **Kullanıcı Ekle'yi**tıklatın.

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Tableau Online döşemesini tıklattığınızda, SSO'yu kurduğunuz Tableau Online'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)