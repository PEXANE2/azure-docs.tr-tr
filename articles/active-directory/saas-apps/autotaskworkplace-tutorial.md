---
title: 'Öğretici: Autotask Workplace ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Otomatik Görev İşyeri arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97a68fd74449ea34438348cbc4c9d09c06d46d71
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73157801"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Öğretici: Autotask Workplace ile Azure Active Directory tümleştirmesi

Bu eğitimde, Otomatik Görev İşyeri'ni Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Otomatik Görev İşyeri'ni Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Otomatik Görev İşyeri'ne erişimi olan Azure AD'da denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik Görev İşyeri'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Autotask Workplace ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Autotask İşyeri tek oturum açma özellikli abonelik
* Etkin abonelikte Otomatik Görev İşyeri tek oturum açma
* İşyeri'nde yönetici veya süper yönetici olmalısınız.
* Azure AD'de bir yönetici hesabınız olmalıdır.
* Bu özelliği kullanacak kullanıcıların Workplace ve Azure AD içinde hesapları olmalı ve her ikisi için de e-posta adresleri eşleşmelidir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Autotask Workplace **SP ve IDP** başlatılan SSO destekler

## <a name="adding-autotask-workplace-from-the-gallery"></a>Galeriden Otomatik Görev İşyeri Ekleme

Otomatik Görev İşyeri'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Autotask Workplace eklemeniz gerekir.

**Galeriden Otomatik Görev İşyeri eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Otomatik Görev İşyeri**yazın, sonuç panelinden Otomatik Görev **İşyeri'ni** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Otomatik Görev İşyeri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Otomatik Görev İşyeri ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile Autotask Workplace'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Otomatik Görev İşyeri ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Otomatik Görev İşyeri Tek Oturum Açma'yı yapılandırın.](#configure-autotask-workplace-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Autotask Workplace test kullanıcısını oluşturun](#create-autotask-workplace-test-user)** - Kullanıcının Azure AD gösterimine bağlı Autotask Workplace'te Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Otomatik Görev İşyeri ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Otomatik Görev İşyeri** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Otomatik görev İşyeri Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Otomatik görev İşyeri Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.awp.autotask.net/loginsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Autotask Workplace Client destek ekibine](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Otomatik **Görev İşyeri Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-autotask-workplace-single-sign-on"></a>Otomatik Görev İşyeri Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, yönetici kimlik bilgilerini kullanarak Workplace Online'da oturum açın.

    > [!Note]
    > IdP yapılandırırken bir alt etki alanı belirtilmesi gerekir. Doğru alt etki alanını onaylamak için Workplace Online'a giriş yapın. Oturum açtıktan sonra URL'deki alt etki alanına not yapın. Alt etki alanı "https://" ve ".awp.autotask.net/" arasındaki bölümdür ve biz, ab, ca veya au olmalıdır.

2. **Configuration** > **Single Sign-On'a** gidin ve aşağıdaki adımları gerçekleştirin:

    ![Otomatik görev Tek Oturum Açma yapılandırması](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)

    a. **XML Metadata Dosyası** seçeneğini seçin ve ardından indirilen **Federasyon Metadata XML'i** Azure portalından yükleyin.

    b. **SSO'ya ETKIN'i**tıklatın.

    ![Otomatik görev Tek Oturum Açma onay yapılandırması](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Bu **bilgilerin doğru olduğunu onaylayın i** seçin ve bu IdP onay kutusuna güveniyorum.

    d. **ONAYLA'yı**tıklatın.

> [!Note]
> Otomatik Görev İşyeri'ni yapılandırma konusunda yardıma ihtiyacınız varsa, İşyeri hesabınızla ilgili yardım almak için lütfen [bu sayfaya](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) bakın.

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

Bu bölümde, Britta Simon'ın Otomatik Görev İşyeri'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından Otomatik **Görev İşyeri'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Otomatik Görev İşyeri'ni**seçin.

    ![Uygulamalar listesindeki Otomatik Görev İşyeri bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-autotask-workplace-test-user"></a>Otomatik Görev İşyeri test kullanıcıoluşturma

Bu bölümde, Autotask Workplace'te Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Autotask Workplace platformuna eklemek için lütfen [Autotask Workplace destek ekibiyle](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Otomatik Görev İşyeri döşemesini tıklattığınızda, Otomatik Görev İşyeri'nde otomatik olarak Oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
