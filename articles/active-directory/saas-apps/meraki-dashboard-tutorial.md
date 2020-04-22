---
title: 'Öğretici: Meraki Dashboard ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Meraki Dashboard arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 09766236-5de0-43fd-8950-5316390ce646
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 571f4421a5d890fab31eda0125802d33918144ef
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726385"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-meraki-dashboard"></a>Öğretici: Meraki Dashboard ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Meraki Panosu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Meraki Panosu'nun Azure REKLAM ile tümleştirildiğinde şunları yapabilirsiniz:

* Meraki Dashboard erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Meraki Panosu'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Meraki Dashboard tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Meraki Dashboard **IDP** başlatılan SSO destekler
* Meraki Panosu'nun yapılandırılmasından sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-meraki-dashboard-from-the-gallery"></a>Galeriden Meraki Panosu Ekleme

Meraki Dashboard'un Azure AD'ye entegrasyonunu yapılandırmak için, Meraki Panosu'nu galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Meraki Panosu** yazın.
1. Sonuç panelinden **Meraki Panosu'nu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-meraki-dashboard"></a>Meraki Panosu için Azure AD oturum açma işlemlerini yapılandırma ve test

Azure AD SSO'nu Meraki Dashboard ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Meraki Panosu'ndaki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Meraki Dashboard ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Meraki Dashboard SSO'yu yapılandırın](#configure-meraki-dashboard-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Meraki Dashboard test kullanıcısını oluşturun](#create-meraki-dashboard-test-user)** - kullanıcının Azure AD gösterimine bağlı Olan B.Simon Meraki Panosu'ndaki bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Meraki Dashboard** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.


1. Meraki Dashboard uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Meraki Dashboard uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı | Kaynak Özniteliği|
    | ---------------| --------- |
    | kullanıcı adı | user.userprincipalname |
    | rol | user.assignedroles |

    > [!NOTE]
    > Azure AD'deki rolleri nasıl yapılandırılayacağımı anlamak için [buraya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)bakın.

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, Parmak İzi Değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. **Meraki Panosu'nu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Meraki Panosu'na erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Meraki Panosu'nu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-meraki-dashboard-sso"></a>Meraki Pano SSO'da Yapılandır

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak meraki panosunda oturum açın.

1. **Kuruluş** -> **Ayarlarına**gidin.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/configure1.png)

1. Kimlik Doğrulama altında **SAML SSO'nun** **SAML SSO etkin olarak**değiştirin.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/configure2.png)

1. **SAML IdP ekle'yi**tıklatın.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/configure3.png)

1. Azure portalından kopyaladığınız **Parmak İzi** Değerini **X.590 cert SHA1 parmak izi** metin kutusuna yapıştırın. Daha sonra **Kaydet**'e tıklayın.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/configure4.png)

### <a name="create-meraki-dashboard-test-user"></a>Meraki Pano test kullanıcıoluşturma

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak meraki panosunda oturum açın.

1. **Kuruluş** -> **Yöneticilerine**gidin.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/user1.png)

1. SAML yönetici rolleri bölümünde, **SAML rol ekle** düğmesini tıklatın.

    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/user2.png)

1. Rol **meraki_full_admin**girin, **Kuruluş erişimini** **Tam** olarak işaretleyin ve **rolü oluştur'u**tıklatın. **meraki_readonly_admin**için işlemi yineleyin, bu zaman yalnızca **Okunur** kutusu olarak **Kuruluş erişimini** işaretleyin.
 
    ![Meraki Pano Yapılandırması](./media/meraki-dashboard-tutorial/user3.png)

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Meraki Pano döşemesini tıklattığınızda, SSO'yu kurduğunuz Meraki Panosu'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Meraki Panosu'ni deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Meraki Panosu'nun gelişmiş görünürlüğü ve kontrolleri ile nasıl korunulur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

