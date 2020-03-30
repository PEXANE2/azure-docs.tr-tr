---
title: 'Öğretici: Coda ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Coda arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: acaf2012-ef2e-4ce0-8467-ceece3bae50e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/23/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74da278dbbc0ac32407c345524e224ca5f7616da
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77194761"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-coda"></a>Öğretici: Coda ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Coda'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Coda'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Coda erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Coda'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Coda tek oturum açma (SSO) GDrive tümleştirmesi devre dışı bırakılmış abonelik (Enterprise) özelliğini etkinleştirildi. Şu anda etkinse, Kuruluşunuz için GDrive tümleştirmesini devre dışı düşürmek için [Coda destek ekibine](mailto:support@coda.io) başvurun.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **Coda, IDP'nin** başlattığı SSO'ya destek verdi

* Coda **Just In Time** kullanıcı sağlama destekler

* Coda'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin sızma ve sızmalarını gerçek zamanlı olarak koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-coda-from-the-gallery"></a>Galeriden Coda ekleme

Coda'nın Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Coda'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Coda** yazın.
1. Sonuç panelinden **Coda'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-coda"></a>Coda için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Coda ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Coda'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Coda ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Coda SSO yapılandırmasına başlayın](#begin-configuration-of-coda-sso)** - Coda'da SSO yapılandırmasına başlamak için.
1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
   * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
   * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Coda SSO'u yapılandırın](#configure-coda-sso)** - Coda'daki tek oturum açma ayarlarının yapılandırmasını tamamlamak için.
   * **[Coda test kullanıcıoluşturun](#create-coda-test-user)** - Coda'da B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="begin-configuration-of-coda-sso"></a>Coda SSO yapılandırmabaşlatın

Başlamak için Coda'da aşağıdaki adımları izleyin.

1. Coda'da **Organizasyon ayarları** panelinizi açın.

   ![Kuruluş Ayarlarını Aç](media/coda-tutorial/org-settings.png)

1. Kuruluşunuzun GDrive Tümleştirmesi'ni kapattığından emin olun. Şu anda etkinse, GDrive'dan geçiş ekolüne yardımcı olmak için [Coda destek ekibine](mailto:support@coda.io) başvurun.

   ![GDrive Devre Dışı](media/coda-tutorial/gdrive-off.png)

1. **SSO (SAML) ile Kimlik Doğrulaması**altında, **SamL'i Yapılandır** seçeneğini seçin.

   ![Saml Ayarları](media/coda-tutorial/saml-settings-link.png)

1. Sonraki adımlarda gereksinim duymanız gereken **Entity ID** ve **SAML Yanıt URL**değerlerine dikkat edin.

   ![Azure'da kullanılacak Varlık Kimliği ve SAML Yanıt URL'si](media/coda-tutorial/azure-settings.png)

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Coda** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

   a. **Tanımlayıcı** metin kutusuna yukarıdan "Entity ID"i girin. Bu desen takip etmelidir:`https://coda.io/samlId/<CUSTOMID>`

   b. **Yanıtla URL** metin kutusuna yukarıdan "SAML Yanıt URL'si" girin. Bu desen takip etmelidir:`https://coda.io/login/sso/saml/<CUSTOMID>/consume`

   > [!NOTE]
   > Değerleriniz yukarıdakilerden farklı olacaktır; değerlerinizi Coda'nın "SamL'i Yapılandır" konsolunda bulabilirsiniz. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Coda'yı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Coda'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Coda'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-coda-sso"></a>Coda SSO'nun yapılandırılsın

Kurulumu tamamlamak için, Coda **Configure Saml** paneline Azure Active Directory'den değerler girersiniz.

1. Coda'da **Organizasyon ayarları** panelinizi açın.
1. **SSO (SAML) ile Kimlik Doğrulaması**altında, **SamL'i Yapılandır** seçeneğini seçin.
1. **SAML Sağlayıcısını** **Azure Etkin Dizini**olarak ayarlayın.
1. **Kimlik Sağlayıcı Giriş URL'sinde,** Oturum Açma **URL'sini** Azure konsolundan yapıştırın.
1. **Kimlik Sağlayıcı Veren'de**Azure **AD Tanımlayıcısını** Azure konsolundan yapıştırın.
1. **Kimlik Sağlayıcı Ortak Sertifikası'nda,** Sertifika **Yükle** seçeneğini seçin ve daha önce indirdiğiniz sertifika dosyasını seçin.
1. **Kaydet'i**seçin.

Bu, SAML SSO bağlantı kurulumu için gerekli çalışmayı tamamlar.

### <a name="create-coda-test-user"></a>Coda test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Coda oluşturulur. Coda, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Coda'da zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Coda döşemesini tıklattığınızda, SSO'yu kurduğunuz Coda'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Coda'yı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Coda'yı gelişmiş görünürlük ve kontrollerle nasıl koruyabilen](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
