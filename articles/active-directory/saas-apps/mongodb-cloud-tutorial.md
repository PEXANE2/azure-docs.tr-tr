---
title: 'Öğretici Azure Active Directory: MongoDB bulutu ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ile MongoDB bulutu arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.openlocfilehash: 6a59df062adf9b5c2e511db29cb8601d4a4df70e
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554575"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mongodb-cloud"></a>Öğretici: MongoDB bulutu ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, MongoDB bulutunu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. MongoDB bulutunu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* MongoDB bulutu, MongoDB Atlas, MongoDB topluluğu, MongoDB Üniversitesi ve MongoDB desteğine erişimi olan Azure AD 'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla MongoDB bulutuna otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) için etkinleştirilen bir MongoDB bulut organizasyonu, [ücretsiz bir küme](https://www.mongodb.com/cloud) için kaydolma gerçekleştirebilirsiniz

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* MongoDB bulutu, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler.
* MongoDB bulutu **, tam zamanında** Kullanıcı sağlamayı destekler.
* MongoDB bulutunu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. Daha fazla bilgi için bkz. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-mongodb-cloud-from-the-gallery"></a>Galeriden MongoDB bulutu ekleme

MongoDB bulutunun Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden MongoDB bulutunu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Active Directory**' yi seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **MongoDB bulutu** yazın.
1. Sonuçlardan **MongoDB bulutu** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-mongodb-cloud"></a>MongoDB bulutu için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak MongoDB bulutuyla Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, MongoDB bulutu 'ndaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

Azure AD SSO 'yu MongoDB bulutu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [MongoDB Cloud SSO 'Yu yapılandırın](#configure-mongodb-cloud-sso) .
    1. MongoDB bulutu 'nda, kullanıcının Azure AD gösterimine bağlı olarak B. Simon 'a karşılık gelen bir [MongoDB bulut test kullanıcısı oluşturun](#create-a-mongodb-cloud-test-user) .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [test SSO 'su](#test-sso) .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **MongoDB Cloud** Application Integration sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma**seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki kalıbı kullanan bir URL yazın:`https://www.okta.com/saml2/service-provider/<Customer_Unique>`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanan bir URL yazın:`https://auth.mongodb.com/sso/saml2/<Customer_Unique>`

1. **Ek URL 'Ler ayarla**' yı seçin ve uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanan bir URL yazın:`https://cloud.mongodb.com/sso/<Customer_Unique>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [MongoDB bulut istemci desteği ekibine](https://support.mongodb.com/)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. MongoDB bulut uygulaması SAML onayları 'nin belirli bir biçimde olmasını bekler, bu da SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektirir. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![Varsayılan özniteliklerin ekran görüntüsü](common/default-attributes.png)

1. Önceki özniteliklere ek olarak, MongoDB Cloud uygulaması, SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre gözden geçirebilirsiniz.
    
    | Adı | Kaynak özniteliği|
    | ---------------| --------- |
    | e-posta | User. UserPrincipalName |
    | firstName | Kullanıcı. |
    | lastName | User. soyadı |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML**'i bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Indirme bağlantısı vurgulanmış şekilde SAML Imzalama sertifikası bölümünün ekran görüntüsü](common/metadataxml.png)

1. **MongoDB bulutu ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![URL vurgulanmış şekilde Mongo DB bulutu ayarlama bölümünün ekran görüntüsü](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu işaretleyin ve parolayı aşağı yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, MongoDB bulutuna erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirirsiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **MongoDB bulutu**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar sayfasının ekran görüntüsü, Kullanıcı Ekle vurgulanmış olarak](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcı listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-mongodb-cloud-sso"></a>MongoDB Cloud SSO 'yu yapılandırma

MongoDB bulut tarafında çoklu oturum açmayı yapılandırmak için, Azure portal kopyalanmış uygun URL 'Lerin olması gerekir. Ayrıca, MongoDB bulut kuruluşunuz için Federasyon uygulamasını yapılandırmanız gerekir. [MongoDB bulut belgelerindeki](https://docs.atlas.mongodb.com/security/federated-auth-azure-ad/)yönergeleri izleyin. Bir sorununuz varsa [MongoDB bulut desteği ekibine](https://support.mongodb.com/)başvurun.

### <a name="create-a-mongodb-cloud-test-user"></a>MongoDB bulut testi kullanıcısı oluşturma

MongoDB bulutu, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Yapmanız gereken başka bir eylem yoktur. Bir Kullanıcı MongoDB bulutu 'nda zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim paneli ' ni kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim paneli 'nde MongoDB bulut kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız MongoDB bulutunda otomatik olarak oturum açtınız demektir. Daha fazla bilgi için bkz. [uygulamalarım portalından oturum açma ve uygulamaları başlatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure 'da MongoDB Atlas 'e kaydolun](https://azuremarketplace.microsoft.com/marketplace/apps/mongodb.mongodb_atlas_may_2020?tab=Overview)

- [Azure AD ile MongoDB bulutu 'nı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle MongoDB bulutunu koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

