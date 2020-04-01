---
title: 'Öğretici: DocuSign ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve DocuSign arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01f969c3bc6f546025b3bbe5826181efdfa69be0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76983670"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Öğretici: DocuSign ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, DocuSign'ı Microsoft Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. DocuSign'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* DocuSign'a kimlerin erişebilenleri denetlemek için Azure AD'yi kullanın.
* Kullanıcılarınız için Azure AD hesapları aracılığıyla DocuSign'da otomatik oturum açma olanağı nı etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure AD'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) etkin bir DocuSign aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, azure AD SSO'u bir test ortamında yapılandıracak ve test edeceksiniz:

* DocuSign, hizmet sağlayıcısını (SP) başlatılan SSO'ya destekler.

* DocuSign *tam zamanında* kullanıcı sağlamayı destekler.

* DocuSign [otomatik kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial)destekler.
* DocuSign'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-docusign-from-the-gallery"></a>Galeriden DocuSign ekleme

DocuSign'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize DocuSign eklemeniz gerekir:

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak oturum açın.
1. Soldaki gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **DocuSign** yazın.
1. Sonuç panelinden **DocuSign'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>DocuSign için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu DocuSign ile **B.Simon**adında bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile DocuSign'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu DocuSign ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanabilmesi için [Azure AD SSO'su](#configure-azure-ad-sso) yapılandırın.
    1. Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için [DocuSign SSO'yu yapılandırın.](#configure-docusign-sso)
    1. DocuSign'da kullanıcının Azure AD gösterimine bağlı b.Simon'Un bir örneğini oluşturmak için [bir DocuSign test kullanıcısı oluşturun.](#create-docusign-test-user)
1. Yapılandırmanın çalıştığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com/), **DocuSign** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve ardından **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları izleyin:

    a. **URL'de Oturum Aç** kutusuna aşağıdaki deseni kullanarak bir URL girin:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. Tanımlayıcı **(Entity ID)** kutusuna aşağıdaki deseni kullanarak bir URL girin:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Bu parantezli değerler yer tutucudur. Bunları gerçek oturum açma URL'si ve Tanımlayıcı'daki değerlerle değiştirin. Bu ayrıntılar daha sonra bu öğreticide "SamL 2.0 Uç Noktalarını Görüntüle" bölümünde açıklanmıştır.

1. **SAML sayfasıyla tek oturum** açma'da, **SAML İmza Sertifikası** bölümünde **Sertifikayı (Base64)** bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **DocuSign'ı ayarla** bölümünde, gereksinimlerinize göre uygun URL'yi (veya URL'leri) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol bölmesinde **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında Yeni **kullanıcıyı**seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına, **B.Simon**girin.  
   1. Kullanıcı **adı** alanına girin. `<username>@<companydomain>.<extension>` Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değere dikkat edin.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, b.Simon'a DocuSign'a erişim izni verirsiniz, böylece bu kullanıcı Azure tek oturum açma'yı kullanabilir.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **DocuSign'ı**seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar **ve gruplar**seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesine basın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesine basın.
1. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

## <a name="configure-docusign-sso"></a>DocuSign SSO'ya yapılandır

1. DocuSign'daki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'i seçerek Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Uzantıyı tarayıcıya ekledikten sonra **Kurulum DocuSign'ı**seçin. DocuSign uygulamasına yönlendirildiniz. Buradan, DocuSign'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı otomatik olarak yapılandırır ve 3'ten 5'e kadar olan adımları otomatikleştirir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. DocuSign'ı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Yönetici olarak DocuSign şirket sitenizde oturum açın.

4. Sayfanın sağ üst köşesinde profil logosunu seçin ve ardından **Yöneticiye Git'i**seçin.
  
    ![Profil altında Yönetici'ye git][51]

5. Etki alanı çözümleri sayfanızda **Etki Alanları'nı**seçin.

    ![Etki Alanı Çözümleri/Etki Alanları][50]

6. Etki **Alanları** **bölümünde, CLAIM DOMAIN'i**seçin.

    ![Alan Adı Talep Seçeneği][52]

7. Etki **Alanı Talep** iletişim kutusunda, **Etki Alanı Adı** kutusunda, şirket etki alanınızı yazın ve ardından **CLAIM'i**seçin. Etki alanını ve durumunun etkin olduğundan emin olun.

    ![Etki Alanı/Etki Alanı Adı iletişim kutusu talep edin][53]

8. Etki alanı çözümleri sayfasında **Kimlik Sağlayıcıları'nı**seçin.
  
    ![Kimlik Sağlayıcılar seçeneği][54]

9. Kimlik **Sağlayıcıları** bölümünde, **KİmLİk SAGLAYICI EKLE'yi**seçin.

    ![Kimlik Sağlayıcı ekle seçeneği][55]

10. Kimlik **Sağlayıcı Ayarları** sayfasında aşağıdaki adımları izleyin:

    ![Kimlik Sağlayıcı Ayarları alanları][56]

    a. **Ad** kutusuna yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. Kimlik **Sağlayıcı Veren kutusuna,** Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. Kimlik **Sağlayıcı Giriş URL** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. Kimlik **Sağlayıcı Giriş URL** kutusuna, Azure portalından kopyaladığınız **Logout URL**değerini yapıştırın.

    e. **Sign AuthN isteğini**seçin.

    f. **Tarafından AuthN isteği gönder**, **POST**seçin .

    g. **Giriş gönder isteği**için **GET'i**seçin.

    h. Özel **Öznitelik Eşleme** **bölümünde, Yenİ EŞLEME EKLE'yi**seçin.

       ![Özel Öznitelik Haritalama Kullanıcı Birası][62]

    i. Azure AD talebiyle eşlenebilmek istediğiniz alanı seçin. Bu örnekte, **e-posta adresi** iddiası değeri `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`ile eşlenir. Bu, e-posta talebi için Azure AD'nin varsayılan talep adıdır. **KAYDET'i**seçin.

       ![Özel Öznitelik Eşleme alanları][57]

       > [!NOTE]
       > Kullanıcıyı Azure AD'den DocuSign kullanıcı eşlenesine kadar eşlemek için uygun **Kullanıcı tanımlayıcısını** kullanın. Uygun alanı seçin ve kuruluş ayarlarınızı temel alan uygun değeri girin.

    j. Kimlik **Sağlayıcı Sertifikaları** **bölümünde, EKLE SERTIFIKASı'nı**seçin, Azure AD portalından indirdiğiniz sertifikayı yükleyin ve **KAYDET'i**seçin.

       ![Kimlik Sağlayıcı Sertifikaları/Sertifika Ekle][58]

    k. Kimlik **Sağlayıcıları** bölümünde **EYLEMLER'i**seçin ve **ardından Bitiş Noktaları'nı**seçin.

       ![Kimlik Sağlayıcılar/Uç Noktalar][59]

    l. DocuSign yönetici portalının **SAML 2.0 Bitiş Noktaları** bölümünde aşağıdaki adımları izleyin:
       1. Servis **Sağlayıcı Veren URL'yi**kopyalayın ve azure portalındaki Temel **SAML Yapılandırması** bölümündeki **Tanımlayıcı** kutusuna yapıştırın.

       1. Servis **Sağlayıcı Giriş URL'sini**kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Oturum Aç URL** kutusuna yapıştırın.

       1. **Kapat**'ı seçin.

       ![SAML 2.0 Uç Noktaları görüntüle][60]

### <a name="create-docusign-test-user"></a>DocuSign test kullanıcısını oluşturma

Bu bölümde, DocuSign'da B.Simon adında bir kullanıcı oluşturulur. DocuSign, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı DocuSign'da zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [DocuSign destek ekibine](https://support.docusign.com/)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki DocuSign döşemesini seçtiğinizde, SSO'yu kurduğunuz DocuSign örneğinde otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure AD ile nasıl entegre edilebildiğini anlatan öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure AD'de uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure AD'de Koşullu Erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile DocuSign'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png