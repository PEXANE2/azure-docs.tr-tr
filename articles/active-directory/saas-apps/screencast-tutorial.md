---
title: 'Öğretici: Screencast-O-Matic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Screencast-O-Matic arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc887e95b6fa6f8b17fbbb3dbaae5105385a07fa
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74132145"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-screencast-o-matic"></a>Öğretici: Screencast-O-Matic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Screencast-O-Matic'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Screencast-O-Matic'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Screencast-O-Matic erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Screencast-O-Matic'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Screencast-O-Matic tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Screencast-O-Matic **SP** başlatılan SSO destekler
* Screencast-O-Matic **Just In Time** kullanıcı sağlama destekler

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Galeriden Screencast-O-Matic ekleme

Screencast-O-Matic'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Screencast-O-Matic eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Screencast-O-Matic** yazın.
1. Sonuçlar panelinden **Screencast-O-Matic'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-screencast-o-matic"></a>Screencast-O-Matic için Azure AD oturumunu yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Screencast-O-Matic ile yapılandırın ve test edin. SSO'nun çalışması için, Screencast-O-Matic'teki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Screencast-O-Matic ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Screencast-O-Matic SSO'yu yapılandırır](#configure-screencast-o-matic-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Screencast-O-Matic test kullanıcısını oluşturun](#create-screencast-o-matic-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Screencast-O-Matic'te B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Screencast-O-Matic** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Screencast-O-Matic Client destek ekibine](mailto:support@screencast-o-matic.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Screencast-O-Matic'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Ekran Yayını-O-Matic'e erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Screencast-O-Matic'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-screencast-o-matic-sso"></a>Screencast-O-Matic SSO'nun yapılandırılması

1. Screencast-O-Matic içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Screencast-O-Matic'i** ayarla'yı tıklatın ve sizi Screencast-O-Matic uygulamasına yönlendirecektir. Buradan Screencast-O-Matic'te oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-11 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Screencast-O-Matic'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Screencast-O-Matic şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. **Abonelik'e**tıklayın.

    ![Abonelik](./media/screencast-tutorial/tutorial_screencast_sub.png)

1. **Access sayfası** bölümü altında **Kurulum'u**tıklatın.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_setup.png)

1. Kurulum **Erişimi Sayfasında**aşağıdaki adımları gerçekleştirin.

1. **Access URL** bölümünde, belirtilen metin kutusuna örnek adınızı yazın.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_access.png)

1. **SAML Kullanıcı Kısıtlaması (isteğe bağlı)** bölümü altında **Etki Alanı Kullanıcısını İste'yi** seçin.

1. **IDP Metadata XML Dosyasını Yükle**altında , Azure portalından indirdiğiniz meta verileri yüklemek için **Dosyayı Seçin'i** tıklatın.

1. **Tamam**'a tıklayın.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-screencast-o-matic-test-user"></a>Screencast-O-Matic test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Screencast-O-Matic oluşturulur. Screencast-O-Matic, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Screencast-O-Matic'te bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur. Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Screencast-O-Matic Client destek ekibine](mailto:support@screencast-o-matic.com)başvurun.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Screencast-O-Matic karounu tıklattığınızda, SSO'yu kurduğunuz Screencast-O-Matic'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Screencast-O-Matic'i deneyin](https://aad.portal.azure.com/)