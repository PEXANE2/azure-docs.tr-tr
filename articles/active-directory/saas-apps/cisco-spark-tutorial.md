---
title: 'Öğretici: Cisco Webex ile Azure Active Directory Tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Cisco Webex arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01fe4d06d5f73eacee1d1cdaf1963232b84daf05
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77046799"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-webex"></a>Öğretici: Cisco Webex ile Azure Active Directory Tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Cisco Webex'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Cisco Webex'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Cisco Webex erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Cisco Webex'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Cisco Webex tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Cisco Webex, **SP** tarafından başlatılan SSO'yu destekler.
* Cisco **Webex, Otomatik** kullanıcı sağlamayı destekler.
* Cisco Webex'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-cisco-webex-from-the-gallery"></a>Galeriden Cisco Webex ekleme

Cisco Webex'in Azure AD'ye entegrasyonunu yapılandırmak için, cisco Webex'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Cisco Webex** yazın.
1. Sonuç panelinden **Cisco Webex'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-webex"></a>Cisco Webex için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Cisco Webex ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Cisco Webex'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Cisco Webex ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[Cisco Webex'i](#configure-cisco-webex)** uygulama tarafındaki SSO ayarlarını yapılandırmak için yapılandırın.
    1. **[Cisco Webex'te](#create-cisco-webex-test-user)** B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için Cisco Webex test kullanıcısını oluşturun.
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Cisco Webex** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, indirilen **Servis Sağlayıcısı meta veri** dosyasını yükleyin ve aşağıdaki adımları gerçekleştirerek uygulamayı yapılandırın:

    >[!Note]
    >Servis Sağlayıcı Meta veri dosyasını, daha sonra öğreticide açıklanan **Cisco Webex'i Yapılandırış** bölümünden alırsınız. 

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    c. Hizmet Sağlayıcısı meta veri dosyasını yüklemeyi başarıyla tamamladıktan sonra **Tanımlayıcı** ve **YanıtURL** değerleri **Temel SAML Yapılandırması** bölümünde otomatik olarak doldurulur:

    URL metin kutusunda **Oturum Aç'ta,** SP meta veri dosyası yüklemesiyle otomatik olarak doldurulan Yanıt **URL'sinin**değerini yapıştırın.

1. Cisco Webex uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Cisco Webex uygulaması, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
  
    | Adı |  Kaynak Özniteliği|
    | ---------------|--------- |
    | Uıd | user.userprincipalname |

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Cisco **Webex'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Cisco Webex'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Cisco Webex'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-cisco-webex"></a>Cisco Webex'i yapılandır

1. Cisco Webex içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra Cisco **Webex'i** Ayarla'yı tıklatın ve sizi Cisco Webex uygulamasına yönlendirecektir. Buradan Cisco Webex'te oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Cisco Webex'i el ile kurmak istiyorsanız, tam yönetici kimlik bilgilerinizle [Cisco Cloud İşbirliği Yönetimi'nde](https://admin.ciscospark.com/) oturum açın.

4. **Ayarlar'ı** seçin ve **Kimlik Doğrulama** bölümünün altında **Değiştir'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/cisco-spark-tutorial/tutorial-cisco-spark-10.png)
  
5. **Üçüncü taraf kimlik sağlayıcısını tümleştir'i seçin. (Gelişmiş)** ve bir sonraki ekrana gidin.

6. Alma **Idp Meta veri** sayfasında, Azure AD meta veri dosyasını sürükleyip sayfaya bırakın veya Azure AD meta veri dosyasını bulup yüklemek için dosya tarayıcısı seçeneğini kullanın. Ardından, **Metadata'da sertifika yetkilisi tarafından imzalanmış sertifikayı talep etme 'yi seçin (daha güvenli)** ve **İleri'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/cisco-spark-tutorial/tutorial-cisco-spark-11.png)

7. **SSO Bağlantısını Test Edin**ve yeni bir tarayıcı sekmesi açıldığında oturum açarak Azure AD ile kimlik doğrulaması yapın.

8. **Cisco Cloud İşbirliği Yönetimi** tarayıcı sekmesine geri dönün. Test başarılı olduysa, **Bu testin başarılı olduğunu seçin. Tek Oturum Açma seçeneğini etkinleştirin** ve **İleri'yi**tıklatın.

### <a name="create-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Bu bölümde, Cisco Webex'te B.Simon adında bir kullanıcı oluşturursunuz. Bu bölümde, Cisco Webex'te B.Simon adında bir kullanıcı oluşturursunuz.

1. Tam yönetici kimlik bilgilerinizle [Cisco Cloud İşbirliği Yönetimi'ne](https://admin.ciscospark.com/) gidin.

2. **Kullanıcıları** tıklatın ve ardından **Kullanıcıları Yönetin.**
   
    ![Tek İşaret-On'u Yapılandır](./media/cisco-spark-tutorial/tutorial-cisco-spark-12.png) 

3. **Kullanıcıyı Yönet** penceresinde, **Kullanıcıları El Ile Ekle veya Değiştir'i** seçin ve **İleri'yi**tıklatın.

4. **Adlar ve E-posta adresini**seçin. Ardından, textbox'ı aşağıdaki gibi doldurun:

    ![Tek İşaret-On'u Yapılandır](./media/cisco-spark-tutorial/tutorial-cisco-spark-13.png) 

    a. Ad **metin** kutusunda, **B**gibi kullanıcının ilk adını yazın.

    b. **Soyadı** metin kutusunda, **Simon**gibi kullanıcının soyadını yazın.

    c. **E-posta adresi** metin kutusuna, kullanıcının e-posta adresini yazın. b.simon@contoso.com

5. B.Simon eklemek için artı işaretini tıklatın. Ardından **İleri'yi**tıklatın.

6. Kullanıcılar **için Hizmetler Ekle** penceresinde **Kaydet'i** tıklatın ve sonra Bitir' i **tıklatın.**

## <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Cisco Webex döşemesini seçtiğinizde, SSO'yu kurduğunuz Cisco Webex'te otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Cisco Webex'i deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Cisco Webex'i gelişmiş görünürlük ve kontrollerle nasıl koruyabilen](https://docs.microsoft.com/cloud-app-security/protect-webex)