---
title: 'Öğretici: Atlassian Cloud ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Atlassian Cloud arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a3a54b096019e9e38bc800ae313016a430062dab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74964336"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Öğretici: Atlassian Cloud'u Azure Active Directory ile tümleştirin

Bu eğitimde, Atlassian Cloud'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Atlassian Cloud'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Atlassian Cloud erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Atlassian Cloud'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Atlassian Cloud tek oturum açma (SSO) aboneliği ni sağladı.
* Atlassian Cloud ürünleri için Güvenlik İddiası Biçimlendirme Dili (SAML) oturum açma özelliğini etkinleştirmek için Atlassian Access'i ayarlamanız gerekir. [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager)hakkında daha fazla bilgi edinin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 

* Atlassian Cloud **SP ve IDP'yi** destekliyor
* Atlassian Cloud [Otomatik kullanıcı sağlama ve deprovisioning](atlassian-cloud-provisioning-tutorial.md) destekler

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Galeriden Atlassian Bulutu Ekleme

Atlassian Cloud'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Atlassian Cloud'u eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Atlassian Cloud** yazın.
1. Sonuçlar panelinden **Atlassian Cloud'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Atlassian Cloud ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Atlassian Cloud'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Atlassian Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Atlassian Cloud SSO'yu yapılandırır](#configure-atlassian-cloud-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Atlassian Cloud test kullanıcısını oluşturun](#create-atlassian-cloud-test-user)** - Kullanıcının Azure AD gösterimine bağlı Atlassian Cloud'da B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Atlassian Cloud** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://auth.atlassian.com/saml/<unique ID>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<instancename>.atlassian.net`

    > [!NOTE]
    > Önceki değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL'si ile güncelleştirin. Bu gerçek değerleri, daha sonra **yapılandırılan Atlassian Cloud Tek İşaret-On** öğreticiaçık **atlassian Cloud SAML Yapılandırma** ekranından alırsınız.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<instancename>.atlassian.net`

    > [!NOTE]
    > URL değerindeki Oturum gerçek değildir. Atlassian Cloud yönetici portalına oturum açmanız için kullandığınız örnekteki değeri yapıştırın.

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. Atlassian Cloud uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. Atlassian Cloud uygulaması **nameidentifier** **user.mail**ile eşlenen bekliyor , bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşleme değiştirmek gerekir.

    ![image](common/edit-attribute.png)

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Atlassian Cloud'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, B.Simon'ın Atlassian Cloud'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Atlassian Cloud'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-atlassian-cloud-sso"></a>Atlassian Cloud SSO'nun yapılandırılsın

1. Atlassian Cloud içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten **sonra, Kurulum Atlassian Cloud'a** tıklayın ve sizi Atlassian Cloud uygulamasına yönlendirecektir. Buradan, Atlassian Cloud'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Atlassian Cloud'u el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Atlassian Cloud şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Tek oturum açma yapılandırmaya gitmeden önce etki alanınızı doğrulamanız gerekir. Daha fazla bilgi için [Atlassian etki alanı doğrulama](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belgesine bakın.

1. Sol bölmede, **Güvenlik** > **SAML tek oturum açma'yı**seçin. Henüz yapmadıysanız, Atlassian Identity Manager'a abone olun.

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

1. **SAML ekle yapılandırma** penceresinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. Kimlik **sağlayıcısı Entity ID** kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısını** yapıştırın.

    b. Kimlik **sağlayıcısı SSO URL** kutusuna, Azure portalından kopyaladığınız **Giriş URL'sini** yapıştırın.

    c. İndirilen sertifikayı Azure portalından .txt dosyasında açın, değeri kopyalayın *(Sertifikayı Başlat* ve *Bitiş Sertifikası* satırları olmadan) ve ardından **Genel X509 sertifika** kutusuna yapıştırın.

    d. **Yapılandırmayı Kaydet'i**tıklatın.

1. Doğru URL'leri ayarladığınızdan emin olmak için Aşağıdakileri yaparak Azure REKLAM ayarlarını güncelleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. SAML penceresinde, **SP Kimlik Kimliğini** kopyalayın ve ardından Azure portalında Atlassian Cloud **Basic SAML Configuration** **altında, Identifier** kutusuna yapıştırın.

    b. SAML penceresinde, **SP Assertion Tüketici Hizmeti URL'sini** kopyalayın ve ardından Azure portalında Atlassian Cloud **Basic SAML Configuration**altında Yanıt **URL** kutusuna yapıştırın. Oturum açma URL'si, Atlassian Cloud'unuzun kiracı URL'sidir.

    > [!NOTE]
    > Varolan bir müşteriyseniz, Azure portalındaki **SP Kimlik Kimliği** ve **SP Assertion Tüketici Hizmeti URL** değerlerini güncelledikten sonra Evet'i seçin ve **yapılandırmayı güncelleştirin.** Yeni bir müşteriyseniz, bu adımı atlayabilirsiniz.

### <a name="create-atlassian-cloud-test-user"></a>Atlassian Cloud test kullanıcısı oluşturma

Azure AD kullanıcılarının Atlassian Cloud'da oturum açmalarını sağlamak için, aşağıdakileri yaparak kullanıcı hesaplarını Atlassian Cloud'da el ile etkinleştirin:

1. **Yönetim** **bölmesinde, Kullanıcıları**seçin.

    ![Atlassian Bulut Kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

1. Atlassian Cloud'da bir kullanıcı oluşturmak için **Kullanıcıyı Davet**et'i seçin.

    ![Atlassian Cloud kullanıcısı oluşturma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

1. **E-posta adres** kutusuna, kullanıcının e-posta adresini girin ve ardından uygulama erişimini atayın.

    ![Atlassian Cloud kullanıcısı oluşturma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

1. Kullanıcıya e-posta daveti göndermek için **kullanıcıları davet et'i**seçin. Kullanıcıya bir e-posta daveti gönderilir ve daveti kabul ettikten sonra kullanıcı sistemde etkindir.

> [!NOTE]
> **Ayrıca, Kullanıcılar** bölümündeki **Toplu Oluştur** düğmesini seçerek toplu olarak oluşturabilirsiniz.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Atlassian Bulut döşemesini seçtiğinizde, SSO'yu kurduğunuz Atlassian Cloud'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Atlassian Cloud'u deneyin](https://aad.portal.azure.com/)
