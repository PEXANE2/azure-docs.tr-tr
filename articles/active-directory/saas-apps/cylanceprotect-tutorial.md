---
title: 'Öğretici: CylancePROTECT ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve CylancePROTECT arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ea392d8c-c8aa-4475-99d0-b08524ef0f3a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/10/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1df0a295cb48925587e9741fa29d4d02376441a8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76983512"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cylanceprotect"></a>Öğretici: CylancePROTECT ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, CylancePROTECT ile Azure Etkin Dizini (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. CylancePROTECT'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* CylancePROTECT erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla CylancePROTECT ile otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* CylancePROTECT tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* CylancePROTECT **IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-cylanceprotect-from-the-gallery"></a>Galeriden CylancePROTECT ekleme

CylancePROTECT'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden CylancePROTECT'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **CylancePROTECT** yazın.
1. Sonuçlar panelinden **CylancePROTECT'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cylanceprotect"></a>CylancePROTECT için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak CylancePROTECT ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile CylancePROTECT'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu CylancePROTECT ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[CylancePROTECT SSO'yu yapılandırın](#configure-cylanceprotect-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[CylancePROTECT test kullanıcısını oluşturun](#create-cylanceprotect-test-user)** - Kullanıcının Azure AD gösterimine bağlı CylancePROTECT'deki B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **CylancePROTECT** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna URL'yi yazın:
    
    | Bölge | URL Değeri |
    |----------|---------|
    | Asya-Pasifik Kuzeydoğu (APNE1)| `https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya-Pasifik Güneydoğu (AU) | `https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml` |
    | Avrupa Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

    b. **Yanıtla URL** metin kutusuna URL'yi yazın:
    
    | Bölge | URL Değeri |
    |----------|---------|
    | Asya-Pasifik Kuzeydoğu (APNE1)|`https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Asya-Pasifik Güneydoğu (AU)|`https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Avrupa Orta (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Kuzey Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Güney Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

1. CylancePROTECT uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. CylancePROTECT uygulaması **nameidentifier** **user.mail** ile eşlenen ve diğer tüm iddiaları kaldırmak için bekliyor, bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşlemi değiştirmek gerekir.

    ![image](common/edit-attribute.png)

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **CylancePROTECT'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

> [!NOTE]
> İndirilen Base64 kodlanmış sertifikayı bir metin düzenleyicisinde açın ve Cylance Admin portalına yapıştırmak için **START** ve **END** etiketleri arasındaki metni kopyalayın.

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

Bu bölümde, CylancePROTECT'e erişim izni vererek B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **CylancePROTECT'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-cylanceprotect-sso"></a>CylancePROTECT SSO'nun yapılandırılSın

**CylancePROTECT** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından [CylancePROTECT destek ekibine](mailto:Ibrahim.nafea@toyota.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar. Daha fazla bilgi için Cylance belgelerini kullanın: [https://support.cylance.com/s/article/Use-Microsoft-Azure-with-CylancePROTECT4](https://support.cylance.com/s/article/Use-Microsoft-Azure-with-CylancePROTECT4).

### <a name="create-cylanceprotect-test-user"></a>CylancePROTECT test kullanıcısı oluşturma

Bu bölümde, CylancePROTECT britta Simon adında bir kullanıcı oluşturun. Kullanıcıları CylancePROTECT platformuna eklemek için konsol yöneticisiyle birlikte çalışın. Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki CylancePROTECT döşemesini tıklattığınızda, SSO'yu kurduğunuz CylancePROTECT'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile CylancePROTECT'i deneyin](https://aad.portal.azure.com/)