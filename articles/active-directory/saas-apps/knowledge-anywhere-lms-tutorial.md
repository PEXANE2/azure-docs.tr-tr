---
title: 'Öğretici: Bilgi Anywhere LMS ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Bilgi Anywhere LMS arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f44324bbdd5af6675dfb4f5664cbbde2627edfec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67098554"
---
# <a name="tutorial-integrate-knowledge-anywhere-lms-with-azure-active-directory"></a>Öğretici: Bilgiyi Azure Active Directory ile Her Yerde LMS'yi Entegre Edin

Bu eğitimde, Bilgi Her Yerde LMS'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Bilgi Anywhere LMS'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* LmS Bilgisi'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Bilgi Alanı LMS'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Knowledge Anywhere LMS tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Knowledge Anywhere **LMS, SP** tarafından başlatılan SSO'yı destekler ve **Just In Time** kullanıcı sağlamayı destekler.

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Galeriden Bilgi Her Yerde LMS Ekleme

Bilgi Anywhere LMS'nin Azure AD'ye entegrasyonunu yapılandırmak için, galerideki Bilgi Anywhere LMS'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Bilgi Anywhere LMS** yazın.
1. Sonuç panelinden **Bilgi Anywhere LMS'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B. Simon**adlı bir test kullanıcısı kullanarak Bilgi Anywhere LMS ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Bilgi Anywhere LMS'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Bilgi Anywhere LMS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için Bilgi Nin **[Her Yerde LMS'yi yapılandırın.](#configure-knowledge-anywhere-lms)**
3. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı Bilgi Anywhere LMS'de B. Simon'ın bir örneğine sahip olmak için **[Bilgi Her Yerde LMS test kullanıcısı oluşturun.](#create-knowledge-anywhere-lms-test-user)**
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Bilgi Anywhere LMS** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    1. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<CLIENTNAME>.knowledgeanywhere.com/`

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek Tanımlayıcı ve Yanıt URL'si ile güncelleştirin.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<CLIENTNAME>.knowledgeanywhere.com/`

    > [!NOTE]
    > Oturum açma URL değeri gerçek değildir. Bu değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değeri elde etmek için Bilgi [Her Yerde LMS İstemci destek ekibine](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Bilgi **Her Yerde LMS'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-knowledge-anywhere-lms"></a>Bilgiyi Her Yerde YapılandırLMS

1. Bilgi Anywhere LMS içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **LmS'nin Her Yerde Kurulum Bilgisi'ni** tıklattıktan sonra sizi Bilgi Anywhere LMS uygulamasına yönlendirecektir. Buradan, Bilgi Anywhere LMS oturum için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Bilgi Anywhere LMS'yi el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Yönetici olarak Bilgi Anywhere LMS şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

4. **Site** sekmesinde seçin.

    ![Bilgi Anywhere LMS Yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure1.png)

5. **SAML Ayarları** sekmesinde seçin.

    ![Bilgi Anywhere LMS Yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure2.png)

6. **Yeni Ekle'ye**tıklayın.

    ![Bilgi Anywhere LMS Yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure3.png)

7. **SAML Ayarları Ekle/Güncelle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Bilgi Anywhere LMS Yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure4.png)

    a. Kuruluşunuza göre IDP Adını girin. Eski için:- `Azure`.

    b. **IDP Entity ID** textbox'ına Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. **IDP URL** textbox'ına, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. İndirilen sertifika dosyasını Azure portalından not defterine açın, sertifikanın içeriğini kopyalayın ve **Sertifika** metin kutusuna yapıştırın.

    e. Oturum **Açma URL** metin kutusunda, Azure portalından kopyaladığınız **Logout URL** değerini yapıştırın.

    f. **Alan Adı**için açılan sayfadan **Ana Site'yi** seçin.

    g. **SP Entity ID** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

    h. **SP Response(ACS) URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **YanıtURL** metin kutusuna yapıştırın.

    i. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B. Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın Bilgi Anywhere LMS'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Bilgi **Anywhere LMS'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B. Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Bilgi Her Yerde LmS test kullanıcısı oluşturun

Bu bölümde, B. Simon adlı bir kullanıcı Bilgi Anywhere LMS oluşturulur. Knowledge Anywhere LMS, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Bilgi Anywhere LMS'de zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Bilgi Anywhere LMS döşemesini seçtiğinizde, SSO'yu kurduğunuz Bilgi Anywhere LMS'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)