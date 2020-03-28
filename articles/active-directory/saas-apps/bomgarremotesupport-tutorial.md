---
title: 'Öğretici: BeyondTrust Uzaktan Destek ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve BeyondTrust Remote Support arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 193b163f-bdee-4974-b16d-777c51b991df
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff21c3ee7721c82232e668ddb9645895080cf79
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74081995"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-beyondtrust-remote-support"></a>Öğretici: BeyondTrust Remote Support ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, BeyondTrust Uzaktan Destek'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. BeyondTrust Uzaktan Destek'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* BeyondTrust Uzaktan Destek'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla BeyondTrust Uzaktan Destek'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* BeyondTrust Uzaktan Destek tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* BeyondTrust Uzaktan Destek **SP** başlatılan SSO destekler
* BeyondTrust Uzaktan Destek **Just In Time** kullanıcı sağlama destekler

## <a name="adding-beyondtrust-remote-support-from-the-gallery"></a>Galeriden BeyondTrust Uzaktan Destek Ekleme

BeyondTrust Uzaktan Destek'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize BeyondTrust Uzaktan Destek eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **BeyondTrust Uzaktan Destek** yazın.
1. Sonuç panelinden **BeyondTrust Uzaktan Destek'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-beyondtrust-remote-support"></a>BeyondTrust Uzaktan Destek için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu BeyondTrust Remote Support ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, BeyondTrust Uzaktan Destek'teki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu BeyondTrust Remote Support ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[BeyondTrust Uzaktan Destek SSO'yu yapılandırın](#configure-beyondtrust-remote-support-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[BeyondTrust Uzaktan Destek test kullanıcısını oluşturun](#create-beyondtrust-remote-support-test-user)** - Kullanıcının Azure AD gösterimine bağlı BeyondTrust Uzaktan Destek'te B.Simon'Un bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **BeyondTrust Uzaktan Destek** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<HOSTNAME>.bomgar.com/saml`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<HOSTNAME>.bomgar.com`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<HOSTNAME>.bomgar.com/saml/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri daha sonra öğretici de açıklanan alırsınız.

1. BeyondTrust Uzaktan Destek uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. BeyondTrust Uzaktan Destek uygulaması, yukarıda ek olarak, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------------| ----------|
    | Givenname | user.givenname |
    | Emailaddress | kullanıcı.posta |
    | Adı | user.userprincipalname |
    | Kullanıcı adı | user.userprincipalname |
    | Gruplar | user.groups |
    | Benzersiz Kullanıcı Tanımlayıcısı | user.userprincipalname |

    > [!NOTE]
    > BeyondTrust Uzaktan Destek uygulaması için Azure REKLAM Grupları atarken, 'Talepte döndürülen gruplar' seçeneğinin None'dan SecurityGroup'a değiştirilmesi gerekir. Gruplar, Nesne Kimliklerini olarak uygulamaya aktarılır. Azure AD Grubu'nun Nesne Kimliği, Azure Etkin Dizin arabirimindeki Özellikleri denetleyerek bulunabilir. Bu, Azure REKLAM Grupları'na başvuru yapmak ve doğru grup ilkelerine atamak için gerekli olacaktır.

1. Benzersiz Kullanıcı Tanımlayıcısı'nı ayarlarken, bu değer NameID-Format: **Persistent**olarak ayarlanmalıdır. Bunun, kullanıcıyı doğru tanımlamak ve izinler için doğru grup ilkelerine ilişkilendirmek için kalıcı bir tanımlayıcı olmasını şart görüyoruz. Benzersiz Kullanıcı Tanımlayıcı değerini yeniden leştirmek için **Kullanıcı Öznitelikleri & Talepler** iletişim kutusunu açmak için edit simgesini tıklatın.

1. **İddiayı Yönet** bölümünde, **Ad Tanımlayıcısını Seç'e** tıklayın ve değeri **Kalıcı** olarak ayarlayın ve **Kaydet'i**tıklatın.

    ![Kullanıcı Öznitelikleri ve İddiaları](./media/bomgarremotesupport-tutorial/attribute-unique-user-identifier.png)

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **BeyondTrust Uzaktan Destek Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, BeyondTrust Uzaktan Destek'e erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **BeyondTrust Uzaktan Destek'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-beyondtrust-remote-support-sso"></a>BeyondTrust Uzaktan Destek SSO'su yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak BeyondTrust Uzaktan Destek'te oturum açın.

1. **DURUM** menüsüne tıklayın ve URL'de **Tanımlayıcı**, **YanıtLA URL** ve **Oturum Aç'ı** kopyalayın ve bu değerleri Azure portalındaki **Temel SAML Yapılandırması** bölümünde kullanın.

    ![BeyondTrust Uzaktan Destek'i yapılandır](./media/bomgarremotesupport-tutorial/config-url-values.png)

1. support.example.com cihazınızın birincil ana bilgisayar adı `https://support.example.com/login` **support.example.com** olduğu ve idari kimlik bilgilerinizi kullanarak kimlik doğrulaması yaptığı BeyondTrust Uzaktan Destek /giriş arabirimine gidin.

1. Güvenlik **Güvenliği** > **Sağlayıcıları**& Kullanıcılara gidin.

1. Açılan menüde **SAML'yi** seçin ve **Sağlayıcı Oluştur** düğmesini tıklatın.

1. Kimlik Sağlayıcı Ayarları bölümünde, Kimlik Sağlayıcı Meta verilerini yükleme seçeneği vardır. Azure portalından indirdiğiniz Metadata XML dosyasını bulun ve **Yükle** düğmesini tıklatın. **Entity ID**, Tek Oturum Açma **Hizmeti URL'si** ve Sertifikası otomatik olarak yüklenir ve Protokol **Bağlama'nın** **HTTP POST**olarak değiştirilmesi gerekir. Aşağıdaki ekran görüntüsüne bakın:

    ![BeyondTrust Uzaktan Destek'i yapılandır](./media/bomgarremotesupport-tutorial/config-uploadfile.png)

### <a name="create-beyondtrust-remote-support-test-user"></a>BeyondTrust Uzaktan Destek test kullanıcıoluşturma

Kullanıcı Sağlama Ayarlarını burada yapılandıracağız. Bu bölümde kullanılan değerler, Azure portalındaki **Kullanıcı Öznitelikleri & Talepler** bölümünden başvurulacaktır. Bunu, oluşturuldurunan anda zaten içe aktarılan varsayılan değerler olarak yapılandırıldık, ancak gerekirse değer özelleştirilebilir.

![Kullanıcı Oluşturma](./media/bomgarremotesupport-tutorial/config-user1.png)

> [!NOTE]
> Gruplar ve e-posta özniteliği bu uygulama için gerekli değildir. Azure AD gruplarını kullanarak ve bunları izinler için BeyondTrust Uzaktan Destek Grubu İlkeleri'ne atayacaksa, grubun Nesne Kimliğinin Azure portalındaki özellikleri aracılığıyla başvurulması ve 'Kullanılabilir Gruplar' bölümüne yerleştirilmesi gerekir. Bu işlem tamamlandıktan sonra, Object ID/AD Grubu artık izinler için bir grup ilkesine atanmak üzere kullanılabilir olacaktır.

![Kullanıcı Oluşturma](./media/bomgarremotesupport-tutorial/config-user2.png)

![Kullanıcı Oluşturma](./media/bomgarremotesupport-tutorial/config-user3.png)

> [!NOTE]
> Alternatif olarak, varsayılan grup ilkesi SAML2 Güvenlik Sağlayıcısı'nda ayarlanabilir. Bu seçeneği tanımlayarak, bu, SAML aracılığıyla kimlik doğrulayan tüm kullanıcıları grup ilkesinde belirtilen izinleri atar. Genel Üyeler ilkesi, kimlik doğrulamasını test etmek ve kullanıcıları doğru ilkelere atamak için kullanılabilecek sınırlı izinlerle BeyondTrust Uzaktan Destek/Ayrıcalıklı Uzaktan Erişim'e dahildir. Kullanıcılar, ilk başarılı kimlik doğrulama denemesine kadar /giriş > Kullanıcılar & Güvenlik üzerinden SAML2 Kullanıcıları listesine girmeyecek. Grup ilkeleri hakkında ek bilgiler aşağıdaki bağlantıda bulunabilir:`https://www.beyondtrust.com/docs/remote-support/getting-started/admin/group-policies.htm`

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki BeyondTrust Uzaktan Destek döşemesini tıklattığınızda, SSO'yu kurduğunuz BeyondTrust Uzaktan Destek'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile BeyondTrust Uzaktan Desteği deneyin](https://aad.portal.azure.com/)
