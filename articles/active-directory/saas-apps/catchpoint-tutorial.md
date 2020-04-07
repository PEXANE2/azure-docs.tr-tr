---
title: 'Öğretici: Catchpoint ile Azure Active Directory tek oturum açma (SSO) entegrasyonu'
description: Azure Active Directory ve Catchpoint arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab3eead7-8eb2-4c12-bb3a-0e46ec899d37
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b19e286d299811a950df05f93d221bd710676ea
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743502"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-catchpoint"></a>Öğretici: Catchpoint ile Azure Active Directory tek oturum açma tümleştirmesi

Bu eğitimde, Catchpoint'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Catchpoint'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'den Catchpoint'e kullanıcı erişimini denetle.
* Azure AD hesabı olan kullanıcılar için otomatik Catchpoint oturum açma'yı etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) etkin leştirilmiş bir Catchpoint aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Catchpoint, SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yı destekler.
* Catchpoint tam zamanında (JIT) kullanıcı sağlamayı destekler.
* Catchpoint'i yapılandırıldıktan sonra oturum denetimini uygulayabilirsiniz. Bu önlem, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızmasına ve sızmasına karşı koruma sağlar. Oturum denetimi Koşullu Erişim'in bir uzantısıdır. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-catchpoint-from-the-gallery"></a>Galeriden Catchpoint ekle

Catchpoint'in Azure AD'ye entegrasyonunu yapılandırmak için Catchpoint'i yönetilen SaaS uygulamaları listenize ekleyin.

