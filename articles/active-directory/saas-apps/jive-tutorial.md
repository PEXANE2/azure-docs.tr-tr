---
title: 'Öğretici: Jive ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Jive arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ccdab373be4bab876ef52ba478076b6a8b6e0845
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76291184"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jive"></a>Öğretici: Jive ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Jive'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Jive'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Jive erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Jive'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Jive tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Jive **SP** başlatılan SSO destekler
* Jive [ **Otomatik** kullanıcı sağlama destekler](jive-provisioning-tutorial.md)
* Jive'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-jive-from-the-gallery"></a>Galeriden Jive ekleme

Jive'in Azure AD'ye entegrasyonunu yapılandırmak için, Jive'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Jive** yazın.
1. Sonuçlar panelinden **Jive'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-jive"></a>Jive için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Jive ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Jive'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Jive ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Jive SSO'yu yapılandırın](#configure-jive-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Jive test kullanıcıoluşturun](#create-jive-test-user)** - Kullanıcının Azure AD gösterimine bağlı Jive B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Jive** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<instance name>.jivecustom.com`

    b. In the **Identifier (Entity ID)** text box, type a URL using the following pattern:
    `https://<instance name>.jiveon.com`

    > [!NOTE]
    > These values are not real. Update these values with the actual Sign on URL and Identifier. Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) to get these values. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Jive'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, B.Simon'ın Jive'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Jive'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-jive-sso"></a>Jive SSO'yu yapılandır

1. **Jive** tarafında tek oturum açma yapılandırmak için, yönetici olarak Jive kiracınızı oturum açın.

1. Üstteki menüde **SAML'yi**tıklatın.

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/jive-tutorial/tutorial_jive_002.png)

    a. **Genel** sekmesi altında **Etkin'i** seçin.

    b. TÜM **SAML AYARLARINI KAYDET** düğmesini tıklatın.

1. **IDP METADATA** sekmesine gidin.

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/jive-tutorial/tutorial_jive_003.png)

    a. İndirilen meta veri XML dosyasının içeriğini kopyalayın ve kimlik **sağlayıcısına (IDP) Metadata** textbox'a yapıştırın.

    b. TÜM **SAML AYARLARINI KAYDET** düğmesini tıklatın.

1. **KULLANICI ÖZNITELIK EŞLEME** sekmesini seçin.

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/jive-tutorial/tutorial_jive_004.png)

    a. **E-posta** metin kutusunda, **posta** değerinin öznitelik adını kopyalayıp yapıştırın.

    b. Ad **metin** kutusunda, **verilen ad** değerinin öznitelik adını kopyalayıp yapıştırın.

    c. **Soyadı** metin kutusunda, **soyadı** değerinin öznitelik adını kopyalayıp yapıştırın.

### <a name="create-jive-test-user"></a>Jive test kullanıcısı oluşturma

Bu bölümün amacı Jive Britta Simon adlı bir kullanıcı oluşturmaktır. Jive varsayılan olarak etkin olan otomatik kullanıcı sağlamayı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](jive-provisioning-tutorial.md) fazla bilgi bulabilirsiniz.

Kullanıcıyı el ile oluşturmanız gerekiyorsa, kullanıcıları Jive platformuna eklemek için [Jive Client destek ekibiyle](https://www.jivesoftware.com/services-support/) birlikte çalışın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Jive döşemesini tıklattığınızda, SSO'yu kurduğunuz Jive'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Jive'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Kullanıcı Sağlama'yı Yapılandır](jive-provisioning-tutorial.md)

- [Jive'i gelişmiş görünürlük ve kontrollerle nasıl koruyabilen](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
