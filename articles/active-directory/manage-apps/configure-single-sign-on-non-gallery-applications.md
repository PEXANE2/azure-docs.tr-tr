---
title: SAML tek oturum açma - galeri dışı uygulamalar - Microsoft kimlik platformu | Microsoft Dokümanlar
description: Microsoft kimlik platformundaki (Azure AD) galeri olmayan uygulamalara tek oturum açma (SSO) yapılandırma
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 07/19/2019
ms.author: celested
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad27ad5e34d9f44fe7d7be80e05e33dd6fb5e7b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244218"
---
# <a name="configure-saml-based-single-sign-on-to-non-gallery-applications"></a>SAML tabanlı tek oturum açmayı galeri dışı uygulamalara yapılandırma

Azure AD Kurumsal Uygulamalarınıza bir galeri uygulaması veya [galeri dışı bir web uygulaması](add-non-gallery-app.md) [eklediğinizde,](add-gallery-app.md) kullanabileceğiniz tek oturum açma seçeneklerinden biri [SAML tabanlı tek oturum açmadır.](what-is-single-sign-on.md#saml-sso) SAML protokollerinden birini kullanarak kimlik doğrulayan uygulamalar için mümkün olduğunca SAML'yi seçin. SAML tek oturum açma ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamaya kimlik doğrulaması sağlar. Azure AD, oturum açma bilgilerini bir bağlantı protokolü aracılığıyla uygulamaya iletir. Kullanıcıları, SAML taleplerinizde tanımladığınız kurallara göre belirli uygulama rolleriile eşleyebilirsiniz. Bu makalede, galeri dışı bir uygulama için SAML tabanlı tek oturum açma nın nasıl yapılandırılabildiğini açıklanmaktadır. 

> [!NOTE]
> Galeri uygulaması mı ekliyorsunuz? [SaaS uygulama öğreticileri listesinde](../saas-apps/tutorial-list.md) adım adım kurulum yönergelerini bulma

Kod yazmadan galeri dışı bir uygulama için SAML tek oturum açma işlemlerini yapılandırmak için Azure AD Premium lisansıyla birlikte bir aboneliğe sahip olmanız gerekir ve uygulamaSAML 2.0'ı desteklemelidir. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)sayfasını ziyaret edin.

## <a name="before-you-begin"></a>Başlamadan önce

Uygulama Azure AD kiracınıza eklenmediyse, [bkz.](add-non-gallery-app.md)

## <a name="step-1-edit-the-basic-saml-configuration"></a>1. Adım. Temel SAML Yapılandırmasını Düzenleme

1. [Azure portalında](https://portal.azure.com) bulut uygulaması yöneticisi veya Azure AD kiracınız için bir uygulama yöneticisi olarak oturum açın.

2. Azure **Active Directory** > **Enterprise uygulamalarına** gidin ve uygulamayı listeden seçin. 
   
   - Uygulamayı aramak için, **Uygulama Türü** menüsünde Tüm **uygulamaları**seçin ve sonra **Uygula'yı**seçin. Arama kutusuna uygulamanın adını girin ve ardından sonuçlardan uygulamayı seçin.

3. **Yönet** bölümünde, **Tek oturum açma'yı**seçin. 

4. **SAML'yi**seçin. **SAML ile Tek Oturum Açma** - Önizleme sayfası görüntülenir.

   ![Adım 1 Temel SAML Yapılandırmasını Düzenleme](media/configure-single-sign-on-non-gallery-applications/step-one-basic-saml-config.png)

5. Temel SAML yapılandırma seçeneklerini düzenlemek **için, Temel SAML Yapılandırması** bölümünün sağ üst köşesindeki **Düzenleme** simgesini (kalem) seçin.

1. Aşağıdaki ayarları girin. Değerleri uygulama satıcısından almalısınız. Alanların değerini ayıklamak için değerleri el ile girebilir veya bir meta veri dosyası yükleyebilirsiniz.

    | Temel SAML Yapılandırma ayarı | SP ile başlatılan | idP ile başlatılan | Açıklama |
    |:--|:--|:--|:--|
    | **Tanımlayıcı (Varlık Kimliği)** | Bazı uygulamalar için gereklidir | Bazı uygulamalar için gereklidir | Uygulamayı benzersiz bir şekilde tanımlar. Azure AD tanımlayıcıyı SAML belirteci hedef kitle midenolarak uygulamaya gönderir. Uygulamanın bunu doğrulanması bekleniyor. Bu değer ayrıca uygulama tarafından sağlanan SAML meta verilerinde Varlık Kimliği olarak da görünür. Aşağıdaki deseni kullanan bir URL<subdomain>girin: 'https:// .contoso.com' *Bu değeri uygulama tarafından gönderilen **AuthnRequest'te** (SAML isteği) **Veren** öğesi olarak bulabilirsiniz.* |
    | **Yanıt URL'si** | Gerekli | Gerekli | Uygulamanın SAML belirtecini almayı beklediği konumu belirtir. Yanıt URL'si, Onay Belgesi Tüketici Hizmeti (ACS) URL'si olarak da bilinir. Birden çok yanıt URL'si belirtmek için ek yanıt URL alanlarını kullanabilirsiniz. Örneğin, birden çok alt etki alanı için ek yanıt URL'leri gerekebilir. Veya, sınama amacıyla aynı anda birden çok yanıt URL'si (yerel ana bilgisayar ve ortak URL'ler) belirtebilirsiniz. |
    | **Oturum açma URL'si** | Gerekli | Belirtme | Kullanıcı bu URL'yi açtığında hizmet sağlayıcısı kimlik doğrulaması ve oturum açma için Azure AD'ye yönlendirir. Azure AD, uygulamayı Office 365 veya Azure AD Access Paneli'nden başlatmak için URL'yi kullanır. Boş olduğunda, Azure AD, bir kullanıcı uygulamayı Office 365, Azure AD Access Paneli veya Azure AD SSO URL'sinden başlattığında IdP tarafından başlatılan oturum açma işlemini gerçekleştirir.|
    | **Geçiş Durumu** | İsteğe bağlı | İsteğe bağlı | Uygulamaya kimlik doğrulaması tamamlandıktan sonra kullanıcının yönlendirileceği yeri belirtir. Genellikle değer, uygulama için geçerli bir URL'dir. Ancak, bazı uygulamalar bu alanı farklı kullanır. Daha fazla bilgi için uygulama satıcısına danışın.
    | **Giriş URL'si** | İsteğe bağlı | İsteğe bağlı | SAML Logout yanıtlarını uygulamaya geri göndermek için kullanılır.

Daha fazla bilgi için tek [oturum açma SAML protokolüne](../develop/single-sign-on-saml-protocol.md)bakın.

## <a name="step-2-configure-user-attributes-and-claims"></a>2. Adım Kullanıcı özniteliklerini ve taleplerini yapılandırma 

Bir kullanıcı uygulamaya kimlik doğruladığında, Azure AD uygulamaya, kullanıcı hakkında benzersiz olarak tanımlayan bilgiler (veya talepler) içeren bir SAML belirteci yayınlar. Varsayılan olarak, bu bilgiler kullanıcının kullanıcı adını, e-posta adresini, adını ve soyadını içerir. Örneğin, uygulama belirli talep değerleri veya kullanıcı adı dışında bir **Ad** biçimi gerektiriyorsa, bu talepleri özelleştirmeniz gerekebilir. Galeri uygulamaları için gereksinimler [uygulamaya özel öğreticilerde](../saas-apps/tutorial-list.md)açıklanmıştır veya uygulama satıcısına sorabilirsiniz. Kullanıcı özniteliklerini ve taleplerini yapılandırmak için genel adımlar aşağıda açıklanmıştır.

1. Kullanıcı **Öznitelikleri ve Talepleri** bölümünde, sağ üst köşedeki **Edit** simgesini (kalem) seçin.

   ![Adım 2 Kullanıcı özniteliklerini ve taleplerini yapılandırma](media/configure-single-sign-on-non-gallery-applications/step-two-user-attributes-claims.png)

2. Ad **Tanımlayıcı Değerini**doğrulayın. Varsayılan değer *user.principalname'dir.* Kullanıcı tanımlayıcısı, uygulama içinde her kullanıcıyı benzersiz olarak tanımlar. Örneğin e-posta adresi hem kullanıcı adı hem de benzersiz tanıtıcı olarak kullanılıyorsa değeri *user.mail* olarak ayarlayın.

3. **Ad Tanımlayıcı Değerini**değiştirmek için, Ad Tanımlayıcı **Değeri** alanı için **Edit** simgesini (kalem) seçin. Gerektiğinde tanımlayıcı biçimi nde ve kaynağında uygun değişiklikleri yapın. Ayrıntılar [için, Bkz. NameId Düzenleme.](../develop/active-directory-saml-claims-customization.md#editing-nameid) İşi bitince değişiklikleri kaydedin. 
 
4. Grup taleplerini yapılandırmak için, talep alanında **döndürülen Gruplar** için **Edit** simgesini seçin. Ayrıntılar için [bkz.](../hybrid/how-to-connect-fed-group-claims.md)

5. Talep eklemek için sayfanın üst kısmında **yeni talep ekle'yi** seçin. **Adı** girin ve uygun kaynağı seçin. **Öznitelik** kaynağını seçerseniz, kullanmak istediğiniz Kaynak **özniteliğini** seçmeniz gerekir. **Çeviri** kaynağını seçerseniz, kullanmak istediğiniz **Dönüşüm** ve **Parametre 1'i** seçmeniz gerekir. Ayrıntılar için uygulamaya [özgü talepler ekleme](../develop/active-directory-saml-claims-customization.md#adding-application-specific-claims)konusuna bakın. İşi bitince değişiklikleri kaydedin. 

6. **Kaydet'i**seçin. Yeni talep tabloda görünür.

   > [!NOTE]
   > Azure AD'deki SAML belirteciyle uygulamanızı özelleştirmenin ek yolları için aşağıdaki kaynaklara bakın.
   >- Azure portalı üzerinden özel roller oluşturmak için [bkz.](../develop/active-directory-enterprise-app-role-management.md)
   >- PowerShell üzerinden iddiaları özelleştirmek için, [iddiaları özelleştir - PowerShell'](../develop/active-directory-claims-mapping.md)e bakın.
   >- Uygulamanız için isteğe bağlı talepleri yapılandırmak için uygulama bildirimini değiştirmek için [bkz.](../develop/active-directory-optional-claims.md)
   >- Belirteçleri, erişim belirteçleri, oturum belirteçleri ve kimlik belirteçleri için belirteç leri ömür [boyu](../develop/active-directory-configurable-token-lifetimes.md)ilkeleri ayarlamak için bkz. Veya Azure AD Koşullu Erişim aracılığıyla kimlik doğrulama oturumlarını kısıtlamak için [kimlik doğrulama oturumu yönetim özelliklerine](https://go.microsoft.com/fwlink/?linkid=2083106)bakın.

## <a name="step-3-manage-the-saml-signing-certificate"></a>3. Adım SAML imzalama sertifikasını yönetme

Azure AD, uygulamaya gönderdiği SAML belirteçlerini imzalamak için bir sertifika kullanır. Azure AD ile uygulama arasındaki güveni ayarlamak için bu sertifikaya ihtiyacınız var. Sertifika biçimiyle ilgili ayrıntılar için uygulamanın SAML belgelerine bakın. Daha fazla bilgi için, [SAML belirtecinde](certificate-signing-options.md) [federe tek oturum açma](manage-certificates-for-federated-single-sign-on.md) ve Gelişmiş sertifika imzalama seçenekleri için sertifikaları yönet'e bakın.

Azure AD'den, Base64 veya Raw formatındaki etkin sertifikayı doğrudan **SAML ile Tek Oturum** Açma ana sayfasından indirebilirsiniz. Alternatif olarak, uygulama metadata XML dosyasını indirerek veya Uygulama federasyonu meta veri URL'sini kullanarak etkin sertifikayı alabilirsiniz. Sertifikalarınızı görüntülemek, oluşturmak veya indirmek (etkin veya etkin olmayan) için aşağıdaki adımları izleyin.

1. **SAML İmza Sertifikası** bölümüne gidin. 

   ![Adım 3 SAML imza sertifikasını yönetme](./media/configure-single-sign-on-non-gallery-applications/step-three-certificate.png)

2. Sertifikanın aşağıdakileri yaptığıdoğrulayın:

   - *İstenilen son kullanma tarihi.* Son kullanma tarihini üç yıla kadar yapılandırabilirsiniz.
   - *İstenilen sertifika için etkin bir durum.* Durum Etkin **değilse,** durumu **Etkin**olarak değiştirin. Durumu değiştirmek için, istenen sertifika nın satırına sağ tıklayın ve **sertifikayı etkin hale'yi**seçin.
   - *Doğru imzalama seçeneği ve algoritması.*
   - *Doğru bildirim e-posta adresi(es).* Etkin sertifika son kullanma tarihine yaklaştığında, Azure AD bu alanda yapılandırılan e-posta adresine bir bildirim gönderir.

2. Sertifikayı indirmek için Base64 biçimi, Ham biçimi veya Federasyon Metadata XML seçeneklerinden birini seçin. Azure AD ayrıca, uygulamaya özgü meta verilere biçimde `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`erişebileceğiniz Uygulama Federasyonu Meta veri **Url'sini** de sağlar.

3. Sertifikayı yönetmek, oluşturmak veya almak için **SAML İmza Sertifikası** bölümünün sağ üst köşesindeki **Edit** simgesini (kalem) seçin.

   ![SAML imzalama sertifikası](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)


   Aşağıdaki eylemlerden herhangi birini alın:

   - Yeni bir sertifika oluşturmak için **Yeni Sertifika'yı**seçin, **Son Kullanma Tarihi'ni**seçin ve ardından **Kaydet'i**seçin. Sertifikayı etkinleştirmek için bağlam menüsünü (**...**) seçin ve **sertifikayı etkin hale'yi**seçin.
   - Özel anahtar ve pfx kimlik bilgilerine sahip bir sertifika yüklemek için **SertifikaYı İçe Aktar'ı** seçin ve sertifikaya göz atın. **PFX Parolasını**girin ve sonra **Ekle'yi**seçin.  
   - Gelişmiş sertifika imzalama seçeneklerini yapılandırmak için aşağıdaki seçenekleri kullanın. Bu seçeneklerin açıklamaları için [Gelişmiş sertifika imzalama seçenekleri](certificate-signing-options.md) makalesine bakın.
      - **İmzalama Seçeneği** açılır listesinde, **Sign SAML yanıtını,** **Sign SAML iddiasını**veya Sign **SAML yanıtını ve iddiasını**seçin.
      - İmza **Algoritması** açılır listesinde **SHA-1** veya **SHA-256'yı**seçin.
   - Etkin sertifikanın son kullanma tarihine yakın olduğunda ek kişileri bilgilendirmek için Bildirim e-posta adresleri alanlarına **e-posta adreslerini** girin.

4. Değişiklik yaptıysanız, **SAML İmza Sertifikası** bölümünün üst kısmında **kaydet'i** seçin. 

## <a name="step-4-set-up-the-application-to-use-azure-ad"></a>4. Adım. Uygulamayı Azure AD'yi kullanacak şekilde ayarlama

**Uygulamayı AyarlamaAdı \<>** bölümü, uygulamada yapılandırılması gereken değerleri listeler, böylece Azure AD'yi SAML kimlik sağlayıcısı olarak kullanır. Gerekli değerler uygulamaya göre değişir. Ayrıntılar için, uygulamanın SAML belgelerine bakın. Belgeleri bulmak için uygulama **adını \<>başlığını ayarla'ya** gidin ve **adım adım yönergeleri görüntüle'yi**seçin. Belgeler, **Yapılandırma oturum açma** sayfasında görünür. Bu sayfa, Oturum Açma **Login URL**URL'si, **Azure AD Tanımlayıcısı**ve Giriş **URL** değerlerini **>\<başlık** başlığında oturum açar.

1. Uygulamayı AyarlaAdı>bölümüne gidin. ** \<** 
   
   ![Adım 4 Uygulamayı ayarlama](media/configure-single-sign-on-non-gallery-applications/step-four-app-config.png)

1. Gerektiğinde bu bölümdeki her satırdaki değeri kopyalayın ve uygulamaya değer eklemek için uygulamaya özgü yönergeleri izleyin. Galeri uygulamaları için, **yönergeleri adım adım görüntüle'yi**seçerek belgeleri görüntüleyebilirsiniz. 
   - **Giriş** URL'si ve **Giriş URL** değerleri, Azure AD örneğiniz için SAML istek işleme bitiş noktası olan aynı uç noktaya kadar çözülür. 
   - **Azure AD Tanımlayıcısı,** uygulamaya verilen SAML belirtecinde **Verenin** değeridir.
2. Tüm değerleri uygun alanlara yapıştırdığınızda **Kaydet'i**seçin.

## <a name="step-5-validate-single-sign-on"></a>5. Adım. Tek oturum açmayı doğrulama

Uygulamanızı Azure AD'yi SAML tabanlı kimlik sağlayıcısı olarak kullanacak şekilde yapılandırdıktan sonra, tek oturum açmanın hesabınızda çalışıp çalışmamadığını görmek için ayarları sınayabilirsiniz. 

2. Bölümle tek oturum açma'yı ** <applicationName> doğrula'ya** kaydırın.

   ![Adım 5 Tek oturum açma yı doğrulayın](media/configure-single-sign-on-non-gallery-applications/step-five-validate.png)

3. **Doğrula**'yı seçin. Test seçenekleri açılır.

4. **Geçerli kullanıcı olarak Oturum Aç'ı**seçin. 

Oturum açma başarılı olursa, kullanıcıları ve grupları SAML uygulamanıza atamaya hazırsınız.
Bir hata iletisi görünüyorsa, aşağıdaki adımları tamamlayın:

1. Hatayla ilgili bilgileri kopyalayıp **Hata neye benziyor?** kutusuna yapıştırın.

    ![Çözüm rehberliği alın](media/configure-single-sign-on-non-gallery-applications/error-guidance.png)

2. **Çözünürlük kılavuzunu al'ı**seçin. Kök neden ve çözümleme kılavuzu görüntülenir.  Bu örnekte, kullanıcı uygulamaya atanmadı.

3. Çözünürlük kılavuzunu okuyun ve mümkünse sorunu düzeltin.

4. Testi başarıyla tamamlana kadar tekrarlayın.

Daha fazla bilgi için bkz: [Azure Etkin Dizini'ndeki uygulamalariçin HATA AyıkLAMA SAML tabanlı tek oturum](../azuread-dev/howto-v1-debug-saml-sso-issues.md)açma.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı veya grupları uygulamaya atama](methods-for-assigning-users-and-groups.md)
- [Otomatik kullanıcı hesabı sağlama yapılandırma](../app-provisioning/configure-automatic-user-provisioning-portal.md)
