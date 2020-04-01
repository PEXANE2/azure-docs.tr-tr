---
title: 'Öğretici: Carbonite Endpoint Backup ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Carbonite Endpoint Backup arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b78091f1-2f06-4c2c-8541-72aee0b5a322
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e078cb7daa787b9fe5e8bc996b36f0fef198f41c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68879699"
---
# <a name="tutorial-integrate-carbonite-endpoint-backup-with-azure-active-directory"></a>Öğretici: Karbonit Uç Nokta Yedekleme'yi Azure Etkin Dizini ile tümleştirin

Bu eğitimde, Carbonite Endpoint Backup ile Azure Etkin Dizin (Azure AD) ile nasıl entegre edileceksiniz. Carbonite Endpoint Backup'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Carbonite Endpoint Backup erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Carbonite Endpoint Backup'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Carbonite Endpoint Backup tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Carbonite Endpoint Backup **SP ve IDP** SSO başlatılan destekler

## <a name="adding-carbonite-endpoint-backup-from-the-gallery"></a>Galeriden Karbonit Uç Noktası Yedeklemesi Ekleme

Carbonite Endpoint Backup'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Carbonite Endpoint Backup eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Karbonit Uç Noktası Yedeklemesi** yazın.
1. Sonuç panelinden **Carbonite Endpoint Backup'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Carbonite Endpoint Backup ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Carbonite Endpoint Yedekleme'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Carbonite Endpoint Backup ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Karbonit Endpoint Backup SSO'yu yapılandırır](#configure-carbonite-endpoint-backup-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Carbonite Endpoint Backup test kullanıcısını oluşturun](#create-carbonite-endpoint-backup-test-user)** - kullanıcının Azure AD gösterimine bağlı Carbonite Endpoint Backup'da B.Simon'ın bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Carbonite Endpoint Backup** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki URL'lerden birini yazın:

    | | |
    |-|-|
    | `https://red-us.mysecuredatavault.com`|
    | `https://red-apac.mysecuredatavault.com`|
    | `https://red-fr.mysecuredatavault.com`|
    | `https://red-emea.mysecuredatavault.com`|
    | `https://kamino.mysecuredatavault.com`|
    | | |

    b. **Yanıtla URL** metin kutusuna aşağıdaki URL'lerden birini yazın:

    | | |
    |-|-|
    | `https://red-us.mysecuredatavault.com/AssertionConsumerService.aspx`|
    | `https://red-apac.mysecuredatavault.com/AssertionConsumerService.aspx`|
    | `https://red-fr.mysecuredatavault.com/AssertionConsumerService.aspx`|
    | `https://red-emea.mysecuredatavault.com/AssertionConsumerService.aspx`|
    | | |

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki URL'lerden birini yazın:

    | | |
    |-|-|
    | `https://red-us.mysecuredatavault.com/`|
    | `https://red-apac.mysecuredatavault.com/`|
    | `https://red-fr.mysecuredatavault.com/`|
    | `https://red-emea.mysecuredatavault.com/`|
    | | |

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Karbonit Uç Nokta Yedeklemesini Ayarla** bölümünde, gereksiniminize göre uygun URL'yi(ler) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-carbonite-endpoint-backup-sso"></a>Karbonit Uç Nokta Yedekleme SSO'su yapılandırın

1. Carbonite Endpoint Backup içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Kurulum Carbonite Endpoint Backup'a** tıklayın ve sizi Carbonite Endpoint Backup uygulamasına yönlendirecektir. Buradan, Carbonite Endpoint Backup'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Carbonite Endpoint Backup'ı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Administrator olarak Carbonite Endpoint Backup şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sol bölmeden **Şirket'e** tıklayın.

    ![Karbonit Uç Nokta Yedekleme yapılandırması ](media/carbonite-endpoint-backup-tutorial/configure1.png)

5. Tek **oturum açma'ya**tıklayın.

    ![Karbonit Uç Nokta Yedekleme yapılandırması ](media/carbonite-endpoint-backup-tutorial/configure2.png)

6. **Etkinleştir'e** tıklayın ve ardından yapılandırmak için **Ayarları Ede'yi** tıklatın.

    ![Karbonit Uç Nokta Yedekleme yapılandırması ](media/carbonite-endpoint-backup-tutorial/configure3.png)

7. Tek **oturum açma** ayarları sayfasında aşağıdaki adımları gerçekleştirin:

    ![Karbonit Uç Nokta Yedekleme yapılandırması ](media/carbonite-endpoint-backup-tutorial/configure4.png)

    1. Kimlik **sağlayıcı ad** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    1. Kimlik **sağlayıcısı URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    1. İndirilen **Sertifika(Base64)** dosyasını Azure portalından yüklemek için **Dosyayı Seç'e** tıklayın.

    1. **Kaydet**'e tıklayın.

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

Bu bölümde, B.Simon'ın Carbonite Endpoint Backup'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Karbonit Uç Nokta Yedekleme'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-carbonite-endpoint-backup-test-user"></a>Karbonit Uç Nokta Yedekleme test kullanıcıoluşturma

1. Farklı bir web tarayıcısı penceresinde, administrator olarak Carbonite Endpoint Backup şirket sitenizde oturum açın.

1. Sol bölmeden **Kullanıcılar'a** tıklayın ve ardından **Kullanıcı Ekle'yi**tıklatın.

    ![Karbonit Uç Nokta Yedekleme Kullanıcı ekle](media/carbonite-endpoint-backup-tutorial/adduser1.png)

1. Kullanıcı **Ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Karbonit Uç Nokta Yedekleme Kullanıcı ekle](media/carbonite-endpoint-backup-tutorial/adduser2.png)

    1. **E-posta,** **Ad**, Kullanıcının **Soyadı** girin ve Kuruluş gereksinimlerine göre kullanıcıya gerekli izinleri sağlayın.

    1. **Kullanıcı Ekle'yi**tıklatın.

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Carbonite Endpoint Yedekleme döşemesini tıklattığınızda, SSO'yu kurduğunuz Carbonite Endpoint Backup'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)