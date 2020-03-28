---
title: 'Öğretici: IamIP Patent Platformu ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve IamIP Patent Platformu arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5b4fc1-e8fd-4418-a369-189272fef80d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7d487aaf7ba4aaf666962cf91ca86d46115055b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78190764"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-iamip-patent-platform"></a>Öğretici: IamIP Patent Platformu ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, IamIP Patent Platform'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. IamIP Patent Platform'u Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* IamIP Patent Platformu'na kimlerin erişebileceğini denetlemek için Azure AD'yi kullanın.
* Kullanıcılarınızın Azure AD hesaplarıyla IamIP Patent Platformu'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)oturum açma'ya bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) özellikli bir IamIP Patent Platformu aboneliği.

## <a name="tutorial-description"></a>Öğretici açıklama

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandıracak ve sınayacaksınız.

IamIP Patent Platformu, SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yı destekler.

IamIP Patent Platformunu yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)


## <a name="add-iamip-patent-platform-from-the-gallery"></a>Galeriden IamIP Patent Platformu ekleyin

IamIP Patent Platformu'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize IamIP Patent Platformu eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com) bir iş veya okul hesabıyla veya kişisel bir Microsoft hesabıyla oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.
1. Kurumsal **uygulamalara** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **IamIP Patent Platformu** yazın.
1. Sonuç panelinden **IamIP Patent Platformu'nu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-iamip-patent-platform"></a>IamIP Patent Platformu için Azure AD SSO'u yapılandırın ve test edin

Azure AD SSO'nu IamIP Patent Platformu ile B.Simon adlı bir test kullanıcısı kullanarak yapılandırAcak ve test edeceksiniz. SSO'nun çalışması için, IamIP Patent Platformu'nda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'u IamIP Patent Platformu ile yapılandırmak ve test etmek için şu üst düzey adımları atacaksınız:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'yu yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * Kullanıcının Azure AD tek oturum açma kullanmasını sağlamak için **[test kullanıcısına erişim izni ver.](#grant-access-to-the-test-user)**

1. Uygulama tarafında **[IamIP Patent Platformu SSO'yu yapılandırın.](#configure-iamip-patent-platform-sso)**
    * **[Bir IamIP Patent Platformu test kullanıcısını,](#create-iamip-patent-platform-test-user)** kullanıcının Azure AD temsilinin karşılığı olarak oluşturun.

1. Yapılandırmanın çalıştığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **IamIP Patent Platformu** uygulama tümleştirme sayfasında, **Yönet** **bölümünde, tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem düğmesini seçin:

   ![Temel SAML Yapılandırması için kalem düğmesi](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, bir Servis Sağlayıcı meta veri dosyanız varsa ve SSO'yu IDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin:

    a. **Meta veri dosyalarını yükle'yi**seçin:

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Klasör düğmesini seçin, meta veri dosyasını seçin ve sonra **Yükle'yi**seçin:

    ![Klasör ve Yükle düğmeleri](common/browse-upload-metadata.png)

    c. Meta veri dosyası yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri **Temel SAML Yapılandırması** bölümünde otomatik olarak doldurulur:

    ![Tanımlayıcı ve Yanıt URL değerleri](common/idp-intiated.png)

    > [!Note]
    > **Tanımlayıcı** ve **YanıtURL** değerleri otomatik olarak doldurulmuyorsa, değerleri gereksinimlerinize göre el ile verin.

1. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL'ler** ayarla'yı seçin ve aşağıdaki adımı tamamlayın:

    Oturum **Aç URL** kutusuna **https\/girin: /patents.iamip.com/login-user**.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı indirmek ve bilgisayarınıza kaydetmek için Sertifika **(Raw)** **İndir** bağlantısını seçin:

    ![Sertifika indirme linki](common/certificateraw.png)

1. **IamIP Patent Platformu'nu Kur** bölümünde, gereksinimlerinize göre uygun URL'yi veya URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/idp-intiated.png))

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol bölmesinde **Azure Etkin Dizini'ni**seçin. **Kullanıcıları**seçin ve ardından **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları tamamlayın:
   1. **Ad** kutusuna, **B.Simon**girin.  
   1. Kullanıcı **adı** kutusuna, \<>@\<companydomain> kullanıcı adını girin. \<uzantısı>. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**seçin.

### <a name="grant-access-to-the-test-user"></a>Test kullanıcısına erişim izni verme

Bu bölümde, B.Simon'ın kullanıcıya IamIP Patent Platformu'na erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin ve ardından **Tüm uygulamaları**seçin.
1. Başvuru listesinde **IamIP Patent Platformu'nu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** **bölümünde, Kullanıcıları ve grupları**seçin:

   ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin:

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

## <a name="configure-iamip-patent-platform-sso"></a>Yapılaşı IamIP Patent Platformu SSO

IamIP Patent Platformu tarafında tek oturum açma yapılandırmak için, indirilen ham sertifikayı ve Azure portalından kopyaladığınız uygun URL'leri [IamIP Patent Platformu destek ekibine](mailto:info@iamip.com)göndermeniz gerekir. SAML SSO bağlantısını her iki tarafta da doğru olacak şekilde yapılandırırlar.

### <a name="create-iamip-patent-platform-test-user"></a>IamIP Patent Platformu test kullanıcı oluşturun

[IamIP Patent Platformu destek ekibi ile IamIP](mailto:info@iamip.com) Patent Platformu B.Simon adlı bir kullanıcı eklemek için çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panel'i kullanarak Azure AD SSO yapılandırmanızı sınayacaksınız.

Access Panel'deki IamIP Patent Platformu döşemesini seçtiğinizde, Otomatik olarak SSO'yu kurduğunuz IamIP Patent Platformu örneğine giriş yapmıştır. Access Panel hakkında daha fazla bilgi için erişim [paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)için bkz.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile IamIP Patent Platform'u deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
