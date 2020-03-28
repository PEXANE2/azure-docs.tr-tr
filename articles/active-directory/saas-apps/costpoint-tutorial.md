---
title: 'Öğretici: Costpoint ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Costpoint arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9ecc5f58-4462-4ade-ab73-0a4f61027504
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01168540e27605db0d240c0774159a710b5d5254
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71840075"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Öğretici: Costpoint'i Azure Etkin Dizini ile tümleştir

Bu eğitimde, Costpoint'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Costpoint'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Costpoint erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Costpoint'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Costpoint tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandıracak ve sınayabilirsiniz. **Costpoint, SP ve IDP** tarafından başlatılan SSO'yu destekler.

## <a name="generate-costpoint-metadata"></a>Costpoint meta verisi oluşturma

Costpoint SAML SSO yapılandırma **DeltekCostpoint711Security.pdf** kılavuzunda açıklanmıştır. Bu kılavuzu Deltek Costpoint destek sitesinden indirin ve Costpoint ve Microsoft Azure bölümü arasındaki **SAML Tek Oturum** > Yapılandırma**sı'na** bakın. Yönergeleri izleyin ve bir **Costpoint SP Federasyonu Metadata XML** dosyası oluşturun. 

![Costpoint Yapılandırma Yardımcı Programı](./media/costpoint-tutorial/config-utility.png)

## <a name="add-costpoint-from-the-gallery"></a>Galeriden Costpoint ekleme

Costpoint'i Azure REKLAMıyla tümleştirmek için, önce Azure portalındaki galeriden yönetilen SaaS uygulamaları listenize Costpoint'i ekleyin:

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.

1. Sol gezinti bölmesinde Azure **Etkin Dizin** hizmetini seçin.

   ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

1. **Kurumsal Uygulamaları** > Seçin**Tüm Uygulamalar**.

   ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.

   ![Yeni uygulama düğmesi](common/add-new-app.png)

1. **Galeriden Ekle** bölümünde, arama kutusuna **Costpoint'i** girin.

   ![Sonuç listesindecostpoint](common/search-new-app.png)

1. Sonuç listesinde **Costpoint'i**seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sgn-on"></a>Azure AD tek sgn-on yapılandırma ve test

Azure AD SSO'yu Costpoint ile **B.Simon**adında bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Costpoint'teki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Costpoint ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
1. **[Costpoint'i](#configure-costpoint)** uygulama tarafındaki SAML SSO ayarlarını yapılandırmak için yapılandırın.
1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Costpoint'te B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için **[bir Costpoint test kullanıcısı oluşturun.](#create-a-costpoint-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin:

1. **Costpoint** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

   ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

1. Temel **SAML Yapılandırması** bölümünde, *Servis Sağlayıcı meta veri dosyanız*varsa, aşağıdaki adımları tamamlayın:

   > [!NOTE]
   > [Maliyet Noktası meta verilerini oluştur'da](#generate-costpoint-metadata)Servis Sağlayıcı meta veri dosyasını alırsınız. Dosyanın nasıl kullanılacağı daha sonra öğreticide açıklanır.
 
   1. Meta **veri dosyasını yükle** düğmesini seçin, ardından Costpoint tarafından daha önce oluşturulan **Costpoint SP Federation Metadata XML** dosyasını seçin ve ardından dosyayı yüklemek için **Ekle** düğmesini seçin.

      ![Meta veri dosyasını yükleme](./media/costpoint-tutorial/upload-metadata.png)
    
   1. Meta veri dosyası başarıyla yüklendiğinde, **Tanımlayıcı** ve **YanıtLA URL** değerleri Costpoint bölümünde otomatik olarak doldurulur.

      > [!NOTE]
      > **Tanımlayıcı** ve **Yanıt URL** değerleri otomatik olarak inceleştirilmiyorsa, gereksiniminize göre değerleri el ile girin. **Tanımlayıcının (Entity ID)** ve Yanıt URL'si **(İddia Tüketici Hizmeti URL'si)** doğru olarak ayarladığını ve **ACS URL'nin** **/LoginServlet.cps**ile biten geçerli bir Costpoint URL'si olduğunu doğrulayın.

   1. **Ek URL'ler Ayarla'yı**seçin. **Röle Durumu**için, aşağıdaki deseni kullanarak bir değer girin:`system=[your system]` (örneğin, **system=DELTEKCP).**

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App Federation **Metadata Url'sini** kopyalamak ve Notepad'e kaydetmek için **Kopyala** simgesini seçin.

   ![SAML İmza Sertifikası](common/copy-metadataurl.png)

### <a name="configure-costpoint"></a>Costpoint'i yapılandır

1. Costpoint Configuration Utility'ye dönün. **IdP Federation Metadata XML** metin kutusuna, *Uygulama Federasyonu Metadata Url* dosyasının içeriğini yapıştırın. 

   ![Costpoint Yapılandırma Yardımcı Programı](./media/costpoint-tutorial/config-utility-idp.png)

1. Costpoint SAML kurulum bitirmek için **DeltekCostpoint711Security.pdf** kılavuzundan yönergeleri devam edin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında B.Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

   !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

1. **Yeni kullanıcıyı**seçin.

   ![Yeni kullanıcı Düğmesi](common/new-user.png)

1. **Kullanıcı** özelliklerinde aşağıdaki adımları tamamlayın:

   ![Kullanıcı iletişim kutusu](common/user-properties.png)

   1. **Ad** alanına, **B.Simon**girin.
   
   1. Kullanıcı **adı** alanına girin `b.simon\@yourcompanydomain.extension` (örneğin, B.Simon@contoso.com).
   
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** alanında görüntülenen değeri yazın.
   
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'a Costpoint'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.

1. Uygulamalar listesinde **Costpoint'i**seçin.

1. Uygulamanın genel bakış sayfasının **Yönet** **bölümünde, Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **B.Simon'u**seçin. Ardından, **Seç'i**seçin.

1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve sonra **Seç'i**seçin.

1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-costpoint-test-user"></a>Costpoint test kullanıcısı oluşturma

Bu bölümde, Costpoint'te bir kullanıcı oluşturursunuz. Kullanıcı kimliğinin **B.SIMON** ve kullanıcının adının **B.Simon**olduğunu varsayalım. Kullanıcıyı Costpoint platformuna eklemek için [Costpoint İstemci destek ekibiyle](https://www.deltek.com/about/contact-us) birlikte çalışın. Tek oturum açma kullanabilmeleri için kullanıcının oluşturulması ve etkinleştirilmesi gerekir.

Kullanıcı oluşturulduktan sonra, kullanıcının **Kimlik Doğrulama Yöntemi** seçimi Active **Directory**olmalıdır, **SAML Tek Oturum Açma** onay kutusu seçilmeli ve Azure Active Directory'deki kullanıcı adı Active **Directory veya Sertifika Kimliği** olmalıdır (aşağıdaki ekran görüntüsünde gösterilir).

![Costpoint kullanıcısı](./media/costpoint-tutorial/costpoint-user.png)

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Costpoint döşemesini seçtiğinizde, SSO'yu ayarladığınız için Costpoint uygulamasında otomatik olarak oturum açmış olmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme eğitimleri listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
