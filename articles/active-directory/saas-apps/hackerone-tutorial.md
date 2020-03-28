---
title: 'Öğretici: HackerOne ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve HackerOne arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: effd0593384190eb1a1cf261305dd61818c3d0b0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76120885"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Öğretici: HackerOne ile Azure Active Directory entegrasyonu

Bu eğitimde, HackerOne'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
HackerOne'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de, HackerOne erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak HackerOne (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini HackerOne ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* HackerOne tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* HackerOne **SP** başlatılan SSO destekler
* HackerOne **Just In Time** kullanıcı sağlama destekler

## <a name="adding-hackerone-from-the-gallery"></a>Galeriden HackerOne ekleme

HackerOne'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize HackerOne eklemeniz gerekir.

**Galeriden HackerOne eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **HackerOne**yazın, sonuç panelinden **HackerOne'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![HackerOne sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Yapılandırıp Test Edebilirsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile HackerOne'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını HackerOne ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[HackerOne Tek Oturum](#configure-hackerone-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[HackerOne test kullanıcısını oluşturun](#create-hackerone-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olan HackerOne'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı HackerOne ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **HackerOne** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![HackerOne Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna aşağıdakileri girin:`https://hackerone.com/users/saml/sign_in?email=<configured domain>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna aşağıdakileri girin:`hackerone.com`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **HackerOne'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-hackerone-single-sign-on"></a>HackerOne Tek Oturum Açma'yı Yapılandır

1. Yönetici olarak HackerOne kiracınıza oturum açın.

2. Üstteki menüde **Ayarlar'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/hackerone-tutorial/tutorial_hackerone_001.png)

3. Kimlik **Doğrulama'ya** gidin ve **SAML ayarlarını ekle'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/hackerone-tutorial/tutorial_hackerone_003.png)

4. **SAML Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/hackerone-tutorial/tutorial_hackerone_004.png)

    a. **E-posta Etki Alanı** metin kutusuna kayıtlı bir etki alanı yazın.

    b. **Tek Oturum URL** metin kutularında, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.

    c. İndirilen **Sertifika dosyanızı** Azure portalından Notepad'e açın, içeriğini panonuza kopyalayın ve **ardından X509 Sertifika** metin kutusuna yapıştırın.

    d. **Kaydet**'e tıklayın.

5. Kimlik Doğrulama Ayarları iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/hackerone-tutorial/tutorial_hackerone_005.png)

    a. **Testi Çalıştır'ı**tıklatın.

6. Test başarıyla bittiğinde ve **Durum** alanı **son test durumunu gösterdiğinde: başarı,** Onay için HackerOne'a göndermek için **İstek Doğrulama** düğmesini seçin.

    ![Onay için HackerOne'a gönderin](./media/hackerone-tutorial/tutorial-hackerone-006.png)

7. HackerOne ayarları onayladıktan sonra, tüm kullanıcılar için SSO kimlik doğrulaması gerektirecek Kullanıcıları **Geçir** düğmesini seçebilirsiniz.

    ![SAML'yi etkinleştir](./media/hackerone-tutorial/tutorial-hackerone-007.png)

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

Bu bölümde, Britta Simon'ın HackerOne'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **HackerOne'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, HackerOne'ı**seçin.

    ![Uygulamalar listesindeki HackerOne bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-hackerone-test-user"></a>HackerOne test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı HackerOne oluşturulur. HackerOne, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Zaten HackerOne yoksa, kimlik doğrulama sonra yeni bir oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki HackerOne döşemesini tıklattığınızda, SSO'yu kurduğunuz HackerOne'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
