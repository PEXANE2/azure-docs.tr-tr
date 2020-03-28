---
title: 'Öğretici: Yodeck ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Yodeck arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b2c8dccb-eeb0-4f4d-a24d-8320631ce819
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: af0748f36e8d2299921f987c517f7a4923bd0d55
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086470"
---
# <a name="tutorial-azure-active-directory-integration-with-yodeck"></a>Öğretici: Yodeck ile Azure Active Directory entegrasyonu

Bu eğitimde, Yodeck'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Yodeck'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Yodeck erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Yodeck'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Yodeck ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Yodeck tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Yodeck **SP** ve **IDP** SSO başlatılan destekler

## <a name="adding-yodeck-from-the-gallery"></a>Galeriden Yodeck ekleme

Yodeck'in Azure AD'ye entegrasyonunu yapılandırmak için, Yodeck'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Yodeck'i galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Yodeck**yazın, sonuç panelinden **Yodeck'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Yodeck sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yodeck ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Yodeck'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Yodeck ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Yodeck Tek Oturum Açma'yı uygulama](#configure-yodeck-single-sign-on)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Yodeck test kullanıcıoluşturun](#create-yodeck-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Yodeck Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Yodeck ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Yodeck** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:

    ![Yodeck Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    **Tanımlayıcı** metin kutusuna bir URL yazın:`https://app.yodeck.com/api/v1/account/metadata/`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![image](common/both-preintegrated-signon.png)

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.yodeck.com/login`

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-yodeck-single-sign-on"></a>Yodeck Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Yodeck şirket sitenizde yönetici olarak oturum açın.

2. Kullanıcı **Ayarları** seçeneğine tıklayın sayfanın sağ üst köşesini doldurun ve **Hesap Ayarları'nı**seçin.

    ![Yodeck Yapılandırma](./media/yodeck-tutorial/configure1.png)

3. **SAML'yi** seçin ve aşağıdaki adımları gerçekleştirin:

    ![Yodeck Yapılandırma](./media/yodeck-tutorial/configure2.png)

    a. **URL'den Içe Aktar'ı**seçin.

    b. URL **textbox'ına,** Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url** değerini yapıştırın ve **İçe Aktar'ı**tıklatın.
    
    c. **App Federation Metadata Url'sini**aldıktan sonra kalan alanlar otomatik olarak doldurulur.

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
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Yodeck'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Yodeck'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Yodeck'i**seçin.

    ![Uygulamalar listesindeki Yodeck bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-yodeck-test-user"></a>Yodeck test kullanıcısı oluşturma

Azure AD kullanıcılarının Yodeck'te oturum açabilmeleri için Yodeck'e dahil edilmeleri gerekir. Yodeck durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yodeck şirket sitenizde yönetici olarak oturum açın.

2. Kullanıcı **Ayarları** seçeneğine tıklayın sayfanın sağ üst köşesini doldurun ve **Kullanıcıları**seçin.

    ![Çalışan Ekle](./media/yodeck-tutorial/user1.png)

3. **Kullanıcı Ayrıntıları** sekmesini açmak için **+Kullanıcı'ya** tıklayın.

    ![Çalışan Ekle](./media/yodeck-tutorial/user2.png)

4. Kullanıcı **Ayrıntıları** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/yodeck-tutorial/user3.png)

    a. Ad **metin** kutusuna, **Britta**gibi kullanıcının ilk adını yazın.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını yazın.

    c. **E-posta** metin kutusuna, kullanıcının brittasimon@contoso.come-posta adresini yazın.

    d. Kuruluş gereksiniminize göre uygun **Hesap İzinleri** seçeneğini seçin.
    
    e. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Yodeck döşemesini tıklattığınızda, SSO'yu kurduğunuz Yodeck'te otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

