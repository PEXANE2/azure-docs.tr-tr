---
title: 'Öğretici: Müşteri için SAP Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Müşteri için Azure Active Directory ve SAP Cloud arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 837787d375a7570b7daf0a149960ca0020bcdced
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72264028"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-cloud-for-customer"></a>Öğretici: Müşteri için SAP Cloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Müşteri için SAP Bulutu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz. Müşteri için SAP Cloud'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Müşteri için SAP Bulutu'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Müşteri için SAP Cloud'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Müşteri için SAP Cloud tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Müşteri için SAP **Cloud, SP** tarafından başlatılan SSO'ya destek

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Galeriden Müşteriye SAP Bulutu Ekleme

MÜŞTERI için SAP Bulutunun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Müşteri için SAP Bulutu'nu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Müşteri için SAP Bulutu** yazın.
1. Sonuç panelinden **Müşteri için SAP Bulut'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-cloud-for-customer"></a>Müşteri için SAP Bulutu için Azure AD oturum açma işlemlerini yapılandırma ve test edin

**B.Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO'nu Müşteri için SAP Bulutu ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile Müşteri için SAP Bulutu'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Müşteri için SAP Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Sap Cloud'u Müşteri SSO'su için yapılandırın](#configure-sap-cloud-for-customer-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Müşteri test kullanıcısı için SAP Bulutu oluşturun](#create-sap-cloud-for-customer-test-user)** - kullanıcının Azure AD gösterimine bağlı müşteri için SAP Cloud'da B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Müşteri için SAP Bulutu** [portalında bulunan Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server name>.crm.ondemand.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server name>.crm.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak [için Müşteri İstemci destek ekibi için SAP Bulut'a](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Müşteri için SAP Bulutu uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. **Kullanıcı Öznitelikleri & Talepler** iletişim kutusundaki Kullanıcı **Öznitelikleri** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için **Edit simgesini** tıklatın.

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermail.png)

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermailedit.png)

    b. **Kaynak**olarak **Dönüşüm'u** seçin.

    c. **Dönüşüm** listesinden **ExtractMailPrefix()** seçeneğini belirleyin.

    d. **Parametre 1** listesinden, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin.
    Örneğin, EmployeeID'yi benzersiz kullanıcı tanımlayıcısı olarak kullanmak istiyorsanız ve Öznitelik değerini ExtensionAttribute2'de depolamışsanız, user.extensionattribute2'yi seçin.

    e. **Kaydet**'e tıklayın.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Müşteri **için SAP Bulutu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Müşteri için SAP Bulut'a erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar **listesinde, Müşteri için SAP Bulut'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sap-cloud-for-customer-sso"></a>Müşteri SSO için SAP Bulutu'nun yapılandırılması

1. Yeni bir web tarayıcısı penceresi açın ve müşteri şirket sitesi için SAP Cloud'da yönetici olarak oturum açın.

2. Menünün sol tarafından, **Kimlik Sağlayıcıları** > **Kurumsal Kimlik Sağlayıcıları** > **Ekle'ye** tıklayın ve açılan pencerede **Azure AD**gibi Kimlik sağlayıcı adını ekleyin , **Kaydet'i** tıklatın ve **ardından SAML 2.0 Yapılandırmasını**tıklatın.

    ![SAP yapılandırması](./media/sap-customer-cloud-tutorial/configure01.png)

3. **SAML 2.0 Yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAP yapılandırması](./media/sap-customer-cloud-tutorial/configure02.png)

    a. Azure portalından indirdiğiniz Federasyon Metadata XML dosyasını yüklemek için **Gözat'ı** tıklatın.

    b. XML dosyası başarıyla yüklendikten sonra, aşağıdaki değerler otomatik olarak doldurulur ve **kaydet'i**tıklatın.

### <a name="create-sap-cloud-for-customer-test-user"></a>Müşteri test kullanıcısı için SAP Bulutu oluşturma

Azure AD kullanıcılarının Müşteri için SAP Cloud'da oturum açmalarını sağlamak için, müşteri için SAP Bulutu'nda oturum açmaları gerekir. Müşteri için SAP Bulut'ta, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Müşteri için Bir Güvenlik Yöneticisi olarak SAP Cloud'da oturum açın.

2. Menünün sol **tarafından, Kullanıcı & Yetkileri** > **Kullanıcı Yönetimi** > **Kullanıcı Ekle Kullanıcı'ya**tıklayın.

    ![SAP yapılandırması](./media/sap-customer-cloud-tutorial/configure03.png)

3. Yeni **Kullanıcı Ekle** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAP yapılandırması](./media/sap-customer-cloud-tutorial/configure04.png)

    a. Ad **metin** kutusuna, **B**gibi kullanıcının adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının adını girin.

    c. **E-Posta** metin kutusuna, kullanıcının `B.Simon@contoso.com`e-postasını girin.

    d. Giriş **Adı** metin kutusuna, **B.Simon**gibi kullanıcının adını girin.

    e. Gereksiniminize göre **Kullanıcı Türü'nü** seçin.

    f. İhtiyaçlarınıza göre **Hesap Etkinleştirme** seçeneğini seçin.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Müşteri için SAP Bulutu'nu tıklattığınızda, SSO'yu kurduğunuz Müşteri için SAP Bulutu'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Müşteri için SAP Bulutu'nun deneyin](https://aad.portal.azure.com/)

