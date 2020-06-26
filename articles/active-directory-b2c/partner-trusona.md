---
title: Trusona ve Azure Active Directory B2C
titleSuffix: Azure AD B2C
description: Parolasız kimlik doğrulamasını etkinleştirmek için Azure AD B2C bir kimlik sağlayıcısı olarak Trusona nasıl ekleneceğini öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/08/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 78de242cb6fd1d670dc9564a2725070b7424b5b5
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85385561"
---
# <a name="integrating-trusona-with-azure-active-directory-b2c"></a>Truslonwith Azure Active Directory B2C tümleştirme

Trusona, passwordless kimlik doğrulaması, Multi-Factor Authentication ve dijital lisans taramasını etkinleştirerek, oturum açma güvenliğini sağlamaya yardımcı olan bağımsız bir yazılım satıcısı (ISV) sağlayıcısıdır. Bu makalede, passwordless kimlik doğrulamasını etkinleştirmek için Azure AD B2C ' de bir kimlik sağlayıcısı olarak Trusona nasıl ekleneceğini öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure aboneliğinize bağlı [bir Azure AD B2C kiracısı](tutorial-create-tenant.md) .
* Trusona 'da bir [deneme hesabı](https://www.trusona.com/aadb2c)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu senaryoda, Trusona, passwordless kimlik doğrulamasını etkinleştirmek için Azure AD B2C bir kimlik sağlayıcısı görevi görür. Aşağıdaki bileşenler çözümü yapar:

* Azure AD B2C Birleşik oturum açma ve kaydolma ilkesi
* Truslona kimlik sağlayıcısı olarak Azure AD B2C eklendi
* İndirilebilir Truslonapp

![Truslonarchitecture diyagramı](media/partner-trusona/trusona-architecture-diagram.png)


|  |  |
|------|------|
|1     | Kullanıcı oturum açmaya veya uygulamaya kaydolma girişiminde bulunur. Kullanıcının kimliği, Azure AD B2C kaydolma ve oturum açma ilkesi aracılığıyla doğrulanır. Kaydolma işlemi sırasında kullanıcının daha önce, Truslonapp 'ten daha önce doğrulanmış e-posta adresi kullanılır.     |
|2     | Azure B2C, dolaylı akışı kullanarak kullanıcıyı Truslonopenıd Connect (OıDC) kimlik sağlayıcısına yönlendirir.     |
|3     | Masaüstü bılgısayar tabanlı oturum açma işlemleri için Trusona, truslonapp ile taramak üzere benzersiz, durumsuz, animasyonlu ve dinamik bir QR kodu görüntüler. Mobil tabanlı oturum açma işlemleri için Trusona, truslonapp 'i açmak için bir "ayrıntılı bağlantı" kullanır. Bu iki yöntem, cihaz ve sonuçta Kullanıcı keşfi için kullanılır.     |
|4     | Kullanıcı, görünen QR kodunu Truslonapp ile tarar.     |
|5     | Kullanıcının hesabı Trusloncloud hizmetinde bulunur ve kimlik doğrulaması hazırlanır.     |
|6     | Trusona bulut hizmeti, truslonapp 'e gönderilen anında iletme bildirimi aracılığıyla kullanıcıya bir kimlik doğrulama sınaması yayınlar:<br>a. Kullanıcıdan kimlik doğrulama sınaması istenir. <br> b. Kullanıcı sınamayı kabul veya reddetmeyi seçer. <br> c. Kullanıcının güvenli şifreleme/güvenilir yürütme ortamında bir özel anahtarla testi onaylamak ve imzalamak için işletim sistemi güvenliği (örneğin, biyometrik, geçiş kodu, PIN veya model) kullanması istenir. <br> d. Truslonapp, gerçek zamanlı olarak kimlik doğrulamanın parametrelerine göre dinamik bir yeniden yürütme önleme yükü üretir. <br> e. Tüm yanıt, güvenli şifreleme/güvenilir yürütme ortamındaki özel bir anahtar tarafından imzalanır (ikinci bir zaman için) ve doğrulama için Trusloncloud Service 'e döndürülür.      |
|7     |  Trusona bulut hizmeti, kullanıcıyı bir id_token Başlatan uygulamaya geri yönlendirir. Azure AD B2C, kimlik sağlayıcısı kurulumu sırasında yapılandırılan Truslonin yayımlanmış OpenID yapılandırmasını kullanarak id_token doğrular.    |
|  |  |

## <a name="onboard-with-trusona"></a>Trusona ekleme

