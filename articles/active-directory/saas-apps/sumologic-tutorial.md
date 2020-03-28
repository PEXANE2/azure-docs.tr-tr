---
title: 'Öğretici: SumoLogic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SumoLogic arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de6d941c6efe42993b6bad7c556582831d179250
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75659755"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sumologic"></a>Öğretici: SumoLogic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SumoLogic'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SumoLogic'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SumoLogic erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla SumoLogic'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SumoLogic tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SumoLogic **IDP** başlatılan SSO destekler

## <a name="adding-sumologic-from-the-gallery"></a>Galeriden SumoLogic ekleme

SumoLogic'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize SumoLogic eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **SumoLogic** yazın.
1. Sonuç panelinden **SumoLogic'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sumologic"></a>SumoLogic için Azure AD tek oturum açma yı yapılandırma ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak SumoLogic ile yapılandırın ve test edin. SSO'nun çalışması için, SumoLogic'teki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SumoLogic ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[SumoLogic SSO'yu yapılandırın](#configure-sumologic-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[SumoLogic test kullanıcısını oluşturun](#create-sumologic-test-user)** - Kullanıcının Azure AD gösterimine bağlı SumoLogic'te B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **SumoLogic** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://service.sumologic.com`|
    | `https://<tenantname>.us2.sumologic.com`|
    | `https://<tenantname>.us4.sumologic.com`|
    | `https://<tenantname>.eu.sumologic.com`|
    | `https://<tenantname>.jp.sumologic.com`|
    | `https://<tenantname>.de.sumologic.com`|
    | `https://<tenantname>.ca.sumologic.com`|

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://service.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.us2.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.us4.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.eu.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.jp.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.de.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.ca.sumologic.com/sumo/saml/consume/<tenantname>`|
    | `https://service.au.sumologic.com/sumo/saml/consume/<tenantname>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [SumoLogic Client destek ekibine](https://www.sumologic.com/contact-us/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. SumoLogic uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, SumoLogic uygulaması aşağıda gösterilen SAML yanıtı geri geçirilecek birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    |  Adı | Kaynak Özniteliği |
    | ---------------| --------------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Roller | user.assignedroles |

    > [!NOTE]
    > Azure AD'de **Rolü** nasıl yapılandıracağınıöğrenmek için [lütfen buraya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) tıklayın.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SumoLogic'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın SumoLogic'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SumoLogic'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sumologic-sso"></a>SumoLogic SSO'yu yapılandırın

1. Farklı bir web tarayıcısı penceresinde, SumoLogic şirket sitenizde yönetici olarak oturum açın.

1. Güvenliği ** \> Yönet'e**gidin.

    ![Yönetme](./media/sumologic-tutorial/ic778556.png "Yönetme")

1. **SAML'yi**tıklatın.

    ![Genel güvenlik ayarları](./media/sumologic-tutorial/ic778557.png "Genel güvenlik ayarları")

1. Yapılandırma **yı seç veya yeni bir** liste oluşturun, Azure **AD'yi**seçin ve sonra **Yapıla'yı**tıklatın.

    ![SAML 2.0'ı yapılandır](./media/sumologic-tutorial/ic778558.png "SAML 2.0'ı yapılandır")

1. **Yapıl SAML 2.0** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![SAML 2.0'ı yapılandır](./media/sumologic-tutorial/ic778559.png "SAML 2.0'ı yapılandır")

    a. Yapılandırma **Adı** metin kutusuna **Azure AD**yazın.

    b. **Hata Ayıklama Modu'nu**seçin.

    c. **Veren** metin kutusunda, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    d. **Authn İstek URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    e. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından tüm Sertifikayı **X.509 Sertifika** metin kutusuna yapıştırın.

    f. **E-posta Özniteliği**olarak, **SAML konusunu kullan'ı**seçin.  

    g. **SP başlatılan Giriş Yapılandırması'nı**seçin.

    h. Giriş **Yolu** metin kutusunda **Azure** yazın ve **Kaydet'i**tıklatın.

### <a name="create-sumologic-test-user"></a>SumoLogic test kullanıcısı oluşturma

Azure AD kullanıcılarının SumoLogic'te oturum açabilmeleri için SumoLogic'e dahil olmaları gerekir. SumoLogic durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **SumoLogic** kiracınıza oturum açın.

1. Kullanıcıları ** \> Yönet'e**gidin.

    ![Kullanıcılar](./media/sumologic-tutorial/ic778561.png "Kullanıcılar")

1. **Ekle**’ye tıklayın.

    ![Kullanıcılar](./media/sumologic-tutorial/ic778562.png "Kullanıcılar")

1. Yeni **Kullanıcı** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/sumologic-tutorial/ic778563.png "Yeni Kullanıcı")

    a. **İlk Ad,** **Soyadı**ve **E-posta** metin kutularına sağlamak istediğiniz Azure REKLAM hesabının ilgili bilgilerini yazın.
  
    b. Bir rol seçin.
  
    c. **Durum**olarak, **Etkin'i**seçin.
  
    d. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için SumoLogic tarafından sağlanan diğer SumoLogic kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SumoLogic döşemesini tıklattığınızda, SSO'yu kurduğunuz SumoLogic'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SumoLogic'i deneyin](https://aad.portal.azure.com/)