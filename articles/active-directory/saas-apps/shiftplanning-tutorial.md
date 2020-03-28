---
title: 'Öğretici: İnsanlık ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Humanity arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd7cfe93aa39982836944230f860c603c7117ceb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233428"
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Öğretici: İnsanlık ile Azure Active Directory entegrasyonu

Bu eğitimde, İnsanlığı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
İnsanlığı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* İnsanlığa erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak İnsanlık (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini İnsanlıkla yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* İnsanlık tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* İnsanlık **SP** başlatılan SSO destekler

## <a name="adding-humanity-from-the-gallery"></a>Galeriden İnsanlığı Ekleme

İnsanlığın Azure AD'ye entegrasyonunu yapılandırmak için, İnsanlığı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**İnsanlığı galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **İnsanlık**yazın, sonuç panelinden **İnsanlığı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde insanlık](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD'nin Tek Oturum Açma'yı **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp sınayın.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile İnsanlık'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD'yi İnsanlıkla tek oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[İnsanlığı Tek Oturum](#configure-humanity-single-sign-on)** Açma 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[İnsanlık testi kullanıcı oluşturun](#create-humanity-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı İnsanlık Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumlarını İnsanlıkla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **İnsanlık** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Humanity Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.humanity.com/includes/saml/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://company.humanity.com/app/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Humanity Client destek ekibine](https://www.humanity.com/support/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **İnsanlığı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-humanity-single-sign-on"></a>İnsanlığı Yapılandırtek İşaret-On

1. Farklı bir web tarayıcısı penceresinde, Bir yönetici olarak **İnsanlık** şirket sitesinde oturum açın.

2. Üstteki menüde **Yönetici'yi**tıklatın.

    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "Yönetici")
3. **Tümleştirme**altında, **Tek Oturum Açma'yı**tıklatın.

    ![Tek Oturum Açma](./media/shiftplanning-tutorial/iC786620.png "Çoklu Oturum Açma")

4. Tek **Oturum Açma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma](./media/shiftplanning-tutorial/iC786905.png "Çoklu Oturum Açma")

    a. **SAML Etkin'i**seçin.

    b. **Parola Girişine İzin Ver'i**seçin.

    c. **SAML Veren URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    d. Uzak **Oturum Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    e. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından X.509 Sertifika** metin kutusuna yapıştırın.

    f. **Ayarları Kaydet'i**tıklatın.

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

Bu bölümde, Britta Simon'ın İnsanlığa erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **İnsanlık'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **İnsanlık'ı**seçin.

    ![Uygulamalar listesinde İnsanlık bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-humanity-test-user"></a>İnsanlık testi kullanıcısı oluşturma

Azure AD kullanıcılarının İnsanlık'ta oturum açabilmeleri için, bunların İnsanlık'a dahil edilmesi gerekir. İnsanlık söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **İnsanlık** şirket sitenize yönetici olarak giriş yapın.

2. **Admin'e**tıklayın.

    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "Yönetici")

3. **Personel'i**tıklatın.

    ![Personel](./media/shiftplanning-tutorial/ic786623.png "Personel")

4. **Eylemler**altında, **Çalışan Ekle'yi**tıklatın.

    ![Çalışan Ekle](./media/shiftplanning-tutorial/iC786624.png "Çalışan Ekle")

5. Çalışan **Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çalışanları Kaydet](./media/shiftplanning-tutorial/iC786625.png "Çalışanları Kaydet")

    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **Adı,** **Soyadı**ve **E-postasını** yazın.

    b. **Çalışanları Kaydet'i**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Humanity tarafından sağlanan diğer İnsanlık kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki İnsanlık döşemesini tıklattığınızda, SSO'yu kurduğunuz İnsanlık'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)