---
title: 'Öğretici: SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Cloud Platform Identity Authentication arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95be73bd125c124409585a478fa9707e7b6a2ac2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76289076"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SAP Bulut Platformu Kimlik Doğrulaması'nı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SAP Bulut Platformu Kimlik Doğrulaması'nı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP Bulut Platformu Kimlik Doğrulaması'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP Cloud Platform Identity Authentication tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SAP Bulut Platformu Kimlik Doğrulama **SP** ve **IDP** sso başlatılan destekler
* SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

Teknik detaylara dalmadan önce, bakacağınız kavramları anlamak çok önemlidir. SAP Bulut Platformu Kimlik Doğrulaması ve Active Directory Federation Services, SSO'yu SAP Bulut tarafından korunan SAP uygulamaları ve hizmetleriyle Azure AD (IdP olarak) tarafından korunan uygulamalar veya hizmetler arasında uygulamanızı sağlar Platform Kimlik Kimlik Doğrulaması.

Şu anda SAP Bulut Platformu Kimlik Kimlik Doğrulaması, SAP uygulamalarına Proxy Kimlik Sağlayıcısı görevi görür. Azure Active Directory de bu kurulumda önde gelen Kimlik Sağlayıcısı olarak görev eder. 

Aşağıdaki diyagram bu ilişkiyi göstermektedir:

