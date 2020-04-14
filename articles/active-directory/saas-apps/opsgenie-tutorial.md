---
title: 'Öğretici: OpsGenie ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve OpsGenie arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: jeedes
ms.openlocfilehash: 8cf82827258b3888f4c30ad39a395d697a518a32
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261115"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-opsgenie"></a>Öğretici: OpsGenie ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, OpsGenie'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. OpsGenie'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* OpsGenie erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla OpsGenie'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* OpsGenie tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* OpsGenie **IDP'nin** başlattığı SSO'ya destek verdi
* OpsGenie'yi yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-opsgenie-from-the-gallery"></a>Galeriden OpsGenie ekleme

OpsGenie'nin Azure AD'ye entegrasyonunu yapılandırmak için, opsgenie'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **OpsGenie** yazın.
1. Sonuç panelinden **OpsGenie'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-opsgenie"></a>OpsGenie için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak OpsGenie ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile OpsGenie'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu OpsGenie ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[OpsGenie SSO'yu yapılandırın.](#configure-opsgenie-sso)**
    * **[OpsGenie test kullanıcısını oluşturun](#create-opsgenie-test-user)** - Kullanıcının Azure AD gösterimine bağlı OpsGenie'deki B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **OpsGenie** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.opsginie.com/auth/saml/<UNIQUEID>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.opsginie.com/auth/saml?id=<UNIQUEID>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, bu öğreticide daha sonra açıklanan gerçek Tanımlayıcı ve Yanıt URL'si ile güncelleştirin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

1. **OpsGenie'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın OpsGenie'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **OpsGenie'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-opsgenie-sso"></a>OpsGenie SSO'nun yapılandırılsın

1. Başka bir tarayıcı örneği açın ve ardından Yönetici olarak OpsGenie'de oturum açın.

2. **Ayarlar'ı**tıklatın ve ardından **Tek İşaret Sekmesini** tıklatın.
   
    ![OpsGenie Tek İşaret-On](./media/opsgenie-tutorial/tutorial-opsgenie-06.png)

3. SSO'yı etkinleştirmek için **Etkin'i**seçin.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial-opsgenie-07.png) 

4. **Sağlayıcı** bölümünde, Azure **Etkin Dizin** sekmesini tıklatın.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial-opsgenie-08.png) 

5. Azure Etkin Dizin iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial-opsgenie-09.png)
    
    a. App **ID URI** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma** bölümündeki **Identifier (Entity ID)** textbox'a yapıştırın.

    a. **Yanıtla URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Yanıtla URL** textbox'ına yapıştırın.

    a. **SAML 2.0 Endpoint** textbox'ına Azure portalından kopyaladığınız **Giriş URL**değerini yapıştırın.
    
    b. **Metadata Url'sinde:** textbox, Azure portalından kopyalamış olduğunuz **Uygulama Federasyonu Metadata Url** değerini yapıştırın.
    
    c. SSO'yu etkinleştirmek **için, tek oturum açma** geçişini etkinleştir'i açın.

    d. **SSO ayarlarını uygula'yı**tıklatın.

### <a name="create-opsgenie-test-user"></a>OpsGenie test kullanıcısı oluşturma

Bu bölümün amacı OpsGenie B.Simon adlı bir kullanıcı oluşturmaktır. 

1. Bir web tarayıcısı penceresinde, OpsGenie kiracınızda yönetici olarak oturum açın.

2. Sol paneldeki **Kullanıcılar'ı** tıklatarak Kullanıcılar listesine gidin.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial-opsgenie-10.png) 

3. **Kullanıcı Ekle**'ye tıklayın.

4. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial-opsgenie-11.png)
   
    a. **E-posta** metin kutusuna, Azure Active Directory'de adreslenen B.Simon'Un e-posta adresini yazın.
   
    b. Tam **Ad** metin kutusuna **B.Simon**yazın.
   
    c. **Kaydet**'e tıklayın. 

> [!NOTE]
> B.Simon kendi profilini kurmak için talimatlar içeren bir e-posta alır.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki OpsGenie karosu tıklattığınızda, SSO'yu kurduğunuz OpsGenie'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile OpsGenie'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)