1. Azure [portalında](https://portal.azure.com) iş, okul veya kişisel Microsoft hesabıyla oturum açın.
1. Sol bölmede **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Catchpoint** yazın.
1. Sonuçlar panelinden **Catchpoint'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-catchpoint"></a>Catchpoint için Azure AD oturum açma işlemlerini yapılandırma ve test edin

SSO'nun çalışması için, bir Azure AD kullanıcısını Catchpoint'teki bir kullanıcıya bağlamanız gerekir. Bu öğretici için, **B.Simon**adlı bir test kullanıcısı yapılandıracağız. 

Aşağıdaki bölümleri tamamlayın:

1. Bu özelliği kullanıcılarınız için etkinleştirmek için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    * Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    * B.Simon'ın Azure AD tek oturum açma'yı kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. [Catchpoint SSO'yu,](#configure-catchpoint-sso)uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * B.Simon Azure AD test hesabının Catchpoint'teki benzer bir kullanıcı hesabına bağlanmasına izin vermek için [Catchpoint test kullanıcısını oluşturun.](#create-a-catchpoint-test-user)
1. [Yapılandırmanın](#test-sso)çalıştığını doğrulamak için SSO'yu test edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure AD SSO'yu etkinleştirmek için Azure portalında aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Catchpoint** uygulama tümleştirme **sayfasında, Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Tek Oturum Açma'yı** Ayarla'da, **Temel SAML Yapılandırma** ayarlarını düzenlemek için kalem simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Catchpoint için başlatılan modu yapılandırın:
   - **IDP**tarafından başlatılan mod için aşağıdaki alanların değerlerini girin:
     - **Tanımlayıcı**için:`https://portal.catchpoint.com/SAML2`
     - **Cevap URL'si**için :`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`
   - **SP**tarafından başlatılan mod için **ek URL'ler** ayarla'yı seçin ve aşağıdaki değeri girin:
     - **Oturum Açma URL'si**için :`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. Catchpoint uygulaması, SAML iddialarını belirli bir biçimde bekler. SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri ekleyin. Aşağıdaki tablo varsayılan özniteliklerin listesini içerir:

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | Givenname | user.givenneame |
    | Soyadı | user.surname |
    | Emailaddress | kullanıcı.posta |
    | Adı | user.userprincipalname |
    | Benzersiz Kullanıcı Tanımlayıcısı | user.userprincipalname |

    ![Kullanıcı Öznitelikleri & Talepler listesi ekran görüntüsü](common/default-attributes.png)

1. Ayrıca, Catchpoint uygulaması saml yanıtı geçirilen başka bir öznitelik bekliyor. Aşağıdaki tabloya bakın. Bu öznitelik de önceden doldurulmuş, ancak gözden geçirebilirsiniz ve gereksinimlerinize uyacak şekilde güncelleştirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | ad alanı | user.assignedrole |

    > [!NOTE]
    > Talebin `namespace` hesap adı ile eşlemesi gerekir. Bu hesap adı, SAML yanıtında geri geçirilecek Azure AD'da bir rol ile ayarlanmalıdır. Azure AD'deki roller hakkında daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen rol iddiasını yapılandır'a](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)bakın.

1. **SAML sayfasıyla Tek Oturum Açma'yı Ayarla sayfasına** gidin. **SAML İmza Sertifikası** bölümünde **Sertifika (Base64)** bulun. Sertifikayı bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Catchpoint'i ayarla** bölümünde, daha sonraki bir adımda gereksinim duyduğunuz URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B.Simon adında bir Azure AD test kullanıcısı oluşturmak için Azure portalını kullanırsınız.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com` girin.
   1. **Parolayı göster** onay kutusunu seçin. Görüntülenen parola değerine dikkat edin.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Catchpoint'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **Catchpoint'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    !["Kullanıcı ekle" bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ekranın alt kısmında **Seç'i** tıklatın.
1. SAML iddiasında bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusuna bakın ve listeden kullanıcının rolünü seçin. Ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-catchpoint-sso"></a>Catchpoint SSO'da yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Catchpoint uygulamasında yönetici olarak oturum açın.

1. **Ayarlar** simgesini ve ardından **SSO Kimlik Sağlayıcısı'nı**seçin.

    ![SSO Identity Provider seçili catchpoint ayarları ekran görüntüsü](./media/catchpoint-tutorial/configuration1.png)

1. Tek **İşaret Aç** sayfasında aşağıdaki alanları girin:

   ![Catchpoint Tek İşaret Sayfa ekran görüntüsü](./media/catchpoint-tutorial/configuration2.png)

   Alan | Değer
   ----- | ----- 
   **Namespace** | Geçerli bir ad alanı değeri.
   **Kimlik Sağlayıcı Veren** | Azure `Azure AD Identifier` portalındaki değer.
   **Url'de Tek İşaret** | Azure `Login URL` portalındaki değer.
   **Sertifika** | Azure portalından indirilen `Certificate (Base64)` dosyanın içeriği. Görüntülemek ve kopyalamak için Not Defteri'ni kullanın.

   Ayrıca, **Meta Verileri Yükle** seçeneğini seçerek Federasyon **Metadata XML'ini** yükleyebilirsiniz.

1. **Kaydet'i**seçin.

### <a name="create-a-catchpoint-test-user"></a>Catchpoint test kullanıcısı oluşturma

Catchpoint, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde hiçbir eylem öğesi yok. B.Simon Catchpoint'te kullanıcı olarak zaten yoksa, kimlik doğrulamadan sonra oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Uygulamalarım portalındaki Catchpoint döşemesini seçtiğinizde, SSO yapılandırılmış Catchpoint uygulamasında otomatik olarak oturum açmalısınız. Uygulamalarım portalı hakkında daha fazla bilgi [için, Oturum Aç ve Uygulamalarım portalından başlatın.](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)

> [!NOTE]
> Giriş sayfası üzerinden Catchpoint uygulamasına giriş yaptığınızda, **Catchpoint Kimlik Bilgilerini**sağladıktan **sonra, Şirket Kimlik Bilgileri(SSO)** alanına geçerli **Ad Alanı** değerini girin ve **Giriş'i**seçin.
> 
> ![Catchpoint yapılandırması](./media/catchpoint-tutorial/loginimage.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Catchpoint'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
