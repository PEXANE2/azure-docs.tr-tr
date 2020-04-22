---
title: 'Öğretici: Nitro Productivity Suite ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Nitro Productivity Suite arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 98c96c9f-18a6-4a20-919b-74fb113ee465
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/02/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ca675e43f4d20898d9f97c175da71f8d5c59e4f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676915"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nitro-productivity-suite"></a>Öğretici: Nitro Productivity Suite ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Nitro Productivity Suite'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Nitro Productivity Suite'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Nitro Productivity Suite erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Nitro Productivity Suite'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Bir Nitro Verimlilik Suite [Enterprise abonelik.](https://www.gonitro.com/pricing)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Nitro Productivity **Suite, SP** ve **IDP** tarafından başlatılan SSO'ya destek veriyor.
* Nitro Productivity **Suite, Just In Time** kullanıcı sağlamayı destekler.
* Nitro Productivity Suite'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin sızma ve sızma larını gerçek zamanlı olarak koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi koşullu erişimden genişler. Daha fazla bilgi için bkz. [Microsoft Bulut Uygulama Güvenliği ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-nitro-productivity-suite-from-the-gallery"></a>Galeriden Nitro Productivity Suite ekleyin

Nitro Productivity Suite'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Nitro Productivity Suite eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol bölmede Azure **Etkin Dizini'ni**seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Nitro Productivity Suite** yazın.
1. Sonuçlardan **Nitro Productivity Suite'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-nitro-productivity-suite"></a>Nitro Productivity Suite için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Nitro Productivity Suite ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Nitro Productivity Suite'teki ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD SSO'yu Nitro Productivity Suite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    1. Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. [Nitro Productivity Suite SSO'yu](#configure-nitro-productivity-suite-sso) uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. Kullanıcının Azure AD gösterimine bağlı Nitro Productivity Suite'te B.Simon'ın bir örneğine sahip olmak için [bir Nitro Productivity Suite test kullanıcısı oluşturun.](#create-a-nitro-productivity-suite-test-user)
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Nitro Productivity Suite** uygulama tümleştirme sayfasında **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML İmza Sertifikası** bölümünde **Sertifika (Base64)** bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![İndirme bağlantısı vurgulanmış SAML İmzalama Sertifikası bölümünün ekran görüntüsü](common/certificatebase64.png)
    
1. Nitro **Productivity Suite'i Ayarla** bölümünde, **Giriş URL'sinin**yanındaki kopya simgesini seçin.
    
    ![URL'ler ve kopyalama simgeleri vurgulanmış Nitro Productivity Suite bölümünün ekran görüntüsü](common/copy-configuration-urls.png)
    
1. Kurumsal **Ayarlar** sayfasındaki [Nitro Admin portalında](https://admin.gonitro.com/) **Tek Oturum Açma** bölümünü bulun. **Kurulum SAML SSO'u**seçin.

    a. **Giriş URL'sini** önceki adımdan **URL'ye** kaydedin.
    
    b. **X509 İmzalama Sertifikası** alanında önceki adımdan **Sertifikayı (Base64)** yükleyin.
    
    c. **Gönder**’i seçin.
    
    d. **Tek Oturum Aç'ı Etkinleştir'i**seçin.


1. [Azure portalına](https://portal.azure.com/)geri dön. **SAML sayfasıyla Tek Oturum** Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfası yla Tek Oturum Açma'nın ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. Tanımlayıcı **metin** kutusunda, [Nitro Admin portalından](https://admin.gonitro.com/) **SAML Entity ID** alanını kopyalayıp yapıştırın. Aşağıdaki desen olmalıdır:`urn:auth0:gonitro-prod:<ENVIRONMENT>`

    b. **Yanıtla URL** metin kutusunda, [Nitro Admin portalından](https://admin.gonitro.com/) **ACS URL** alanını kopyalayıp yapıştırın. Aşağıdaki desen olmalıdır:`https://gonitro-prod.eu.auth0.com/login/callback?connection=<ENVIRONMENT>`

1. **Ek URL'ler ayarla'yı**seçin ve uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna URL'yi yazın:`https://sso.gonitro.com/login`

1. **Kaydet**’i seçin.

1. Nitro Productivity Suite uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren, SAML iddialarının belirli bir biçimde olmasını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![Varsayılan özniteliklerin ekran görüntüsü](common/default-attributes.png)

1. Önceki özniteliklere ek olarak, Nitro Productivity Suite uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler önceden doldurulmuş, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı  |  Kaynak özniteliği|
    | ---------------| --------------- |
    | çalışanNumarası |  user.objectid |


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

Bu bölümde, B.Simon'ın Nitro Productivity Suite'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulama listesinde **Nitro Productivity Suite'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-nitro-productivity-suite-sso"></a>Nitro Productivity Suite SSO'da yapılandırın

Nitro Productivity Suite tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve azure portalından uygun kopyalanmış URL'leri [Nitro Productivity Suite destek ekibine](https://www.gonitro.com/support)gönderin. Destek ekibi, SAML SSO bağlantısının her iki tarafta da düzgün şekilde ayarlanınmasını sağlar.

### <a name="create-a-nitro-productivity-suite-test-user"></a>Nitro Productivity Suite test kullanıcısı oluşturma

Nitro Productivity Suite, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. İşlem yapmak için ek bir işlem yoktur. Bir kullanıcı Nitro Productivity Suite'te zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Panel'deki Nitro Productivity Suite döşemesini seçtiğinizde, Otomatik olarak SSO'yu kurduğunuz Nitro Productivity Suite'te oturum açmış olursunuz. Daha fazla bilgi [için, Windows Portalım portalından Oturum Aç ve uygulamaları başlat'](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)a bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Nitro Productivity Suite'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Nitro Productivity Suite'i gelişmiş görünürlük ve kontrollerle koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

