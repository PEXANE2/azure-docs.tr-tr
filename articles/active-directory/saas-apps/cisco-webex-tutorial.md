---
title: 'Öğretici: Cisco Webex Toplantıları ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Cisco Webex Toplantıları arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4e497c556bde1be4e498cd85a68282a0e3b2666
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026257"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-webex-meetings"></a>Öğretici: Cisco Webex Toplantıları ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Cisco Webex Toplantılarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Cisco Webex Toplantılarını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Cisco Webex Toplantıları'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Cisco Webex Toplantıları'nda otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Cisco Webex Meetings tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Cisco Webex **Meetings, SP ve IDP'nin** başlattığı SSO'yu destekliyor

* Cisco Webex Meetings **Just In Time** kullanıcı sağlama destekler

## <a name="adding-cisco-webex-meetings-from-the-gallery"></a>Galeriden Cisco Webex Toplantıları Ekleme

Cisco Webex Toplantıları'nın Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Cisco Webex Toplantıları eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Cisco Webex Toplantıları** yazın.
1. Sonuç panelinden **Cisco Webex Toplantıları'nı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-webex-meetings"></a>Cisco Webex Toplantıları için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Cisco Webex Toplantıları ile yapılandırın ve test edin. SSO'nun çalışması için, Cisco Webex Toplantıları'nda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Cisco Webex Toplantıları ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[Cisco Webex Meetings SSO'yu yapılandırır](#configure-cisco-webex-meetings-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Cisco Webex Meetings test kullanıcısını oluşturun](#create-cisco-webex-meetings-test-user)** - Cisco Webex Meetings'de b.simon'ın bir muadili olması için kullanıcının Azure AD gösterimine bağlı.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Cisco Webex Toplantıları** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML ile Tek Oturum** Açma'da, **Servis Sağlayıcı meta veri** dosyasını aşağıdaki gibi yükleyerek uygulamayı **IDP'nin** başlattığı modda yapılandırabilirsiniz:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    c. Hizmet Sağlayıcısı meta veri dosyasını yüklemeyi başarıyla tamamladıktan sonra **Tanımlayıcı** ve **YanıtURL** değerleri **Temel SAML Yapılandırması** bölümünde otomatik olarak doldurulur.

    >[!Note]
    >Servis Sağlayıcı Meta veri dosyasını, daha sonra öğreticide açıklanan **Cisco Webex Meetings SSO** bölümünü yapılandırmabölümünden alırsınız. 

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:  

    a. Temel **SAML Yapılandırması** bölümünde, düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)
    
    b. URL metin kutusunda **Oturum Aç'ta,** URL'yi aşağıdaki deseni kullanarak yazın:` https://<customername>.my.webex.com`

5. Cisco Webex Meetings uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

6. Yukarıdakilere ek olarak, Cisco Webex Meetings uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Kullanıcı Öznitelikleri iletişim kutusundaki Kullanıcı Talepleri bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin: 

    | Adı | Kaynak Özniteliği|
    | ---------------|  --------- |
    |   Ad    | user.givenname |
    |   Soyadı    | user.surname |
    |   e-posta       | kullanıcı.posta |
    |   Uıd    | kullanıcı.posta |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, açılan listeden bu satır için gösterilen öznitelik değerini seçin.

    f. **Kaydet**'e tıklayın.

4. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Cisco **Webex Toplantılarını Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Cisco Webex Toplantıları'na erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Cisco Webex Toplantıları'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-cisco-webex-meetings-sso"></a>Cisco Webex Toplantılarını Yapılandırın SSO

1. Yönetim `https://<customername>.webex.com/admin` kimlik bilgilerinizle URL'ye gidin.

2. Ortak **Site Ayarları'na** gidin ve **SSO Yapılandırması'na**gidin.
 
    ![Çoklu oturum açmayı yapılandırma](./media/cisco-webex-tutorial/tutorial-cisco-webex-11.png)

3. **Webex Yönetimi** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/cisco-webex-tutorial/tutorial-cisco-webex-10.png)

    a. **Federasyon Protokolü**olarak **SAML 2.0'ı** seçin.

    b. Azure portalından indirdiğiniz meta veri dosyasını yüklemek için **SAML Meta veri** verilerini içe aktarma bağlantısını tıklatın.

    c. Servis Sağlayıcı Metaveri dosyasını indirmek ve Azure portalındaki **Temel SAML Yapılandırması** bölümüne yüklemek için **Dışa** Aktar düğmesini tıklatın.

    d. **AuthContextClassRef** metin kutusunda, `urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified` Azure AD kullanarak MFA'yı yazın ve etkinleştirmek istiyorsanız, aşağıdaki iki değeri yazın`urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport;urn:oasis:names:tc:SAML:2.0:ac:classes:X509`

    e. **Otomatik Hesap Oluşturma'yı**seçin.

    >[!NOTE]
    >**Tam zamanında** kullanıcı sağlamayı etkinleştirmek için Otomatik **Hesap Oluşturma'yı**kontrol etmeniz gerekir. Buna ek olarak SAML belirteç öznitelikleri SAML yanıtı geçirilmelidir.

    f. **Kaydet**'e tıklayın.

    >[!NOTE]
    >Bu yapılandırma yalnızca Webex UserID'yi e-posta biçiminde kullanan müşteriler içindir.

### <a name="create-cisco-webex-meetings-test-user"></a>Cisco Webex Meetings test kullanıcıoluşturma

Bu bölümün amacı Cisco Webex Toplantıları'nda B.Simon adında bir kullanıcı oluşturmaktır. Cisco Webex Meetings, varsayılan olarak etkinleştirilen **tam zamanında** sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Cisco Webex Toplantıları'nda bir kullanıcı zaten yoksa, Cisco Webex Toplantıları'na erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Cisco Webex Toplantıları döşemesini tıklattığınızda, SSO'yu kurduğunuz Cisco Webex Toplantıları'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ServiceNow'u deneyin](https://aad.portal.azure.com)
