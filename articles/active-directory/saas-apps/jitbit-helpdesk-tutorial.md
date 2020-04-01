---
title: 'Öğretici: Jitbit Yardım Masası ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Jitbit Yardım Masası arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 29addcd62afd193af83196b2d942e9778ff3f031
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67099419"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Öğretici: Jitbit Yardım Masası ile Azure Active Directory entegrasyonu

Bu eğitimde, Jitbit Yardım Masası'nı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Jitbit Yardım Masası'nı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Jitbit Yardım Masası'na erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Jitbit Yardım Masası'nda (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Jitbit Yardım Masası ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Jitbit Helpdesk tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Jitbit Yardım Masası **SP** başlatılan SSO destekler

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Galeriden Jitbit Yardım Masası ekleme

Jitbit Yardım Masası'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Jitbit Yardım Masası eklemeniz gerekir.

**Galeriden Jitbit Yardım Masası eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Jitbit Yardım Masası**yazın, sonuç panelinden **Jitbit Yardım Masası'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Jitbit Helpdesk](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Jitbit Helpdesk ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Jitbit Yardım Masası'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmaişlemlerini Jitbit Helpdesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Jitbit Yardım Masası Tek Oturum Açma](#configure-jitbit-helpdesk-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Jitbit Yardım Masası test kullanıcısını oluşturun](#create-jitbit-helpdesk-test-user)** - Jitbit Yardım Masası'nda kullanıcının Azure AD gösterimine bağlı Britta Simon'ın bir muadili olsun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Jitbit Helpdesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Jitbit Yardım Masası** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Jitbit Yardım Masası Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    | |
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değeri almak için [Jitbit Helpdesk Müşteri destek ekibine](https://www.jitbit.com/support/) başvurun.

    b. Tanımlayıcı **(Entity ID)** metin kutusuna aşağıdaki gibi bir URL yazın:`https://www.jitbit.com/web-helpdesk/`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Jitbit Yardım Masası'nı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-jitbit-helpdesk-single-sign-on"></a>Jitbit Yardım Masası'nı YapılandırTek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Jitbit Helpdesk şirket sitenizde oturum açın.

1. Üstteki araç çubuğunda **İdare'yi**tıklatın.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. **Genel ayarları**tıklatın.

    ![Kullanıcılar, şirketler ve izinler](./media/jitbit-helpdesk-tutorial/ic777680.png "Kullanıcılar, şirketler ve izinler")

1. Kimlik **Doğrulama ayarları** yapılandırma bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama ayarları](./media/jitbit-helpdesk-tutorial/ic777683.png "Kimlik doğrulama ayarları")

    a. **OneLogin**ile Tek Oturum Açma (SSO) kullanarak oturum açmak için **SAML 2.0 tek oturum**aç'ı etkinleştir'i seçin.

    b. **EndPoint URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. **Taban-64** kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından X.509 Sertifika** metin kutusuna yapıştırın

    d. **Değişiklikleri Kaydet'i**tıklatın.

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

Bu bölümde, Britta Simon'ın Jitbit Yardım Masası'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Jitbit Yardım Masası'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Jitbit Yardım Masası'nı**seçin.

    ![Uygulamalar listesindeki Jitbit Yardım Masası bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-jitbit-helpdesk-test-user"></a>Jitbit Yardım Masası test kullanıcıoluşturma

Azure AD kullanıcılarının Jitbit Yardım Masası'nda oturum açabilmeleri için Jitbit Yardım Masası'nda oturum açmaları gerekir. Jitbit Yardım Masası söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Jitbit Yardım Masası** kiracınızla oturum açın.

1. Üstteki menüde **İdare'yi**tıklatın.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. **Kullanıcılar, şirketler ve izinler'i**tıklatın.

    ![Kullanıcılar, şirketler ve izinler](./media/jitbit-helpdesk-tutorial/ic777682.png "Kullanıcılar, şirketler ve izinler")

1. **Kullanıcı Ekle'yi**tıklatın.

    ![Kullanıcı ekle](./media/jitbit-helpdesk-tutorial/ic777685.png "Kullanıcı ekle")

1. Oluştur bölümünde, sağlamak istediğiniz Azure REKLAM hesabının verilerini aşağıdaki gibi yazın:

    ![Oluştur](./media/jitbit-helpdesk-tutorial/ic777686.png "Oluştur")

   a. Kullanıcı **adı** metin **kutusuna, BrittaSimon**gibi kullanıcı adını yazın.

   b. **E-posta** metin kutusunda, kullanıcının **BrittaSimon@contoso.com**e-postasını yazın.

   c. Ad **metin** kutusunda, **Britta**gibi kullanıcının ilk adını yazın.

   d. **Soyadı** metin kutusunda, **Simon**gibi kullanıcının soyadını yazın.

   e. **Oluştur'u**tıklatın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Jitbit Helpdesk tarafından sağlanan diğer Jitbit Helpdesk kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Jitbit Yardım Masası döşemesini tıklattığınızda, SSO'yu kurduğunuz Jitbit Yardım Masası'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
