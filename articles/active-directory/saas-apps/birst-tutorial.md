---
title: 'Öğretici: Birst Çevik İş Analitiği ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Birst Çevik İş Analitiği arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46bea4afcd59a385a8e9d2ccf1aa0d70fdae198c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158837"
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a>Öğretici: Birst Çevik İş Analitiği ile Azure Active Directory entegrasyonu

Bu eğitimde, Birst Çevik İş Analitiğini Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Birst Çevik İş Analitiğini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Birst Çevik İş Analitiği'ne erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Birst Çevik İş Analitiği (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Birst Çevik İş Analitiği ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Birst Agile Business Analytics tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Birst Çevik İş Analitiği **SP** başlatılan SSO destekler

## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a>Galeriden Birst Çevik İş Analitiği Ekleme

Birst Çevik İş Analitiği'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Birst Çevik İş Analitiği eklemeniz gerekir.

**Galeriden Birst Çevik İş Analitiği eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Birst Çevik İş Analitiği**yazın, sonuç panelinden **Birst Çevik İş Analitiği'ni** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Birst Çevik İş Analitiği sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Birst Çevik İş Analitiği ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Birst Çevik İş Analitiği'ndeki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Birst Agile Business Analytics ile Azure AD oturumaçmaişlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Birst Çevik İş Analitiği Tek Oturum Açma'yı yapılandırın.](#configure-birst-agile-business-analytics-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Birst Çevik İş Analitiği test kullanıcısını oluşturun](#create-birst-agile-business-analytics-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Olan Birst Çevik İş Analitiği'nde Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Birst Agile Business Analytics ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Birst Çevik İş Analitiği** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Birst Çevik İş Analitiği Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-intiated.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

    URL, Birst hesabınızın bulunduğu veri merkezine bağlıdır:

   * ABD veri merkezi için aşağıdaki desen kullanımı:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

   * Avrupa veri merkezi için aşağıdaki deseni kullanın:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

     > [!NOTE]
     > Bu değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Birst Çevik İş Analitiği Müşteri destek ekibiyle](mailto:info@birst.com) iletişime geçin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Birst Çevik İş Analitiği Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-birst-agile-business-analytics-single-sign-on"></a>Yapılandırma Birst Çevik İş Analitiği Tek Oturum Açma

**Birst Çevik İş Analitiği** tarafında tek oturum açma işlemi yapmak için, indirilen **Sertifikayı (Base64)** ve azure portalından uygun kopyalanmış URL'leri [Birst Çevik Business Analytics destek ekibine](mailto:info@birst.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

> [!NOTE]
> Birst ekibine bu tümleştirmenin SHA256 Algoritması'na (SHA1 desteklenmez) ihtiyaç duyduğundan bahsedin, böylece SSO'yu **app2101** vb. gibi uygun sunucuda ayarlayabilirsiniz.

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

Bu bölümde, Britta Simon'ın Birst Çevik İş Analitiği'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Birst Çevik İş Analitiği'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Birst Çevik İş Analitiği'ni**seçin.

    ![Uygulamalar listesindeki Birst Çevik İş Analitiği bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-birst-agile-business-analytics-test-user"></a>Birst Çevik İş Analitiği test kullanıcıoluşturun

Bu bölümde, Birst Çevik İş Analitiği'nde Britta Simon adında bir kullanıcı oluşturursunuz. Birst Çevik İş Analitiği platformundaki kullanıcıları eklemek için [Birst Çevik Business Analytics destek ekibiyle](mailto:info@birst.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Birst Çevik İş Analitiği döşemesini tıklattığınızda, SSO'yu kurduğunuz Birst Çevik İş Analitiği'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