1. Truslonaccount oluşturmak ve başlamak için [formu](https://www.trusona.com/aadb2c) doldurun.

2. Truslonmobile uygulamasını App Store 'dan indirin. Uygulamayı yükleyip e-postanızı kaydedin.

3. Yazılım tarafından gönderilen güvenli "sihirli bağlantı" üzerinden e-postanızı doğrulayın.  

4. Self servis için [Truslondeveloper panosuna](https://dashboard.trusona.com) gidin.

5. **Başlamaya çalışıyorum ' ı** seçin ve truslonapp 'inizle kimlik doğrulaması yapın.

6. Sol Gezinti panelinden **OIDC tümleştirmeler**' i seçin.

7. **OpenID Connect tümleştirmesi oluştur**' u seçin.

8. Tercih ettiğiniz bir **ad** belirtin ve **Istemci yeniden yönlendirme Konağı alanında**daha önce sağlanmış olan etki alanı bilgilerini (örneğin, Contoso) kullanın.  

   > [!NOTE]
   > Azure Active Directory ilk etki alanı adı Istemci yeniden yönlendirme Konağı olarak kullanılır.

9. [Truslonıntegration kılavuzundaki](https://docs.trusona.com/integrations/aad-b2c-integration/)yönergeleri izleyin. İstendiğinde, önceki adımda başvurulan ilk etki alanı adını (örneğin, Contoso) kullanın.  

## <a name="integrate-with-azure-ad-b2c"></a>Azure AD B2C ile tümleştirme

### <a name="add-a-new-identity-provider"></a>Yeni bir kimlik sağlayıcısı ekleyin

> [!NOTE]
> Henüz bir tane yoksa, Azure aboneliğinize bağlı [bir Azure AD B2C kiracı oluşturun](tutorial-create-tenant.md) .

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.

4. **Pano**  >  **Azure Active Directory B2C**  >  **kimlik sağlayıcıları**' na gidin.

3. **Kimlik sağlayıcıları**' nı seçin.

4. **Ekle**'yi seçin.

### <a name="configure-an-identity-provider"></a>Kimlik sağlayıcısı yapılandırma  

1. **Kimlik sağlayıcısı türü**  >  **OpenID Connect (Önizleme)** öğesini seçin.

2. Kimlik sağlayıcısını ayarlamak için formu doldurun:  

   | Özellik | Değer  |
   | :--- | :--- |
   | Meta veri URL 'SI | `https://gateway.trusona.net/oidc/.well-known/openid-configuration`|
   | İstemci Kimliği | Trusona e-postayla gönderilir |
   | Kapsam | OpenID profili e-postası |
   | Yanıt türü | Id_token |
   | Yanıt modu  | Form_post |

3. **Tamam**’ı seçin.  

4. **Bu kimlik sağlayıcısının taleplerini eşle**' yi seçin.  

5. Kimlik sağlayıcısını eşlemek için formu doldurun:

   | Özellik | Değer  |
   | :--- | :--- |
   | UserID | Alt  |
   | Görünen ad | takma ad |
   | Ad | given_name |
   | Soyadı | Family_name |
   | Yanıt modu | e-posta |

6. Yeni OıDC Identity sağlayıcınızın kurulumunu gerçekleştirmek için **Tamam ' ı** seçin.

### <a name="create-a-user-flow-policy"></a>Kullanıcı akış ilkesi oluşturma

1. Şimdi, B2C kimlik sağlayıcılarınızda listelenen **Yeni bir OpenID Connect kimlik sağlayıcısı** olarak truslonsee görmeniz gerekir.

2. Sol Gezinti panelinden **Kullanıcı akışları ' nı (ilkeler)** seçin.

3. **Add**  >  **Yeni Kullanıcı akışı**Ekle ' yi seçin  >  **ve oturum açın**.

### <a name="configure-the-policy"></a>Ilkeyi yapılandırma

1. İlkenizi adlandırın.

2. Yeni oluşturduğunuz **Truslonıdentity sağlayıcınızı**seçin.

3. Trusloni doğal olarak Multi-Factor olduğundan, Multi-Factor Authentication 'ı devre dışı bırakmak en iyisidir.

4. **Oluştur**'u seçin.

5. **Kullanıcı öznitelikleri ve talepler**altında **daha fazla göster**' i seçin. Formunda, önceki bölümde kimlik sağlayıcınızın kurulumu sırasında belirttiğiniz en az bir öznitelik seçin.

6. **Tamam**’ı seçin.  

### <a name="test-the-policy"></a>Ilkeyi test etme

1. Yeni oluşturduğunuz ilkeyi seçin.

2. **Kullanıcı akışını Çalıştır**' ı seçin.

3. Formunda, yanıtlama URL 'sini girin.

4. **Kullanıcı akışını Çalıştır**' ı seçin. Truslonoıdc Gateway 'e yeniden yönlendirilmelisiniz. Truslongateway 'de, Görünen güvenli QR kodunu truslonapp ile veya truslonmobile SDK kullanarak özel bir uygulamayla tarayın.

5. Güvenli QR kodu tarandıktan sonra, adım 3 ' te tanımladığınız yanıt URL 'sine yeniden yönlendirilmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar  

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure AD B2C'deki özel ilkeler](custom-policy-overview.md)

- [AAD B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md?tabs=applications)
