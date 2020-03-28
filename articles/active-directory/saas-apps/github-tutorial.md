---
title: 'Öğretici: GitHub ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve GitHub arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e812e1b03637a3ecd7a45f02664c4e3547f1aef1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79138986"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-github"></a>Öğretici: GitHub ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, GitHub'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. GitHub'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* GitHub Kurumsal Bulut Kuruluşunuza erişimi olan Azure AD'de denetim.
* GitHub Kurumsal Bulut Organizasyonunuza erişimi tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini GitHub ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* [GitHub Enterprise Cloud'da](https://help.github.com/articles/github-s-products/#github-enterprise)oluşturulan ve [GitHub Kurumsal faturalandırma planını](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations) gerektiren bir GitHub kuruluşu

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* GitHub **SP** başlatılan SSO destekler

* GitHub [ **Otomatik** kullanıcı sağlama (kuruluş davetleri) destekler](github-provisioning-tutorial.md)
* GitHub'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-github-from-the-gallery"></a>Galeriden GitHub ekleme

GitHub'ın Azure AD'ye entegrasyonunu yapılandırmak için, github'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **GitHub** yazın.
1. Sonuçlar panelinden **GitHub'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-github"></a>GitHub için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak GitHub ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile GitHub'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu GitHub ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[GitHub SSO'yu yapılandırın](#configure-github-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[GitHub test kullanıcısını oluşturun](#create-github-test-user)** - Kullanıcının Azure AD gösterimine bağlı github'da B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **GitHub** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

   a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://github.com/orgs/<entity-id>/sso`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Bunların gerçek değerler olmadığını lütfen unutmayın. Bu değerleri URL ve Tanımlayıcı'da gerçek Oturum İşareti ile güncelleştirmeniz gerekir. Burada tanımlayıcıda dizenin benzersiz değerini kullanmanızı öneririz. Bu değerleri almak için GitHub Yöneticisi bölümüne gidin.

5. GitHub uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. GitHub uygulaması **nameidentifier** **user.mail**ile eşlenen bekliyor, bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşlemi değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **GitHub'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, B.Simon'ın GitHub'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **GitHub'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-github-sso"></a>GitHub SSO'yu yapılandır

1. Farklı bir web tarayıcısı penceresinde, GitHub kuruluş sitenize yönetici olarak giriş yapın.

2. **Ayarlar'a** gidin ve **Güvenlik'e** tıklayın

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_03.png)

3. Tek Oturum Açma yapılandırma alanlarını ortaya çıkaran **SAML kimlik doğrulama kutusunu etkinleştir'i** denetleyin. Ardından, Azure AD yapılandırması üzerindeki Tek oturum açma URL'sini güncelleştirmek için tek oturum açma URL değerini kullanın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_13.png)

4. Aşağıdaki alanları yapılandırın:

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. URL metin kutusunda **Oturum Aç'** a Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    b. **Veren** metin kutusunda, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. Azure portalından indirilen sertifikayı not defterine açın, içeriği **Herkese Açık Sertifika** metin kutusuna yapıştırın.

    d. Aşağıda gösterildiği gibi **RSA-SHA1** ve **SHA1'den** **RSA-SHA256** ve **SHA256'ya** imza **yöntemini** ve **Sindirimi Yöntemini** yeniden düzenleyen **ediyi** tıklatın.
    
    e. Varsayılan URL'den varsayılan URL'den varsayılan tüketici **hizmeti URL'sini (YanıtURL'si)** güncelleştirerek Github'daki URL'nin Azure uygulama kaydındaki URL ile eşleşin.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

5. SSO sırasında doğrulama hatası veya hatası olmadığını doğrulamak için **SAML yapılandırmasını test** edin.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_06.png)

6. **Kaydet'i** tıklatın

> [!NOTE]
> GitHub'daki tek oturum açma, GitHub'daki belirli bir kuruluşa kimlik doğrulaması yapar ve GitHub'ın kimlik doğrulamasını değiştirmez. Bu nedenle, kullanıcının github.com oturumunun süresi dolmuşsa, tek oturum açma işlemi sırasında GitHub'ın kimliği/parolasıyla kimlik doğrulamanız istenebilir.

### <a name="create-github-test-user"></a>GitHub test kullanıcısı oluşturma

Bu bölümün amacı GitHub Britta Simon adlı bir kullanıcı oluşturmaktır. GitHub varsayılan olarak etkin olan otomatik kullanıcı sağlamayı destekler. Burada otomatik kullanıcı sağlama yapılandırmak için nasıl [daha](github-provisioning-tutorial.md) fazla bilgi bulabilirsiniz.

**Kullanım kılavuzunu el ile oluşturmanız gerekiyorsa, aşağıdaki adımları gerçekleştirin:**

1. GitHub şirket sitenizde yönetici olarak oturum açın.

2. **Kişiler'i**tıklatın.

    ![People](./media/github-tutorial/tutorial_github_config_github_08.png "People")

3. **Üye davet**et'i tıklatın.

    ![Kullanıcıları Davet Et](./media/github-tutorial/tutorial_github_config_github_09.png "Kullanıcıları Davet Et")

4. Üye **davet** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    a. **E-posta** metin kutusuna Britta Simon hesabının e-posta adresini yazın.

    ![Kişileri Davet Et](./media/github-tutorial/tutorial_github_config_github_10.png "Kişileri Davet Et")

    b. **Davetiye Gönder'i**tıklatın.

    ![Kişileri Davet Et](./media/github-tutorial/tutorial_github_config_github_11.png "Kişileri Davet Et")

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki GitHub döşemesini tıklattığınızda, SSO'yu kurduğunuz GitHub'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile GitHub'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
