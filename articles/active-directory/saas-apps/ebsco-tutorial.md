---
title: 'Öğretici: EBSCO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve EBSCO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f37085744b9a0e7785ef3a411d53e4df5d15e494
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72595020"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ebsco"></a>Öğretici: EBSCO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, EBSCO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. EBSCO'nun Azure AD ile tümleştirildiğinde şunları yapabilirsiniz:

* EBSCO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla EBSCO'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* EBSCO tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* EBSCO, **SP ve IDP'nin** başlattığı SSO'ya destek verdi
* EBSCO **Just In Time** kullanıcı sağlama destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-ebsco-from-the-gallery"></a>Galeriden EBSCO ekleme

EBSCO'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize EBSCO eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **EBSCO** yazın.
1. Sonuç panelinden **EBSCO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-ebsco"></a>EBSCO için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak EBSCO ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile EBSCO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu EBSCO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[EBSCO SSO'yu yapılandırır](#configure-ebsco-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[EBSCO test kullanıcısını oluşturun](#create-ebsco-test-user)** - Kullanıcının Azure AD gösterimine bağlı EBSCO'daki B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **EBSCO** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Tanımlayıcı** metin kutusuna bir URL yazın:`pingsso.ebscohost.com`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > Oturum açma URL değeri gerçek değildir. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [EBSCO İstemci destek ekibine](mailto:support@ebsco.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

    o **Benzersiz unsurlar:**  

    o **Custid** = Benzersiz EBSCO müşteri kimliğini girin 

    o **Profil** = İstemciler bağlantıyı belirli bir profile (EBSCO'dan ne satın aldıklarına bağlı olarak) yönlendirecek şekilde uyarlayabilirler. Belirli bir profil kimliği girebilirler. Ana dis (EBSCO Discovery Service) ve ehost (EBSOCOhost veritabanları) vardır. Aynı talimatlar [burada](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile)verilmiştir.

1. EBSCO uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

    > [!Note]
    > **Ad** özniteliği zorunludur ve EBSCO uygulamasında **Ad Tanımlayıcı değeri** ile eşlenir. Bu, varsayılan olarak eklenir, bu nedenle bunu el ile eklemenize gerek yoktur.

1. Yukarıdakilere ek olarak, EBSCO uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ---------------| --------------- |
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | kullanıcı.posta |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **EBSCO'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın EBSCO'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **EBSCO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-ebsco-sso"></a>EBSCO SSO'nun yapılandırılsın

**EBSCO** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'ini** ve uygun kopyalanmış URL'lerini Azure portalından [EBSCO destek ekibine](mailto:support@ebsco.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-ebsco-test-user"></a>EBSCO test kullanıcısı oluşturma

EBSCO durumunda, kullanıcı sağlama otomatiktir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

Azure AD, gerekli verileri EBSCO uygulamasına aktarıyor. EBSCO'nun kullanıcı sağlama otomatik veya bir kerelik form gerektirir. Bu, istemcinin kişisel ayarları kaydedilmiş önceden varolan EBSCOhost hesapları bir sürü olup olmadığına bağlıdır. Aynı uygulama sırasında [EBSCO destek ekibi](mailto:support@ebsco.com) ile tartışılabilir. Her iki şekilde de, istemci sınama önce herhangi bir EBSCOhost hesapları oluşturmak zorunda değildir.

   > [!Note]
   > EBSCOhost kullanıcı sağlama/kişiselleştirme otomatikleştirebilirsiniz. Just-In-Time kullanıcı sağlama hakkında [EBSCO destek ekibine](mailto:support@ebsco.com) başvurun.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

1. Erişim Paneli'ndeki EBSCO döşemesini tıklattığınızda, EBSCO uygulamanızda otomatik olarak oturum açmış olmalısınız.
Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../user-help/active-directory-saas-access-panel-introduction.md)bakın.

1. Uygulamaya giriş yaptıktan sonra sağ üst köşedeki **oturum açma** düğmesine tıklayın.

    ![EBSCO Başvuru listesinde oturum açma](./media/ebsco-tutorial/tutorial_ebsco_signin.png)

1. Kurumsal/SAML oturum unu bir Bağlantı ile eşleştirmek için bir kerelik bir istek alacaksınız **mevcut MyEBSCOhost hesabınızı şimdi kurum hesabınıza** bağlayacak veya **yeni bir MyEBSCOhost hesabı oluşturun ve kurum hesabınıza bağla.** Hesap EBSCOhost uygulamasında kişiselleştirme için kullanılır. **Yeni bir hesap oluştur** seçeneğini seçin ve kişiselleştirme formunun aşağıdaki ekran görüntüsünde gösterildiği gibi saml yanıtındaki değerlerle önceden tamamlandığını göreceksiniz. Bu seçimi kaydetmek için **'Devam Et'i** tıklatın.
    
     ![Uygulamalar listesindeki EBSCO kullanıcısı](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Yukarıdaki kurulumu tamamladıktan sonra çerezleri/önbelleği temizleyin ve yeniden giriş yapın. Yeniden oturum açmanız gerekmez ve kişiselleştirme ayarları hatırlanır.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile EBSCO'u deneyin](https://aad.portal.azure.com/)