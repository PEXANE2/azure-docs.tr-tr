---
title: 'Öğretici: FreshDesk ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve FreshDesk arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e75a45bc4ccd2614520fda39e09996c2286bdbd0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227608"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: FreshDesk ile Azure Active Directory entegrasyonu

Bu eğitimde, FreshDesk'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
FreshDesk'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* FreshDesk erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla FreshDesk'te (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini FreshDesk ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* FreshDesk tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **FreshDesk, SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden FreshDesk ekleme

FreshDesk'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize FreshDesk eklemeniz gerekir.

**Galeriden FreshDesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **FreshDesk**yazın, sonuç panelinden **FreshDesk'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuçlar listesinde FreshDesk](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini FreshDesk ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile FreshDesk'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumaçma işlemlerini FreshDesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. FreshDesk Tek Oturum Açma'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-freshdesk-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[FreshDesk test kullanıcısını oluşturun](#create-freshdesk-test-user)** - FreshDesk'te Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini FreshDesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **FreshDesk** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![FreshDesk Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL **metin** kutusunda Oturum Aç'ta, aşağıdaki deseni kullanarak bir URL yazın: `https://<tenant-name>.freshdesk.com` veya Freshdesk'in önerdiği herhangi bir değer.

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir `https://<tenant-name>.freshdesk.com` URL yazın: veya Freshdesk'in önerdiği herhangi bir değer.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [FreshDesk Client destek ekibine](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. FreshDesk uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü bunun için bir örnek gösterir. Benzersiz Kullanıcı **Tanımlayıcısı'nın** varsayılan değeri **user.userprincipalname'dir** ancak FreshDesk bunun kullanıcının e-posta adresiyle eşlenmesini bekler. Bunun için listeden **user.mail** özniteliğini kullanabilir veya kuruluş yapılandırmanıza göre uygun öznitelik değerini kullanabilirsiniz. 

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, **Edit simgesini** kullanarak talepleri düzenleme veya yukarıdaki resimde gösterildiği gibi SAML belirteç özniteliğini yapılandırmak için **yeni talep ekle** kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:
    
    | Adı | Kaynak Özniteliği |
    | ---------------| --------------- |
    | Benzersiz Kullanıcı Tanımlayıcısı | kullanıcı.posta |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **Komut İstem'i** açın ve aşağıdaki komutları çalıştırın:

    a. Komut `certutil.exe -dump FreshDesk.cer` istemine değer girin.

    > [!NOTE]
    > Burada **FreshDesk.cer** Azure portalından indirdiğiniz sertifikadır.

    b. **Cert Hash(sha256)** değerini kopyalayın ve Not Defterine yapıştırın. 

9. **FreshDesk'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-freshdesk-single-sign-on"></a>FreshDesk Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Freshdesk şirket sitenize yönetici olarak giriş yapın.

2. Ayarlar **simgesini** seçin ve **Güvenlik** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu Oturum Açma](./media/freshdesk-tutorial/IC776770.png "Çoklu Oturum Açma")
  
    a. **Tek İşaret Açma (SSO) için,** **Aç'ı**seçin.

    b. **SAML SSO'yi**seçin.

    c. **SAML Giriş URL** metin kutusunda, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. Oturum **Açma URL** metin kutusuna, Azure portalından kopyaladığınız **Logout URL** değerini yapıştırın.

    e. Güvenlik **Sertifikası Parmak İzi** metin kutusuna, daha önce elde ettiğiniz **Cert Hash(sha256)** değerini yapıştırın.
  
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

Bu bölümde, Britta Simon'ın FreshDesk'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **FreshDesk'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **FreshDesk**yazın ve seçin.

    ![Uygulamalar listesindeki FreshDesk bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-freshdesk-test-user"></a>FreshDesk test kullanıcısı oluşturun

Azure AD kullanıcılarının FreshDesk'e giriş yapabilmeleri için FreshDesk'e dahil edilmeleri gerekir.  
FreshDesk durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Freshdesk** kiracınıza giriş yapın.

2. Üstteki menüde **Yönetici'yi**tıklatın.

    ![Yönetici](./media/freshdesk-tutorial/IC776772.png "Yönetici")

3. Genel **Ayarlar** sekmesinde, **Aracılar'ı**tıklatın.
  
    ![Aracılar](./media/freshdesk-tutorial/IC776773.png "Aracılar")

4. **Yeni Aracı'yı**tıklatın.

    ![Yeni Ajan](./media/freshdesk-tutorial/IC776774.png "Yeni Ajan")

5. Aracı Bilgileri iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Acente Bilgileri](./media/freshdesk-tutorial/IC776775.png "Acente Bilgileri")

    a. **E-posta** metin kutusuna, sağlamak istediğiniz Azure REKLAM hesabının Azure AD e-posta adresini yazın.

    b. Tam **Ad** metin kutusuna, sağlamak istediğiniz Azure REKLAM hesabının adını yazın.

    c. **Başlık** metin kutusuna, sağlamak istediğiniz Azure REKLAM hesabının başlığını yazın.

    d. **Kaydet**'e tıklayın.

    >[!NOTE]
    >Azure AD hesap sahibi, etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
    >
    >[!NOTE]
    >Azure AD kullanıcı hesaplarını FreshDesk'e sağlamak için Freshdesk tarafından sağlanan diğer Freshdesk kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki FreshDesk döşemesini tıklattığınızda, SSO'yu kurduğunuz FreshDesk'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

