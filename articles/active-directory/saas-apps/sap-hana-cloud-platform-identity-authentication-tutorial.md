---
title: 'Öğretici: SAP Cloud Platform Identity Authentication ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve SAP bulut platformu kimlik doğrulaması arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: e92cba923bc197d82083973b3fac476ab93117ad
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231382"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP bulut platformu kimlik doğrulaması ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide SAP bulut platformu kimlik doğrulamasını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SAP Cloud Platform kimliği kimlik doğrulamasını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP bulut platformu kimlik doğrulamasına erişimi olan Azure AD 'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP bulut platformu kimlik doğrulaması için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP Cloud Platform kimlik kimlik doğrulaması çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* SAP Cloud Platform kimlik doğrulaması **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler
* SAP Cloud Platform kimlik kimlik doğrulamasını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimlerini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

Teknik ayrıntıları kullanmadan önce, bakacağı kavramların anlaşılması çok önemlidir. SAP Cloud Platform kimliği kimlik doğrulaması ve Active Directory Federasyon Hizmetleri (AD FS), SAP bulut platformu kimlik doğrulaması ile korunan SAP uygulamaları ve Hizmetleri ile Azure AD tarafından korunan uygulamalar veya hizmetler arasında SSO uygulamanızı sağlar.

SAP bulut platformu kimlik doğrulaması şu anda SAP uygulamaları için bir proxy kimlik sağlayıcısı görevi görür. Sırasıyla Azure Active Directory, Bu kurulumda önde gelen kimlik sağlayıcısı olarak davranır. 

Aşağıdaki diyagramda bu ilişki gösterilmektedir:

