---
title: 'Öğretici: Moconavi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve moconavi arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 038520e662a205bc173035fd19656cbaf4800239
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160530"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Öğretici: Moconavi ile Azure Active Directory entegrasyonu

Bu eğitimde, moconavi'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Moconavi'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Moconavi erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla moconavi (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini moconavi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* moconavi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* moconavi **SP** başlatılan SSO destekler

## <a name="adding-moconavi-from-the-gallery"></a>Galeriden moconavi ekleme

Moconavi'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden moconavi'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden moconavi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **moconavi**yazın, sonuç panelinden **moconavi'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![sonuç listesinde moconavi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Moconavi ile Azure AD tek oturum açma işlemini yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile moconavi'deki ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Moconavi ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Moconavi Tek Oturum Açma](#configure-moconavi-single-sign-on)** 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Moconavi test kullanıcıoluşturun](#create-moconavi-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı moconavi Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Moconavi ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Moconavi** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![moconavi Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<yourserverurl>/moconavi-saml2/saml/login`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<yourserverurl>/moconavi-saml2`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [moconavi Müşteri destek ekibine](mailto:support@recomot.co.jp) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Moconavi'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-moconavi-single-sign-on"></a>Moconavi Tek İşaret-On yapıla

**Moconavi** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'i** ve azure portalından uygun kopyalanmış URL'leri [moconavi destek ekibine](mailto:support@recomot.co.jp)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın moconavi erişimi sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **moconavi'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **moconavi'yi**seçin.

    ![Uygulamalar listesindeki moconavi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-moconavi-test-user"></a>moconavi test kullanıcısı oluşturma

Bu bölümde, moconavi Britta Simon adlı bir kullanıcı oluşturun. Moconavi platformunda kullanıcıları eklemek için [moconavi destek ekibiyle](mailto:support@recomot.co.jp) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

1. Microsoft mağazasından moconavi yükleyin.

2. Moconavi'yi başlatın.

3. **Ayara Bağlan düğmesini** tıklatın.

    ![Tek oturum açma yı test etme](./media/moconavi-tutorial/testing1.png)

4. URL `https://mcs-admin.moconavi.biz/gateway` **textbox'a bağlan'a** girin ve ardından **Bitti** düğmesini tıklatın.

    ![Tek oturum açma yı test etme](./media/moconavi-tutorial/testing2.png)

5. Aşağıdaki ekran görüntüsünde aşağıdaki adımları gerçekleştirin:

    ![Tek oturum açma yı test etme](./media/moconavi-tutorial/testing3.png)

    a. **Giriş Kimlik Doğrulama Anahtarıgirini**Girin :`azureAD` Giriş Kimlik Doğrulama **Anahtarı** metin kutusuna.

    b. **Giriş Kullanıcı Kimliği** `your ad account` girin : **Giriş Kullanıcı Kimliği** textbox içine.

    c. **Gİrİş 'e**tıklayın.

6. Azure AD parolanızı **Parola** textbox'a girin ve ardından **Giriş düğmesini** tıklatın.

    ![Tek oturum açma yı test etme](./media/moconavi-tutorial/testing4.png)

7. Azure AD kimlik doğrulaması, menü görüntülendiğinde başarılı olur.

    ![Tek oturum açma yı test etme](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

