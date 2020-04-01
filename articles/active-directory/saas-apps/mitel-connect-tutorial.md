---
title: 'Öğretici: Mitel Connect ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Mitel Connect arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: 30a4dcbd15d8a47d99b3a61879a7e61938d3b133
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160542"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect"></a>Öğretici: Mitel MiCloud Connect ile Azure Active Directory entegrasyonu

Bu eğitimde, Mitel MiCloud Connect'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. MiCloud Connect'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Kurumsal kimlik bilgilerini kullanarak MiCloud Connect uygulamalarına erişimi olan Azure AD'da kontrol edebilirsiniz.
* Hesabınızdaki kullanıcıların Azure AD hesaplarıyla MiCloud Connect (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.


Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini MiCloud Connect ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği

  Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bir Mitel MiCloud Connect hesabı

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD tek oturum açma (SSO) yapılandırmave test edin.

* Mitel **Connect, SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-mitel-connect-from-the-gallery"></a>Galeriden Mitel Connect ekleme

Mitel Connect'in Azure AD'ye entegrasyonunu yapılandırmak için Galeriden Mitel Connect'i Azure portalındaki yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Mitel Connect eklemek için aşağıdaki adımları yapın:**

1. **[Azure portalda](https://portal.azure.com)** sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. **Kurumsal Uygulamalar'ı** tıklatın ve ardından **Tüm Uygulamalar'ı**tıklatın.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. **Yeni uygulamayı**tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama alanına **Mitel Connect** yazın, sonuçlar panelinden **Mitel Bağlan'ı** tıklatın ve sonra **Ekle'yi**tıklatın.

     ![Mitel Connect sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini MiCloud Connect ile yapılandıracak ve test edeceksiniz. Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile MiCloud Connect'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

MiCloud Connect ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki adımları tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak ve uygulama tarafındaki SSO ayarlarını yapılandırmak **[için SSO için MiCloud Connect'i Azure AD ile yapılandırın.](#configure-micloud-connect-for-sso-with-azure-ad)**
2. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
3. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
4. MiCloud Connect hesabınızda britta Simon'ın bir örneğine sahip olmak için **[bir Mitel MiCloud Connect test kullanıcısı oluşturun](#create-a-mitel-micloud-connect-test-user)** ve kullanıcının Azure REKLAM gösterimine bağlı olun.
5. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>Azure AD ile SSO için MiCloud Connect'i yapılandır

Bu bölümde, Azure portalında MiCloud Connect için Azure AD oturumunu etkinleştirecek ve MiCloud Connect hesabınızı Azure AD'yi kullanarak SSO'ya izin verecek şekilde yapılandıracaksınız.

Azure AD için MiCloud Connect'i SSO ile yapılandırmak için Azure portalını ve Mitel Hesabı portalını yan yana açmak en kolayıdır. Azure portalından Mitel Hesabı portalına ve bazı bilgileri Mitel Hesabı portalına kopyalamanız gerekir.


1. [Azure portalında](https://portal.azure.com/)yapılandırma sayfasını açmak için aşağıdakileri yapın:

    a. **Mitel Connect** uygulama tümleştirme **sayfasında, Tek oturum açma'yı**tıklatın.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

    b. Tek **oturum açma yöntemi** iletişim kutusunda **SAML'yi**tıklatın.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)
    
    SAML tabanlı oturum açma sayfası görüntülenir.

2. Mitel Hesabı portalındaki yapılandırma iletişim kutusunu açmak için aşağıdakileri yapın:

    a. Telefon **Sistemi** menüsünde **Eklenti Özellikleri'ni**tıklatın.

    b. Tek Oturum **Açma'nın**sağında, **Etkinleştir** veya **Ayarlar'ı**tıklatın.
    
    Tek Oturum Açma Ayarlarını Bağla iletişim kutusu görüntülenir.
    
3. Tek **Oturum Açma onay** kutusunu etkinleştir'i seçin.
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_Enable.png)


4. Azure portalında, **Temel SAML Yapılandırması** bölümündeki **Düzenleme** simgesini tıklatın.
    ![image](common/edit-urls.png)

    Temel SAML Yapılandırma iletişim kutusu görüntülenir.

5.  URL'yi Mitel Hesabı portalındaki **Mitel Tanımlayıcı (Entity ID)** alanından kopyalayın ve Azure portalındaki **Tanımlayıcı (Entity ID)** alanına yapıştırın.

6. URL'yi Mitel Hesabı portalındaki **Yanıt URL'sinden (İddia Tüketici Hizmeti URL'si)** kopyalayın ve Azure portalındaki **Yanıt URL'sine (İddia Tüketici Hizmeti URL'si)** yapıştırın.  
   ![image](./media/mitel-connect-tutorial/Mitel_Azure_BasicConfig.png)

7. URL metin kutusunda **Oturum Aç** kutusuna aşağıdaki URL'lerden birini yazın:

    * **https://portal.shoretelsky.com**- Mitel Hesabı portalını varsayılan Mitel uygulaması olarak kullanmak
    * **https://teamwork.shoretel.com**- Teamwork'i varsayılan Mitel uygulamanız olarak kullanmak

    **NOT**: Varsayılan Mitel uygulaması, kullanıcı Erişim Paneli'ndeki Mitel Connect döşemesini tıklattığında erişilen uygulamadır. Bu, Azure AD'den bir test kurulumu yaparken erişilen uygulamadır.

8. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda **Kaydet'i** tıklatın.

9. Azure portalındaki **SAML tabanlı oturum açma** sayfasındaki **SAML İmza Sertifikası** bölümünde, İmza **Sertifikasını** indirmek ve bilgisayarınıza kaydetmek için Sertifikanın yanına **İndir'i (Base64)** tıklatın. **Download**
    ![image](./media/mitel-connect-tutorial/Azure_SigningCert.png)

10. İmzaSertifikası dosyasını bir metin düzenleyicisinde açın, dosyadaki tüm verileri kopyalayın ve ardından Mitel Hesabı portalındaki **İmza Sertifikası** alanına verileri yapıştırın. 
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_SigningCert.png)

11. Azure portalının **SAML tabanlı oturum** açma sayfasındaki **Kurulum Mitel Connect** bölümünde aşağıdakileri yapın:

    a. **URL'yi Giriş URL** alanından kopyalayın ve Mitel Hesabı portalındaki Oturum Açma **URL** alanına yapıştırın.

    b. URL'yi **Azure AD Tanımlayıcı** alanından kopyalayın ve Mitel Hesabı portalındaki Entity **ID** alanına yapıştırın.
    ![image](./media/mitel-connect-tutorial/Mitel_Azure_SetupConnect.png)

12. Mitel Hesabı portalındaki **Tek Oturum Açma Ayarları** bağlantı kutusuna **Kaydet'i** tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**tıklatın, **Kullanıcılar'ı**tıklatın ve ardından **Tüm Kullanıcılar'ı**tıklatın.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni kullanıcıyı** tıklatın.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özellikleri iletişim kutusunda aşağıdaki adımları yapın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**yazın.
  
    b. Kullanıcı **adı** alanında, şirketinizin etki\>alanı brittasimon@\<yazın. \<uzantısı\>.  
Örneğin, BrittaSimon@contoso.com.

    c. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Mitel Connect'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**tıklatın ve ardından **Tüm uygulamalar'ı**tıklatın.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mitel Connect'i**tıklatın.

    ![Uygulamalar listesindeki Mitel Connect bağlantısı](common/all-applications.png)

3. Soldaki menüde Kullanıcılar **ve gruplar'ı**tıklatın.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**tıklatın, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar'ı** tıklatın.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **Britta Simon'ı** seçin ve ardından ekranın alt kısmında **seç'i** tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusundaki listeden kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmında **Seç'i** tıklatın.

7. Atama **Ekle** iletişim **kutusunda, Atay'ı**tıklatın.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Mitel MiCloud Connect test kullanıcısı oluşturma

Bu bölümde, MiCloud Connect hesabınızda Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcılar tek oturum açmadan önce oluşturulmalı ve etkinleştirilmelidir.

Mitel Hesabı portalına kullanıcı ekleme hakkında daha fazla bilgi için Mitel Bilgi Tabanı'ndaki [Kullanıcı ekleme](https://docs.shoretel.com/connectcloud/Account/Users/AddUser) makalesine bakın.

MiCloud Connect hesabınızda aşağıdaki ayrıntılarla bir kullanıcı oluşturun:

  * **Adı:** Britta Simon

* **İş E-posta Adresi:**`brittasimon@<yourcompanydomain>.<extension>`   
(Örnek: [brittasimon@contoso.com](mailto:brittasimon@contoso.com))

* **Kullanıcı adı:**`brittasimon@<yourcompanydomain>.<extension>`  
(Örnek: [brittasimon@contoso.com](mailto:brittasimon@contoso.com); kullanıcının kullanıcı adı genellikle kullanıcının iş e-posta adresi yle aynıdır)

**NOT:** Kullanıcının MiCloud Connect kullanıcı adı, kullanıcının Azure'daki e-posta adresiyle aynı olmalıdır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınayacaksınız.

Erişim Paneli'ndeki Mitel Connect döşemesini tıklattığınızda, **URL'de Oturum Aç** alanında varsayılan olarak yapılandırılan MiCloud Connect uygulamasına oturum açmanız için otomatik olarak yeniden yönlendirilmelisiniz. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
