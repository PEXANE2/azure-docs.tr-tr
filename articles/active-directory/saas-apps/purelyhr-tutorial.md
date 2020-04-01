---
title: 'Öğretici: Azure Active Directory purelyhr ile tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve PurelyHR arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56049e1b1253cd749a8e16061957c6b5b8786e3c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72594526"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-purelyhr"></a>Öğretici: PurelyHR ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, PurelyHR'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SaltHR'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SaltHR erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla PurelyHR'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* PurelyHR tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **PurelyHR, SP ve IDP'nin** başlattığı SSO'ya destek verdi
* PurelyHR **Just In Time** kullanıcı sağlama destekler

## <a name="adding-purelyhr-from-the-gallery"></a>Galeriden PurelyHR ekleme

PurelyHR'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize PurelyHR eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **PurelyHR** yazın.
1. Sonuç panelinden **PurelyHR'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-purelyhr"></a>Azure AD oturum açma işlemlerini PurelyHR için yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak PurelyHR ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile PurelyHR'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu PurelyHR ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[PurelyHR SSO'yu yapılandırın](#configure-purelyhr-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[PurelyHR test kullanıcısını oluşturun](#create-purelyhr-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olan PurelyHR'deki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **PurelyHR** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyID>.purelyhr.com/sso-consume`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyID>.purelyhr.com/sso-initiate`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [PurelyHR İstemci destek ekibine](https://support.purelyhr.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SaltHR'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın PurelyHR'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **PurelyHR'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-purelyhr-sso"></a>PurelyHR SSO'nun yapılandırılsın

1. Yapılandırmayı PurelyHR içinde otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **PurelyHR'i** ayarla'yı tıklatın ve sizi PurelyHR uygulamasına yönlendirecektir. Buradan, PurelyHR'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. PurelyHR'yi el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve SaltHR şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Araç çubuğundaki seçeneklerden **Pano'yu** açın ve **SSO Ayarları'nı**tıklatın.

1. Aşağıda açıklandığı gibi kutulardaki değerleri yapıştırın-

    ![Tek İşaret-On'u Yapılandır](./media/purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)  

    a. Azure portalından indirilen **Sertifikayı(Bas64)** not defterinde açın ve sertifika değerini kopyalayın. Kopyalanan değeri **X.509 Sertifika** kutusuna yapıştırın.

    b. **Idp Veren URL** kutusuna, Azure portalından kopyalanan **Azure AD Tanımlayıcısını** yapıştırın.

    c. **Idp Endpoint URL** kutusuna Azure portalından kopyalanan **Giriş URL'sini** yapıştırın. 

    d. PurelyHR'de otomatik kullanıcı sağlamayı etkinleştirmek için **Otomatik Kullanıcı Oluştur** onay kutusunu işaretleyin.

    e. Ayarları kaydetmek için **Değişiklikleri Kaydet'i** tıklatın.

### <a name="create-purelyhr-test-user"></a>PurelyHR test kullanıcısı oluşturma

Uygulama tam zamanında kullanıcı sağlamayı desteklediği için bu adım genellikle gerekli değildir. Otomatik kullanıcı sağlama etkinleştirilmezse, aşağıda açıklandığı gibi manuel kullanım oluşturma işlemi yapılabilir.

Velpic SAML şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Yönet sekmesine tıklayın ve Kullanıcılar bölümüne gidin, ardından kullanıcı eklemek için Yeni düğmesine tıklayın.

    ![kullanıcı ekleme](./media/velpicsaml-tutorial/velpic_7.png)

2. **"Yeni Kullanıcı Oluştur"** iletişim sayfasında aşağıdaki adımları gerçekleştirin.

    ![kullanıcı](./media/velpicsaml-tutorial/velpic_8.png)

    a. Ad **metin** kutusuna, B'nin adını yazın.

    b. **Soyadı** metin kutusuna Simon'ın soyadını yazın.

    c. Kullanıcı **Adı** metin kutusuna B.Simon kullanıcı adını yazın.

    d. **E-posta** metin kutusuna B.Simon@contoso.com hesabın e-posta adresini yazın.

    e. Bilgilerin geri kalanı isteğe bağlıdır, gerekirse doldurabilirsiniz.

    f. **KAYDET'i**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki PurelyHR döşemesini tıklattığınızda, SSO'yu kurduğunuz PurelyHR'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile PurelyHR'i deneyin](https://aad.portal.azure.com/)
