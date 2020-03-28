---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) Yeni Emanet (Sınırlı Sürüm) ile tümleştirme | Microsoft Dokümanlar'
description: Azure Etkin Dizini ve Yeni Emanet (Sınırlı Sürüm) arasında tek oturum açma yı nasıl yapılandırılayacağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ffbcf4a-b6b4-45ac-922f-358abe4c05e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad229e7964b6e7ad0e3f0d58cfc09669ee2fb301
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77088203"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic-limited-release"></a>Öğretici: Yeni Emanet (Sınırlı Sürüm) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Yeni Emanet'i (Sınırlı Sürüm) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Yeni Emanet'i (Sınırlı Sürüm) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Yeni Emanet'e (Sınırlı Sürüm) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik olarak Yeni Emanet 'te (Sınırlı Sürüm) oturum açılmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Yeni Emanet (Sınırlı Sürüm) tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Yeni Emanet (Sınırlı Sürüm) **IDP** başlatılan SSO destekler

* Yeni Emanet'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-new-relic-limited-release-from-the-gallery"></a>Galeriden Yeni Emanet (Sınırlı Sürüm) Ekleme

Yeni Emanet'in (Sınırlı Sürüm) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Yeni Emanet (Sınırlı Sürüm) eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Yeni Kalıntı (Sınırlı Sürüm)** yazın.
1. Sonuç panelinden **Yeni Emanet 'i (Sınırlı Sürüm)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-new-relic-limited-release"></a>Yeni Emanet (Sınırlı Sürüm) için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Yeni Emanet (Sınırlı Sürüm) ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Yeni Emanet (Sınırlı Sürüm)'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Yeni Kalıntı (Sınırlı Sürüm) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Yeni Emanet (Sınırlı Sürüm) SSO'yu yapılandırır](#configure-new-relic-limited-release-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Yeni Emanet (Sınırlı Sürüm) test kullanıcıoluşturun](#create-new-relic-limited-release-test-user)** - Yeni Emanet (Sınırlı Sürüm) kullanıcının Azure AD gösterimi ile bağlantılı B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Yeni Emanet (Sınırlı Sürüm)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![image](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel SAML Yapılandırması bölümünde otomatik olarak doldurulur:

    ![image](common/idp-intiated.png)

    > [!Note]
    > **Tanımlayıcı** ve **Yanıt URL** değerleri otomatik olarak yorumlanmıyorsa, gereksiniminize göre değerleri el ile doldurun.

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın Yeni Emanet'e (Sınırlı Sürüm) erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Yeni Emanet (Sınırlı Sürüm)** seçeneğini belirleyin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-new-relic-limited-release-sso"></a>Yeni Emaneti Yapılandır (Sınırlı Sürüm) SSO

**Yeni Emanet (Sınırlı Sürüm)** tarafında tek oturum açma yapılandırmak için, **App Federation Metadata Url'sini** Yeni Emanet [(Sınırlı Sürüm) destek ekibine](https://support.newrelic.com/)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.


### <a name="create-new-relic-limited-release-test-user"></a>Yeni Emanet (Sınırlı Sürüm) test kullanıcısı oluşturma

Bu bölümde, Yeni Emanet (Sınırlı Sürüm) b.simon adlı bir kullanıcı oluşturun. Kullanıcıları Yeni Emanet (Sınırlı Sürüm) platformuna eklemek için [Yeni Emanet (Sınırlı Sürüm) destek ekibiyle](https://support.newrelic.com/) çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Yeni Emanet (Sınırlı Sürüm) döşemesini tıklattığınızda, SSO'yu kurduğunuz Yeni Emanet'te (Sınırlı Sürüm) otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Yeni Emaneti (Sınırlı Sürüm) deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)