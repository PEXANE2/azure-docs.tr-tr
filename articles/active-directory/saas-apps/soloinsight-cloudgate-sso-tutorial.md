---
title: 'Öğretici: Soloinsight-CloudGate SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Soloinsight-CloudGate SSO arasında tek oturum açma işlemlerini nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b88822f164e0fe8cdf55eddfa981644f725e01f3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159920"
---
# <a name="tutorial-integrate-soloinsight-cloudgate-sso-with-azure-active-directory"></a>Öğretici: Soloinsight-CloudGate SSO'yu Azure Active Directory ile tümleştirin

Bu eğitimde, Soloinsight-CloudGate SSO'nun Azure Active Directory (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Soloinsight-CloudGate SSO'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Soloinsight-CloudGate SSO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Soloinsight-CloudGate SSO'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Soloinsight-CloudGate SSO tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Soloinsight-CloudGate SSO, **SP** tarafından başlatılan SSO'ya destek verir.

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Galeriden Soloinsight-CloudGate SSO ekleme

Soloinsight-CloudGate SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Soloinsight-CloudGate SSO eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Soloinsight-CloudGate SSO** yazın.
1. Sonuç panelinden **Soloinsight-CloudGate SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak Soloinsight-CloudGate SSO ile yapılandırın ve test edin. SSO'nun çalışması için, Soloinsight-CloudGate SSO'da bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Soloinsight-CloudGate SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Soloinsight-CloudGate SSO'yu](#configure-soloinsight-cloudgate-sso)** uygulama tarafındaki SSO ayarlarını yapılandırmak için yapılandırın.
3. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Soloinsight-CloudGate SSO test kullanıcısını,](#create-soloinsight-cloudgate-sso-test-user)** kullanıcının Azure REKLAM gösterimine bağlı Soloinsight-CloudGate SSO'da Britta Simon'ın bir muadili olması için oluşturun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Soloinsight-CloudGate SSO** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    1. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.sigateway.com/login`

    1. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.sigateway.com/process/sso`

   > [!NOTE]
   > Bu değerler gerçek değildir. Bu değerleri, öğreticinin Daha sonra **Yapılandırılan Soloinsight-CloudGate SSO Tek İşaret-On** bölümünde açıklanan URL ve Tanımlayıcı'daki gerçek İşaret ile güncelleştirin.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Soloinsight-CloudGate SSO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-soloinsight-cloudgate-sso"></a>Soloinsight-CloudGate SSO'ya yapılandır

1. Soloinsight-CloudGate SSO içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Setup Soloinsight-CloudGate SSO'ya** tıklayın ve sizi Soloinsight-CloudGate SSO uygulamasına yönlendirecektir. Buradan, Soloinsight-CloudGate SSO'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Soloinsight-CloudGate SSO'yu el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Soloinsight-CloudGate SSO şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Temel SAML'yi yapılandırırken Azure portalına yapıştırılacak değerleri almak için, kimlik bilgilerinizi kullanarak CloudGate Web Portalı'nda oturum açın ve ardından Aşağıdaki yolüzerinde bulunabilen SSO ayarlarına erişin **Home>Administration>System ayarları>Genel**.

    ![CloudGate SSO Ayarları](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

5. **SAML Tüketici URL'si**

    * Kullanılabilir bağlantıları **Saml Tüketici** URL'si ve **Yeniden Yönlendirme URL** alanlarına kopyalayın ve bunları Sırasıyla Tanımlayıcı **(Entity ID)** ve **YanıtLA URL** alanları için Azure portalı Temel **SAML Yapılandırma** sı bölümüne yapıştırın.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

6. **SAML İmza Sertifikası**

    * Azure portalı SAML İmza Lama Sertifikası listelerinden indirilen Sertifika (Base64) dosyasının kaynağına gidin ve üzerine sağ tıklayın. Listeden **Notepad++ seçeneğiyle Edit** seçeneğini seçin. 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * Sertifika (Base64) Notepad++ dosyasındaki içeriği kopyalayın.

        ![Sertifika kopyası](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * İçeriği CloudGate Web Portal SSO ayarları **Sertifika** alanına yapıştırın ve Kaydet düğmesine tıklayın.

        ![Sertifika portalı](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

7. **Varsayılan Grup**

    * CloudGate Web Portalı'ndaki Varsayılan **Grup** seçeneğinin açılır listesinden **İş Yöneticisi'ni** seçin

        ![Varsayılan grup](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

8. **AD Tanımlayıcı ve Giriş URL'si**

    * Azure portalı **Set up Soloinsight-CloudGate SSO** yapılandırmalarından kopyalanan **Giriş URL'si** CloudGate Web Portal SSO ayarları bölümüne girilmelidir.

    * **CloudGate** Web Portalı **AD Girişi URL** alanına Azure portalından Giriş URL bağlantısını yapıştırın.

    * Azure portalından **Azure AD Tanımlayıcı** bağlantısını CloudGate Web Portalı **AD Tanımlayıcı** alanına yapıştırın

        ![Reklam girişi](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Soloinsight-CloudGate SSO'ya erişim sağlayarak Azure oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Soloinsight-CloudGate SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Soloinsight-CloudGate SSO test kullanıcısı oluşturma

Bir test kullanıcısı oluşturmak için CloudGate Web Portalınızın ana menüsünden **Çalışanları** seçin ve Yeni Ek çalışan formunu doldurun. Test kullanıcısına atanacak Olan Yetki Düzeyi, gerekli tüm alanlar doldurulduğunda **İş Yöneticisi'nin** **Oluştur'a** tıklamasIdır.

![Çalışan testi](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Soloinsight-CloudGate SSO döşemesini seçtiğinizde, SSO'yu kurduğunuz Soloinsight-CloudGate SSO'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)