---
title: 'Öğretici: Salesforce ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Salesforce arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a875cee7e6796a2c865bde4a62f2f0463eb12130
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78967717"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce"></a>Öğretici: Salesforce ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Salesforce'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Salesforce'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Salesforce erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Salesforce'ta otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Salesforce tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **Salesforce, SP** tarafından başlatılan SSO'ya destek verdi

* Salesforce [ **Otomatik** kullanıcı sağlama ve deprovisioning](salesforce-provisioning-tutorial.md) destekler (önerilir)

* Salesforce **Just In Time** kullanıcı sağlama destekler

* Salesforce Mobile uygulaması artık SSO'yu etkinleştirmek için Azure AD ile yapılandırılabilir. Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.
* Salesforce'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-from-the-gallery"></a>Galeriden Salesforce ekleme

Salesforce'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Salesforce eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Salesforce** yazın.
1. Sonuç panelinden **Salesforce'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce"></a>Salesforce için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Salesforce ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Salesforce'taki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Salesforce ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Salesforce SSO'yu yapılandırır](#configure-salesforce-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Salesforce test kullanıcısını oluşturun](#create-salesforce-test-user)** - Salesforce'ta B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Salesforce** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna, aşağıdaki deseni kullanarak değeri yazın:

    Kurumsal hesap:`https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı:`https://<subdomain>-dev-ed.my.salesforce.com`

    b. **Tanımlayıcı** textbox'ında, değeri aşağıdaki deseni kullanarak yazın:

    Kurumsal hesap:`https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve Tanımlayıcıile güncelleştirin. Bu değerleri almak için [Salesforce Client destek ekibine](https://help.salesforce.com/support) başvurun.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Salesforce'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Salesforce'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Salesforce'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-salesforce-sso"></a>Salesforce SSO'nun yapılandırılsın

1. Salesforce içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyi'ni tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Salesforce'u Ayarla'yı** tıklatın ve sizi Salesforce Tek Oturum Açma uygulamasına yönlendirecektir. Buradan Salesforce Single Sign-On'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-13 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Salesforce'u el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Salesforce şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Sayfanın sağ üst köşesindeki **ayarlar simgesinin** altındaki **Kurulum** simgesine tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/configure1.png)

1. Gezinti bölmesindeki **AYARLAR'a** gidin, ilgili bölümü genişletmek için **Kimlik'i** tıklatın. Ardından **Tek Oturum Açma Ayarları'nı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-admin-sso.png)

1. Tek **Oturum Açma Ayarları** **sayfasında, Düzelt** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Salesforce hesabınız için Tek Oturum Açma ayarlarını etkinleştiremiyorsanız [Salesforce Client destek ekibine](https://help.salesforce.com/support)başvurmanız gerekebilir.

1. **SAML Etkin'i**seçin ve ardından **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-enable-saml.png)

1. SAML tek oturum açma ayarlarınızı yapılandırmak için **Meta veri dosyasından Yeni'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-admin-sso-new.png)

1. Azure portalından indirdiğiniz meta data XML dosyasını yüklemek için **Dosyayı Seç'i** tıklatın ve **Oluştur'u**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/xmlchoose.png)

1. **SAML Tek Oturum Açma Ayarları** sayfasında, alanlar otomatik olarak doldurulur, Kullanıcı Sağlama **Etkin'i** seçin ve sonra **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/salesforcexml.png)

1. Salesforce'un sol gezinti bölmesinde, ilgili bölümü genişletmek için **Şirket Ayarları'nı** tıklatın ve ardından **Etki Alanım'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-my-domain.png)

1. Kimlik Doğrulama **Yapılandırması** bölümüne gidin ve **Düzenleme** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-edit-auth-config.png)

1. Kimlik **Doğrulama Yapılandırması** bölümünde, SAML SSO yapılandırmanızın **Kimlik Doğrulama Hizmeti** olarak **AzureSSO'yu** denetleyin ve ardından **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Birden fazla kimlik doğrulama hizmeti seçilirse, kullanıcılardan Salesforce ortamınızda tek oturum açma işlemi başlatırken oturum açmayı beğendikleri kimlik doğrulama hizmetini seçmeleri istenir. Bunun olmasını istemiyorsanız, **diğer tüm kimlik doğrulama hizmetlerini işaretsiz bırakmalısınız.**

### <a name="create-salesforce-test-user"></a>Salesforce test kullanıcısı oluşturma

Bu bölümde Salesforce'ta B.Simon adında bir kullanıcı oluşturulur. Salesforce, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Salesforce'ta bir kullanıcı zaten yoksa, Salesforce'a erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur. Salesforce da otomatik kullanıcı sağlama destekler, otomatik kullanıcı sağlama yapılandırmak için nasıl daha fazla bilgi [burada](salesforce-provisioning-tutorial.md) bulabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Salesforce döşemesini tıklattığınızda, SSO'yu kurduğunuz Salesforce'ta otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="test-sso-for-salesforce-mobile"></a>Salesforce (Mobil) için Test SSO

1. Salesforce mobil uygulamasını açın. Oturum açma sayfasında, **Özel Etki Alanı Kullan'ı**tıklatın.

    ![Salesforce mobil uygulaması](media/salesforce-tutorial/mobile-app1.png)

1. Özel **Etki Alanı** metin kutusuna, kayıtlı özel alan adınızı girin ve **Devam et'i**tıklatın.

    ![Salesforce mobil uygulaması](media/salesforce-tutorial/mobile-app2.png)

1. Salesforce uygulamasında oturum açabilmek için Azure REKLAM kimlik bilgilerinizi girin ve **İleri'yi**tıklatın.

    ![Salesforce mobil uygulaması](media/salesforce-tutorial/mobile-app3.png)

1. Aşağıda gösterildiği gibi **Erişime İzin Ver** sayfasında Salesforce uygulamasına erişim sağlamak için **İzin Ver'i** tıklatın.

    ![Salesforce mobil uygulaması](media/salesforce-tutorial/mobile-app4.png)

1. Son olarak başarılı oturum açmadan sonra uygulama ana sayfası görüntülenir.

    ![Salesforce mobil](media/salesforce-tutorial/mobile-app5.png) ![uygulama Salesforce mobil uygulaması](media/salesforce-tutorial/mobile-app6.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı Sağlama'yı Yapılandır](salesforce-provisioning-tutorial.md)

- [Azure AD ile Salesforce'u deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Salesforce'u gelişmiş görünürlük ve kontrollerle koruma](https://docs.microsoft.com/cloud-app-security/protect-salesforce)