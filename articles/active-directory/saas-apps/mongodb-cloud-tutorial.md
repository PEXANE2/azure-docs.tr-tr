---
title: 'Öğretici: MongoDB Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve MongoDB Cloud arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ef392044-235b-4d80-8a33-eeba9b142849
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef8094464b428f1f46017b0d4abaac5aeb1fa428
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024547"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mongodb-cloud"></a>Öğretici: MongoDB Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, MongoDB Bulutu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz öğreneceksiniz. MongoDB Cloud'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* MongoDB Cloud, MongoDB Atlas, MongoDB topluluğu, MongoDB Üniversitesi ve MongoDB Desteği'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla MongoDB Cloud'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için gerekli olanlar:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) için etkinleştirilen bir MongoDB Cloud kuruluşu, [serbest küme](https://www.mongodb.com/cloud) için kaydolabilirsiniz

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* MongoDB **Cloud, SP** ve **IDP'nin** başlattığı SSO'ya destek veriyor.
* MongoDB **Cloud, Just In Time** kullanıcı sağlamayı destekler.
* MongoDB Cloud'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızma larını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi koşullu erişimden genişler. Daha fazla bilgi için bkz. [Microsoft Bulut Uygulama Güvenliği ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-mongodb-cloud-from-the-gallery"></a>Galeriden MongoDB Bulutu ekleyin

MongoDB Cloud'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden MongoDB Cloud'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol bölmede Azure **Etkin Dizini'ni**seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **MongoDB Bulutu** yazın.
1. Sonuçlardan **MongoDB Cloud'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-mongodb-cloud"></a>MongoDB Cloud için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak MongoDB Cloud ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile MongoDB Bulutu'ndaki ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD SSO'yu MongoDB Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    1. Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için [MongoDB Cloud SSO'yu yapılandırın.](#configure-mongodb-cloud-sso)
    1. MongoDB Bulut'ta B.Simon'Un bir muadili olması için [bir MongoDB Bulut testi kullanıcısı oluşturun](#create-a-mongodb-cloud-test-user) ve kullanıcının Azure AD gösterimine bağlanın.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **MongoDB Bulut** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Tek Oturum** Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfası yla Tek Oturum Açma'nın ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://www.okta.com/saml2/service-provider/<Customer_Unique>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://auth.mongodb.com/sso/saml2/<Customer_Unique>`

1. **Ek URL'ler ayarla'yı**seçin ve uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://cloud.mongodb.com/sso/<Customer_Unique>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [MongoDB Cloud Client destek ekibine](https://support.mongodb.com/)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. MongoDB Cloud uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren, SAML iddialarının belirli bir biçimde olmasını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![Varsayılan özniteliklerin ekran görüntüsü](common/default-attributes.png)

1. Önceki özniteliklere ek olarak, MongoDB Cloud uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı | |  Kaynak özniteliği|
    | ---------------| --------------- | --------- |
    | e-posta | | user.userprincipalname |
    | firstName | | user.givenname |
    | lastName | | user.surname |

1. **SAML** ile tek oturum açma'da, **SAML İmza Sertifikası** bölümünde **Federasyon Metadata XML'i**bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![İndirme bağlantısı vurgulanmış SAML İmzalama Sertifikası bölümünün ekran görüntüsü](common/metadataxml.png)

1. **MongoDB Bulut'u Ayarla** bölümünde, gereksiniminize göre uygun URL'leri kopyalayın.

    ![URL'ler vurgulanmış Mongo DB Cloud bölümünün ekran görüntüsü](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından parolayı aşağı yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın MongoDB Bulut'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **MongoDB Cloud'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-mongodb-cloud-sso"></a>MongoDB Bulut SSO'su yapılandır

MongoDB Cloud tarafında tek oturum açma yapılandırmak için Azure portalından kopyalanan uygun URL'lere ihtiyacınız vardır. Ayrıca, MongoDB Bulut Kuruluşunuz için Federasyon Uygulamasını yapılandırmanız gerekir. [MongoDB Bulut belgelerindeki](https://docs.atlas.mongodb.com/security/federated-auth-azure-ad/)yönergeleri izleyin. Bir sorununuzun varsa, [MongoDB Cloud destek ekibine](https://support.mongodb.com/)başvurun.

### <a name="create-a-mongodb-cloud-test-user"></a>MongoDB Bulut testi kullanıcısı oluşturma

MongoDB Cloud, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. İşlem yapmak için ek bir işlem yoktur. MongoDB Cloud'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Panel'de MongoDB Bulut döşemesini seçtiğinizde, SSO'yu kurduğunuz MongoDB Bulut'ta otomatik olarak oturum açmış olursunuz. Daha fazla bilgi [için, Windows Portalım portalından Oturum Aç ve uygulamaları başlat'](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)a bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure'da MongoDB Atlas'a kaydolun](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/mongodb.mongodb_atlas_azure_08082019?tab=Overview)

- [Azure AD ile MongoDB Bulutu'nun deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle MongoDB Bulutu'yu koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

