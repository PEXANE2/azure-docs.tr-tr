---
title: 'Öğretici: Azure Active Directory iProva ile tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve iProva arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 98458f8be162d0903f5ea0d1f7d4651d46f78e8e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048445"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-iprova"></a>Öğretici: Azure Active Directory iProva ile tek oturum açma (SSO) entegrasyonu

Bu eğitimde, iProva'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. iProva'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de iProva erişimi olan denetimi denetler.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla iProva'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* iProva tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* iProva **SP** başlatılan SSO destekler

* iProva'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-iprova-from-the-gallery"></a>Galeriden iProva ekleme

iProva'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize iProva eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **iProva** yazın.
1. Sonuç panelinden **iProva'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-iprova"></a>iProva için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **b.simon**adlı bir test kullanıcısı kullanarak iProva ile yapılandırın ve test edin. SSO'nun çalışması için, iProva'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu iProva ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Sonraki adımlara hazırlık olarak **[iProva'dan yapılandırma bilgilerini alın.](#retrieve-configuration-information-from-iprova)**
1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[iProva test kullanıcısını oluşturun](#create-iprova-test-user)** - iProva'da kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[iProva SSO'yu yapılandırın.](#configure-iprova-sso)**
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="retrieve-configuration-information-from-iprova"></a>iProva'dan yapılandırma bilgilerini alma

Bu bölümde, Azure AD oturum açma işlemlerini yapılandırmak için iProva'dan bilgi alırsınız.

1. Bir web tarayıcısı açın ve aşağıdaki URL deseni kullanarak iProva'daki **SAML2 bilgi** sayfasına gidin:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

    ![iProva SAML2 bilgi sayfasını görüntüleyin](media/iprova-tutorial/iprova-saml2-info.png)

1. Başka bir tarayıcı sekmesinde sonraki adımlara devam ederken tarayıcı sekmesini açık bırakın.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **iProva** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** kutusunu **iProva SAML2 bilgi** sayfasındaki Oturum Açma **URL'sinin** arkasında görüntülenen değerle doldurun. Bu sayfa diğer tarayıcı sekmenizde hala açıktır.

    b. **IProva** **SAML2 bilgi** sayfasında **EntityID** etiketinin arkasında görüntülenen değerle tanımlayıcı kutusunu doldurun. Bu sayfa diğer tarayıcı sekmenizde hala açıktır.

    c. **Yanıt-URL** kutusunu **iProva SAML2 bilgi** sayfasındaki yanıt **url** etiketinin arkasında görüntülenen değerle doldurun. Bu sayfa diğer tarayıcı sekmenizde hala açıktır.

1. iProva uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, iProva uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği| Ad Alanı  |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

## <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

## <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın iProva'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **iProva'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="create-iprova-test-user"></a>iProva test kullanıcısı oluşturma

1. **Yönetici** hesabını kullanarak iProva'da oturum açın.

2. Git **menüsünü** açın.

3. **Uygulama yönetimini**seçin.

4. Kullanıcılar ve **kullanıcı grupları** panelinde **Kullanıcıları** seçin.

5. **Ekle'yi**seçin.

6. Kullanıcı **adı** kutusuna, kullanıcı nın kullanıcı `B.Simon@contoso.com`adını girin.

7. Tam **ad** kutusuna, **B.Simon**gibi kullanıcının tam adını girin.

8. Parola yok seçeneğini belirleyin **(tek oturum açma kullanın) seçeneğini belirleyin.**

9. **E-posta adres** kutusuna, kullanıcının e-posta adresini girin. `B.Simon@contoso.com`

10. Sayfanın sonuna doğru ilerleyin ve **Bitir'i**seçin.

## <a name="configure-iprova-sso"></a>iProva SSO'da yapılandır

1. **Yönetici** hesabını kullanarak iProva'da oturum açın.

2. Git **menüsünü** açın.

3. **Uygulama yönetimini**seçin.

4. **Sistem ayarları** panelinde **Genel'i** seçin.

5. **Edit'i**seçin.

6. Access denetimine aşağı **kaydırın.**

    ![iProva Erişim denetim ayarları](media/iprova-tutorial/iprova-accesscontrol.png)

7. **Kullanıcıların ağ hesaplarıyla otomatik olarak oturum açtığı**ayarı bulun ve **SAML üzerinden kimlik doğrulamasını Evet**olarak değiştirin. Artık ek seçenekler görüntülenir.

8. **Ayarla** seçeneğini belirleyin.

9. **Sonraki'ni**seçin.

10. iProva, federasyon verilerini bir URL'den indirmek veya bir dosyadan yüklemek isteyip istemediğinizi sorar. **URL'den seçeneğini** seçin.

    ![Azure AD meta verilerini indirin](media/iprova-tutorial/iprova-download-metadata.png)

11. "Azure AD tek oturum aç"ı bölümünün son adımında kaydettiğiniz meta veri URL'sini yapıştırın.

12. Meta verileri Azure AD'den indirmek için ok şeklindeki düğmeyi seçin.

13. İndirme tamamlandığında, indirilen onay iletisi **Geçerli Federasyon Veri dosyası** görüntülenir.

14. **Sonraki'ni**seçin.

15. Şimdilik **Test giriş** seçeneğini atlayın ve **İleri'yi**seçin.

16. Açılan **kutuyu kullanma Talebi'nde** **windowsaccountname'yi**seçin.

17. **Bitiş'i**seçin.

18. Şimdi **genel ayarları edin** ekranına geri dönüyorsunuz. Sayfanın altına doğru ilerleyin ve yapılandırmanızı kaydetmek için **Tamam'ı** seçin.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki iProva döşemesini tıklattığınızda, SSO'yu kurduğunuz iProva'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile iProva'yı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle iProva nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)