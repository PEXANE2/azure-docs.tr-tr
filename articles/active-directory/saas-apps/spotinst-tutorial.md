---
title: 'Öğretici: Spotinst ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Spotinst arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2f6dbd70-c2db-4ae9-99ee-976c3090d214
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5490ff6c6143dff258d74e013bb9d4c821aab625
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76263294"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-spotinst"></a>Öğretici: Spotinst ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Spotinst'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Spotinst'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Spotinst erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Spotinst'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Spotinst tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Spotinst **SP ve IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-spotinst-from-the-gallery"></a>Galeriden Spotinst ekleme

Spotinst'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Spotinst'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Spotinst** yazın.
1. Sonuç panelinden **Spotinst'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-spotinst"></a>Spotinst için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Spotinst ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Spotinst'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Spotinst ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Spotinst SSO'yu yapılandırın](#configure-spotinst-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Spotinst test kullanıcısını oluşturun](#create-spotinst-test-user)** - Spotinst'te B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Spotinst** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    a. **Ek URL'leri Ayarla'yı kontrol edin.**

    b. **Röle Durumu** metin kutusuna bir değer yazın:`<ID>`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımları gerçekleştirin:

    Oturum **Açma URL** metin kutusuna URL'yi yazın:`https://console.spotinst.com/auth/saml`

    > [!NOTE]
    > Röle Durumu değeri gerçek değil. Röle Durumu değerini, daha sonra öğreticide açıklanan gerçek Röle Durumu değeriyle güncelleştireceksiniz.

1. **Kaydet**'e tıklayın.

1. Spotinst uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Spotinst uygulaması, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | -----| --------------- |
    | Email | kullanıcı.posta |
    | FirstName | user.givenname |
    | LastName | user.surname |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Spotinst'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Spotinst'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Spotinst'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-spotinst-sso"></a>Spotinst SSO'nun yapılandırılsın

1. Farklı bir web tarayıcısı penceresinde, Spotinst'te Güvenlik Yöneticisi olarak oturum açın.

2. Ekranın sağ üst tarafındaki **kullanıcı simgesine** tıklayın ve **Ayarlar'ı**tıklatın.

    ![Spotinst ayarları](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

3. Üstteki **GÜVENLİk** sekmesine tıklayın ve ardından **Kimlik Sağlayıcıları'nı** seçin ve aşağıdaki adımları gerçekleştirin:

    ![Spotinst güvenlik](./media/spotinst-tutorial/tutorial_spotinst_security.png)

    a. Örneğiniz için **Röle Durumu** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma** bölümündeki Relay **State** textbox'a yapıştırın.

    b. Azure portalından indirdiğiniz meta data xml dosyasını yüklemek için **BROWSE'yi** tıklatın

    c. **KAYDET'i**tıklatın.

### <a name="create-spotinst-test-user"></a>Spotinst test kullanıcısı oluşturma

Bu bölümün amacı Spotinst Britta Simon adlı bir kullanıcı oluşturmaktır.

1. Uygulamayı **SP** başlatılan modda yapılandırmışsanız, aşağıdaki adımları gerçekleştirin:

   a. Farklı bir web tarayıcısı penceresinde, Spotinst'te Güvenlik Yöneticisi olarak oturum açın.

   b. Ekranın sağ üst tarafındaki **kullanıcı simgesine** tıklayın ve **Ayarlar'ı**tıklatın.

    ![Spotinst ayarları](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

    c. **Kullanıcılar'ı** tıklatın ve **KULLANICI EKLE'yi**seçin.

    ![Spotinst ayarları](./media/spotinst-tutorial/adduser1.png)

    d. Kullanıcı ekle bölümünde aşağıdaki adımları gerçekleştirin:

    ![Spotinst ayarları](./media/spotinst-tutorial/adduser2.png)

    * Tam **Ad** textbox, **BrittaSimon**gibi kullanıcının tam adını girin.

    * **E-posta** metin kutusuna, kullanıcının e-posta adresini girin. `brittasimon\@contoso.com`

    * **Kuruluş Rolü, Hesap Rolü ve Hesaplar**için kuruluşa özel ayrıntılarıseçin.

2. **Uygulamayı IDP** başlatılan modda yapılandırıldıysanız, bu bölümde sizin için bir eylem öğesi yoktur. Spotinst, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Spotinst'e henüz var değilse erişme girişimi sırasında yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Spotinst döşemesini tıklattığınızda, SSO'yu kurduğunuz Spotinst'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Spotinst'i deneyin](https://aad.portal.azure.com/)

