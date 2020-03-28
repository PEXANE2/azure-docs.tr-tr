---
title: 'Öğretici: Yardım İzci ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Help Scout arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b71ccbc6cfdb9d3d37fc46b0e932fa98eee2fb43
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159098"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Öğretici: Yardım İzci ile Azure Active Directory entegrasyonu

Bu eğitimde, Yardım İzci'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Yardım İzleyicisini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Yardım İzci'sine erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Scout'a (Tek Oturum Açma) Yardımcı Olmak için otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Help Scout ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Scout tek oturum açma özellikli aboneliğine yardım edin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Yardım Scout **SP ve IDP** SSO başlatılan destekler
* Yardım Scout **Just In Time** kullanıcı sağlama destekler

## <a name="adding-help-scout-from-the-gallery"></a>Galeriden Yardım İzci Ekleme

Yardım İzci'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Yardım İzci'sini eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Yardım İzci** yazın.
1. Sonuç panelinden **Yardım İzle'sini** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **B.Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemlerini Help Scout ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Help Scout'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Yardım İzci ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Yardım İzci SSO'yu yapılandırın.](#configure-help-scout-sso)**
    * **[Yardım İzci test kullanıcısını oluşturun](#create-help-scout-test-user)** - Kullanıcının Azure AD gösterimine bağlı Yardım İzci'sinde B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Yardım İzci ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Help Scout** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Scout Etki Alanı ve URL'lere tek oturum açma bilgilerine yardım edin](common/idp-intiated.png)

    a. **Tanımlayıcı,** Help Scout'un **Hedef Kitlesi URI (Servis Sağlayıcı Varlık Kimliği)**`urn:`

    b. **Yanıt** URL'si, Help Scout'un geri dönüş sonrası URL'si **(Assertion Consumer Service URL)**`https://` 

    > [!NOTE]
    > Bu URL'lerde değerler yalnızca gösteri içindir. Bu değerleri gerçek Yanıt URL'sinden ve Tanımlayıcı'dan güncelleştirmeniz gerekir. Bu değerleri, daha sonra öğreticide açıklanan Kimlik Doğrulama bölümünün altındaki **Tek Oturum** Açma sekmesinden alırsınız.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Scout Etki Alanı ve URL'lere tek oturum açma bilgilerine yardım edin](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna, aşağıdaki gibi bir URL yazın:`https://secure.helpscout.net/members/login/`

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Yardım **İzci'sini Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında B.Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **B.Simon**girin.
  
    b. Kullanıcı **adı** alanı **türünde\@B.Simon şirketinizetki alanı.uzantı**  
    Örneğin, B.Simon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Yardım İzci'sine erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **İzle'ye Yardım Et'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, Yardım **İzci'yi**seçin.

    ![Uygulamalar listesindeyardım scout bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda Kullanıcılar listesinde **B.Simon'u** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

## <a name="configure-help-scout-sso"></a>YapılaşıYardım Scout SSO

1. Help Scout içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Yardım İzci'sini Kur'a** tıklayın ve sizi Yardım İzci uygulamasına yönlendirecektir. Buradan, Yardım Scout'u oturum etmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Help Scout'u el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi penceresi açın ve Help Scout şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Üst menüden **Yönet'e** tıklayın ve açılan menüden **Şirket'i** seçin.

    ![Tek İşaret-On'u Yapılandır](./media/helpscout-tutorial/settings1.png)

1. Sol gezinti bölmesinden **Kimlik Doğrulama'yı** seçin.

    ![Tek İşaret-On'u Yapılandır](./media/helpscout-tutorial/settings2.png)

1. Bu, sizi SAML ayarları bölümüne götürür ve aşağıdaki adımları gerçekleştirir:

    ![Tek İşaret-On'u Yapılandır](./media/helpscout-tutorial/settings3.png)

    a. Geri **dönüş sonrası URL değerini (İddia Tüketici Hizmeti URL'si)** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Yanıtla URL** metin kutusuna değeri yapıştırın.

    b. Hedef **Kitle URI (Servis Sağlayıcı Varlık Kimliği)** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna değeri yapıştırın.

1. **SAML'yi etkinleştir'i** geçiş eve getirin ve aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/helpscout-tutorial/settings4.png)

    a. **Tek Oturum Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin**değerini yapıştırın.

    b. Azure portalından indirilen **Sertifikayı (Base64)** yüklemek için **Sertifika** yükle'yi tıklatın.

    c. Kuruluşunuzun e-posta etki alanını(lar) e.x.- `contoso.com` **E-posta Etki Alanları** metin kutusuna girin. Birden çok etki alanını virgülle ayırabilirsiniz. [Yardım İzci oturum açma sayfasında](https://secure.helpscout.net/members/login/) belirli bir etki alanına giren bir Yardım İzci Kullanıcısı veya Yöneticisi, kimlik bilgileriyle birlikte kimlik bilgilerini doğrulamak üzere Kimlik Sağlayıcısı'na yönlendirilecektir.

    d. Son olarak, Kullanıcıların sadece bu yöntem le Scout'a giriş yapmak istiyorsanız **Force SAML Sign-on'u** geçiş yapabilirsiniz. Yine de Yardım İzci kimlik bilgileriyle oturum açma seçeneğini bırakmak istiyorsanız, bu seçeneği değiştirebilirsiniz. Bu etkin olsa bile, bir Hesap Sahibi her zaman hesap parolasıyla Scout'a Yardım Etmek için oturum açabilir.

    e. **Kaydet**'e tıklayın.

### <a name="create-help-scout-test-user"></a>Yardım Scout test kullanıcısı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Yardım Scout oluşturulur. Yardım Scout varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlama destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Yardım İzci'sinde bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Yardım İzci döşemesini tıklattığınızda, SSO'yu kurduğunuz Yardım İzci'sinde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Yardım İzle'yi deneyin](https://aad.portal.azure.com/)