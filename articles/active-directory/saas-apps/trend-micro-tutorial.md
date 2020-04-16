---
title: 'Öğretici: Trend Micro Web Security(TMWS) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Trend Micro Web Security (TMWS) arasında tek oturum açma yı nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a4c2cddbc9086c80922fcf9c5d96cd197ab4778
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422532"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-trend-micro-web-securitytmws"></a>Öğretici: Trend Micro Web Security(TMWS) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Trend Micro Web Security'yi (TMWS) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Trend Micro Web Security(TMWS) ile Azure AD'yi tümleştirdiğinizde şunları yapabilirsiniz:

* Trend Micro Web Security'ye (TMWS) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Trend Micro Web Security'de (TMWS) otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Trend Micro Web Security(TMWS) tek oturum açma (SSO) aboneliğini etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Trend Micro Web Security(TMWS) **SP** başlatılan SSO destekler
* Trend Micro Web Security'yi (TMWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-trend-micro-web-securitytmws-from-the-gallery"></a>Galeriden Trend Micro Web Security(TMWS) ekleme

Trend Micro Web Security'nin (TMWS) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Trend Micro Web Security(TMWS) eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Trend Micro Web Security(TMWS)** yazın.
1. Sonuç panelinden **Trend Micro Web Security(TMWS)** seçeneğini belirleyin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-trend-micro-web-securitytmws"></a>Trend Micro Web Security(TMWS) için Azure AD oturum açma işlemlerini yapılandırma ve test etme

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Trend Micro Web Security (TMWS) ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Trend Micro Web Security(TMWS)'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Trend Micro Web Security (TMWS) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
    1. **[Azure AD'de kullanıcı ve grup eşitleme ayarlarını yapılandırma](#configure-user-and-group-synchronization-settings-in-azure-ad)** - Azure AD'de kullanıcı ve grup eşitleme ayarlarını yapılandırma
1. **[Trend Micro Web Security(TMWS) SSO'yu yapılandırır](#configure-trend-micro-web-security-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Trend Micro Web Security(TMWS)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})`

    b. **Yanıtla URL** metin kutusuna bir URL yazın:`https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değildir. Bu değeri gerçek Tanımlayıcı ile güncelleştirin. Tanımlayıcı değeri almak için [Trend Micro Web Security(TMWS) İstemci destek ekibine](https://success.trendmicro.com/contact-support-north-america) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Trend Micro Web Security(TMWS) uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Trend Micro Web Security (TMWS) uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı | Kaynak Özniteliği|
    | --------------- | --------- |
    | sAMAccountName | user.onpremisessamaccountname |
    | Upn | user.userprincipalname |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Trend **Micro Web Security(TMWS) kurulumu** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Trend Micro Web Security'ye (TMWS) erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Trend Micro Web Security(TMWS) seçeneğini belirleyin.**
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="configure-user-and-group-synchronization-settings-in-azure-ad"></a>Azure AD'de kullanıcı ve grup eşitleme ayarlarını yapılandırma

1. Sol gezintiden **Azure Etkin Dizin'i**tıklatın.

1. **Yönet'in**altında, **Uygulama kayıtlarını** tıklatın ve ardından **Tüm uygulamalar** alanı altında yeni kurumsal uygulamanızı tıklatın.

1. **Yönet**altında, **Sertifikalar & sırları**tıklatın.

1. Görünen İstemci sırları alanının altında, **Yeni istemci sırrını**tıklatın.

1. Görünen istemci gizli ekranında isteğe bağlı olarak bir açıklama ekleyin ve bu istemci sırrı için bir son kullanma süresi seçin ve sonra **Ekle'yi**tıklatın. Yeni eklenen istemci sırrı, İstemci sırları alanının altında görünür.

1. Değeri kaydedin. Daha sonra, bilgileri TMWS'ye yazarsınız.

1. **Yönet**altında, **API izinlerini**tıklatın. 

1. Görünen API izinleri ekranında **izin ekle'yi**tıklatın.

1. Görünen İstek API izinleri ekranının Microsoft API'leri sekmesinde, **Microsoft Graph'ı** ve ardından **Uygulama izinlerini**tıklatın.

1. Aşağıdaki izinleri bulun ve ekleyin: 

    * Group.Read.All
    * Kullanıcı.Read.All

1. **İzin Ekle'yi**tıklatın. Ayarlarınızın başarıyla kaydedildiğini onaylayan bir ileti görüntülenir. Yeni eklenen izinler API izinleri ekranında görünür.

1. Hibe onay alanı altında, **yönetici hesabınızı > (Varsayılan Dizin) < için Yönetici Onayı'nı tıklatın** ve ardından **Evet'** seçeneğini tıklatın. İstenen izinler için yönetici onayına başarıyla verildiğini onaylayan bir ileti görüntülenir.

1. **Genel Bakış**'a tıklayın. 

1. Görünen sağ bölmede, Uygulama (istemci) kimliğini ve Dizin (kiracı) Kimliğini kaydedin. Daha sonra, bilgileri TMWS'ye yazarsınız. Ayrıca Azure **Etkin Dizin >** Altında Özel alan **adlarını** tıklatabilir ve alan adını sağ bölmeye kaydedebilirsiniz.

## <a name="configure-trend-micro-web-security-sso"></a>Trend Micro Web Security SSO'da yapılandırın

**Trend Micro Web Security(TMWS)** tarafında tek oturum açma işlemi yapmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından [Trend Micro Web Security(TMWS) destek ekibine](https://success.trendmicro.com/contact-support-north-america)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

## <a name="test-sso"></a>Test SSO 

Azure AD hizmetini başarıyla yapılandırdıktan ve Azure AD'yi kullanıcı kimlik doğrulama yöntemi olarak belirttikten sonra, kurulumunuzu doğrulamak için TMWS proxy sunucusunda oturum açabilirsiniz. Azure AD oturumu hesabınızı doğruladıktan sonra Internet'i ziyaret edebilirsiniz.

> [!NOTE]
> TMWS, Azure AD portalından tek oturum açma testini desteklemez, Genel > Tek oturum açma > Yeni kurumsal uygulamanızın SAML > Testi ile Tek Oturum Açma'yı ayarlama altında.

1. Tarayıcıyı tüm tanımlama bilgilerini temizleyin ve ardından tarayıcıyı yeniden başlatın. 

1. Tarayıcınızı TMWS proxy sunucusuna yönlendirin. Ayrıntılar için [PAC Dosyalarını Kullanarak Trafik Yönlendirme'ye](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B)bakın.

1. Herhangi bir Internet web sitesini ziyaret edin. TMWS sizi TMWS esir portalına yönlendirecektir.

1. Active Directory hesabı (biçim: etki alanı\sAMAccountName veya sAMAccountName@domain), veya e-posta adresi veya UPN belirtin ve ardından Oturum **Aç'ı**tıklatın. TMWS sizi Azure AD oturumuna gönderir.

1. Azure AD oturum açmada, AD hesap kimlik bilgilerinizi yazın. TMWS'de başarılı bir şekilde oturum açmalısınız.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Trend Micro Web Security(TMWS) deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Trend Micro Web Security(TMWS) gelişmiş görünürlük ve denetimlerle nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

