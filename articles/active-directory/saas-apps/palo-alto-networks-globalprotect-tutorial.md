---
title: 'Öğretici: Palo Alto Networks ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - GlobalProtect | Microsoft Dokümanlar'
description: Azure Active Directory ve Palo Alto Networks - GlobalProtect arasında tek oturum açma işlemlerini nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 03bef6f2-3ea2-4eaa-a828-79c5f1346ce5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8faa1d0ecfec629487c9cddee6a01fb9f809393f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70166418"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-palo-alto-networks---globalprotect"></a>Öğretici: Palo Alto Networks ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - GlobalProtect

Bu eğitimde, Palo Alto Ağları - GlobalProtect ile Azure Active Directory (Azure AD) ile nasıl entegre edileceksiniz öğreneceksiniz. Palo Alto Networks - GlobalProtect ile Azure AD'yi entegre ettiğinizde şunları yapabilirsiniz:

* Palo Alto Networks erişimi olan Azure AD'de denetim - GlobalProtect.
* Kullanıcılarınızın Palo Alto Networks - GlobalProtect ile Azure AD hesaplarıyla otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Palo Alto Networks - GlobalProtect tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Palo Alto Networks - **GlobalProtect, SP** tarafından başlatılan SSO'ya destek veriyor
* Palo Alto Networks - GlobalProtect **Just In Time** kullanıcı sağlama destekler

## <a name="adding-palo-alto-networks---globalprotect-from-the-gallery"></a>Palo Alto Networks ekleme - GlobalProtect galeriden

Palo Alto Networks -GlobalProtect ile Azure AD entegrasyonuna yapılandırılan için, Galeriden Palo Alto Networks - GlobalProtect'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama **kutusuna Palo Alto Networks - GlobalProtect** yazın.
1. **Palo Alto Networks - GlobalProtect** sonuçları panelinden seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-palo-alto-networks---globalprotect"></a>Palo Alto Networks için Azure AD oturum açma işlemlerini yapılandırın ve test edin - GlobalProtect

Azure AD SSO'nu Palo Alto Networks ile yapılandırın ve test edin - **B.Simon**adlı bir test kullanıcısı kullanarak GlobalProtect . SSO'nun çalışması için, Palo Alto Networks - GlobalProtect'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Palo Alto Networks - GlobalProtect ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Palo Alto Networks -GlobalProtect SSO-](#configure-palo-alto-networks---globalprotect-sso)** uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. Palo Alto Networks'te B.Simon'ın bir muadili olan **[Palo Alto Networks -GlobalProtect'](#create-palo-alto-networks---globalprotect-test-user)** i oluşturun ve kullanıcının Azure AD gösterimine bağlıdır.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Palo Alto Networks - GlobalProtect** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<Customer Firewall URL>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<Customer Firewall URL>/SAML20/SP`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Palo Alto Networks - GlobalProtect Client destek ekibiyle](https://support.paloaltonetworks.com/support) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Palo Alto Networks - GlobalProtect** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Palo Alto Networks - GlobalProtect'e erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Palo Alto Networks - GlobalProtect'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-palo-alto-networks---globalprotect-sso"></a>Palo Alto Ağlarını Yapılandırın - GlobalProtect SSO

1. Palo Alto Networks Güvenlik Duvarı Yönetici UI'sini başka bir tarayıcı penceresinde yönetici olarak açın.

2. **Cihaz'a**tıklayın.

    ![Palo Alto Tek Oturum Açma'yı Yapılandır](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Sol daki gezinti çubuğundan **SAML Identity Provider'ı** seçin ve meta veri dosyasını almak için "İçe Aktar"ı tıklatın.

    ![Palo Alto Tek Oturum Açma'yı Yapılandır](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin2.png)

4. İçe Aktarma penceresinde aşağıdaki eylemleri gerçekleştirin

    ![Palo Alto Tek Oturum Açma'yı Yapılandır](./media/paloaltoglobalprotect-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. Profil **Adı** metin kutusunda, Azure AD GlobalProtect gibi bir ad sağlayın.

    b. **Kimlik Sağlayıcı Metadata'da,** Azure portalından indirdiğiniz metadata.xml dosyasına **Gözat'ı** tıklayın ve

    c. **Tamam'ı** tıklatın

### <a name="create-palo-alto-networks---globalprotect-test-user"></a>Palo Alto Networks oluştur - GlobalProtect test kullanıcısı

Bu bölümde, B.Simon adlı bir kullanıcı Palo Alto Networks - GlobalProtect oluşturulur. Palo Alto Networks - GlobalProtect, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Palo Alto Networks - GlobalProtect'de zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki Palo Alto Networks - GlobalProtect döşemesini tıklattığınızda, SSO'yu kurduğunuz Palo Alto Networks - GlobalProtect ile otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Palo Alto Networks'ü deneyin - Azure AD ile GlobalProtect](https://aad.portal.azure.com/)