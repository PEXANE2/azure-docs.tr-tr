---
title: 'Öğretici: TeamSeer ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve TeamSeer arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 3e505e4823db8c9f42bbff216f30326081110dcf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67088974"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Öğretici: TeamSeer ile Azure Active Directory entegrasyonu

Bu eğitimde, TeamSeer'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
TeamSeer'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* TeamSeer'a erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla TeamSeer 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini TeamSeer ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* TeamSeer tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* TeamSeer **SP** başlatılan SSO destekler

## <a name="adding-teamseer-from-the-gallery"></a>Galeriden TeamSeer ekleme

TeamSeer'In Azure AD'ye entegrasyonunu yapılandırmak için, galeriden TeamSeer'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden TeamSeer eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **TeamSeer**yazın, sonuç panelinden **TeamSeer'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde TeamSeer](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre TeamSeer ile Azure AD tek oturum açma işlemini yapılandırın ve test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile TeamSeer'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

TeamSeer ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[TeamSeer Tek Oturum Açma](#configure-teamseer-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[TeamSeer test kullanıcısını oluşturun](#create-teamseer-test-user)** - TeamSeer'da Kullanıcının Azure AD gösterimine bağlı Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

TeamSeer ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **TeamSeer** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![TeamSeer Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.teamseer.com/<companyid>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [TeamSeer Client destek ekibine](https://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **TeamSeer'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-teamseer-single-sign-on"></a>TeamSeer Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, TeamSeer şirket sitenizde yönetici olarak oturum açın.

1. İk **Admin**gidin.

    ![İk Yöneticisi](./media/teamseer-tutorial/ic789634.png "İk Yöneticisi")

1. **Kurulum'u**tıklatın.

    ![Kurulum](./media/teamseer-tutorial/ic789635.png "Kurulum")

1. **SAML sağlayıcı ayrıntılarını ayarla'yı**tıklatın.

    ![SAML Ayarları](./media/teamseer-tutorial/ic789636.png "SAML Ayarları")

1. SAML sağlayıcı ayrıntıları bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Ayarları](./media/teamseer-tutorial/ic789637.png "SAML Ayarları")

    a. **URL** metin kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    b. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **IdP Genel Sertifika** metin kutusuna yapıştırın.

1. SAML sağlayıcı yapılandırmasını tamamlamak için aşağıdaki adımları gerçekleştirin:

    ![SAML Ayarları](./media/teamseer-tutorial/ic789638.png "SAML Ayarları")

    a. Test **E-posta**Adresleri'nde, test kullanıcısının e-posta adresini yazın.
  
    b. **Veren** metin kutusuna, servis sağlayıcısının Veren URL'sini yazın.
  
    c. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın TeamSeer'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **TeamSeer'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **TeamSeer'ı**seçin.

    ![Uygulamalar listesindeteamSeer bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-teamseer-test-user"></a>TeamSeer test kullanıcısı oluşturma

Azure AD kullanıcılarının TeamSeer'da oturum açabilmeleri için ShiftPlanning'de oturum açmaları gerekir. TeamSeer durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **TeamSeer** şirket sitenizde yönetici olarak oturum açın.

1. İk ** \> Yönetici Kullanıcıları'na** gidin ve ardından **Yeni Kullanıcı sihirbazını çalıştır'ı**tıklatın.

    ![İk Yöneticisi](./media/teamseer-tutorial/ic789640.png "İk Yöneticisi")

1. Kullanıcı **Ayrıntıları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Detayları](./media/teamseer-tutorial/ic789641.png "Kullanıcı Detayları")

    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **Adı,** **Soyadı,** **Kullanıcı adı (E-posta adresi)** yazın.
  
    b. **İleri**'ye tıklayın.

1. Yeni bir kullanıcı eklemek için ekrandaki yönergeleri izleyin ve **Bitir'i**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için TeamSeer tarafından sağlanan diğer TeamSeer kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki TeamSeer döşemesini tıklattığınızda, SSO'yu kurduğunuz TeamSeer'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
