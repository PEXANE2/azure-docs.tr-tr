---
title: 'Öğretici: Zendesk ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zendesk arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4887457f457ff533a0eaf10be7db1627a950f5bd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233260"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Öğretici: Zendesk ile Azure Active Directory entegrasyonu

Bu eğitimde, Zendesk'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zendesk'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zendesk erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Zendesk'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zendesk ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Zendesk tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **Zendesk, SP** tarafından başlatılan SSO'ya destek verdi

* Zendesk [ **Otomatik** kullanıcı sağlama destekler](zendesk-provisioning-tutorial.md)

## <a name="adding-zendesk-from-the-gallery"></a>Galeriden Zendesk ekleme

Zendesk'in Azure AD'ye entegrasyonunu yapılandırmak için, zendesk'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Zendesk**yazın, sonuç panelinden **Zendesk'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Zendesk sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Zendesk'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Zendesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Zendesk Tek Oturum Açma'yı yapılandırın.](#configure-zendesk-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zendesk test kullanıcısını oluşturun](#create-zendesk-test-user)** - Britta Simon'ın zendesk'te kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Zendesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Zendesk** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Zendesk Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.zendesk.com`

   b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.zendesk.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Zendesk Client destek ekibine](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Zendesk uygulaması, SAML iddialarını belirli bir biçimde bekler. Zorunlu SAML öznitelikleri yoktur, ancak isteğe bağlı olarak uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, uygun öznitelik değerini seçin.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Varsayılan olarak Azure AD'de olmayan öznitelikler eklemek için uzantı özniteliklerini kullanırsınız. **Zendesk'in** kabul ettiği SAML özniteliklerinin tam listesini almak için [SAML'de ayarlanabilen Kullanıcı özniteliklerini](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tıklatın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** **bölümünde, Thumbprint'i** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-certificatethumbprint.png)

8. **Zendesk'i Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

9. Zendesk'in yapılandırılabildiği iki yol vardır : Otomatik ve Manuel.
  
10. Zendesk içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![image](./media/zendesk-tutorial/install_extension.png)

11. Tarayıcıya uzantı ekledikten **sonra, Kurulum Zendesk'e** tıklayın ve sizi Zendesk uygulamasına yönlendirecektir. Buradan Zendesk'te oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı sizin için uygulamayı otomatik olarak yapılandıracak ve zendesk Tek Oturum Açma bölümünü **Yapılandır'ı otomatikleştirin.**

    ![image](./media/zendesk-tutorial/d2_saml.png)

### <a name="configure-zendesk-single-sign-on"></a>Zendesk Tek İşaretli Yapıla

1. Zendesk'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Yönetici olarak Zendesk şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

2. **Admin'e**tıklayın.

3. Sol daki gezinti bölmesinde **Ayarlar'ı**tıklatın ve ardından **Güvenlik'i**tıklatın.

4. **Güvenlik** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Güvenlik](././media/zendesk-tutorial/ic773089.png "Güvenlik")

    ![Çoklu oturum açma](././media/zendesk-tutorial/ic773090.png "Çoklu oturum açma")

    a. Yönetici **& Aracıları** sekmesini tıklatın.

    b. **Tek oturum açma (SSO) ve SAML'yi**seçin ve ardından **SAML'yi**seçin.

    c. **SAML SSO URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    d. **Uzaktan Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    e. **Sertifika Parmak İzi** metin kutusuna, Azure portalından kopyalamış olduğunuz sertifikanın **Parmak Izi** değerini yapıştırın.

    f. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın Zendesk'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Zendesk'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Zendesk**yazın ve seçin.

    ![Uygulamalar listesindeki Zendesk bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-zendesk-test-user"></a>Zendesk test kullanıcısı oluşturma

Bu bölümün amacı Zendesk Britta Simon adlı bir kullanıcı oluşturmaktır. Zendesk varsayılan olarak etkin olan otomatik kullanıcı sağlamayı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](Zendesk-provisioning-tutorial.md) fazla bilgi bulabilirsiniz.

**El ile kullanım oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları gerçekleştirin:**

> [!NOTE]
> **Oturum** açma yaparken son kullanıcı hesapları otomatik olarak sağlanır. **Temsilci** ve **Yönetici** hesaplarının oturum açmadan önce **Zendesk'te** el ile sağlanması gerekir.

1. **Zendesk** kiracınıza giriş yapın.

2. Müşteri **Listesi** sekmesini seçin.

3. **Kullanıcı** sekmesini seçin ve **Ekle'yi**tıklatın.

    ![Kullanıcı ekle](././media/zendesk-tutorial/ic773632.png "Kullanıcı ekle")
4. Sağlamak istediğiniz mevcut bir Azure REKLAM hesabının **Adını** ve **E-postasını** yazın ve ardından **Kaydet'i**tıklatın.

    ![Yeni kullanıcı](././media/zendesk-tutorial/ic773633.png "Yeni kullanıcı")

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Zendesk tarafından sağlanan diğer Zendesk kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Zendesk döşemesini tıklattığınızda, SSO'yu kurduğunuz Zendesk'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı Sağlama'yı Yapılandır](zendesk-provisioning-tutorial.md)