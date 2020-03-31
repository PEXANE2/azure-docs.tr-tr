---
title: 'Öğretici: Saba TalentSpace ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Saba TalentSpace arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/20/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76c13a649e1c1888a11e02b83d969255615cdc67
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77561398"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-saba-talentspace"></a>Öğretici: Saba TalentSpace ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Saba TalentSpace'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Saba TalentSpace'i Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Saba TalentSpace erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Saba TalentSpace'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Saba TalentSpace tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Saba TalentSpace **SP** başlatılan SSO destekler
* Saba TalentSpace'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-saba-talentspace-from-the-gallery"></a>Galeriden Saba TalentSpace ekleme

Saba TalentSpace'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Saba TalentSpace'i eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Saba TalentSpace** yazın.
1. Sonuç panelinden **Saba TalentSpace'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-saba-talentspace"></a>Saba TalentSpace için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu Saba TalentSpace ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Saba TalentSpace'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Saba TalentSpace ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Saba TalentSpace SSO'yu yapılandırın](#configure-saba-talentspace-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Saba TalentSpace test kullanıcısını oluşturun](#create-saba-talentspace-test-user)** - B.Simon'ın Saba TalentSpace'deki bir muadili kullanıcının Azure AD temsiline bağlı olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Saba TalentSpace** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/login`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/metadata`

    c. **YanıtLA URL'sine (Tüketici Hizmeti URL'sini İddia Et)** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://global.hgncloud.com/[companyname]/saml/SSO`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Saba TalentSpace Müşteri destek ekibiyle](https://support.saba.com/) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Saba **TalentSpace'i Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Saba TalentSpace'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **Saba TalentSpace'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-saba-talentspace-sso"></a>Saba TalentSpace SSO'ya yapı

1. Farklı bir tarayıcı penceresinde, yönetici olarak **Saba TalentSpace** uygulamanızı oturum açın.

2. **Seçenekler** sekmesini tıklatın.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-12.png)

3. Sol gezinti bölmesinde **SAML Configuration'ı**tıklatın.
  
    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-13.png)

4. **SAML Yapılandırma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-14.png)

    a. **Benzersiz Tanımlayıcı**olarak **NameID'yi**seçin.

    b. **Benzersiz Tanımlayıcı Haritalar Olarak ,** Kullanıcı **Adı**seçin.
  
    c. İndirdiğiniz meta veri dosyanızı yüklemek için dosyayı seçmek için **Gözat'ı** tıklatın ve ardından **Dosyayı Yükle'yi tıklatın.**

    d. Yapılandırmayı test etmek için **Testi Çalıştır'ı**tıklatın.

    > [!NOTE]
    > "SAML testi tamamlandı" iletisini beklemeniz*gerekir. Lütfen bu pencereyi kapatın"* Ardından, açılan tarayıcı penceresini kapatın. **SAML'yi etkinleştir** onay kutusu yalnızca test tamamlandığında etkinleştirilir.

    e. **SAML'yi Etkinleştir'i**seçin.

    f. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-saba-talentspace-test-user"></a>Saba TalentSpace test kullanıcıoluşturun

Bu bölümün amacı Saba TalentSpace Britta Simon adlı bir kullanıcı oluşturmaktır.

**Saba TalentSpace'de Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. **Saba TalentSpace** uygulamanızda yönetici olarak oturum açın.

2. Kullanıcı **Merkezi** sekmesini tıklatın ve ardından **Kullanıcı Oluştur'u**tıklatın.

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-300.png)  

3. Yeni **Kullanıcı** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Azure AD Connect nedir?](./media/halogen-software-tutorial/tutorial-halogen-301.png)

    a. Ad **metin** kutusunda, **kullanıcının**ilk adını B gibi yazın.

    b. **Soyadı** metin kutusunda, **Simon**gibi kullanıcının soyadını yazın.

    c. Kullanıcı **adı** metin kutusunda, Azure portalındaki kullanıcı adı olan **B.Simon**yazın.

    d. **Parola** metin kutusuna, B.Simon için bir parola yazın.

    e. **Kaydet**'e tıklayın.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Saba TalentSpace döşemesini tıklattığınızda, SSO'yu kurduğunuz Saba TalentSpace'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Saba TalentSpace'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)