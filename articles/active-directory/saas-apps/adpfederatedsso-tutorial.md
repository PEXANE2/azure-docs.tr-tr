---
title: 'Öğretici: ADP ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ADP arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/26/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b031ded2022078c31bd8570c6a6317c398715480
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70162651"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-adp"></a>Öğretici: ADP ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, ADP'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. ADP'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* ADP erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ADP'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ADP tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **ADP'den IDP'ye** destek

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-adp-from-the-gallery"></a>Galeriden ADP ekleme

ADP'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize ADP eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **ADP** yazın.
1. Sonuç panelinden **ADP'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-adp"></a>ADP için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak ADP ile yapılandırma ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile ADP'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ADP ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[ADP SSO'yu yapılandırır](#configure-adp-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
    1. **[ADP test kullanıcısını oluşturun](#create-adp-test-user)** - ADP'de B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure portalında, **ADP** uygulama tümleştirme sayfasında **Özellikler sekmesini** tıklatın ve aşağıdaki adımları gerçekleştirin: 

    ![Tek oturum açma özellikleri](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Kullanıcıların **oturum açma** alanı değerini **Evet**olarak ayarlama.

    b. Kullanıcı **erişim URL'sini** kopyalayın ve daha sonra öğreticide açıklanan **Sign-on URL bölümüne**yapıştırmak zorunda sınız.

    c. Kullanıcı **ataması gerekli** alan değerini **Evet**olarak ayarlayın.

    d. **Görünür'ün kullanıcılara göre** alan değerini **No**olarak ayarlayın.

1. Azure [portalında](https://portal.azure.com/), **ADP** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    Tanımlayıcı **(Entity ID)** metin kutusuna bir URL yazın:`https://fed.adp.com`

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **ADP'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın ADP'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **ADP'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-adp-sso"></a>ADP SSO'nun yapılandırılsın

**ADP** tarafında tek oturum açma yapılandırmak için indirilen **Metadata XML'yi** [ADP web sitesine](https://adpfedsso.adp.com/public/login/index.fcc)yüklemeniz gerekir.

> [!NOTE]  
> Bu işlem birkaç gün sürebilir.

### <a name="configure-your-adp-services-for-federated-access"></a>Efedere erişim için ADP hizmetinizi yapılandırın

>[!Important]
> ADP hizmetlerinize federe erişim gerektiren çalışanlarınız ADP hizmet uygulamasına atanmalı ve daha sonra kullanıcılar belirli ADP hizmetine yeniden atanmalıdır.
ADP temsilcinizden onay aldıktan sonra, ADP hizmetinizi yapılandırın ve belirli ADP hizmetine kullanıcı erişimini denetlemek için kullanıcıları atayın/yönetin.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **ADP** yazın.
1. Sonuç panelinden **ADP'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.
1. Azure portalında, **ADP** uygulama tümleştirme sayfanızda **Özellikler sekmesini** tıklatın ve aşağıdaki adımları gerçekleştirin:  

    ![Tek oturum açma bağlantılı özellikler](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Kullanıcıların **oturum açma** alanı değerini **Evet**olarak ayarlama.

    b.  Kullanıcı **ataması gerekli** alan değerini **Evet**olarak ayarlayın.

    c.  Görünür **kullanıcı** alanı değerini **Evet**olarak ayarlayın.

1. Azure [portalında](https://portal.azure.com/), **ADP** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.

1. Tek **oturum açma yöntemi** iletişim kutusunda, **Bağlantılı**olarak **Mod'u** seçin. uygulamanızı **ADP'ye**bağlamak için.

    ![Tek oturum açma bağlantılı](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

1. **Oturum Aç'ı URL'sini Yapılandır'** bölümüne gidin, aşağıdaki adımları gerçekleştirin:

    ![Tek işaretli pervane](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)

    a. Yukarıdaki **özellikler sekmesinden** (ana ADP uygulamasından) kopyalamış olduğunuz **Kullanıcı erişim URL'sini**yapıştırın.
                                                             
    b. Aşağıda farklı **Röle Durum URL'lerini**destekleyen 5 uygulama yer alıyor. Belirli bir uygulama için uygun **Röle Durumu URL** değerini **Kullanıcı erişim URL'sine**el ile ekleniz gerekir.
    
    * **ADP İşgücü Şimdi**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP İşgücü Artık Zaman Artırıldı**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Kurumsal İk**

        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. Değişikliklerinizi **kaydedin.**

10. ADP temsilcinizden onay aldıktan sonra, bir veya iki kullanıcıyla teste başlayın.

    a. Federe erişimi test etmek için ADP hizmet uygulamasına birkaç kullanıcı atayın.

    b. Kullanıcılar galerideki ADP hizmet uygulamasına eriştiklerinde ve ADP hizmetlerine erişebiliyorsa test başarılı olur.
 
11. Başarılı bir testin onaylanması üzerine, federe ADP hizmetini tek tek kullanıcılara veya kullanıcı gruplarına atayın, bu hizmet daha sonra öğreticide açıklanır ve çalışanlarınıza dağıtın.

### <a name="create-adp-test-user"></a>ADP test kullanıcısı oluşturma

Bu bölümün amacı ADP'de B.Simon adında bir kullanıcı oluşturmaktır. Kullanıcıları ADP hesabına eklemek için [ADP destek ekibiyle](https://www.adp.com/contact-us/overview.aspx) birlikte çalışın. 

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki ADP döşemesini tıklattığınızda, SSO'yu kurduğunuz ADP'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ADP'yi deneyin](https://aad.portal.azure.com)
