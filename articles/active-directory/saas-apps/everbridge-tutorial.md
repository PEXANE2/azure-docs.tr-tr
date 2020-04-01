---
title: 'Öğretici: Everbridge ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Everbridge arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 60463a00c6864bed7b3a18e816ef0143d3573782
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67103251"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: Everbridge ile Azure Active Directory entegrasyonu

Bu eğitimde, Everbridge'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Everbridge'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Everbridge erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Everbridge'de otomatik olarak oturum açabilmelerine izin verin. Bu erişim denetimine tek oturum açma (SSO) adı verilir.
* Azure portalını kullanarak hesaplarınızı tek bir merkezi konumda yönetin.
Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bkz.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Everbridge ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma kullanan bir Everbridge aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Everbridge, IDP tarafından başlatılan SSO'ya destek veriyor.

## <a name="add-everbridge-from-the-azure-marketplace"></a>Azure Marketinden Everbridge Ekle

Everbridge'in Azure AD'ye entegrasyonunu yapılandırmak için Azure Marketi'nden Everbridge'i yönetilen SaaS uygulamaları listenize ekleyin.

Azure Marketi'nden Everbridge eklemek için aşağıdaki adımları izleyin.

1. Sol daki gezinti bölmesinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Everbridge'i**girin. Sonuç panelinden **Everbridge'i** seçin ve **Ekle'yi**seçin.

     ![Sonuç listesinde Everbridge](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Test kullanıcısı Britta Simon'a göre Azure AD oturum açma işlemini Everbridge ile yapılandırın ve test esiniz.
Tek oturum açma nın çalışması için, Bir Azure REKLAM kullanıcısı ile Everbridge'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurun.

Azure AD oturumaçmayı Everbridge ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

- Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)
- [Everbridge'i Everbridge yönetici portalı olarak yapılandırın tek oturum açma](#configure-everbridge-as-everbridge-manager-portal-single-sign-on) uygulaması tarafındaki tek oturum açma ayarlarını yapılandırın.
- [Everbridge'i Everbridge üye portalı olarak yapılandırın tek oturum açma](#configure-everbridge-as-everbridge-member-portal-single-sign-on) uygulaması tarafındaki tek oturum açma ayarlarını yapılandırın.
- Britta Simon ile Azure AD tek oturum açma işlemini test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
- Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
- Everbridge'de Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için [bir Everbridge test kullanıcısı oluşturun.](#create-an-everbridge-test-user)
- Yapılandırmanın çalışıp çalışmadığını doğrulamak için [tek oturum](#test-single-sign-on) açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Everbridge ile yapılandırmak için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Everbridge** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme'yi** seçin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

    >[!NOTE]
    >Uygulamayı yönetici portalı *veya* hem Azure portalında hem de Everbridge portalında üye portalı olarak yapılandırın.

4. **Everbridge** uygulamasını **Temel SAML Yapılandırması** bölümündeEverbridge **yöneticisi portalı**olarak yapılandırmak için aşağıdaki adımları izleyin:

    ![Everbridge etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusuna, deseni izleyen bir URL girin`https://sso.everbridge.net/<API_Name>`

    b. **Yanıtla URL** kutusuna, deseni izleyen bir URL girin`https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek Tanımlayıcı ve Yanıtla URL değerleriyle güncelleştirin. Bu değerleri almak için [Everbridge destek ekibine](mailto:support@everbridge.com)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **Everbridge** uygulamasını **Temel SAML Yapılandırması** bölümündeEverbridge **üye portalı**olarak yapılandırmak için aşağıdaki adımları izleyin:

  * Uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları izleyin:

     ![IDP tarafından başlatılan mod için Everbridge etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusuna, deseni izleyen bir URL girin`https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    b. **Yanıtla URL** kutusuna, deseni izleyen bir URL girin`https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız, **ek URL'ler** ayarla'yı seçin ve şu adımı izleyin:

     ![SP tarafından başlatılan mod için Everbridge etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

     a. **URL'de Oturum Aç** kutusuna, deseni izleyen bir URL girin`https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Bu değerler gerçek değil. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL ve URL değerlerinde oturum açarak güncelleştirin. Bu değerleri almak için [Everbridge destek ekibine](mailto:support@everbridge.com)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'i**indirmek için **İndir'i** seçin. Bilgisayarınıza kaydedin.

    ![Sertifika indirme linki](common/metadataxml.png)

7. **Everbridge'i Ayarla** bölümünde, gereksinimleriniz için ihtiyacınız olan URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    - Oturum Açma URL’si
    - Azure AD Tanımlayıcısı
    - Giriş URL'si

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>Everbridge'i Everbridge yönetici portalı olarak yapılandırın tek oturum açma

**Everbridge'de** SSO'yu **Everbridge yönetici portalı** uygulaması olarak yapılandırmak için aşağıdaki adımları izleyin.
 
1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Everbridge'de oturum açın.

1. Üstteki menüde **Ayarlar** sekmesini seçin. **Güvenlik**Altında, **Tek Oturum Açma'yı**seçin.
   
     ![Çoklu oturum açmayı yapılandırma](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. **Ad** kutusuna tanımlayıcı sağlayıcısının adını girin. Örnek olarak şirket adınız verilmiştir.
   
     b. **API Adı** kutusuna API adını girin.
   
     c. Azure portalından indirdiğiniz meta veri dosyasını yüklemek için **Dosyayı Seç'i** seçin.
   
     d. **SAML Identity Location**için, select **Identity Konu deyiminin NameIdentifier öğesindedir.**
   
     e. Kimlik **Sağlayıcı Giriş URL** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.
   
     f. **Hizmet Sağlayıcı için başlatılan İstek Bağlama**, HTTP Yeniden **Yönlendirme**seçin.

     g. **Kaydet'i**seçin.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>Everbridge'i Everbridge üye portalı olarak yapılandırTek oturum açma

**Everbridge'de** **everbridge üye portalı**olarak tek oturum açma yı yapılandırmak için, indirilen Federasyon **Metadata XML'i** [Everbridge destek ekibine](mailto:support@everbridge.com)gönderin. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Azure portalında test kullanıcısı Britta Simon'ı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalında, sol bölmede **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

    ![Kullanıcılar ve Tüm kullanıcı bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** **kutusuna BrittaSimon**girin.
  
    b. **Kullanıcı adı** kutusuna `brittasimon@yourcompanydomain.extension` girin. BrittaSimon@contoso.com bunun bir örneğidir.

    c. **Parolayı Göster** onay kutusunu seçin. **Parola** kutusuna görünen değeri yazın.

    d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Britta Simon'ın Everbridge'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını etkinleştirin.

1. Azure portalında Kurumsal **uygulamaları** > seçtüm uygulamalar**Everbridge'****i** >seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Everbridge'i**seçin.

    ![Uygulamalar listesinde Everbridge bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcılar ve gruplar bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Atama iletişim kutusu ekle](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin. Ekranın alt kısmında **seç'i** seçin.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ekranın alt kısmında **seç'i** seçin.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-an-everbridge-test-user"></a>Everbridge test kullanıcısı oluşturma

Bu bölümde, Everbridge test kullanıcı Britta Simon oluşturun. Everbridge platformunda kullanıcı eklemek için [Everbridge destek ekibiyle](mailto:support@everbridge.com)çalışın. Tek oturum açmadan önce everbridge'de kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test edin.

Access Paneli'ndeki Everbridge döşemesini seçtiğinizde, SSO'yu kurduğunuz Everbridge hesabında otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

