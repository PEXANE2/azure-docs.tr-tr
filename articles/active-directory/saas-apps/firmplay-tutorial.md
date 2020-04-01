---
title: 'Öğretici: FirmPlay ile Azure Active Directory entegrasyonu - İşe Alım için Çalışan Savunuculuğu | Microsoft Dokümanlar'
description: Azure Active Directory ve FirmPlay - İşe Alım için Çalışan Savunuculuğu arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 2bdc3a13582f079cc9325ef1c6949c3ae10138eb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67102519"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Öğretici: FirmPlay ile Azure Active Directory entegrasyonu - İşe Alım için Çalışan Savunuculuğu

Bu eğitimde, FirmPlay - Çalışan Savunuculuğu ile Azure Active Directory (Azure AD) ile nasıl entegre edilebilirsiniz.
FirmPlay'i Entegre Etme - Azure AD ile İşe Alma için Çalışan Savunuculuğu size aşağıdaki avantajları sağlar:

* FirmPlay - Recruiting için Çalışan Savunuculuğu'na erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla FirmPlay - Çalışan İşe Alım Için Savunuculuk (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini FirmPlay - Recruiting için Çalışan Savunuculuğu ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* FirmPlay - Tek oturum açma özellikli aboneliği Işe almak için Çalışan Savunuculuğu

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* FirmPlay - İşe Alım için Çalışan Savunuculuğu **SP SSO** başlatılan destekler

## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>FirmPlay ekleme - Galeriden İşe Personel Için Çalışan Savunuculuğu

FirmPlay - Çalışan Savunması, Azure AD'ye işe alma için tümleştirmeyi yapılandırmak için, galeriden Işe alma için FirmPlay - Çalışan Savunuculuğu'nu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**FirmPlay - Galeriden İşe Alım için Çalışan Savunuculuğu'nu eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **FirmPlay yazın - İşe Alma için Çalışan Savunuculuğu**, FirmPlay seçin - Sonuç panelinden **Işe Alma için Çalışan Savunuculuğu** sonra uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![FirmPlay - Sonuç listesinde Işe Alma için Çalışan Savunuculuğu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak FirmPlay - Çalışan Bağlılığı için Çalışan Savunuculuğu ile Azure AD oturumlarını yapılandırın ve test esiniz.
Tek oturum açma nın işe yarayacağı nda, FirmPlay - İşe Alım için Çalışan Savunuculuğu'nda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumaçma işlemlerini FirmPlay - Recruiting için Çalışan Savunuculuğu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak **[için FirmPlay -Tek Oturum Açma için Çalışan Savunuculuğu](#configure-firmplay---employee-advocacy-for-recruiting-single-sign-on)** - yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[FirmPlay oluşturun - Test kullanıcısını işe almak için Çalışan Savunuculuğu](#create-firmplay---employee-advocacy-for-recruiting-test-user)** - FirmPlay'de Britta Simon'ın bir muadili olması - Kullanıcının Azure AD temsiline bağlı İşe Alım için Çalışan Savunuculuğu.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

FirmPlay - Recruiting için Çalışan Savunuculuğu ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **FirmPlay - Recruiting uygulama** tümleştirme sayfasında Çalışan Savunuculuğu **sayfasında, Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![FirmPlay - Alan Adı ve URL'leri tek oturum açma bilgileri için Çalışan Savunuculuğu](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<your-subdomain>.firmplay.com/`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. FirmaPlay - [Müşteri destek ekibini işe almak için çalışan savunuculuğu](mailto:engineering@firmplay.com) ile iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **FirmPlay-Recruiting için Çalışan Savunuculuğu** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-firmplay---employee-advocacy-for-recruiting-single-sign-on"></a>FirmPlay'i Yapılandır - Tek Oturum Açma Için Çalışan Savunuculuğu

FirmPlay'de tek oturum açma - İşe Alma tarafı **için Çalışan Savunuculuğu'nu** yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından [FirmPlay- Çalışan Destek ekibine](mailto:engineering@firmplay.com)yardımcı olmak için çalışan savunuculuğu'na göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın FirmPlay - Recruiting için Çalışan Savunuculuğu'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında Kurumsal **Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **FirmPlay - İşe Alım için Çalışan Savunuculuğu'nu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **FirmPlay - İşe Alım için Çalışan Savunuculuğu'nu**seçin.

    ![FirmPlay - Uygulamalar listesinde işe bağlantı için Çalışan Savunuculuk](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-firmplay---employee-advocacy-for-recruiting-test-user"></a>FirmPlay oluşturun - Test kullanıcıİşe için Çalışan Savunuculuk

Bu bölümde, FirmPlay Britta Simon adlı bir kullanıcı oluşturmak - İşe Çalışan Savunuculuk. FirmPlay ile çalışın - FirmPlay kullanıcıları eklemek için Destek Ekibi İşe Çalışan [Savunuculuğu](mailto:engineering@firmplay.com) - İşe Alma platformu için Çalışan Savunuculuğu. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Kiremit Alımı için FirmPlay - Çalışan Savunuculuğu'nu tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz FirmPlay - Çalışan Savunuculuğu'nda oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

