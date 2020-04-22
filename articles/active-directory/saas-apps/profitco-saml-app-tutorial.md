---
title: 'Öğretici: Profit.co SAML Uygulaması ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Profit.co SAML Uygulaması arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 27f42934-c636-43dd-9c20-a3c6316f2763
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 564ca97210d85c5118901f30261abe3de9df1053
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770935"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-profitco-saml-app"></a>Öğretici: Profit.co SAML Uygulaması ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Profit.co SAML Uygulaması'nı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SAML App Profit.co Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAML App'Profit.co erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Otomatik olarak oturum açarak SamL App'Profit.co Azure AD hesaplarıyla oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda, yani Azure portalında yönetin.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Profit.co SAML App tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Profit.co SAML App IDP başlatılan SSO destekler.

* SAML App Profit.co yapılandırdıktan sonra oturum denetimini uygulayabilirsiniz. Bu, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızma larını önler. Oturum denetimi koşullu erişimden genişler. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-profitco-saml-app-from-the-gallery"></a>Galeriden SAML Uygulaması Profit.co ekleyin

saml app Profit.co Azure AD'ye entegrasyonunu yapılandırmak için galeriden Profit.co SAML Uygulaması'nı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAML Uygulaması Profit.co** yazın.
1. Sonuç panelinden **SAML AppProfit.coi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-profitco-saml-app"></a>Profit.co SAML Uygulaması için Azure AD oturum açma işlemlerini yapılandırma ve test Profit.co

Azure AD SSO'Profit.co SAML Uygulaması ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Profit.co SAML Uygulamasında bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlantılı bir ilişki kurun.

Azure AD SSO'yu Profit.co SAML Uygulamasıyla yapılandırmak ve test etmek için gereken genel adımlar şunlardır:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[SAML App SSOProfit.co](#configure-profitco-saml-app-sso)** yapılandırın.
    1. Profit.co SAML App B.Simon bir meslektaşı olması için **[Profit.co SAML App test kullanıcı oluşturun.](#create-a-profitco-saml-app-test-user)** Bu karşılık, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Profit.co SAML App** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfasıyla tek oturum açma ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure'da zaten önceden doldurulmuşdurumda. Kullanıcıların **Kaydet** düğmesini seçerek yapılandırmayı kaydetmeleri gerekir.

1. **SAML** ile tek oturum açma'da, **SAML İmza Sertifikası** bölümünde **Kopyala** düğmesini seçin. Bu, **App Federation Metadata Url'yi** kopyalar ve bilgisayarınıza kaydeder.

    ![Kopya düğmesi vurgulanmış SAML İmza Sertifikası'nın ekran görüntüsü](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** alanında gösterilen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Profit.co SAML Uygulamasına erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **SAML App Profit.co**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-profitco-saml-app-sso"></a>SamL App SSO Profit.co yapılandır

Profit.co SAML App tarafında tek oturum açma yapılandırmak için, App Federation Metadata URL'sini [Profit.co SAML App destek ekibine](mailto:support@profit.co)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için yapılandırır.

### <a name="create-a-profitco-saml-app-test-user"></a>saml app test kullanıcısı Profit.co oluşturma

Bu bölümde, SAML App Profit.co B.Simon adlı bir kullanıcı oluşturun. Profit.co SAML App platformundaki kullanıcıları eklemek için [Profit.co SAML App destek ekibiyle](mailto:support@profit.co) birlikte çalışın. Kullanıcıları oluşturmadan ve etkinleştirene kadar tek oturum açma kullanamazsınız.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Panel'de Profit.co SAML Uygulama döşemesini seçtiğinizde, SSO'yu kurduğunuz Profit.co SAML Uygulamasında otomatik olarak oturum açmalısınız. Daha fazla bilgi için [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)e bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAML Uygulamasını Profit.co deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle Profit.co SAML App nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
