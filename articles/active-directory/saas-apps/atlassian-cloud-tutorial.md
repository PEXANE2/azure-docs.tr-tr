---
title: 'Öğretici: Atlasduyi bulutu ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Atlasduyi bulutu arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/04/2020
ms.author: jeedes
ms.openlocfilehash: 8548e7449056ecb1f211d5dd026544bf7e1375cc
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708198"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Öğretici: Atlasduyi bulutunu Azure Active Directory tümleştirin

Bu öğreticide, Atlasme bulutunu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Atlasme bulutunu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Atlasme bulutuna erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte bulunan bulut için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü edinebilirsiniz.
* Atlasme bulutu çoklu oturum açma (SSO) etkin aboneliği.
* Atlasme bulut ürünleri için Security Assertion Markup Language (SAML) çoklu oturum açmayı etkinleştirmek için, Atlasbir erişim ayarlamanız gerekir. [Atlasduyi erişimi]( https://www.atlassian.com/enterprise/cloud/identity-manager)hakkında daha fazla bilgi edinin.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. 

* Atlasme bulutu **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Atlasme bulutu [Otomatik Kullanıcı sağlamayı ve sağlamayı kaldırmayı](atlassian-cloud-provisioning-tutorial.md) destekler
* Atlasıı bulutunu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Galeriden Atlasme bulutu ekleme

Atlasduyi bulutu 'nın Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize atlasduyisi bulutu eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **atlasduyisi bulutu** yazın.
1. Sonuçlar panelinden **Atlasduyi bulutu** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Atlasbir Cloud Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Atlasduyi bulutu 'ndaki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Atlasduyi bulutu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD 'Yi Atlasme Cloud SSO Ile yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın, atlasduyi bulutu Ile Azure AD tabanlı SAML SSO 'yu kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Atlasbir **[Cloud test kullanıcısı oluşturun](#create-atlassian-cloud-test-user)** -kullanıcının Azure AD gösterimine bağlı olan atlasduyi bulutu 'nda B. Simon 'a karşılık gelen bir karşılığı vardır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Atlasduyi bulutu 'ndaki yapılandırmayı otomatikleştirmek için, **uzantıyı yüklemek**üzere **uygulamalar güvenli oturum açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **atlasme bulutunu ayarla** ' ya tıklayarak sizi atlasme bulut uygulamasına yönlendirirsiniz. Buradan, Atlasduyi bulutu 'nda oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracaktır.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Atlasduyi bulutu 'nı el ile ayarlamak istiyorsanız, Atlasme bulut şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin.

1. Başlamadan önce Atlasbir ürün örneğinize gidip örnek URL 'sini kopyalayıp kaydedin
   > [!NOTE]
   > URL, modele sığması gerekir `https://<instancename>.atlassian.net`

   ![image](./media/atlassian-cloud-tutorial/get-atlassian-instance-name.png)

1. [Atlasme Yönetici portalını](https://admin.atlassian.com/) açın ve kuruluşunuzun adına tıklayın

   ![image](./media/atlassian-cloud-tutorial/click-on-organization-in-atlassian-access.png)

1. Çoklu oturum açmayı yapılandırmadan önce etki alanınızı doğrulamanız gerekir. Daha fazla bilgi için bkz. [Atlasme etki alanı doğrulama](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belgesi.
1. Atlasme Yönetici portalı ekranından sol çekmeceden **güvenlik** ' i seçin

   ![image](./media/atlassian-cloud-tutorial/click-on-security-in-atlassian-access.png)

1. Atlas, Yönetici portalı güvenlik ekranından sol çekmeceden **SAML çoklu oturum açma** seçeneğini belirleyin

   ![image](./media/atlassian-cloud-tutorial/click-on-saml-sso-in-atlassian-access-security.png)

1. **SAML Yapılandırması Ekle** ' ye tıklayın ve sayfayı açık tutun

   ![image](./media/atlassian-cloud-tutorial/saml-configuration-in-atlassian-access-security-saml-sso.png)

   ![image](./media/atlassian-cloud-tutorial/add-saml-configuration.png)

1. [Azure Portal](https://portal.azure.com/), **Atlasme Cloud** Application Integration sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açmayı ayarla**' yı seçin.

   ![image](./media/atlassian-cloud-tutorial/set-up-sso.png)

1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.

   ![image](./media/atlassian-cloud-tutorial/saml-in-azure.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Atlasıı bulutu 'nı ayarlamak** için aşağı kaydırın
   
   a. **Yapılandırma URL 'lerine** tıklayın

   ![image](./media/atlassian-cloud-tutorial/configuration-urls.png)
   
   b. **Azure AD tanımlayıcı** değerini Azure Portal 'den kopyalayın, Atlasme 'de **KIMLIK sağlayıcısı varlık kimliği** metin kutusuna yapıştırın.
   
   c. Azure portal **oturum açma URL 'si** değerini, Atlasbir 'Da **KIMLIK sağlayıcısı SSO URL** metin kutusuna yapıştırın.

   ![image](./media/atlassian-cloud-tutorial/configuration-urls-azure.png)

   ![image](./media/atlassian-cloud-tutorial/entity-id-and-ss.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![image](./media/atlassian-cloud-tutorial/certificate.png)

   ![image](./media/atlassian-cloud-tutorial/certificate-1.png)

1. Atlasbir 'da SAML yapılandırması **ekleme/kaydetme**

1. Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak Istiyorsanız, Azure 'daki **SAML Ile çoklu oturum açmayı ayarlama** sayfasındaki **temel SAML yapılandırması** bölümünü düzenleyin ve atlasıı yönetici portalında **SAML çoklu oturum açma sayfasını** açın

   a. Atlasbir 'dan **SP VARLıK kimliği** değerini kopyalayın, Azure 'daki **tanımlayıcı (varlık kimliği)** kutusuna yapıştırın ve varsayılan olarak ayarlayın
   
   b. Atlasbir 'dan **SP onaylama tüketici hizmeti URL 'si** değerini kopyalayın, Azure 'DAKI **yanıt URL 'Si (onaylama tüketici hizmeti URL 'si)** kutusuna yapıştırın ve varsayılan olarak ayarlayın
   
   c. 1. adımda kopyaladığınız **örnek URL** değerini kopyalayın ve Azure 'Daki **geçiş durumu** kutusuna yapıştırın.

   ![image](./media/atlassian-cloud-tutorial/copy-urls.png)

   ![image](./media/atlassian-cloud-tutorial/edit-button.png)

   ![image](./media/atlassian-cloud-tutorial/urls.png)
   
1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız, Azure 'da **SAML Ile çoklu oturum açma ayarlama** sayfasındaki **temel SAML yapılandırması** bölümünü düzenleyin. **Örnek URL** 'nizi (1. adımdan) kopyalayın ve Azure 'Da **oturum açma URL 'si** kutusuna yapıştırın

   ![image](./media/atlassian-cloud-tutorial/edit-button.png)

   ![image](./media/atlassian-cloud-tutorial/sign-on-URL.png)
   
1. Atlasme bulut uygulamanız, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. **Düzenle** simgesine tıklayarak öznitelik eşlemesini düzenleyebilirsiniz. 

   ![image](./media/atlassian-cloud-tutorial/default-attributes.png)
   
   1. Microsoft 365 lisansıyla bir Azure AD kiracısı için öznitelik eşlemesi
      
      a. **Benzersiz kullanıcı tanımlayıcısı (ad kimliği)** talebine tıklayın

      ![image](./media/atlassian-cloud-tutorial/user-attributes-and-claims.png)
      
      b. Atlasme bulutu, **NameIdentifier** 'ın (**benzersiz kullanıcı tanımlayıcısı**) kullanıcının e-postasına (**Kullanıcı. email**) eşleştirilmesini bekliyor. **Kaynak özniteliğini** düzenleyin ve **Kullanıcı. Mail**olarak değiştirin. Talepte yapılan değişiklikleri kaydedin.

      ![image](./media/atlassian-cloud-tutorial/unique-user-identifier.png)
      
      c. Son öznitelik eşlemeleri aşağıdaki gibi görünmelidir.

      ![image](common/default-attributes.png)
      
   1. Microsoft 365 lisansı olmadan bir Azure AD kiracısı için öznitelik eşlemesi 

      a. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`Talebe tıklayın.

      ![image](./media/atlassian-cloud-tutorial/email-address.png)
         
      b. Azure, Azure AD kiracılarında oluşturulan kullanıcılara Microsoft 365 lisansı olmadan **Kullanıcı. Mail** özniteliğini doldurmamışken, bu gibi kullanıcı için e-postayı **userPrincipalName** özniteliğinde depolamaz. Atlasme bulutu, **NameIdentifier** 'ın (**benzersiz kullanıcı tanımlayıcısı**) kullanıcının e-postasına (**User. UserPrincipalName**) eşlenmesini bekliyor.  **Kaynak özniteliğini** düzenleyin ve **User. UserPrincipalName**olarak değiştirin. Talepte yapılan değişiklikleri kaydedin.

      ![image](./media/atlassian-cloud-tutorial/set-email.png)
         
      c. Son öznitelik eşlemeleri aşağıdaki gibi görünmelidir.

      ![image](common/default-attributes.png)
     
### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Atlasme bulutuna erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Atlasduyi bulutu**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](./media/atlassian-cloud-tutorial/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-atlassian-cloud-test-user"></a>Atlasduyi bulutu test kullanıcısı oluşturma

Azure AD kullanıcılarının atlasi bulutu 'nda oturum açmasını etkinleştirmek için aşağıdakileri yaparak Atlasme bulutunda Kullanıcı hesaplarını el ile sağlayın:

1. **Yönetim** bölmesinde, **Kullanıcılar**' ı seçin.

    ![Atlasme bulut kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

1. Atlasduyi bulutu 'nda bir kullanıcı oluşturmak için **kullanıcıyı davet et**' i seçin.

    ![Atlasduyi bulutu kullanıcısı oluşturma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

1. **E-posta adresi** kutusuna kullanıcının e-posta adresini girin ve ardından uygulama erişimini atayın.

    ![Atlasduyi bulutu kullanıcısı oluşturma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

1. Kullanıcıya e-posta Daveti göndermek için **kullanıcıları davet et**' i seçin. Kullanıcıya bir e-posta daveti gönderilir ve daveti kabul ettikten sonra Kullanıcı sistemde etkin olur.

> [!NOTE]
> **Kullanıcılar bölümünde** **toplu oluştur** düğmesini seçerek de kullanıcıları toplu olarak oluşturabilirsiniz.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Atlascu bulutu kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Atlascu bulutu 'nda otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Atlasme bulutu 'nı deneyin](https://aad.portal.azure.com/)

- [Gelişmiş görünürlük ve denetimlerle Atlasme bulutunu koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)