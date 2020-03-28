---
title: 'Öğretici: Microsoft Yol Tarifi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Directions arasında Microsoft'ta tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e6928a54692e52960c80e5279c2868d9b7026c4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158408"
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Öğretici: Microsoft Yol Tarifi ile Azure Active Directory entegrasyonu

Bu eğitimde, Microsoft'taki Yol Tarifleri'ni Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Yol Tariflerini Azure AD ile Microsoft'a tümleştirmek size aşağıdaki avantajları sağlar:

* Microsoft'ta Yol Tarifi'ne erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Microsoft'taki Yol Tarifleri'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Microsoft'taki Yol tarifi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Microsoft tek oturum açma özellikli abonelikle ilgili yol tarifleri

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Microsoft'un yol tarifi **SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-directions-on-microsoft-from-the-gallery"></a>Galeriden Microsoft'a Yol Tarifi Ekleme

Yol Tarifleri'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Microsoft'taki Yol Tarifleri'ni eklemeniz gerekir.

**Galeriden Microsoft'a Yol Tarifi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Microsoft'ta Yol Tarifi**yazın, sonuç panelinden **Microsoft'ta Yol Tarifi'ni** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Microsoft ile ilgili yol tarifleri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayanarak Azure AD oturum açma işlemlerini Microsoft'ta yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Microsoft'taki Yol Tarifi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Microsoft'ta yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Microsoft Tek Oturum Açma'da Yol Tariflerini Yapılandırın.](#configure-directions-on-microsoft-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Microsoft test kullanıcısı üzerinde Yol Tarifi oluşturun](#create-directions-on-microsoft-test-user)** - Kullanıcının Azure AD gösterimine bağlı olan Microsoft Yol Tarifi'nde Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Microsoft'ta Yol tarifi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), Microsoft uygulama tümleştirme **sayfasındaki Yol tariflerinde** **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Microsoft Etki Alanı ve URL'ler tek oturum açma bilgileri yle ilgili talimatlar](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Microsoft İstemci destek ekibindeki Yol Tarifleri'ne](mailto:service@DirectionsOnMicrosoft.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Microsoft'ta Yol Tarifi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-directions-on-microsoft-single-sign-on"></a>Microsoft Tek Oturum Açma'da Yol Tariflerini Yapılandırma

Microsoft tarafında **Yol tarifinde** tek oturum açma yapılandırmak için, indirilen **Metadata XML'yi** [Microsoft destek ekibindeki Yol Tarifleri'ne](mailto:service@DirectionsOnMicrosoft.com)göndermeniz gerekir. Microsoft destek ekibindeki Yönergelerin federe site üyeliğinizi bulmasını sağlamak için, şirket bilgilerinizi e-postanıza ekleyin.
    
>[!NOTE]
>Microsoft'ta Yol Tarifi için tek oturum açma [özelliğinin, Microsoft İstemci destek ekibindeki Yol Tarifi](mailto:service@DirectionsOnMicrosoft.com)tarafından etkinleştirilmesi gerekir. Tek oturum açma etkinleştirildiğinde bir bildirim alırsınız.

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

Bu bölümde, Britta Simon'ın Microsoft'taki Yol Tarifleri'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Microsoft'ta Yol Tarifi'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Microsoft'ta Yol Tarifi'ni**seçin.

    ![Uygulamalar listesinde Microsoft bağlantısı üzerinde Yol Tarifi](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-directions-on-microsoft-test-user"></a>Microsoft test kullanıcısında Yol Tarifi Oluşturma

Microsoft'ta Yol tarifine kullanıcı sağlama yapılandırmanız için bir eylem öğesi yoktur.  

Atanmış bir kullanıcı erişim panelini kullanarak Microsoft'ta Yol Tarifi'nde oturum açmaya çalıştığında, Microsoft'taki Yol tarifi kullanıcının var olup olmadığını denetler. Henüz kullanılabilir bir kullanıcı hesabı yoksa, otomatik olarak Directions on Microsoft tarafından oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Microsoft döşemesinde Yol Tarifi'ni tıklattığınızda, SSO'yu kurduğunuz Microsoft Yol Tarifi'nde otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

