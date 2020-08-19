---
title: 'Öğretici: Nitro üretkenlik paketiyle çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve nitro üretkenlik paketi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/02/2020
ms.author: jeedes
ms.openlocfilehash: b67e280824c5e2336aa79e14d3e09d3a670118a5
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554351"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nitro-productivity-suite"></a>Öğretici: Nitro üretkenlik paketiyle çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Nitro üretkenlik paketini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Nitro üretkenlik paketini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Nitro üretkenlik paketine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Nitro üretkenlik Suite 'te otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Bir nitro üretkenlik paketi [Kurumsal aboneliği](https://www.gonitro.com/pricing).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Nitro üretkenlik Suite, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler.
* Nitro üretkenlik paketi **, tam zamanında** Kullanıcı sağlamayı destekler.
* Nitro üretkenlik paketini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. Daha fazla bilgi için bkz. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-nitro-productivity-suite-from-the-gallery"></a>Galeriden Nitro üretkenlik paketini ekleme

Nitro üretkenlik Suite 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden Nitro üretkenlik paketini, yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Active Directory**' yi seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Nitro üretkenlik Suite** yazın.
1. Sonuçlardan **Nitro üretkenlik paketi** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-nitro-productivity-suite"></a>Nitro üretkenlik paketi için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Nitro üretkenlik PAKETIYLE Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Nitro üretkenlik Suite içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

Azure AD SSO 'yu Nitro üretkenlik paketiyle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [Nitro üretkenlik PAKETI SSO 'Yu yapılandırın](#configure-nitro-productivity-suite-sso) .
    1. Kullanıcının Azure AD gösterimine bağlı olarak Nitro üretkenlik Suite 'te B. Simon 'a karşılık gelen bir [Nitro üretkenlik paketi test kullanıcısı oluşturun](#create-a-nitro-productivity-suite-test-user) .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [test SSO 'su](#test-sso) .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Nitro üretkenlik paketi** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma**seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Imzalama sertifikası** bölümünde, **sertifika (base64)** bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Indirme bağlantısı vurgulanmış şekilde SAML Imzalama sertifikası bölümünün ekran görüntüsü](common/certificatebase64.png)
    
1. **Nitro üretkenlik paketini ayarla** bölümünde, **oturum açma URL 'sinin**yanındaki Kopyala simgesini seçin.
    
    ![URL 'Ler ve kopya simgeleri vurgulanmış şekilde, Nitro üretkenlik Suite 'i ayarlama ekranının ekran görüntüsü](common/copy-configuration-urls.png)
    
1. [Nitro yönetim portalındaki](https://admin.gonitro.com/) **Kurumsal ayarlar** sayfasında, **Çoklu oturum açma** bölümünü bulun. **SAML SSO kurulumunu ayarla**' yı seçin.

    a. Önceki adımdan **oturum açma** URL 'Sini, **oturum açma URL 'si** alanına yapıştırın.
    
    b. **Sertifikayı (base64)** **x509 imza sertifikası** alanındaki önceki adımdan yükleyin.
    
    c. **Gönder**’i seçin.
    
    d. **Çoklu oturum açmayı etkinleştir '** i seçin.


1. [Azure portalına](https://portal.azure.com/) geri dönün. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, [Nitro yönetim PORTALıNDAN](https://admin.gonitro.com/) **SAML varlık kimliği** alanını kopyalayıp yapıştırın. Aşağıdaki düzende olmalıdır: `urn:auth0:gonitro-prod:<ENVIRONMENT>`

    b. **Yanıt URL 'si** metin kutusunda, [Nitro yönetim portalından](https://admin.gonitro.com/) **ACS URL 'si** alanını kopyalayıp yapıştırın. Aşağıdaki düzende olmalıdır: `https://gonitro-prod.eu.auth0.com/login/callback?connection=<ENVIRONMENT>`

1. **Ek URL 'Ler ayarla**' yı seçin ve uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://sso.gonitro.com/login`

1. **Kaydet**’i seçin.

1. Nitro üretkenlik paketi uygulaması, SAML onayları 'nin belirli bir biçimde olmasını bekler, bu da SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektirir. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![Varsayılan özniteliklerin ekran görüntüsü](common/default-attributes.png)

1. Önceki özniteliklere ek olarak, Nitro üretkenlik Suite uygulaması, SAML yanıtına birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler önceden doldurulur, ancak gereksinimlerinize göre gözden geçirebilirsiniz.
    
    | Adı  |  Kaynak özniteliği|
    | ---------------| --------------- |
    | employeeNumber |  User. ObjectID |


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

Bu bölümde, Nitro üretkenlik paketine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirirsiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Nitro üretkenlik paketi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar sayfasının ekran görüntüsü, Kullanıcı Ekle vurgulanmış olarak](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcı listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-nitro-productivity-suite-sso"></a>Nitro üretkenlik paketi SSO 'SU yapılandırma

Nitro üretkenlik Suite tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (base64)** ve uygun kopyalanmış url 'Leri Azure Portal [Nitro üretkenlik paketi destek ekibine](https://www.gonitro.com/support)gönderin. Destek ekibi, SAML SSO bağlantısının her iki tarafında da düzgün şekilde ayarlanmış olmasını sağlar.

### <a name="create-a-nitro-productivity-suite-test-user"></a>NIRO üretkenlik paketi test kullanıcısı oluşturma

Nitro üretkenlik paketi, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Yapmanız gereken başka bir eylem yoktur. Bir kullanıcı zaten Nitro üretkenlik paketinde yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim paneli ' ni kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim paneli 'nde Nitro üretkenlik paketi kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Nitro üretkenlik paketinde otomatik olarak oturum açtınız demektir. Daha fazla bilgi için bkz. [uygulamalarım portalından oturum açma ve uygulamaları başlatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Nitro üretkenlik paketini deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Nitro üretkenlik paketini koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

