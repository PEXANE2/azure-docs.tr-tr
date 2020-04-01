---
title: 'Öğretici: LinkedIn Learning ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LinkedIn Learning arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15509866980cdf85f54fc03cb77eed36f83c982f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76983376"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-learning"></a>Öğretici: LinkedIn Learning ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, LinkedIn Learning'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. LinkedIn Learning'i Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* LinkedIn Learning erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla LinkedIn Learning'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* LinkedIn Learning tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* LinkedIn **Learning, SP ve IDP** tarafından başlatılan SSO'ya destek veriyor
* LinkedIn Learning **Just In Time** kullanıcı sağlama destekler
* LinkedIn Learning'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-linkedin-learning-from-the-gallery"></a>Galeriden LinkedIn Learning ekleme

LinkedIn Learning'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize LinkedIn Learning'i eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **LinkedIn Learning** yazın.
1. Sonuç panelinden **LinkedIn Learning'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-learning"></a>LinkedIn Learning için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak LinkedIn Learning ile yapılandırın ve test edin. SSO'nun çalışması için, LinkedIn Learning'deki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu LinkedIn Learning ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[LinkedIn Learning SSO'yu yapılandırır](#configure-linkedin-learning-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[LinkedIn Learning test kullanıcısını oluşturun](#create-linkedin-learning-test-user)** - LinkedIn Learning'de Kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **LinkedIn Learning** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

     a. **Tanımlayıcı** textbox'ına LinkedIn Portal'dan kopyalanan **Varlık Kimliği'ni** girin. 

    b. **Yanıtla URL** metin kutusuna LinkedIn Portal'dan kopyalanan **İddia Tüketici Hizmeti (ACS) Url'sini** girin.

    c. Uygulamayı **SP Başlatılan** modda yapılandırmak istiyorsanız, oturum açma URL'nizi belirteceğiniz **Temel SAML Yapılandırması** bölümünde **ek URL'ler seç** seçeneğini tıklatın. Giriş Url'nizi oluşturmak için **Assertion Consumer Service (ACS) Url'sini** kopyalayın ve /saml/ ile /login/ ile değiştirin. Bu yapıldıktan sonra, oturum açma URL'si aşağıdaki deseni olmalıdır:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![LinkedIn Learning Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Bu değerler gerçek değer değildir. Bu değerleri, öğreticinin **LinkedIn Learning SSO'yu Yapılandırışla** bölümünde daha sonra açıklanan gerçek Tanımlayıcı ve Yanıt URL'si ile güncelleştireceksiniz.

1. LinkedIn Learning uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. LinkedIn Learning uygulaması **nameidentifier** **user.mail**ile eşlenir bekliyor, bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşlemi değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **LinkedIn Learning'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın LinkedIn Learning'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **LinkedIn Learning'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-linkedin-learning-sso"></a>LinkedIn Learning SSO'da yapılandırın

1. Farklı bir web tarayıcısı penceresinde, LinkedIn Learning kiracınızı yönetici olarak oturum açın.

2. **Hesap Merkezi'nde** **Ayarlar**altında **Genel Ayarlar'ı** tıklatın. Ayrıca, açılan listeden **Öğrenme - Varsayılan'ı** seçin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. **Formdan tek tek alanları yüklemek ve kopyalamak için Buraya tıklayın** ve **Entity Id** ve Assertion Consumer **Service (ACS) Url'sini** kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** bölümüne yapıştırın.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. **LinkedIn Yönetici Ayarları** bölümüne gidin. Azure portalından indirdiğiniz **XML dosyasını Upload XML dosya** seçeneğini tıklayarak yükleyin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. SSO'ya etkinleştirmek için **Üzerine** tıklayın. Bağlı Olmayan'dan **Bağlı** **Olmayan'a** SSO durumu değişir

    ![Tek İşaret-On'u Yapılandır](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-learning-test-user"></a>LinkedIn Learning test kullanıcılarını oluşturun

LinkedIn Learning Application tam zamanında kullanıcı sağlamayı destekler ve kimlik doğrulama sonrası kullanıcılar uygulamada otomatik olarak oluşturulur. LinkedIn Learning portalındaki yönetici ayarları sayfasında, anahtarı otomatik olarak aktife **otomatik olarak atayın lisansları** sadece zaman sağlama da yapın ve bu da kullanıcıya bir lisans atayacaktır.

   ![Azure AD test kullanıcısı oluşturma](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LinkedIn Learning döşemesini tıklattığınızda, SSO'yu kurduğunuz LinkedIn Learning'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile LinkedIn Learning'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)