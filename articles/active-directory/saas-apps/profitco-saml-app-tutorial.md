---
title: 'Öğretici: Profit.co SAML uygulaması ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Profit.co SAML uygulaması arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81770935"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-profitco-saml-app"></a>Öğretici: Profit.co SAML uygulamasıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Profit.co SAML uygulamasını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Profit.co SAML uygulamasını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Profit.co SAML uygulamasına erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Profit.co SAML uygulamasına otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin, Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Profit.co SAML uygulaması çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Profit.co SAML uygulaması ıDP tarafından başlatılan SSO 'yu destekler.

* Profit.co SAML uygulamasını yapılandırdıktan sonra, oturum denetimini zorunlu kılabilirsiniz. Bu, kuruluşunuzun hassas verilerinin, gerçek zamanlı olarak ayıklanma ve içe taşmayı korur. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-profitco-saml-app-from-the-gallery"></a>Galeriden Profit.co SAML uygulaması ekleme

Profit.co SAML uygulamasının tümleştirmesini Azure AD ile yapılandırmak için, galerideki Profit.co SAML uygulamasını, yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Profit.co SAML uygulaması** yazın.
1. Sonuçlar panelinden **PROFIT.co SAML uygulaması** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-profitco-saml-app"></a>Profit.co SAML uygulaması için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'YU Profit.co SAML uygulamasıyla yapılandırın ve test edin. SSO 'nun çalışması için, Profit.co SAML uygulamasındaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlı bir ilişki kurun.

Profit.co SAML uygulaması ile Azure AD SSO 'yu yapılandırmak ve test etmek için genel adımlar aşağıda verilmiştir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[PROFIT.co SAML uygulama SSO 'Yu yapılandırın](#configure-profitco-saml-app-sso)** .
    1. Profit.co SAML uygulamasında B. Simon 'ya karşılık gelen **[PROFIT.co SAML uygulama test kullanıcısı oluşturun](#create-a-profitco-saml-app-test-user)** . Bu karşılık gelen bu, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Profit.co SAML uygulama** uygulaması tümleştirme sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma**seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL 'ler Azure 'da zaten önceden doldurulur. Kullanıcıların **Kaydet** düğmesini seçerek yapılandırmayı kaydetmesi gerekir.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **Kopyala** düğmesini seçin. Bu, **uygulama Federasyon meta veri URL 'sini** kopyalar ve bilgisayarınıza kaydeder.

    ![SAML Imzalama sertifikasının, Kopyala düğmesi vurgulanmış şekilde ekran görüntüsü](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmeden,**Kullanıcılar** >  **Azure Active Directory** > **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** alanında gösterilen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Profit.co SAML uygulamasına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **PROFIT.co SAML uygulaması**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar sayfasının ekran görüntüsü, Kullanıcı Ekle vurgulanmış olarak](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-profitco-saml-app-sso"></a>Profit.co SAML uygulaması SSO 'yu yapılandırma

Profit.co SAML uygulama tarafında çoklu oturum açmayı yapılandırmak için, uygulama Federasyon meta verileri URL 'sini [PROFIT.co SAML uygulama destek ekibine](mailto:support@profit.co)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak için bu ayarı yapılandırır.

### <a name="create-a-profitco-saml-app-test-user"></a>Profit.co SAML uygulaması test kullanıcısı oluşturma

Bu bölümde, Profit.co SAML uygulamasında B. Simon adlı bir Kullanıcı oluşturacaksınız. Profit.co SAML uygulama platformunda kullanıcıları eklemek için [PROFIT.co SAML uygulama destek ekibi](mailto:support@profit.co) ile çalışın. Kullanıcıları oluşturup etkinleştirene kadar çoklu oturum açma kullanamazsınız.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim paneli ' ni kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Profit.co SAML uygulaması kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Profit.co SAML uygulamasında otomatik olarak oturum açmış olmanız gerekir. Daha fazla bilgi için bkz. [erişim paneli 'Ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Profit.co SAML uygulamasını deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Profit.co SAML uygulamasını koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