![Azure AD test kullanıcısı oluşturma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Bu kurulumla SAP Bulut Platformu Kimlik Kimlik Doğrulaması kiracınız Azure Active Directory'de güvenilir bir uygulama olarak yapılandırılır.

Bu şekilde korumak istediğiniz tüm SAP uygulamaları ve hizmetleri daha sonra SAP Bulut Platformu Kimlik Kimlik Doğrulama yönetim konsolunda yapılandırılır.

Bu nedenle, SAP uygulamalarına ve hizmetlerine erişim izni verme yetkisinin SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nda (Azure Etkin Dizin'inin aksine) gerçekleşmesi gerekir.

SAP Bulut Platformu Kimlik Doğrulaması'nı Azure Active Directory Marketplace aracılığıyla bir uygulama olarak yapılandırarak, tek tek talepleri veya SAML iddialarını yapılandırmanız gerekmez.

> [!NOTE]
> Şu anda her iki taraftarafından da yalnızca Web SSO test edilmiştir. App-to-API veya API-API iletişimi için gerekli olan akışlar çalışmalıdır, ancak henüz test edilmemiştir. Sonraki etkinlikler sırasında test edilecektir.

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP Bulut Platformu Kimlik Kimlik Doğrulaması Ekleme

SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize SAP Bulut Platformu Kimlik Kimlik Doğrulaması eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP Bulut Platformu Kimlik Doğrulaması** yazın.
1. Sonuç panelinden **SAP Bulut Platformu Kimlik Doğrulaması'nı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-cloud-platform-identity-authentication"></a>SAP Bulut Platformu Kimlik Kimlik Doğrulaması için Azure AD oturumunu yapılandırma ve test edin

**B.Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO'nu SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile yapılandırın ve test edin. SSO'nun çalışması için, SAP Bulut Platformu Kimlik Doğrulaması'nda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[SAP Bulut Platformu Kimlik Doğrulaması SSO'yu uygulama](#configure-sap-cloud-platform-identity-authentication-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[SAP Bulut Platformu Kimlik Doğrulama testi kullanıcısını oluşturun](#create-sap-cloud-platform-identity-authentication-test-user)** - kullanıcının Azure AD gösterimine bağlı SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nda B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **SAP Bulut Platformu Kimlik Kimlik Doğrulama** uygulaması tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, **IDP**tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![SAP Bulut Platformu Kimlik Kimlik Doğrulama Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<IAS-tenant-id>.accounts.ondemand.com`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [SAP Bulut Platformu Kimlik Kimlik Doğrulama İstemci destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) başvurun. Tanımlayıcı değerini anlamıyorsanız, [Kiracı SAML 2.0 yapılandırması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html)hakkındaki SAP Bulut Platformu Kimlik Kimlik Doğrulama belgelerini okuyun.

5. Uygulamayı **SP**tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![SAP Bulut Platformu Kimlik Kimlik Doğrulama Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`{YOUR BUSINESS APPLICATION URL}`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri gerçek oturum açma URL'si ile güncelleştirin. Lütfen özel iş uygulamaoturum açma URL'nizi kullanın. Herhangi bir şüpheniz varsa [SAP Bulut Platformu Kimlik Kimlik Doğrulama İstemci destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) başvurun.

1. SAP Bulut Platformu Kimlik Kimlik Doğrulaması uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Sap Bulut Platformu Kimlik Kimlik Doğrulama uygulaması, yukarıda ek olarak, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ---------------| --------------- |
    | firstName | user.givenname |

8. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, **metadata XML'yi** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. SAP **Bulut Platformu Kimlik Kimlik Doğrulaması Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, SAP Bulut Platformu Kimlik Kimlik Doğrulaması'na erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SAP Bulut Platformu Kimlik Doğrulaması'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sap-cloud-platform-identity-authentication-sso"></a>SAP Bulut Platformu Kimlik Doğrulaması SSO'da Yapılandır

1. SSO'nun uygulamanız için yapılandırılması için SAP Bulut Platformu Kimlik Kimlik Doğrulama yönetim konsoluna gidin. URL'de aşağıdaki desen `https://<tenant-id>.accounts.ondemand.com/admin`vardır: . Ardından [Microsoft Azure AD ile Tümleştirmede](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html)SAP Bulut Platformu Kimlik Doğrulaması ile ilgili belgeleri okuyun.

2. Azure portalında **Kaydet** düğmesini seçin.

3. Yalnızca başka bir SAP uygulaması için SSO eklemek ve etkinleştirmek istiyorsanız aşağıdakilerle devam edin. **Galeriden SAP Bulut Platformu Kimlik Kimlik Doğrulaması Ekleme bölümünün**altındaki adımları yineleyin.

4. Azure portalında, **SAP Bulut Platformu Kimlik Kimlik Doğrulama** uygulaması tümleştirme sayfasında Bağlantılı Oturum **Açma'yı**seçin.

    ![Bağlantılı İşaret-On'u yapılandır](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

5. Yapılandırmayı kaydedin.

> [!NOTE]
> Yeni uygulama, önceki SAP uygulamasının tek oturum açma yapılandırmasını kullanabılır. SAP Bulut Platformu Kimlik Kimlik Doğrulama yönetim konsolunda aynı Kurumsal Kimlik Sağlayıcılarını kullandığınızdan emin olun.

### <a name="create-sap-cloud-platform-identity-authentication-test-user"></a>SAP Bulut Platformu Kimlik Kimlik Doğrulama testi kullanıcısı oluşturma

SAP Bulut Platformu Kimlik Doğrulaması'nda bir kullanıcı oluşturmanız gerekmez. Azure AD kullanıcı mağazasında bulunan kullanıcılar SSO işlevini kullanabilir.

SAP Bulut Platformu Kimlik Kimlik Doğrulaması, Kimlik Federasyonu seçeneğini destekler. Bu seçenek, uygulamanın kurumsal kimlik sağlayıcısı tarafından kimlik doğrulaması yapılan kullanıcıların SAP Bulut Platformu Kimlik Kimlik Doğrulaması kullanıcı deposunda bulunup bulunmadığını denetlemesine olanak tanır.

Kimlik Federasyonu seçeneği varsayılan olarak devre dışı bırakılır. Kimlik Federasyonu etkinse, uygulamaya yalnızca SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nda alınan kullanıcılar erişebilir.

SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile Kimlik Federasyonu'nu etkinleştirme veya devre dışı bırakma hakkında daha fazla bilgi için, SAP Bulut Platformu Kimlik Kimlik Doğrulaması Kullanıcı Mağazası ile Kimlik Doğrulamayı [Yapılandır'da](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html)"SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile Kimlik Federasyonu'nu Etkinleştir" bölümüne bakın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki SAP Bulut Platformu Kimlik Kimlik Doğrulaması döşemesini tıklattığınızda, SSO'yu kurduğunuz SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAP Bulut Platformu Kimlik Doğrulaması'nı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [SAP Bulut Platformu Kimlik Doğrulaması'nı gelişmiş görünürlük ve kontrollerle nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
