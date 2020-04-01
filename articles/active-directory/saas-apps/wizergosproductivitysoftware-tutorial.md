---
title: 'Öğretici: Wizergos Üretkenlik Yazılımı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Wizergos Üretkenlik Yazılımı arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: b6491013cb35f2473eff6c2019fe2a80dd9e9b08
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086979"
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Öğretici: Wizergos Üretkenlik Yazılımı ile Azure Active Directory entegrasyonu

Bu eğitimde, Wizergos Üretkenlik YazılımLarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Wizergos Üretkenlik Yazılımını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Wizergos Üretkenlik Yazılımı'na erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Wizergos Üretkenlik Yazılımında (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Wizergos Üretkenlik Yazılımı ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Wizergos Verimlilik Yazılımı tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Wizergos Verimlilik Yazılımı **IDP** başlatılan SSO destekler

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Galeriden Wizergos Verimlilik Yazılımı Ekleme

Wizergos Üretkenlik Yazılımı'nın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Wizergos Üretkenlik Yazılımı'nı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Wizergos Üretkenlik Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Wizergos Üretkenlik Yazılımı**yazın, sonuç panelinden **Wizergos Üretkenlik Yazılımı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Wizergos Verimlilik Yazılımı sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Azure AD tek oturum açma işlemini Wizergos Üretkenlik Yazılımı ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Wizergos Üretkenlik Yazılımı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Wizergos Üretkenlik Yazılımı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Wizergos Üretkenlik Yazılımı Tek Oturum Açma](#configure-wizergos-productivity-software-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Wizergos Üretkenlik Yazılımı test kullanıcısını oluşturun](#create-wizergos-productivity-software-test-user)** - Wizergos Üretkenlik Yazılımı'nda Britta Simon'ın bir muadili olması için kullanıcının Azure AD temsiline bağlı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Wizergos Üretkenlik Yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Wizergos Üretkenlik Yazılımı** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Wizergos Verimlilik Yazılımı Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    **Tanımlayıcı** metin kutusuna bir URL yazın:`https://www.wizergos.net`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Wizergos Üretkenlik Yazılımı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-wizergos-productivity-software-single-sign-on"></a>Wizergos Verimlilik Yazılımı Tek Oturum Açma'yı yapılandırın

1. Farklı bir web tarayıcısı penceresinde, wizergos Üretkenlik Yazılımı kiracınızı yönetici olarak oturum açın.

2. Hamburger menüsünden **Admin'i**seçin.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

3. Sol menüdeki Admin sayfasında **AUTHENTICATION'ı** seçin ve **Azure AD'yi**tıklatın.

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

4. AUTHENTICATION bölümünde aşağıdaki adımları **gerçekleştirin.**

    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
    
    a. İndirilen sertifikayı Azure AD'den yüklemek için **UPLOAD** düğmesini tıklatın.
    
    b. Veren **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.
    
    c. Tek **Oturum Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.
    
    d. Tek **Oturum Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.
    
    e. **Kaydet** düğmesini tıklatın.

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

Bu bölümde, Britta Simon'ın Wizergos Üretkenlik Yazılımına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Wizergos Üretkenlik Yazılımı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Wizergos Verimlilik Yazılımı'nı**seçin.

    ![Uygulamalar listesindeki Wizergos Verimlilik Yazılımı bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-wizergos-productivity-software-test-user"></a>Wizergos Verimlilik Yazılımı test kullanıcısı oluşturun

Bu bölümde, Wizergos Üretkenlik Yazılımı'nda Britta Simon adında bir kullanıcı oluşturursunuz. [Wizergos Üretkenlik Yazılımı destek ekibiyle](mailTo:support@wizergos.com) çalışarak kullanıcıları Wizergos Üretkenlik Yazılımı platformuna ekleyin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Wizergos Üretkenlik Yazılımı döşemesini tıklattığınızda, SSO'yu kurduğunuz Wizergos Üretkenlik Yazılımı'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