![Azure AD test kullanıcısı oluşturma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Bu kurulumla, SAP bulut platformu kimlik doğrulama kiracınız Azure Active Directory güvenilir bir uygulama olarak yapılandırılır.

Bu şekilde korumak istediğiniz tüm SAP uygulamaları ve Hizmetleri, daha sonra SAP bulut platformu kimlik doğrulama yönetim konsolunda yapılandırılır.

Bu nedenle, SAP uygulamalarına ve hizmetlerine erişim verme yetkilendirmesinin SAP Cloud Platform kimlik kimlik doğrulamasında (Azure Active Directory aksine) gerçekleşmesi gerekir.

SAP Cloud Platform kimliği kimlik doğrulamasını Azure Active Directory Marketi aracılığıyla bir uygulama olarak yapılandırarak, ayrı talepler veya SAML onayları yapılandırmanız gerekmez.

> [!NOTE]
> Şu anda yalnızca Web SSO 'SU tarafından test edilmiştir. Uygulamadan API veya API-API iletişimi için gerekli olan akışlar çalışır, ancak henüz sınanmamıştır. Bunlar, sonraki etkinlikler sırasında test edilecek.

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP bulut platformu kimlik doğrulaması ekleme

SAP Cloud Platform kimliği kimlik doğrulamasının Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden SAP bulut platformu kimlik doğrulama kimlik doğrulamasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, ara kutusuna **SAP Cloud Platform Identity Authentication** yazın.
1. Sonuçlar panelinden **SAP Cloud Platform kimliği kimlik doğrulaması** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-cloud-platform-identity-authentication"></a>SAP bulut platformu kimlik doğrulaması için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test KULLANıCıSı kullanarak SAP bulut platformu kimlik doğrulaması Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, SAP bulut platformu kimlik doğrulaması 'ndaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu SAP bulut platformu kimlik doğrulamasıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[SAP Cloud Platform Identity AUTHENTICATION SSO 'Yu yapılandırma](#configure-sap-cloud-platform-identity-authentication-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * SAP Cloud **[Platform kimlik kimlik doğrulaması test kullanıcısı oluşturma](#create-sap-cloud-platform-identity-authentication-test-user)** -SAP bulut platformu kimlik doğrulama, kullanıcının Azure AD gösterimi ile bağlantılı bir B. Simon
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **SAP Cloud Platform kimlik doğrulaması** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, **IDP**tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![SAP Cloud Platform kimlik doğrulama etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<IAS-tenant-id>.accounts.ondemand.com`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [SAP Cloud Platform kimliği kimlik doğrulama istemci destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) başvurun. Tanımlayıcı değerini anlamıyorsanız, [KIRACı SAML 2,0 yapılandırması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html)Ile Ilgili SAP bulut platformu kimlik doğrulama belgelerini okuyun.

5. Uygulamayı **SP**tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![SAP Cloud Platform kimlik doğrulama etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`{YOUR BUSINESS APPLICATION URL}`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Lütfen belirli bir iş uygulaması oturum açma URL 'sini kullanın. Şüpheniz varsa [SAP Cloud Platform kimliği kimlik doğrulama istemci desteği ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) başvurun.

1. SAP bulut platformu kimlik doğrulama uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. SAP bulut platformu kimlik doğrulama uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | firstName | Kullanıcı. |

8. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. **SAP Cloud Platform kimlik doğrulamasını ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SAP bulut platformu kimlik doğrulama kimlik doğrulamasına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **SAP Cloud Platform kimlik doğrulaması**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-sap-cloud-platform-identity-authentication-sso"></a>SAP bulut platformu kimlik doğrulama SSO 'yu yapılandırma

1. Uygulamanız için yapılandırılmış SSO 'yu almak için SAP Cloud Platform kimlik doğrulaması yönetim konsoluna gidin. URL aşağıdaki düzene sahiptir: `https://<tenant-id>.accounts.ondemand.com/admin`. Ardından [Microsoft Azure AD Ile tümleştirmede](https://help.sap.com/viewer/6d6d63354d1242d185ab4830fc04feb1/Cloud/en-US/626b17331b4d4014b8790d3aea70b240.html)SAP Cloud Platform kimlik doğrulama hakkındaki belgeleri okuyun.

2. Azure portal **Kaydet** düğmesini seçin.

3. Yalnızca başka bir SAP uygulaması için SSO eklemek ve etkinleştirmek istiyorsanız aşağıdaki ile devam edin. **GALERIDEN SAP Cloud Platform kimliği kimlik doğrulaması ekleme**bölümündeki adımları yineleyin.

4. Azure portal, **SAP Cloud Platform kimlik doğrulama** uygulaması tümleştirmesi sayfasında, **bağlantılı oturum açma**' yı seçin.

    ![Bağlantılı oturum açmayı Yapılandır](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

5. Yapılandırmayı kaydedin.

> [!NOTE]
> Yeni uygulama, önceki SAP uygulamasının çoklu oturum açma yapılandırmasından yararlanır. SAP bulut platformu kimlik doğrulaması yönetim konsolunda aynı kurumsal kimlik sağlayıcılarını kullandığınızdan emin olun.

### <a name="create-sap-cloud-platform-identity-authentication-test-user"></a>SAP bulut platformu kimlik doğrulaması test kullanıcısı oluşturma

SAP Cloud Platform kimlik kimlik doğrulamasında bir kullanıcı oluşturmanız gerekmez. Azure AD Kullanıcı deposunda olan kullanıcılar SSO işlevini kullanabilir.

SAP Cloud Platform kimlik doğrulama kimlik doğrulaması, Kimlik Federasyonu seçeneğini destekler. Bu seçenek, uygulamanın, kurumsal kimlik sağlayıcısı tarafından kimliği doğrulanmış kullanıcıların SAP bulut platformu kimlik doğrulama kullanıcı deposunda mevcut olup olmadığını kontrol etmesine izin verir.

Kimlik Federasyonu seçeneği varsayılan olarak devre dışıdır. Kimlik Federasyonu etkinleştirilmişse, yalnızca SAP bulut platformu kimlik doğrulaması 'nda içeri aktarılan kullanıcılar uygulamaya erişebilir.

SAP Cloud Platform kimlik doğrulaması ile kimlik Federasyonu etkinleştirme veya devre dışı bırakma hakkında daha fazla bilgi için bkz. SAP Cloud Platform [Identity Authentication Kullanıcı deposunda kimlik Federasyonu yapılandırma](https://help.sap.com/viewer/6d6d63354d1242d185ab4830fc04feb1/Cloud/en-US/c029bbbaefbf4350af15115396ba14e2.html)KONUSUNDAKI "sap Cloud Platform Identity Authentication Ile kimlik doğrulamasını etkinleştirme".

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde SAP Cloud Platform kimliği kimlik doğrulama kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SAP bulut platformu kimlik kimlik doğrulamasında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAP bulut platformu kimlik doğrulamasını deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle SAP bulut platformu kimlik doğrulamasını koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
