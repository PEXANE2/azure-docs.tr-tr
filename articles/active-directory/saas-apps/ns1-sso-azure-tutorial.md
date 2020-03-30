---
title: 'Öğretici: Azure için NS1 SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure için Azure Active Directory ve NS1 SSO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 24a1afb6-b8b6-4787-bd4b-8fe3a32f8def
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/12/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e917265f4bf6f857a0eada2433f0a0e4e24d7c5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77565596"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ns1-sso-for-azure"></a>Öğretici: Azure için NS1 SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Azure için NS1 SSO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz öğreneceksiniz. Azure için NS1 SSO'su Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure için NS1 SSO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure için NS1 SSO'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda, yani Azure portalında yönetin.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure için NS1 SSO tek oturum açma (SSO) aboneliği etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Azure için NS1 SSO, SP ve IDP'yi destekler.
* Azure için NS1 SSO'su yapılandırdıktan sonra oturum denetimini uygulayabilirsiniz. Bu, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızma larını önler. Oturum denetimi koşullu erişimden genişler. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)


## <a name="add-ns1-sso-for-azure-from-the-gallery"></a>Galeriden Azure için NS1 SSO ekleme

Azure için NS1 SSO'nun Azure'a entegrasyonunu Azure AD'ye yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Azure için NS1 SSO eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Azure için NS1 SSO** yazın.
1. Sonuçlar panelinden **Azure için NS1 SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-ns1-sso-for-azure"></a>Azure için NS1 SSO için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Azure için NS1 SSO ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Azure için NS1 SSO'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bağlantılı bir ilişki kurun.

Azure AD SSO'yu Azure için NS1 SSO ile yapılandırmak ve test etmek için gereken genel adımlar şunlardır:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**

    a. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**

    b. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak **[için Azure SSO için NS1 SSO'yu yapılandırın.](#configure-ns1-sso-for-azure-sso)**

    a. **[Azure için NS1 SSO'da](#create-an-ns1-sso-for-azure-test-user)** B.Simon'ın bir örneğine sahip olması için Azure test kullanıcısı için bir NS1 SSO oluşturun. Bu karşılık, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Azure için NS1 SSO** uygulama tümleştirme sayfasında **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfasıyla tek oturum açma ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki URL'yi yazın:`https://api.nsone.net/saml/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:`https://api.nsone.net/saml/sso/<ssoid>`

1. **Ek URL'ler ayarla'yı**seçin ve uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki URL'yi yazın:`https://my.nsone.net/#/login/sso`

    > [!NOTE]
    > YanıtURL değeri gerçek değil. Yanıtla URL değerini gerçek YanıtURL'i ile güncelleştirin. Değeri almak [için Azure İstemci destek ekibine](mailto:techops@nsone.net) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Azure için NS1 SSO uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri & Talepler** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için kalem simgesini seçin.

    ![Kalem simgesi vurgulanmış Kullanıcı Öznitelikleri & Talepler bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/attribute-edit-option.png)

1. Talebi güncellemek için öznitelik adını seçin.

    ![Öznitelik adı vurgulanmış Kullanıcı Öznitelikleri & Talepler bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/attribute-claim-edit.png)

1. **Dönüşüm'u**seçin.

    ![Dönüşüm vurgulandığı claim'i yönet bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/prefix-edit.png)

1. **Dönüşümleri Yönet** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çeşitli alanların vurgulandığı dönüşüm üyüldünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/prefix-added.png)

    1. **Dönüşüm**olarak **ExactMailPrefix()'yi** seçin.

    1. **User.userprincipalname'yi** **Parametre 1**olarak seçin.

    1. **Ekle'yi**seçin.

    1. **Kaydet'i**seçin.

1. **SAML** ile tek oturum açma'da, **SAML İmza Sertifikası** bölümünde kopyala düğmesini seçin. Bu, **App Federation Metadata Url'yi** kopyalar ve bilgisayarınıza kaydeder.

    ![Kopya düğmesi vurgulanmış SAML İmza Sertifikası'nın ekran görüntüsü](common/copy-metadataurl.png)

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

Bu bölümde, Azure için NS1 SSO'ya erişim sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde Azure **için NS1 SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-ns1-sso-for-azure-sso"></a>Azure SSO için NS1 SSO'ya yapı

Azure tarafı için NS1 SSO'da tek oturum açma yapılandırmak için, Uygulama Federasyonu Metaveri URL'sini [Azure destek ekibi için NS1 SSO'ya](mailto:techops@nsone.net)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için yapılandırır.

### <a name="create-an-ns1-sso-for-azure-test-user"></a>Azure test kullanıcısı için Bir NS1 SSO oluşturma

Bu bölümde, Azure için NS1 SSO'da B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Azure platformu için NS1 SSO'ya eklemek için Azure destek ekibi için NS1 SSO ile birlikte çalışın. Kullanıcıları oluşturmadan ve etkinleştirene kadar tek oturum açma kullanamazsınız.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Azure için NS1 SSO'yu seçtiğinizde, SSO'yu ayarladığınız Azure için NS1 SSO'da otomatik olarak oturum açmış olmalısınız. Daha fazla bilgi için [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)e bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Azure için NS1 SSO'u deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)