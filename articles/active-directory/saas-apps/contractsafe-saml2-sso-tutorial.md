---
title: 'Öğretici: ContractSafe Saml2 SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ContractSafe Saml2 SSO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9d8c9eba-6a90-4c8f-b387-a6ead4af00af
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 12/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ab2dc71f39164988e0d229fc994548a00447986
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77185628"
---
# <a name="tutorial-integrate-azure-active-directory-single-sign-on-sso-with-contractsafe-saml2-sso"></a>Öğretici: Azure Active Directory'yi (SSO) ContractSafe Saml2 SSO ile tümleştirin

Bu eğitimde, ContractSafe Saml2 SSO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz. ContractSafe Saml2 SSO'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de ContractSafe Saml2 SSO'ya kimlerin erişebilenleri kontrol edin.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ContractSafe Saml2 SSO'da otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SSO özellikli bir ContractSafe Saml2 SSO aboneliği etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. ContractSafe Saml2 **SSO, IDP**tarafından başlatılan SSO'ya destek veriyor.

## <a name="add-contractsafe-saml2-sso-from-the-gallery"></a>Galeriden ContractSafe Saml2 SSO ekle

ContractSafe Saml2 SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize ContractSafe Saml2 SSO eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **ContractSafe Saml2 SSO** yazın.
1. Sonuç panelinden **ContractSafe Saml2 SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-contractsafe-saml2-sso"></a>Azure AD SSO'nun SözleşmeSafe Saml2 SSO için yapılandırılın ve test edin

Azure AD SSO'nu ContractSafe Saml2 SSO ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile ContractSafe Saml2 SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ContractSafe Saml2 SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
   * **B.Simon** hesabını kullanarak Azure AD SSO'yu test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
   * **B.Simon'ın** Azure AD SSO'yu kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)

1. Uygulama tarafındaki SSO ayarlarını yapılandırmak için [ContractSafe Saml2 SSO'yu yapılandırın.](#configure-contractsafe-saml2-sso)
   * Kullanıcının Azure AD gösterimine bağlı ContractSafe Saml2 SSO'da **B.Simon'ın** bir muadili olması için [bir ContractSafe Saml2 SSO test kullanıcısı oluşturun.](#create-a-contractsafe-saml2-sso-test-user)
2. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **ContractSafe Saml2 SSO** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme (kalem) simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da,** ilgili alanlara aşağıdaki değerleri girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki biçimi kullanarak bir URL girin:`https://app.contractsafe.com/saml2_auth/<UNIQUEID>/acs/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki biçimi kullanarak bir URL girin:`https://app.contractsafe.com/saml2_auth/<UNIQUEID>/acs/`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek tanımlayıcı ve yanıt URL'si ile güncelleştirin. Bu değerleri almak için [ContractSafe Saml2 SSO Müşteri destek ekibine](mailto:support@contractsafe.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen biçimlere de bakabilirsiniz.

1. ContractSafe Saml2 SSO, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![Ortak varsayılan öznitelikler](common/default-attributes.png)

1. Varsayılan özniteliklere ek olarak, ContractSafe Saml2 SSO uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler önceden doldurulmuş, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz. Aşağıdaki liste ek öznitelikleri gösterir.

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | e-posta adı | user.userprincipalname |
    | e-posta | user.inpremisesuserprincipalname |

1. **SAML** ile tek oturum açma'da, **SAML İmza Sertifikası** bölümünde **Federasyon Metadata XML'i**bulun. Sertifikayı indirmek için **İndir'i** seçin ve ardından bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **ContractSafe Saml2 SSO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

## <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında **B.Simon**adında bir test kullanıcısı oluşturacaksınız.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin. **Kullanıcıları**seçin ve ardından **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** `username@companydomain.extension` alanına, biçimde bir e-posta adresi girin. `B.Simon@contoso.com` bunun bir örneğidir.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**seçin.

## <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, **B.Simon'ın** ContractSafe Saml2 SSO'ya erişim sağlayarak Azure SSO'su kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **ContractSafe Saml2 SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve ardından **Kullanıcılar ve gruplar**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B.Simon'ı** seçin. Ardından, ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından, ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

## <a name="configure-contractsafe-saml2-sso"></a>Sözleşmeyi YapılandırınGüvenli Saml2 SSO

SSO'yu **ContractSafe Saml2 SSO** tarafında yapılandırmak için, indirilen **Federasyon Metadata XML'i** ve uygun kopyalanmış URL'leri Azure portalından [ContractSafe Saml2 SSO destek ekibine](mailto:support@contractsafe.com)göndermeniz gerekir. Takım, SAML SSO bağlantısını her iki tarafta düzgün bir şekilde ayarlamakla yükümlüdür.

## <a name="create-a-contractsafe-saml2-sso-test-user"></a>ContractSafe Saml2 SSO test kullanıcısı oluşturma

ContractSafe Saml2 SSO'da B.Simon adında bir kullanıcı oluşturun. Kullanıcıları ContractSafe Saml2 SSO platformuna eklemek için [ContractSafe Saml2 SSO destek ekibiyle](mailto:support@contractsafe.com) birlikte çalışın. SSO'u kullanmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Access Panelini kullanarak Azure AD SSO yapılandırmanızı test edin. Access Paneli'ndeki ContractSafe Saml2 SSO karosu seçtiğinizde, SSO'yu kurduğunuz ContractSafe Saml2 SSO'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ContractSafe Saml2 SSO'u deneyin](https://aad.portal.azure.com/)
