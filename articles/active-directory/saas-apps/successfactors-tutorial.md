---
title: 'Öğretici: Azure Active Directory, SuccessFactors ile tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SuccessFactors arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d49915271580b5665981bf7e212f3d5712c86456
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76292988"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-successfactors"></a>Öğretici: SuccessFactors ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Başarı Faktörleri'ni Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Başarı Faktörlerini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Başarı Faktörleri'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Başarı Faktörleri'nde otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SuccessFactors tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SuccessFactors **SP** başlatılan SSO destekler.
* SuccessFactors'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-successfactors-from-the-gallery"></a>Galeriden SuccessFactors Ekleme

Başarı Faktörleri'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Başarı Faktörleri eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Başarı Faktörleri** yazın.
1. Sonuç panelinden **SuccessFactors'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-successfactors"></a>Azure AD SSO'nun Başarı Faktörleri için yapılandırılVe test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak SuccessFactors ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Başarı Faktörleri'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Başarı Faktörleri ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[Yapılandırılan SuccessFactors SSO](#configure-successfactors-sso)** - uygulama tarafında Tek Oturum Açma ayarlarını yapılandırmak için.
    1. **[SuccessFactors test kullanıcı oluşturun](#create-successfactors-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı SuccessFactors B.Simon bir meslektaşı olması.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **SuccessFactors** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [SuccessFactors İstemci destek ekibine](https://www.successfactors.com/content/ssf-site/en/support.html) başvurun.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **SuccessFactors'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Başarı Faktörleri'ne erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar **listesinde, SuccessFactors**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-successfactors-sso"></a>Yapılandırılan SuccessFactors SSO

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak **SuccessFactors yönetici portalınıza** giriş yapın.

2. **Uygulama Güvenliği'ni** ziyaret edin ve Tek İşaret **Açma Özelliği'ne**yerel .

3. **Sıfırlama Belirteci'ne** herhangi bir değer yerleştirin ve SAML SSO'yu etkinleştirmek için **Token'i Kaydet'i** tıklatın.

    ![Uygulama tarafında tek oturum açma yapılandırma][11]

    > [!NOTE]
    > Bu değer açma/kapama düğmesi olarak kullanılır. Herhangi bir değer kaydedilirse, SAML SSO AİTTIR. Boş bir değer kaydedilirse SAML SSO KAPALIDIR.

4. Aşağıda ekran görüntüsüne yerel ve aşağıdaki eylemleri gerçekleştirin:

    ![Uygulama tarafında tek oturum açma yapılandırma][12]
  
    a. **SAML v2 SSO** Radyo Düğmesini seçin
  
    b. **SAML Asserting Parti Adını**(örneğin, SAML veren + şirket adı) ayarlayın.

    c. Veren **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    d. **Zorunlu İmza Gerektir**olarak **İddia'yı** seçin.

    e. **SAML Bayrağını Etkinleştir**olarak **Etkin'i** seçin.

    f. **Giriş İsteği İmzası(SF Oluşturulan/SP/RP)** olarak **Hayır'ı** seçin.

    g. **SAML Profili**olarak **Tarayıcı/Gönderi Profilini** seçin.

    h. **Sertifika Geçerlilik Süresini Uygula**olarak **Hayır'ı** seçin.

    i. İndirilen sertifika dosyasının içeriğini Azure portalından kopyalayın ve **ardından SAML Doğrulama Sertifikası** metin kutusuna yapıştırın.

    > [!NOTE] 
    > Sertifika içeriğinin başlangıç sertifikası ve son sertifika etiketleri olmalıdır.

5. SAML V2'ye gidin ve ardından aşağıdaki adımları gerçekleştirin:

    ![Uygulama tarafında tek oturum açma yapılandırma][13]

    a. **Destek SP tarafından başlatılan Global Logout**olarak **Evet'i** seçin.

    b. Genel **Oturum Açma Hizmeti URL'si (LogoutRequest hedef)** textbox'ına, kopyaladığınız **Oturum Açma URL** değerini Azure portalına yapıştırın.

    c. **SP'nin tüm NameID öğesini şifrelemesi gerekir.** **No**

    d. **AdKimliği Biçimi**olarak **belirtilmemiş** seçin.

    e. **SP başlatılan girişi etkinleştirolarak Evet'i seçin (AuthnRequest)**. **Yes**

    f. Şirket **Genelinde veren** textbox olarak gönder isteğinde, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

6. Giriş kullanıcı adları Case Duyarsız yapmak istiyorsanız bu adımları gerçekleştirin.

    ![Tek İşaret-On'u Yapılandır][29]

    a. **Şirket Ayarlarını**ziyaret edin (alta yakın).

    b. **Büyük/Küçük Harf Duyarlı Olmayan Kullanıcı Adını Etkinleştir'in**yanındaki onay kutusunu seçin.

    c. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Bunu etkinleştirmeye çalışırsanız, sistem yinelenen bir SAML giriş adı oluştursun da denetler. Örneğin, müşterinin kullanıcı adı User1 ve user1 varsa. Büyük/küçük harf hassasiyetini ortadan kaldırarak bu yinelemeleri yapar. Sistem size bir hata iletisi verir ve özelliği etkinleştirmez. Müşterinin kullanıcı adlarından birini farklı yazması için değiştirmesi gerekir.

### <a name="create-successfactors-test-user"></a>SuccessFactors test kullanıcısı oluşturma

Azure AD kullanıcılarının SuccessFactors'te oturum açabilmeleri için, başarı faktörleri olarak sağlanmalıdır. SuccessFactors durumunda, sağlama manuel bir görevdir.

Başarı Faktörleri'nde oluşturulan kullanıcıları elde etmek için [SuccessFactors destek ekibine](https://www.successfactors.com/content/ssf-site/en/support.html)başvurmanız gerekir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Başarı Faktörleri döşemesini tıklattığınızda, SSO'yu kurduğunuz SuccessFactors'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Başarı Faktörlerini Deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle SuccessFactors nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

<!--Image references-->

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png
