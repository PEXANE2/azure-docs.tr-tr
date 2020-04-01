---
title: 'Öğretici: Veracode ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Veracode arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcec326ddab1e74f43e1bb7ef446998a40799fd0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73043566"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-veracode"></a>Öğretici: Veracode ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Veracode'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Veracode'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Veracode erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Veracode'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Veracode tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Veracode, kimlik sağlayıcısının başlattığı SSO'yı ve tam zamanında kullanıcı sağlamayı destekler.

## <a name="add-veracode-from-the-gallery"></a>Galeriden Veracode ekle

Veracode'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Veracode ekleyin.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna "Veracode" yazın.
1. Sonuç panelinden **Veracode'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-veracode"></a>Veracode için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Veracode ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Veracode'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD SSO'yu Veracode ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Veracode SSO'yu](#configure-veracode-sso)** uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * Veracode'daki B.Simon'ın bir örneğine sahip olmak için **[bir Veracode test kullanıcısı oluşturun](#create-veracode-test-user)** ve kullanıcının Azure AD gösterimine bağlı olun.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Veracode** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml ile Tek Oturum Açma'nın ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. **Kaydet'i**seçin.

1. **SAML sayfasıyla tek oturum** açma'da, **SAML İmza Sertifikası** bölümünde **Sertifikayı (Base64)** bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![İndirme bağlantısı vurgulanmış SAML İmzalama Sertifikası bölümünün ekran görüntüsü](common/certificatebase64.png)

1. Veracode, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![Kullanıcı Öznitelikleri & Talepler bölümünün ekran görüntüsü](common/default-attributes.png)

1. Veracode ayrıca SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | Ad |Kullanıcı.givenname |
    | Soyadı |Kullanıcı.soyadı |
    | e-posta |Kullanıcı.posta |

1. **Veracode'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'leri vurgulanmış Veracode bölümünü ayarlama ekran görüntüsü](common/copy-configuration-urls.png)

## <a name="configure-veracode-sso"></a>Veracode SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, Veracode şirket sitenizde yönetici olarak oturum açın.

1. Üstteki menüden **Ayarlar** > **Yöneticisi'ni**seçin.
   
    ![Ayarlar simgesi ve Admin vurgulanmış Veracode İdaresi ekran görüntüsü](./media/veracode-tutorial/ic802911.png "Yönetim")

1. **SAML** sekmesini seçin.

1. Organizasyon **SAML Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Organizasyon SAML Ayarları bölümünün ekran görüntüsü](./media/veracode-tutorial/ic802912.png "Yönetim")

    a.  **Veren için,** Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın** değerini yapıştırın.

    b. **İddia İmzalama Sertifikası**için, İndirilen sertifikanızı Azure portalından yüklemek için Dosya'yı **seçin'i** seçin.

    c. **Kendi Kendine Kayıt**için, Kendi Kendine Kaydı **Etkinleştir'i**seçin.

1. Kendi **Kendine Kayıt Ayarları** bölümünde aşağıdaki adımları gerçekleştirin ve ardından **Kaydet'i**seçin:

    ![Çeşitli seçeneklervurgulanmış Self Registration Settings bölümünün ekran görüntüsü](./media/veracode-tutorial/ic802913.png "Yönetim")

    a. **Yeni Kullanıcı Etkinleştirme**için, **Gerekli Etkinleştirme yok**seçin.

    b. **Kullanıcı Veri Güncelleştirmeleri**için **Tercih Veracode Kullanıcı Verilerini**seçin.

    c. **SAML Öznitelik Ayrıntıları**için aşağıdakileri seçin:
      * **Kullanıcı Rolleri**
      * **İlke Yöneticisi**
      * **Inceleme**
      * **Güvenlik Müşteri Adayı**
      * **Executive**
      * **Submitter**
      * **Oluşturucu**
      * **Tüm Tscan Türleri**
      * **Takım Üyelikleri**
      * **Varsayılan Takım**

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** >**Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:

   1. **Ad**için `B.Simon`, girin .  
   1. **Kullanıcı adı**için username@companydomain.extension, . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster'i**seçin ve ardından gösterilen değeri yazın.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Veracode'a erişim izni vererek Azure tek oturum açma'yı kullanmasını etkinleştirin.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **Veracode'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılardan**, **B.Simon seçin.** Ardından ekranın alt kısmında **Seç'i** seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-veracode-test-user"></a>Veracode test kullanıcısı oluşturma

Veracode'da oturum açabilmek için Azure AD kullanıcılarının Veracode'da sağlanması gerekir. Bu görev otomatiktir ve el ile bir şey yapmanız gerekmez. İlk tek oturum açma denemesi sırasında gerekirse kullanıcılar otomatik olarak oluşturulur.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Veracode tarafından sağlanan diğer Veracode kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'nde **Veracode'u** seçtiğinizde, SSO'yu kurduğunuz Veracode'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Veracode'u deneyin](https://aad.portal.azure.com/)