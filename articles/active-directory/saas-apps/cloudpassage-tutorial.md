---
title: 'Öğretici: CloudPassage ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve CloudPassage arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0bcf0b4781e7ecb4301672f511a9f01f7fd2082
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75561241"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cloudpassage"></a>Öğretici: CloudPassage ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, CloudPassage'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. CloudPassage'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* CloudPassage erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla CloudPassage'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* CloudPassage tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* CloudPassage **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-cloudpassage-from-the-gallery"></a>Galeriden CloudPassage ekleme

CloudPassage'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden CloudPassage'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **CloudPassage** yazın.
1. Sonuçlar panelinden **CloudPassage'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-cloudpassage"></a>CloudPassage için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak CloudPassage ile yapılandırın ve test edin. SSO'nun çalışması için, CloudPassage'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu CloudPassage ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[CloudPassage SSO'yu yapılandırır](#configure-cloudpassage-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[CloudPassage test kullanıcısını oluşturun](#create-cloudpassage-test-user)** - Kullanıcının Azure AD gösterimine bağlı CloudPassage'da B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **CloudPassage** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

     a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://portal.cloudpassage.com/saml/init/accountid`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni `https://portal.cloudpassage.com/saml/consume/accountid`kullanarak bir URL yazın: . CloudPassage portalınızın **Tek Oturum Açma Ayarları** bölümündesi'ndeki **SSO Kurulum belgelerini** tıklayarak bu öznitelik için değerinizi alabilirsiniz.

    ![Tek İşaret-On'u Yapılandır](./media/cloudpassage-tutorial/tutorial_cloudpassage_05.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [CloudPassage İstemci destek ekibine](https://www.cloudpassage.com/company/contact/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. CloudPassage uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, CloudPassage uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ---------------| --------------- |
    | Ad |user.givenname |
    | Soyadı |user.surname |
    | e-posta |kullanıcı.posta |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **CloudPassage'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın CloudPassage'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **CloudPassage'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-cloudpassage-sso"></a>CloudPassage SSO'ya yapılandır

1. Farklı bir tarayıcı penceresinde, CloudPassage şirket sitenize yönetici olarak oturum açın.

1. Üstteki menüde **Ayarlar'ı**tıklatın ve ardından **Site Yönetimi'ni**tıklatın. 
   
    ![Tek İşaret-On'u Yapılandır][12]

1. Kimlik **Doğrulama Ayarları** sekmesini tıklatın. 
   
    ![Tek İşaret-On'u Yapılandır][13]

1. Tek **Oturum Açma Ayarları** bölümünde aşağıdaki adımları gerçekleştirin: 
   
    ![Tek İşaret-On'u Yapılandır][14]

    a. **Tek oturum açma(SSO)(SSO Kurulum Belgeleri)** onay kutusunu etkinleştir'i seçin.
    
    b. Azure **AD Tanımlayıcısını** **SAML veren URL** textbox'a yapıştırın.
  
    c. Giriş **URL'sini** **SAML uç noktası URL** textbox'ına yapıştırın.
  
    d. Giriş **Sayfası URL'sini** **Giriş sayfası** metin kutusuna yapıştırın.
  
    e. İndirilen sertifikanızı not defterinde açın, indirilen sertifikanın içeriğini panonuza kopyalayın ve **ardından x 509 sertifika** metin kutusuna yapıştırın.
  
    f. **Kaydet**'e tıklayın.

### <a name="create-cloudpassage-test-user"></a>CloudPassage test kullanıcısı oluşturma

Bu bölümün amacı CloudPassage'da B.Simon adında bir kullanıcı oluşturmaktır.

**CloudPassage'da B.Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. **CloudPassage** şirket sitenizde yönetici olarak oturum açın. 

1. Üstteki araç çubuğunda **Ayarlar'ı**tıklatın ve ardından **Site Yönetimi'ni**tıklatın. 
   
    ![CloudPassage test kullanıcısı oluşturma][22] 

1. **Kullanıcılar** sekmesini tıklatın ve sonra **Yeni Kullanıcı Ekle'yi**tıklatın. 
   
    ![CloudPassage test kullanıcısı oluşturma][23]

1. Yeni **Kullanıcı Ekle** bölümünde aşağıdaki adımları gerçekleştirin: 
   
    ![CloudPassage test kullanıcısı oluşturma][24]
    
    a. Ad **metin** kutusuna Britta yazın. 
  
    b. **Soyadı** metin kutusuna Simon yazın.
  
    c. Kullanıcı **adı** metin kutusunda, **E-posta** metin kutusunda ve **E-postayı Yeniden Yazın** metin kutusuna, Azure AD'ye Britta'nın kullanıcı adını yazın.
  
    d. **Access Türü**olarak, Halo Portal **Erişimini Etkinleştir'i**seçin.
  
    e. **Ekle**’ye tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki CloudPassage döşemesini tıklattığınızda, SSO'yu kurduğunuz CloudPassage'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile CloudPassage'ı deneyin](https://aad.portal.azure.com/)

<!--Image references-->

[12]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_17.png

