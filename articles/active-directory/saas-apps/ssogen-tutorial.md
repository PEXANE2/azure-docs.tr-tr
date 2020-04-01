---
title: 'Öğretici: SSOGEN ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE | Microsoft Dokümanlar'
description: Azure Active Directory ve SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f4311031-5a4b-468e-be58-324d06220869
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74b56e30eb8851913f91cbee377204a575311375
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76293683"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde"></a>Öğretici: SSOGEN ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE

Bu eğitimde, Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'yi Azure Active Directory (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'yi Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE ile otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE tek oturum açma (SSO) aboneliği etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE **SP ve IDP** tarafından başlatılan SSO'ları destekler.
* Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde-from-the-gallery"></a>Galeriden Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi ekleme - EBS, PeopleSoft ve JDE

Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'nin Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Yönetilen SaaS uygulamaları listenize SSOGEN - Azure AD SSO Ağ Geçidi' ni eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama **kutusuna Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE** yazın.
1. **Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi ' ni seçin - EBS, PeopleSoft ve JDE** sonuç panelinden ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde"></a>SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu SSOGEN - Azure AD SSO Ağ Geçidi ile Oracle E-Business Suite - EBS, PeopleSoft ve JDE ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SSOGEN - Azure AD SSO Ağ Geçidi ile yapılandırmak ve test etmek için Oracle E-Business Suite - EBS, PeopleSoft ve JDE aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Oracle E Business Suite EBS, PeopleSoft ve JDE SSO için SSOGEN Azure AD SSO Ağ Geçidi'ni uygulama](#configure-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[Oracle E Business Suite EBS, PeopleSoft ve JDE test kullanıcısı için SSOGEN Azure AD SSO Ağ Geçidi'ni oluşturun](#create-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-test-user)** - SSOGEN'de B.Simon'Un bir muadili - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve Kullanıcının Azure AD temsiline bağlı JDE.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<customer_name>.ssogen.com/ssogen/login?client_name=<customer_name>`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<customer_name>.ssogen.com/ssogen/login?client_name=<customer_name>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak [için Oracle E-Business Suite - EBS, PeopleSoft ve JDE Client destek ekibine](mailto:support@ssogen.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. SSOGEN - Oracle E-Business Suite için Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE uygulaması **nameidentifier'in** **user.onpremisessamaccountname**ile eşleştirilmesini bekler, bu nedenle **Düzenleme** simgesine tıklayarak öznitelik eşlemini düzenlemeniz ve öznitelik eşlemini değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

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

Bu bölümde, Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'ye erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde Oracle **E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-sso"></a>Oracle E Business Suite EBS, PeopleSoft ve JDE SSO için SSOGEN Azure AD SSO Ağ Geçidi'ni yapılandırın

**Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE** tarafında tek oturum açma yapılandırmak için lütfen uygulamaya özel SSO kayıt belgelerini aşağıda bulun:

* Oracle EBS - Azure AD SSO Entegrasyonu:[https://www.ssogen.com/oracle-ebs-sso-ldap/](https://www.ssogen.com/oracle-ebs-sso-ldap/)
* PeopleSoft - Azure AD SSO Tümleştirmesi:[https://www.ssogen.com/peoplesoft-sso/](https://www.ssogen.com/peoplesoft-sso/)
* JD Edwards - Azure AD SSO Entegrasyonu:[https://www.ssogen.com/oracle-jde-sso/](https://www.ssogen.com/oracle-jde-sso/)
* Apache - Azure AD SSO Entegrasyonu:[https://www.ssogen.com/apache-sso-authentication/](https://www.ssogen.com/apache-sso-authentication/)

### <a name="create-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-test-user"></a>Oracle E Business Suite EBS, PeopleSoft ve JDE test kullanıcısı için SSOGEN Azure AD SSO Ağ Geçidi oluşturma

Azure AD, kimlik doğrulaması başarılı olduktan sonra kullanıcı uygulamasına Benzersiz Kullanıcı Tanımlayıcısı (Ad Kimliği) gönderir.  Lütfen Benzersiz Kullanıcı Tanımlayıcısı'nın (Ad Kimliği) FND_USER uygulamanızdaki kullanıcı kayıtlarıyla eşleştiğinden emin olun. Örneğin Oracle EBS'de USER_NAME.

Lütfen [info@ssogen.com](mailto:info@ssogen.com) iletişim [support@ssogen.com](mailto:support@ssogen.com) ve herhangi bir destek için.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE döşemesini tıklattığınızda, SSO'yu kurduğunuz Oracle E-Business Suite için Otomatik Olarak SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'ye oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidini - EBS, PeopleSoft ve Azure AD ile JDE'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Oracle E-Business Suite için SSOGEN - Azure AD SSO Ağ Geçidi - EBS, PeopleSoft ve JDE'yi gelişmiş görünürlük ve kontrollerle nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
