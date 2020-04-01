---
title: 'Öğretici: Renklere Göre İzle ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Watch by Colors arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 586a029c-fb8d-4233-b280-103b9ba7102d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6d8fdc8ba2337a3be49a4645e48a45120fb0ccec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026098"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-watch-by-colors"></a>Öğretici: Renklere Göre İzle ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Renklere Göre İzle'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Renklere Göre İzle'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Renklere Göre İzle'ye erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Renklere Göre İzleme kullanabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Renklere göre tek oturum açma (SSO) özellikli aboneliği izleyin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Watch by **Colors, SP ve IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-watch-by-colors-from-the-gallery"></a>Galeriden Renklere Göre İzle Ekleme

Watch by Colors'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Renklere Göre İzle'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Renklere Göre İzle** yazın.
1. Sonuçlar panelinden **Renklere Göre İzle'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-watch-by-colors"></a>Azure AD oturumaçmayı Renklere Göre İzle'ye göre yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Renklere Göre İzle ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Renklere Göre İzle'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Renklere Göre İzle ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[İzlemeyi Renkler SSO'ya göre yapılandırın.](#configure-watch-by-colors-sso)**
    1. **[Renklere Göre İzle testi kullanıcısı oluşturun](#create-watch-by-colors-test-user)** - kullanıcının Azure AD gösterimine bağlı Renklere Göre İzle'deki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Renklere Göre İzle** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.colorscorporation.com/login`

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın Renklere Göre İzle'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Renklere Göre İzle'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-watch-by-colors-sso"></a>İzle Renklere Göre YapılandırSın SSO

1. Renklere Göre İzle içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, Renklere Göre **İzle kurulumuna** tıklayın ve sizi Renklere Göre İzle uygulamasına yönlendirecektir. Buradan, Renklere Göre İzle'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Renklere göre izle'yi el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Watch by Colors şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst köşesinde **profil** > **Hesap Ayarları** > **SSO (Tek Oturum Açma)** tıklayın.

    ![Renklere göre izlemeyi izleyin yapılandırması](./media/watch-by-colors-tutorial/config01.png)

5. **SSO (Tek Oturum Açma)** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Renklere göre izlemeyi izleyin yapılandırması](./media/watch-by-colors-tutorial/config02.png)

    a. **SAML'yi** **AÇIYA**İDEE'yi geçiş e.

    b. **URL** metin kutusuna, Azure portalından kopyaladığınız **Federasyon Metaveri Url'sini**yapıştırın.

    c. **İçe Aktar'ı**tıklatın, ardından aşağıdaki alanlar sayfada otomatik olarak doldurulur.

    d. **Kaydet**'e tıklayın.

### <a name="create-watch-by-colors-test-user"></a>Renklere Göre İzle test kullanıcısı oluştur

Azure AD kullanıcılarının Renklere Göre İzle'de oturum açabilmeleri için Renklere Göre İzle'ye göre etkinleştirilmeleri gerekir. Renklere Göre İzle'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi Olarak Renklere Göre İzlemek için oturum açın.

1. Sayfanın sağ üst köşesinde, kullanıcı**Users** > **ekle** **profiline** > tıklayın.

    ![Renklere göre izlemeyi izleyin yapılandırması](./media/watch-by-colors-tutorial/config03.png)

1. Kullanıcı **Ayrıntıları** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Renklere göre izlemeyi izleyin yapılandırması](./media/watch-by-colors-tutorial/config04.png)

    a. **Ad metin** kutusuna, **B**gibi kullanıcının ilk adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    c. **E-posta** metin kutusuna, gibi `B.Simon@contoso.com`kullanıcının e-posta girin.

    d. **Parola** metin kutusuna parolayı girin.

    e. Kuruluşunuza göre **Hesap İzinleri'ni** seçin.

    f. **Kaydet**'e tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Renklere Göre İzle'yi tıklattığınızda, SSO'yu kurduğunuz Renklere Göre İzle'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Renklere Göre İzle'yi deneyin](https://aad.portal.azure.com/)

