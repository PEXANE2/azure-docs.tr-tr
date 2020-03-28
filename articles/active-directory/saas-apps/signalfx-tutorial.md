---
title: 'Öğretici: SignalFx ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SignalFx arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/24/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 149718dcd325ef6bd6a6754ba100ffdc34be0a07
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79136423"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-signalfx"></a>Öğretici: SignalFx ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SignalFx'i Azure Active Directory (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. SignalFx'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SignalFx erişimi olan Azure AD'den denetim;
* Kullanıcılarınızın Azure REKLAM hesaplarıyla SignalFx'te otomatik olarak oturum açabilmelerini sağlamak; Ve
* Hesaplarınızı tek bir konumda (Azure portalı) yönetin.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

* Azure AD aboneliği
    * Aboneliğiniz yoksa, [buradan ücretsiz hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SignalFx tek oturum açma (SSO) özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandıracak ve sınayabilirsiniz.

* SignalFx **IDP** başlatılan SSO destekler
* SignalFx **Just In Time** kullanıcı sağlama destekler
* SignalFx'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="step-1-add-the-signalfx-application-in-azure"></a>Adım 1: Azure'da SignalFx uygulamasını ekleme

SignalFx uygulamasını yönetilen SaaS uygulamaları listenize eklemek için bu yönergeleri kullanın.

1. [Azure portalına](https://portal.azure.com)giriş yapın.
1. Sol taraftaki gezinti penceresinde **Azure Etkin Dizini'ni**seçin.
1. **Kurumsal uygulamaları**seçin ve ardından **Tüm uygulamaları**seçin.
1. **Yeni uygulama**’yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SignalFx'i**girin ve seçin.
     * Uygulamanın kiracınıza eklenmesi için birkaç dakika beklemeniz gerekebilir.
1. Azure portalını açık bırakın ve ardından yeni bir web sekmesi açın.    

## <a name="step-2-begin-signalfx-sso-configuration"></a>Adım 2: SignalFx SSO yapılandırmabaşlatın

SignalFx SSO için yapılandırma işlemini başlatmak için bu yönergeleri kullanın.

1. Yeni açılan sekmede SignalFx UI'ye erişin ve oturum açın. 
1. Üst menüde **Tümleştirmeler'i**tıklatın. 
1. Arama alanında Azure Etkin **Dizini'ni**girin ve seçin.
1. **Yeni Tümleştirme Oluştur'u**tıklatın.
1. **Ad'da,** kullanıcılarınızın anlayacağı kolayca tanınabilir bir ad girin.
1. İşaret **giriş sayfasında göster.**
    * Bu özellik, kullanıcılarınızın tıklayabileceği giriş sayfasında özelleştirilmiş bir düğme görüntüler. 
    * **Ad'a** girdiğiniz bilgiler düğmede görünür. Sonuç olarak, kullanıcılarınızın tanıyacağı bir **Ad** girin. 
    * Bu seçenek yalnızca SignalFx uygulaması için **yourcompanyname.signalfx.com**gibi özel bir alt etki alanı kullanırsanız çalışır. Özel bir alt etki alanı elde etmek için SignalFx desteğine başvurun. 
1. **Tümleştirme Kimliğini**kopyalayın. Daha sonraki bir adımda bu bilgilere ihtiyacınız olacaktır. 
1. SignalFx UI'yi açık bırakın. 

## <a name="step-3-configure-azure-ad-sso"></a>Adım 3: Azure AD SSO'su yapılandırın

Azure portalında Azure AD SSO'yu etkinleştirmek için bu yönergeleri kullanın.

1. [Azure portalına](https://portal.azure.com/)ve **SignalFx** uygulama tümleştirme sayfasına dönün, **Yönet** bölümünü bulun ve ardından Tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML** Yapılandırması'nın kalem (düzenleme) simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanları tamamlayın: 

    a. **Tanımlayıcı'da,** aşağıdaki URL'yi `https://api.<realm>.signalfx.com/v1/saml/metadata` girin `<realm>` ve SignalFx bölgenizi değiştirin. 

    b. **YanıtURL'inde,** aşağıdaki `https://api.<realm>.signalfx.com/v1/saml/acs/<integration ID>` URL'yi girin ve SignalFx `<integration ID>` bölgenizi ve SignalFx UI'den daha önce kopyaladığınız Entegrasyon `<realm>` **Kimliği'ni** değiştirin.

1. SignalFx uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. 
    
1. Aşağıdaki taleplerin Etkin Dizini'nde doldurulan kaynak öznitelikleriyle eşlenindiğini gözden geçirin ve doğrulayın. 

    | Adı |  Kaynak Özniteliği|
    | ------------------- | -------------------- |
    | Kullanıcı.Ad Adı  | user.givenname |
    | Kullanıcı.e-posta  | kullanıcı.posta |
    | PersonImmutableID       | user.userprincipalname    |
    | Kullanıcı.Soyadı       | user.surname    |

    > [!NOTE]
    > Bu işlem, Etkin Dizininizin en az bir doğrulanmış özel etki alanıyla yapılandırılmasının yanı sıra bu etki alanındaki e-posta hesaplarına erişimi olmasını gerektirir. Bu yapılandırma yla ilgili emin değilseniz veya yardıma ihtiyacınız varsa, lütfen SignalFx desteğine başvurun.  

1. **SAML sayfasıyla tek oturum** açma,SAML **İmza Sertifikası** bölümünde Sertifika **(Base64)** bul ve ardından **İndir'i**seçin. Sertifikayı indirin ve bilgisayarınıza kaydedin. Ardından, **App Federation Metadata Url** değerini kopyalayın; SignalFx UI'de daha sonraki bir adımda bu bilgilere ihtiyacınız olacaktır. 

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SignalFx'i ayarla** bölümünde **Azure AD Tanımlayıcı** değerini kopyalayın. SignalFx UI'de daha sonraki bir adımda bu bilgilere ihtiyacınız olacaktır. 

## <a name="step-4-create-an-azure-ad-test-user"></a>Adım 4: Azure AD test kullanıcısı oluşturma

Azure portalında **B.Simon**adında bir test kullanıcısı oluşturmak için bu yönergeleri kullanın.

1. Azure portalında, sol taraftaki gezinti penceresinde **Azure Etkin Dizin'i**seçin, ardından **Kullanıcılar'ı**seçin ve ardından **Tüm kullanıcıları**seçin.
1. Sayfanın üst kısmında **Yeni kullanıcıyı**seçin.
1. **Kullanıcı** özelliklerinde:
   1. **Kullanıcı adına**, `username@companydomain.extension`girin `b.simon@contoso.com`, gibi .
   1. **Ad**, `B.Simon`girin .
   1. Parolayı **Göster'i**işaretleyin ve ardından **parolada**görüntülenen değeri kopyalayın. Bu tümleştirmeyi test etmek için daha sonraki adımda bu bilgilere ihtiyacınız olacaktır. 
   1. **Oluştur'u**tıklatın.

## <a name="step-5-assign-the-azure-ad-test-user"></a>Adım 5: Azure AD test kullanıcısını atama

Test kullanıcısının SignalFx için Azure tek oturum açma'yı kullanmasını sağlamak için bu yönergeleri kullanın.

1. Azure portalında **Kurumsal uygulamaları**seçin ve ardından **Tüm uygulamaları**seçin.
1. Uygulama listesinde **SignalFx'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve ardından **Kullanıcılar ve gruplar**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar **ve gruplar**seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B.Simon'ı**seçin ve sayfanın en altında **Seç'i**tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, rolü **seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve sayfanın altındaki **Seç'i** tıklatın.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**tıklatın.

## <a name="step-6-complete-the-signalfx-sso-configuration"></a>Adım 6: SignalFx SSO yapılandırmasını tamamlayın 

1. Önceki sekmeyi açın ve geçerli Azure Etkin Dizin tümleştirme sayfasını görüntülemek için SignalFx UI'ye dönün. 
1. Sertifika **(Base64)** yanında, **Dosyayı Yükle'yi**tıklatın ve ardından azure portalından daha önce indirdiğiniz **Base64 kodlanmış sertifika** dosyasını bulun.
1. Azure **AD**Tanımlayıcısı'nın yanına, Azure portalından daha önce kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın. 
1. Federation **Metadata URL'sinin**yanında, Azure portalından daha önce kopyaladığınız **Uygulama Federasyonu Metaveri Url** değerini yapıştırın. 
1. **Kaydet**'e tıklayın.

## <a name="step-7-test-sso"></a>Adım 7: Test SSO

SSO'nun nasıl test edileceklerine ilişkin aşağıdaki bilgileri ve SignalFx'e ilk kez giriş beklentilerini gözden geçirin. 

### <a name="test-logins"></a>Test girişleri

* Girişi test etmek için özel / gizli bir pencere kullanmanız gerekir veya Azure portalından çıkış yapabilirsiniz. Değilse, uygulamayı yapılandıran kullanıcıya yönelik tanımlama bilgileri, test kullanıcısıyla başarılı bir oturum açmayı engeller ve engeller.

* Yeni bir test kullanıcısı ilk kez oturum açtığınızda, Azure parola değişikliğini zorlar. Bu durumda, SSO oturum açma işlemi tamamlanmaz; test kullanıcısı Azure portalına yönlendirilir. Sorun gidermek için, test kullanıcısı parolasını değiştirmeli ve SignalFx giriş sayfasına veya Access Paneli'ne gitmeli ve yeniden denemelidir.
    * Access Paneli'ndeki SignalFx döşemesini tıklattığınızda, SignalFx'e otomatik olarak giriş yapılmalıdır. 
        * Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

* SignalFx uygulamasına Access Paneli'nden veya kuruluşa atanmış özel bir giriş sayfası üzerinden erişilebilir. Test kullanıcısı, bu konumdan başlayarak tümleştirmeyi test etmelidir.
    * Test kullanıcısı, bu işlemde **b.simon@contoso.com**daha önce oluşturulan kimlik bilgilerini .

### <a name="first-time-logins"></a>İlk kez giriş

* Bir kullanıcı SAML SSO'dan SignalFx'e ilk kez giriş yaptığında, kullanıcıya bağlantı içeren bir SignalFx e-postası gönderilecektir. Kullanıcı kimlik doğrulama amacıyla bağlantıyı tıklatmalıdır. Bu e-posta doğrulama yalnızca ilk kez kullananlar için gerçekleşir. 

* **SignalFx, Just In Time** kullanıcı oluşturmayı destekler, bu da signalfx'te bir kullanıcı yoksa, ilk giriş denemesinde kullanıcının hesabının oluşturulacağı anlamına gelir.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Azure AD ile SignalFx'i deneyin](https://aad.portal.azure.com/)