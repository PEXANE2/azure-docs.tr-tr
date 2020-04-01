---
title: 'Öğretici: ThousandEyes ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ThousandEyes arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd179049f88454c62244cf1819cee08ef78d0633
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373247"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-thousandeyes"></a>Öğretici: ThousandEyes ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, ThousandEyes'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. ThousandEyes'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* BinEyes erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ThousandEyes'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ThousandEyes tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* ThousandEyes **SP ve IDP** SSO başlatılan destekler
* ThousandEyes [ **Otomatik** kullanıcı sağlama destekler](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-thousandeyes-from-the-gallery"></a>Galeriden ThousandEyes ekleme

ThousandEyes'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden BinEyes'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ThousandEyes** yazın.
1. Sonuçlar panelinden **ThousandEyes'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-thousandeyes"></a>ThousandEyes için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak ThousandEyes ile yapılandırın ve test edin. SSO'nun çalışması için, ThousandEyes'te bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ThousandEyes ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[BinEyes SSO'yu yapılandırın](#configure-thousandeyes-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[ThousandEyes test kullanıcısını oluşturun](#create-thousandeyes-test-user)** - Kullanıcının Azure AD gösterimine bağlı ThousandEyes'te B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **ThousandEyes** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.thousandeyes.com/login/sso`

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **ThousandEyes'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, BinEyes'e erişim izni vererek B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **ThousandEyes'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-thousandeyes-sso"></a>BinEyes SSO'ya yapıla

1. Farklı bir web tarayıcısı penceresinde, **BinEyes** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde **Ayarlar'ı**tıklatın.

    ![Ayarlar](./media/thousandeyes-tutorial/ic790066.png "Ayarlar")

3. **Hesap'ı** tıklatın

    ![Hesap](./media/thousandeyes-tutorial/ic790067.png "Hesap")

4. Güvenlik **& Kimlik Doğrulama** sekmesini tıklatın.

    ![Güvenlik & Kimlik Doğrulama](./media/thousandeyes-tutorial/ic790068.png "Güvenlik & Kimlik Doğrulama")

5. Kurulum **Tek Oturum Açma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kurulum Tek Oturum Açma](./media/thousandeyes-tutorial/ic790069.png "Kurulum Tek Oturum Açma")

    a. **Tek Oturum Aç'ı Etkinleştir'i**seçin.

    b. **Giriş Sayfası URL** textbox'ına, Azure portalından kopyaladığınız Giriş **URL'sini**yapıştırın.

    c. **Giriş Sayfası URL** textbox'ına, Azure portalından kopyaladığınız **Logout URL'sini**yapıştırın.

    d. **Kimlik Sağlayıcı Veren** textbox, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nı**yapıştırın.

    e. **Doğrulama Sertifikası'nda** **Dosyayı Seç'i**tıklatın ve ardından Azure portalından indirdiğiniz sertifikayı yükleyin.

    f. **Kaydet**'e tıklayın.

### <a name="create-thousandeyes-test-user"></a>ThousandEyes test kullanıcısı oluşturma

Bu bölümün amacı ThousandEyes Britta Simon adlı bir kullanıcı oluşturmaktır. ThousandEyes varsayılan olarak etkin olan otomatik kullanıcı sağlama yı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](thousandeyes-provisioning-tutorial.md) fazla bilgi bulabilirsiniz.

**Kullanım kılavuzunu el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:**

1. BinEyes şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar**'a tıklayın.

    ![Ayarlar](./media/thousandeyes-tutorial/IC790066.png "Ayarlar")

3. **Hesap'ı**tıklatın.

    ![Hesap](./media/thousandeyes-tutorial/IC790067.png "Hesap")

4. Kullanıcılar **& Hesaplar** sekmesini tıklatın.

    ![Hesaplar & Kullanıcılar](./media/thousandeyes-tutorial/IC790073.png "Hesaplar & Kullanıcılar")

5. Kullanıcı **& Hesapları Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Hesapları Ekle](./media/thousandeyes-tutorial/IC790074.png "Kullanıcı Hesapları Ekle")

    a. **Ad** metin kutusuna, **B.Simon**gibi kullanıcının adını yazın.

    b. **E-posta** textbox olarak, gibi b.simon@contoso.comkullanıcının e-posta yazın.

    b. **Hesaba Yeni Kullanıcı Ekle'yi**tıklatın.

    > [!NOTE]
    > Azure Active Directory hesap sahibi, hesabı onaylamak ve etkinleştirmek için bir bağlantı içeren bir e-posta alır.

> [!NOTE]
> Azure Active Directory kullanıcı hesaplarını sağlamak için ThousandEyes tarafından sağlanan diğer ThousandEyes kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.


## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki ThousandEyes döşemesini tıklattığınızda, SSO'yu kurduğunuz ThousandEyes'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ThousandEyes'i deneyin](https://aad.portal.azure.com/)

- [Kullanıcı Sağlama'yı Yapılandır](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)