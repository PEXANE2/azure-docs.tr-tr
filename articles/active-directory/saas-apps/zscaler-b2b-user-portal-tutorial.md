---
title: 'Öğretici: Zscaler B2B Kullanıcı Portalı ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler B2B Kullanıcı Portalı arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 75b90449-095d-4e47-a1fd-8b5cbbfe7e3d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/24/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c28af94c92fa56d5217aee91053521d2c12b0aef
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668933"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zscaler-b2b-user-portal"></a>Öğretici: Zscaler B2B Kullanıcı Portalı ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Zscaler B2B Kullanıcı Portalı'nı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Zscaler B2B Kullanıcı Portalı'nı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Zscaler B2B Kullanıcı Portalı'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler B2B Kullanıcı Portalı'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler B2B Kullanıcı Portalı tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Zscaler B2B Kullanıcı Portalı **IDP** başlatılan SSO destekler

* Zscaler B2B Kullanıcı Portalı **Just In Time** kullanıcı sağlama destekler

* Zscaler B2B Kullanıcı Portalı'nı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-zscaler-b2b-user-portal-from-the-gallery"></a>Galeriden Zscaler B2B Kullanıcı Portalı Ekleme

Zscaler B2B Kullanıcı Portalı'nın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Zscaler B2B Kullanıcı Portalı eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Zscaler B2B Kullanıcı Portalı** yazın.
1. Sonuç panelinden **Zscaler B2B Kullanıcı Portalı'nı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zscaler-b2b-user-portal"></a>Zscaler B2B Kullanıcı Portalı için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu Zscaler B2B Kullanıcı Portalı ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Zscaler B2B Kullanıcı Portalı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Zscaler B2B Kullanıcı Portalı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Zscaler B2B Kullanıcı Portalı SSO'yu uygulama](#configure-zscaler-b2b-user-portal-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. **[Zscaler B2B Kullanıcı Portalı test kullanıcısını oluşturun](#create-zscaler-b2b-user-portal-test-user)** - Zscaler B2B Kullanıcı Portalı'nda kullanıcının Azure AD gösterimine bağlı B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Zscaler B2B Kullanıcı Portalı** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://samlsp.private.zscaler.com/auth/metadata/<UniqueID>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://samlsp.private.zscaler.com/auth/login?domain=EXAMPLE`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Zscaler B2B Kullanıcı Portalı İstemci destek ekibine](https://help.zscaler.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Zscaler B2B Kullanıcı Portalı** Oluşturma bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Zscaler B2B Kullanıcı Portalı'na erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Zscaler B2B Kullanıcı Portalı'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-zscaler-b2b-user-portal-sso"></a>Zscaler B2B Kullanıcı Portalı SSO'nun yapılandırılması

1. Yeni bir web tarayıcısı penceresi açın ve Zscaler B2B Kullanıcı Portalı şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Menünün sol tarafından **İdare'yi** tıklatın ve **Kimlik Doğrulama** bölümüne gidin **IdP Yapılandırması'nı**tıklatın.

    ![Zscaler Özel Erişim Yöneticisi yönetimi](./media/zscaler-b2b-user-tutorial/tutorial-zscaler-private-access-administration.png)

1. Sağ üst **köşede, IdP Yapılandırması Ekle'yi**tıklatın. 

    ![Zscaler Özel Erişim Yöneticisi iDP](./media/zscaler-b2b-user-tutorial/tutorial-zscaler-private-access-idp.png)

1. **IdP Yapılandırma ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler Özel Erişim Yöneticisi seçin](./media/zscaler-b2b-user-tutorial/tutorial-zscaler-private-access-select.png)

    a. **IdP Meta data File Upload** alanında Azure AD'den indirilen Meta veri dosyasını yüklemek için **Dosyayı Seç'i** tıklatın.

    b. Azure AD'deki **IdP meta verilerini** okur ve aşağıda gösterildiği gibi tüm alan bilgilerini doldurur.

    ![Zscaler Özel Erişim Yöneticisi config](./media/zscaler-b2b-user-tutorial/config.png)

    c. **Etki Alanları** alanından etki alanınızı seçin.
    
    d. **Kaydet**'e tıklayın.

### <a name="create-zscaler-b2b-user-portal-test-user"></a>Zscaler B2B Kullanıcı Portalı test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Zscaler B2B Kullanıcı Portalı oluşturulur. Zscaler B2B Kullanıcı Portalı, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Zscaler B2B Kullanıcı Portalı'nda bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zscaler B2B Kullanıcı Portalı döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler B2B Kullanıcı Portalı'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Zscaler B2B Kullanıcı Portalı'nı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)