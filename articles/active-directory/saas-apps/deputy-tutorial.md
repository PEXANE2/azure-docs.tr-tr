---
title: 'Öğretici: Yardımcısı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Yardımcısı arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd021b9547048e9f5218ce4f2b8b110b15d05963
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048570"
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Öğretici: Yardımcısı ile Azure Active Directory entegrasyonu

Bu eğitimde, Yardımcı'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Yardımcı Yardımcısı'nı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Yardımcısı'na erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Yardımcısı (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Yardımcısı ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Tek oturum açma özellikli abonelik yardımcısı

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Milletvekili **SP** ve **IDP'yi** destekliyor SSO'ya destek verdi

## <a name="adding-deputy-from-the-gallery"></a>Galeriden Vekil Ekleme

Yardımcısı'nın Azure AD'ye tümleştirilmesini yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Yardımcısı eklemeniz gerekir.

**Galeriden Yardımcısı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Yardımcısı**yazın , sonuç panelinden **Yardımcısı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde vekil](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırır ve test esunarsınız.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Yardımcısı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisikurulması gerekir.

Azure AD'yi Yardımcısı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Uygulama](#configure-deputy-single-sign-on)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Tek Oturum Açma'yı yapılandırır.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Yardımcı test kullanıcısı oluşturun](#create-deputy-test-user)** - Kullanıcının Azure AD gösterimine bağlı Yardımcısı'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Yardımcısı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Uygulama Yardımcısı** tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Etki Alanı Yardımcısı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs` |

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Etki Alanı Yardımcısı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<your-subdomain>.<region>.deputy.com`

    >[!NOTE]
    > Yardımcı bölge soneki isteğe bağlıdır veya bunlardan birini kullanmalıdır: au | na | ab |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [İstemci Yardımcısı destek ekibine](https://www.deputy.com/call-centers-customer-support-scheduling-software) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Yardımcı **Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-deputy-single-sign-on"></a>Yapılandırılan Yardımcısı Tek Oturum Açma

1. Aşağıdaki URL'ye`https://(your-subdomain).deputy.com/exec/config/system_config`gidin: . Güvenlik **Ayarları'na** gidin ve **Edit'i**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/deputy-tutorial/tutorial_deputy_004.png)

2. Bu **Güvenlik Ayarları** sayfasında aşağıdaki adımları gerçekleştirin.

    ![Tek İşaret-On'u Yapılandır](./media/deputy-tutorial/tutorial_deputy_005.png)
    
    a. **Sosyal Girişi**Etkinleştirin.
   
    b. Azure portalından indirilen Base64 kodlu sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından OpenSSL Sertifikası** metin kutusuna yapıştırın.
   
    c. SAML SSO URL textbox'ında,`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. SAML SSO URL textbox'ında alt etki alanınızın değiştirin. `<your subdomain>`
   
    e. SAML SSO URL textbox'ında, Azure portalından kopyalamış olduğunuz Giriş `<saml sso url>` **URL'sini** değiştirin.
   
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

Bu bölümde, Britta Simon'ın Yardımcısı'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Yardımcısı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Yardımcısı'nı**seçin.

    ![Uygulamalar listesindeki Yardımcı bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-deputy-test-user"></a>Yardımcı test kullanıcısı oluşturma

Azure AD kullanıcılarının Yardımcısı'nda oturum açabilmeleri için, bu kullanıcıların Yardımcısı'na sağlanması gerekir. Vekil durumunda, sağlama manuel bir görevdir.

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Yönetici olarak Yardımcı şirket sitenizde oturum açın.

2. Üst gezinti bölmesine, **Kişiler'i**tıklatın.
   
    ![People](./media/deputy-tutorial/tutorial_deputy_001.png "People")

3. Kişi **Ekle** düğmesini tıklatın ve **tek bir kişi ekle'yi**tıklatın.
   
    ![Kişi Ekle](./media/deputy-tutorial/tutorial_deputy_002.png "Kişi Ekle")

4. Aşağıdaki adımları gerçekleştirin ve Davet & **Kaydet'i**tıklatın.
   
    ![Yeni Kullanıcı](./media/deputy-tutorial/tutorial_deputy_003.png "Yeni Kullanıcı")

    a. **Ad** metin kutusunda, **BrittaSimon**gibi kullanıcının adını yazın.
   
    b. **E-posta** metin kutusuna, sağlamak istediğiniz bir Azure REKLAM hesabının e-posta adresini yazın.
   
    c. **Textbox'taki Çalışma'ya** işletme adını yazın.
   
    d. **Kaydet & Davet et** düğmesini tıklatın.

5. Azure AD hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler. Azure AD kullanıcı hesaplarını sağlamak için Yardımcısı tarafından sağlanan diğer kullanıcı yardımcısı kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Yardımcı döşemeyi tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Yardımcı ile oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

