---
title: 'Öğretici: Catchpoint ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
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
ms.openlocfilehash: c56d34a331821ffbc3d0d4f2cf5e9b033f4011ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78968477"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-catchpoint"></a>Öğretici: Catchpoint ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Catchpoint'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Catchpoint'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Catchpoint erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Catchpoint'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Catchpoint tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Catchpoint **SP ve IDP** başlatılan SSO destekler
* Catchpoint **Just In Time** kullanıcı sağlama destekler
* Catchpoint'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-catchpoint-from-the-gallery"></a>Galeriden Catchpoint ekleme

Catchpoint'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Catchpoint'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Catchpoint** yazın.
1. Sonuçlar panelinden **Catchpoint'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-catchpoint"></a>Catchpoint için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Catchpoint ile yapılandırın ve test edin. SSO'nun çalışması için, Catchpoint'teki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Catchpoint ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Catchpoint SSO'yu yapılandırır](#configure-catchpoint-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Catchpoint test kullanıcısını oluşturun](#create-catchpoint-test-user)** - Kullanıcının Azure AD gösterimine bağlı Catchpoint'teki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Catchpoint** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Kurulum tek oturum açmada,** ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenle/kalem simgesine tıklayın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna URL'yi yazın:`https://portal.catchpoint.com/SAML2`

    b. **Url'yi Yanıtla** metin kutusuna URL'yi yazın:`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna URL'yi yazın:`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. Catchpoint uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Catchpoint uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ------------ | --------- |
    | ad alanı | user.assignedrole |

    > [!NOTE]
    > ad alanı talebinin hesap adı ile eşlemesi gerekir. Bu hesap adı, SAML yanıtında geri geçirilecek Azure AD'deki roller olarak düzenlenmelidir. Rolleri nasıl kuracağız öğrenmek için lütfen bu [makaleye](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) bakın

1. **SAML** ile Kurulum tek oturum açma bölümünde, **SAML İmza Sertifikası** bölümünde **Sertifikayı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Catchpoint'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

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

Bu bölümde, B.Simon'ın Catchpoint'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Catchpoint'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-catchpoint-sso"></a>Catchpoint SSO'da yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Catchpoint uygulamasında yönetici olarak oturum açın.

1. **Ayarlar** simgesine tıklayın ve **SSO Kimlik Sağlayıcısı'nı**seçin.

    ![Catchpoint yapılandırması](./media/catchpoint-tutorial/configuration1.png)

1. Tek **İşaret Açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Catchpoint yapılandırması](./media/catchpoint-tutorial/configuration2.png)

    1. Ad Alanı metin **kutusuna** geçerli bir ad alanı değeri girin.

    1. Kimlik **Sağlayıcı Veren** metin kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini girin.

    1. **Url'de Tek İşaret** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini girin.

    1. İndirilen **Sertifika (Base64)** dosyasını Notepad'e açın, sertifika dosyasının içeriğini kopyalayın ve **Sertifika** metin kutusuna yapıştırın.

    1. Ayrıca, **Meta Verileri Yükle** seçeneğini tıklayarak Federasyon **Metadata XML'ini** yükleyebilirsiniz.

    1. **Kaydet**'e tıklayın.

### <a name="create-catchpoint-test-user"></a>Catchpoint test kullanıcısı oluşturma

Bu bölümde, Catchpoint'te Britta Simon adında bir kullanıcı oluşturulur. Catchpoint, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Catchpoint'te bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Catchpoint döşemesini tıklattığınızda, SSO'yu kurduğunuz Catchpoint'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

> [!NOTE]
> Giriş sayfası üzerinden Catchpoint uygulamasına giriş yaptığınızda, **Catchpoint Kimlik Bilgilerini**sağladıktan **sonra, Şirket Kimlik Bilgileri(SSO)** metin kutusuna geçerli **Ad alanı** değerini girin ve **Giriş'i**tıklatın.

![Catchpoint yapılandırması](./media/catchpoint-tutorial/loginimage.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Catchpoint'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)