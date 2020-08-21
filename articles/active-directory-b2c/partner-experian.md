---
title: Experian ile Azure Active Directory B2C yapılandırma öğreticisi
titleSuffix: Azure AD B2C
description: Kimlik doğrulaması için Experian ile Azure AD B2C kimlik doğrulamasını tümleştirmeyi ve sahtekarlığı önlemek için Kullanıcı özniteliklerine göre doğrulama yapmayı öğrenin.
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/22/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 5c2fb330149d3e6530e7cb0fc3350d5db3fa24cf
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88683887"
---
# <a name="tutorial-for-configuring-experian-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile Experian yapılandırma öğreticisi

Bu örnek öğreticide, [Experian](https://www.experian.com/decision-analytics/account-opening-fraud/microsoft-integration)ile Azure AD B2C tümleştirme hakkında rehberlik sağlıyoruz. Experian, [burada](https://www.experian.com/)bulabileceğiniz çeşitli çözümler sunar.

Bu örnekte, Experian 'in tümleşik dijital kimliği ve sahtekarlık risk platformu **çapraz puanı** kullanılır. Çapraz puan, Kullanıcı kimliğini doğrulamak için kullanılan bir KIMLIK doğrulama hizmetidir. Kaydolma akışı sırasında Kullanıcı tarafından sunulan çeşitli bilgi parçalarını temel alarak risk analizini yapar. Çapraz puan, kullanıcının oturum açmaya devam etmesine izin verilip verilmeyeceğini tespit etmek için kullanılır. Aşağıdaki öznitelikler, çapraz puan Risk analizinde kullanılabilir:

- E-posta
- IP Adresi
- Verilen Ad
- İkinci Ad
- Soyadı
- Adres
- City
- Eyalet/İl
- Posta Kodu
- Ülke/Bölge
- Telefon Numarası

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

- Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

- Azure aboneliğinize bağlı [bir Azure AD B2C kiracısı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) .

## <a name="scenario-description"></a>Senaryo açıklaması

Experian tümleştirmesi aşağıdaki bileşenleri içerir:

- Azure AD B2C – kimlik sağlayıcısı olarak da bilinen kullanıcının kimlik bilgilerini doğrulamadan sorumlu yetkilendirme sunucusu

- Experian – Experian hizmeti kullanıcı tarafından sağlanmış girdileri alır ve kullanıcının kimliğini doğrular

- Özel REST API 'SI – bu API, Azure AD B2C ile Experian hizmeti arasındaki tümleştirmeyi uygular.

Aşağıdaki mimari diyagram uygulamayı gösterir.

![Experian mimarisi için ekran görüntüsü-diyagram](media/partner-experian/experian-architecture-diagram.png)

|Adım | Açıklama |
|:-----| :-----------|
| 1. | Kullanıcı bir oturum açma sayfasına ulaştı. Kullanıcı, yeni bir hesap oluşturmak için kaydolma seçer ve sayfaya bilgi girer. Azure AD B2C Kullanıcı özniteliklerini toplar.
| 2. | Azure AD B2C orta katman API 'sini çağırır ve Kullanıcı özniteliklerine geçirir.
| 3. | Orta katman API 'SI Kullanıcı özniteliklerini toplar ve Experian API 'sinin tüketebileceği bir biçime dönüştürür. Ardından, Experian 'e gönderir.
| 4. | Experian, bilgileri kullanır ve risk analizine göre Kullanıcı kimliğini doğrulamak için onu işler. Ardından, sonucu orta katman API 'sine döndürür.
| 5. | Orta katman API 'SI bilgileri işler ve ilgili bilgileri doğru JSON biçiminde geri gönderir Azure AD B2C.
| 6. | Azure AD B2C, bilgileri ortadaki katman API 'sinden geri alır. Hata yanıtı gösteriyorsa, kullanıcıya bir hata iletisi görüntülenir. Başarılı bir yanıt gösteriyorsa, kullanıcının kimliği doğrulanır ve dizine yazılır.

## <a name="onboard-with-experian"></a>Experian ile ekleme

1. Bir Experian hesabı oluşturmak için, [Experian](https://www.experian.com/decision-analytics/account-opening-fraud/microsoft-integration) ile iletişim kurun

2. Bir hesap oluşturulduktan sonra, API yapılandırması için gereken bilgileri alacaksınız. Aşağıdaki bölümlerde işlem açıklanır.

## <a name="configure-azure-ad-b2c-with-experian"></a>Experian ile Azure AD B2C yapılandırma

### <a name="part-1---deploy-the-api"></a>1. Bölüm-API 'YI dağıtma

Belirtilen API kodunu bir Azure hizmetine dağıtın. Kod, bu [yönergeleri](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019)Izleyerek Visual Studio 'dan yayımlanabilir.

>[!NOTE]
>Azure AD 'yi gerekli ayarlarla yapılandırmak için dağıtılan hizmetin URL 'sine ihtiyacınız vardır.

### <a name="part-2---deploy-the-client-certificate"></a>Bölüm 2-istemci sertifikasını dağıtma

Experian API çağrısı bir istemci sertifikası tarafından korunur. Bu istemci sertifikası, Experian tarafından sağlanacak. Bu [belgede](https://docs.microsoft.com/azure/app-service/environment/certificates#private-client-certificate)bahsedilen yönergeleri izleyerek, sertifikanın Azure App Service 'e yüklenmesi gerekir. Örnek ilke, işlemdeki şu anahtar adımlarını kullanır:

- Sertifikayı güncelleştirin

- `WEBSITE_LOAD_ROOT_CERTIFICATES`Anahtarı sertifikanın parmak iziyle ayarlayın.

### <a name="part-3---configure-the-api"></a>3. kısım-API 'YI yapılandırma

Uygulama ayarları [Azure 'Daki App Service 'te yapılandırılabilir](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings). Bu yöntemde, ayarlar bir depoya denetlenmeden güvenle yapılandırılabilir. REST API için aşağıdaki ayarları sağlamanız gerekir:

| Uygulama ayarları | Kaynak | Notlar |
| :-------- | :------------| :-----------|
|CrossCoreConfig: Tenantıd | Experian hesabı yapılandırması |     |
|CrossCoreConfig: OrgCode | Experian hesabı yapılandırması |     |
|Çapraz puan: ApiEndpoint |Experian hesabı yapılandırması|  |
|Çapraz puan: ClientReference | Experian hesabı yapılandırması | |
| Çapraz puan: ModelCode |Experian hesabı yapılandırması|
| Çapraz puan: OrgCode | Experian hesabı yapılandırması |
| Çapraz puan: SignatureKey  | Experian hesabı yapılandırması |
| Çapraz puan: Tenantıd  | Experian hesabı yapılandırması |
| Çapraz puan: CertificateThumbprint | Experian sertifikası |
| BasicAuth: ApiUsername | API için Kullanıcı adı tanımlama | Extıd yapılandırmasında kullanılır |
| BasicAuth: ApiPassword | API için bir parola tanımlayın | Extıd yapılandırmasında kullanılır

### <a name="part-4---create-api-policy-keys"></a>4. Bölüm-API ilkesi anahtarları oluşturma

Bu [belgeye](https://docs.microsoft.com/azure/active-directory-b2c/secure-rest-api#add-rest-api-username-and-password-policy-keys) başvurun ve API Kullanıcı adı için bir tane olmak üzere iki ilke anahtarı oluşturun, diğeri de daha önce http temel kimlik doğrulaması IÇIN tanımladığınız API parolasıdır.

>[!NOTE]
>İlkeleri daha sonra yapılandırmak için anahtarlar gerekir.

### <a name="part-5---replace-the-configuration-values"></a>5. bölüm-yapılandırma değerlerini değiştirme

Belirtilen özel ilkelerde, aşağıdaki yer tutucuları bulun ve örneğinizi karşılık gelen değerlerle değiştirin

|                      Yer tutucu                       |                                   Değer ile Değiştir                                 |                   Örnek                    |
| ------------------------------------------------------ | -------------------------------------------------------------------------------- | -------------------------------------------- |
| {your_tenant_name}                                     | Kiracınızın kısa adı                                                           | yourtenant.onmicrosoft.com 'dan "kiracınız" |
| {your_trustframeworkbase_policy}                       | TrustFrameworkBase ilkenizin Azure AD B2C adı                  | B2C_1A_experian_TrustFrameworkBase           |
| {your_tenant_IdentityExperienceFramework_appid}        | Azure AD B2C kiracınızda yapılandırılan IdentityExperienceFramework uygulamasının uygulama KIMLIĞI      | 01234567-89ab-cdef-0123-456789ABCDEF         |
| {your_tenant_ ProxyIdentityExperienceFramework _appid} | Azure AD B2C kiracınızda yapılandırılan ProxyIdentityExperienceFramework uygulamasının uygulama KIMLIĞI | 01234567-89ab-cdef-0123-456789ABCDEF         |
| {your_tenant_extensions_appid}                         | Kiracının depolama uygulamasının uygulama KIMLIĞI                                      | 01234567-89ab-cdef-0123-456789ABCDEF         |
| {your_tenant_extensions_app_objectid}                  | Kiracınızın depolama uygulamasının nesne KIMLIĞI                                   | 01234567-89ab-cdef-0123-456789ABCDEF         |
| {your_api_username_key_name}                           | [Burada](#part-4---create-api-policy-keys) oluşturduğunuz Kullanıcı adı anahtarının adı             | B2C \_ 1a \_ RestApiUsername                     |
| {your_api_password_key_name}                           | [Burada](#part-4---create-api-policy-keys) oluşturduğunuz parola anahtarının adı             | B2C \_ 1a \_ RestApiPassword                     |
| {your_app_service_URL}                                 | Ayarladığınız App Service 'in URL 'SI                                             | `https://yourapp.azurewebsites.net`          |

### <a name="part-6---configure-the-azure-ad-b2c-policy"></a>Bölüm 6-Azure AD B2C ilkesini yapılandırma

Azure AD B2C kiracınızı ayarlama ve ilkeleri yapılandırma hakkında yönergeler için bu [belgeye](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications#custom-policy-starter-pack) başvurun.

>[!NOTE]
>Bu örnek ilke [yerel hesaplar başlangıç paketini](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts)temel alır.

>[!NOTE]
> En iyi uygulama olarak, müşterilerin öznitelik koleksiyonu sayfasına onay bildirimi eklemesini öneririz. Kimlik doğrulaması için bilgilerin üçüncü taraf hizmetlere gönderileceğini kullanıcılara bildirin.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Azure AD B2C kiracıyı açın ve Ilkeler altında **Kullanıcı akışları ' nı**seçin.

2. Daha önce oluşturduğunuz **Kullanıcı akışınızı**seçin.

3. **Kullanıcı akışını Çalıştır** ' ı seçin ve ayarları seçin:

   a. **Uygulama**: kayıtlı uygulamayı seçin (örnek JWT)

   b. **Yanıt URL 'si**: **yeniden yönlendirme URL** 'sini seçin

   c. **Kullanıcı akışını Çalıştır**' ı seçin.

4. Kaydolma akışına gidin ve hesap oluşturun

5. Oturumu Kapat

6. Oturum açma akışına git  

7. Çapraz puan bulmaca, **devam**' i girdikten sonra açılır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure AD B2C'deki özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Azure AD B2C özel ilkeleri kullanmaya başlama](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
