---
title: 'Öğretici: LinkedIn Elevate ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LinkedIn Yükseltme arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03efae5f9dec904f141a6776766850aa1f328892
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74892184"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-elevate"></a>Öğretici: LinkedIn Elevate ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, LinkedIn Yükseltme'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. LinkedIn Yükseltme'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* LinkedIn Yükseltme'ye erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla LinkedIn Yükseltme'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* LinkedIn Yükseltme tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.



* LinkedIn **Elevate, SP ve IDP'nin** SSO'ya başlattığı destekler
* LinkedIn Elevate **Just In Time** kullanıcı sağlama destekler
* LinkedIn Elevate [ **Otomatik** kullanıcı sağlama yı destekler](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>Galeriden LinkedIn Yükseltme ekleme

LinkedIn Yükseltme'nin Azure AD'ye tümleştirilmesini yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden LinkedIn Yükseltme eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **LinkedIn Yükseltme** yazın.
1. Sonuçlar panelinden **LinkedIn Yükseltme'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-elevate"></a>LinkedIn Yükseltme için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak LinkedIn Yükseltme ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile LinkedIn Yükseltme'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu LinkedIn Yükseltme ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[LinkedIn Elevate SSO'yu yapılandırır](#configure-linkedin-elevate-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[LinkedIn Yükseltme test kullanıcısını oluşturun](#create-linkedin-elevate-test-user)** - LinkedIn Elevate'de kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **LinkedIn Yükseltme** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, **Entity ID** değerini girin, bu öğreticide daha sonra açıklanan Linkedin Portalı'ndan Entity ID değerini kopyalayaceksiniz.

    b. **Yanıtla URL** metin kutusuna, **Assertion Consumer Access (ACS) Url** değerini girin, bu öğreticide daha sonra açıklanan Linkedin Portalı'ndan Assertion Consumer Access (ACS) Url değerini kopyalayaceksiniz.

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

1. LinkedIn Yükseltme uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. LinkedIn Yükseltme uygulaması nameidentifier **user.mail**ile eşlenir bekliyor , bu yüzden düzenleme simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşlemi değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, LinkedIn Yükseltme uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | -------| -------------|
    | bölüm | user.department |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **LinkedIn Yükseltmeyi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın LinkedIn Elevate'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **LinkedIn Yükseltme'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-linkedin-elevate-sso"></a>LinkedIn Yükseltme SSO'su yapılandır

1. Farklı bir web tarayıcısı penceresinde, LinkedIn Elevate kiracınızı yönetici olarak oturum açın.

1. **Hesap Merkezi'nde** **Ayarlar**altında **Genel Ayarlar'ı** tıklatın. Ayrıca, açılan listeden **Yükselt - AAD Testini** Yükselt'i'yi seçin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Formdan tek tek alanları yüklemek ve kopyalamak ve aşağıdaki adımları gerçekleştirmek **için OR'a tıklayın:**

    ![Tek İşaret-On'u Yapılandır](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    a. **Varlık Kimliğini** kopyalayın ve Azure portalındaki **Temel SAML** Yapılandırması'ndaki **Tanımlayıcı** metin kutusuna yapıştırın.

    b. **İddia Tüketici Erişimi (ACS) Url'sini** kopyalayın ve Azure portalındaki Temel **SAML** Yapılandırması'ndaki **YanıtLA URL** metin kutusuna yapıştırın.

1. **LinkedIn Yönetici Ayarları** bölümüne gidin. Azure portalından indirdiğiniz XML dosyasını Upload XML dosya seçeneğine tıklayarak yükleyin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. SSO'ya etkinleştirmek için **Üzerine** tıklayın. SSO durumu **Bağlı Değil'ten** **Bağlı'ya** değişecek

    ![Tek İşaret-On'u Yapılandır](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-elevate-test-user"></a>LinkedIn Yükseltme test kullanıcını oluşturun

LinkedIn Yükseltme Uygulaması tam zamanında kullanıcı sağlamayı destekler ve kimlik doğrulamadan sonra kullanıcılar uygulamada otomatik olarak oluşturulur. LinkedIn Yükseltme portalındaki yönetici ayarları sayfasında anahtarı otomatik olarak devre dışı bırakarak yalnızca zaman sağlama da etkine **lisans atayın** ve bu da kullanıcıya bir lisans atayacaktır. LinkedIn Yükseltme de otomatik kullanıcı sağlama destekler, [otomatik](linkedinelevate-provisioning-tutorial.md) kullanıcı sağlama yapılandırmak için nasıl daha fazla bilgi burada bulabilirsiniz.

   ![Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LinkedIn Yükseltme döşemesini tıklattığınızda, SSO'yu kurduğunuz LinkedIn Yükseltme'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile LinkedIn Yükseltme'yi deneyin](https://aad.portal.azure.com/)

