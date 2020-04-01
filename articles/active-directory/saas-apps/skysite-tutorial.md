---
title: 'Öğretici: SKYSITE ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SKYSITE arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 04c6a47a-1730-4acf-bc5c-a04daccff9b3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93ef7f4201e9613cc6fa4391bc28d257272fa1c7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026119"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-skysite"></a>Öğretici: SKYSITE ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SKYSITE'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SKYSITE'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SKYSITE erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla SKYSITE'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SKYSITE tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SKYSITE **IDP'nin** başlattığı SSO'ya destek verdi

* SKYSITE **Just In Time** kullanıcı sağlama destekler

## <a name="adding-skysite-from-the-gallery"></a>Galeriden SKYSITE ekleme

SKYSITE'in Azure AD'ye entegrasyonunu yapılandırmak için, SKYSITE'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SKYSITE** yazın.
1. Sonuçlar panelinden **SKYSITE'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-skysite"></a>SKYSITE için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak SKYSITE ile yapılandırın ve test edin. SSO'nun çalışması için, SKYSITE'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SKYSITE ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[SkySITE SSO'yu yapılandırın](#configure-skysite-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[SKYSITE test kullanıcısını oluşturun](#create-skysite-test-user)** - SkySITE'de Kullanıcının Azure AD gösterimine bağlı b.simon'Un bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **SKYSITE** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Özellikler sekmesine** tıklayın ve aşağıdaki adımı gerçekleştirin: 

    ![Tek oturum açma özellikleri](./media/skysite-tutorial/config05.png)

    * Kullanıcı **erişim URL'sini** kopyalayın ve daha sonra öğreticide açıklanan **SKYSITE SSO'yu Yapılandır'a**yapıştırmak zorunda sınız.

1. **SKYSITE** uygulama tümleştirme sayfasında, **tek oturum açma**sayfasına gidin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. SKYSITE uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. Yukarıda ek olarak, SKYSITE uygulaması SAML yanıtı geri geçirilecek birkaç daha fazla öznitelikleri bekliyor. **Grup Talepleri (Önizleme)** iletişim kutusundaki **Kullanıcı Öznitelikleri & Talepler** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Talepte döndürülen Gruplar'ın**yanındaki **kalemi** tıklatın.

    ![image](./media/skysite-tutorial/config01.png)

    ![image](./media/skysite-tutorial/config02.png)

    b. Radyo listesinden **Tüm Gruplar'ı** seçin.

    c. **Grup Kimliğinin**Kaynak **Özniteliği'ni** seçin.

    d. **Kaydet**'e tıklayın.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SKYSITE'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, SkySITE'e erişim izni vererek B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SKYSITE'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="configure-skysite-sso"></a>SKYSITE SSO'nun yapılandırılSın

1. Yeni bir web tarayıcısı penceresi açın ve SKYSITE şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst tarafındaki **Ayarlar'a** tıklayın ve ardından **Hesap ayarına**gidin.

    ![Yapılandırma](./media/skysite-tutorial/config03.png)

5. Tek **oturum (SSO)** sekmesine geçin, aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma](./media/skysite-tutorial/config04.png)

    a. URL metin kutusunda **ki Kimlik Sağlayıcı oturum açın,** Azure portalındaki **özellikler** sekmesinden kopyalamış olduğunuz Kullanıcı **erişim URL'sinin**değerini yapıştırın.

    b. Azure portalından indirdiğiniz Base64 kodlu sertifikayı yüklemek için **Sertifika yükle'yi**tıklatın.

    c. **Kaydet**'e tıklayın.

### <a name="create-skysite-test-user"></a>SKYSITE test kullanıcısı oluşturma

Bu bölümde, SkySITE'de Britta Simon adında bir kullanıcı oluşturulur. SKYSITE, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. SkySITE'de bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SKYSITE döşemesini tıklattığınızda, SSO'yu kurduğunuz SKYSITE'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SKYSITE'yi deneyin](https://aad.portal.azure.com/)

