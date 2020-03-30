---
title: 'Öğretici: Skytap için Tek Oturum Açma ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Skytap için Azure Active Directory ve Single Sign-on arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d6cb7ab2-da1a-4015-8e6f-c0c47bb6210f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/13/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a8035f16f531dbb17177d1c2f4d5cd344e5a28
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77565800"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-single-sign-on-for-skytap"></a>Öğretici: Skytap için Tek Oturum Açma ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Skytap için Tek Oturum Açma'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Skytap için Tek Oturum Açma'yı Azure REKLAM ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Skytap için Tek Oturum Açma erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Skytap için Tek Oturum Açma'da otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda, yani Azure portalında yönetin.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Skytap için tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Skytap için Tek Oturum Açma, SP ve IDP'nin Başlattığı SSO'yı destekler.
* Skytap için Tek Oturum Açma'yı yapılandırdıktan sonra oturum denetimini uygulayabilirsiniz. Bu, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızma larını önler. Oturum denetimi koşullu erişimden genişler. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-single-sign-on-for-skytap-from-the-gallery"></a>Galeriden Skytap için Tek Oturum Açma ekleme

Skytap için Tek Oturum Açma'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Skytap için Tek Oturum Açma'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Skytap için Tek Oturum** Açma yazın.
1. Sonuçlar panelinden **Skytap için Tek Oturum Açma'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-single-sign-on-for-skytap"></a>Azure AD oturum açma işlemlerini Skytap için Tek Oturum Açma için yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Skytap için Tek Oturum Açma ile yapılandırın ve test edin. SSO'nun çalışması için, Skytap için Tek Oturum Açma'da bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlantılı bir ilişki kurun.

Azure AD SSO'yu Skytap için Tek Oturum Açma ile yapılandırmak ve test etmek için gereken genel adımlar şunlardır:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**

    a. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**

    b. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak **[için Skytap SSO için Tek Oturum](#configure-single-sign-on-for-skytap-sso)** Açma'yı yapılandırın.

    a. **[Skytap için Tek Oturum](#create-single-sign-on-for-skytap-test-user)** Açma'da B.Simon'ın bir muadili olması için Skytap test kullanıcısı için tek oturum açma alanı oluşturun. Bu karşılık, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Skytap** uygulama tümleştirmesi için Tek Oturum Açma sayfasında **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfasıyla tek oturum açma ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`http://pingone.com/<custom EntityID>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://sso.connect.pingidentity.com/sso/sp/ACS.saml2`

1. **Ek URL'ler Ayarla'yı**seçin ve uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://sso.connect.pingidentity.com/sso/sp/initsso?saasid=<saasid>&idpid=<idpid>`

    
    b. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://pingone.com/1.0/<custom ID>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, Yanıt URL'si, Oturum Açma URL'si ve Röle Durumu ile güncelleştirin. Bu değerleri almak [için Skytap Client destek ekibine](mailto:support@skytap.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** ile tek oturum açma'da, **SAML İmza Sertifikası** bölümünde **Federasyon Metadata XML'i**bulun. Meta veri dosyasını indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısının ekran görüntüsü](common/metadataxml.png)

1. **Skytap için Tek Oturum Açma** bölümünde, gereksiniminize göre uygun URL'yi veya URL'leri kopyalayın.

    ![Kopyalama yapılandırma URL'lerinin ekran görüntüsü](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** alanında gösterilen değeri yazın.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Skytap için Tek Oturum Açma erişimi sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **Skytap için Tek Oturum Açma'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-single-sign-on-for-skytap-sso"></a>Skytap SSO için Tek Oturum Açma'yı yapılandırın

Skytap tarafı için Tek Oturum Açma'da tek oturum açma yı yapılandırmak için, indirilen **Federasyon Metadata XML'ini**ve uygun kopyalanmış URL'lerini Azure portalından [Skytap İstemci destek ekibinin Tek Oturum Açma](mailto:support@skytap.com)ekibine göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için yapılandırır.


### <a name="create-single-sign-on-for-skytap-test-user"></a>Skytap test kullanıcısı için Tek Oturum Açma oluşturma

Bu bölümde, Skytap için Tek Oturum Açma'da B.Simon adında bir kullanıcı oluşturursunuz. [Skytap Istemcisi destek ekibi için Tek Oturum](mailto:support@skytap.com) Açma ekibiyle çalışarak kullanıcıları Skytap platformu için Tek Oturum Açma platformuna ekleyin. Kullanıcıları oluşturmadan ve etkinleştirene kadar tek oturum açma kullanamazsınız.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Skytap döşemesi için Tek Oturum Açma'yı seçtiğinizde, SSO'yu kurduğunuz Skytap için Tek Oturum Açma'da otomatik olarak oturum açmanız gerekir. Daha fazla bilgi için [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)e bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Bolluğu Deneyin](https://aad.portal.azure.com/)

