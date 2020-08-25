---
title: Jumio ile Azure Active Directory B2C yapılandırma öğreticisi
titleSuffix: Azure AD B2C
description: Otomatik KIMLIK doğrulama için Jumio ile Azure Active Directory B2C yapılandırma öğreticisi, müşteri verilerini koruma
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/20/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 47e06c84eb2e0463b31b7bdea5897ceca4339419
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817806"
---
# <a name="tutorial-for-configuring-jumio-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile Jumio yapılandırma öğreticisi

Bu örnek öğreticide, Azure AD B2C [Jumio](https://www.jumio.com/)ile tümleştirme hakkında rehberlik sağlıyoruz. Jumio, gerçek zamanlı otomatik KIMLIK doğrulamaya izin veren ve müşteri verilerini koruma sağlayan bir KIMLIK doğrulama hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

- Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

- [Azure AD B2C kiracısı](https://review.docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant). Kiracı, Azure aboneliğinize bağlı.

## <a name="scenario-description"></a>Senaryo açıklaması

Jumio tümleştirmesi aşağıdaki bileşenleri içerir:

- Azure AD B2C – kimlik sağlayıcısı olarak da bilinen kullanıcının kimlik bilgilerini doğrulamadan sorumlu yetkilendirme sunucusu

- Jumio: Kullanıcı tarafından sunulan KIMLIK ayrıntılarını alan ve doğrulayan hizmet.

- Ara REST API 'SI – bu API, Azure AD B2C ile Jumio hizmeti arasındaki tümleştirmeyi uygular.

- BLOB depolama: Azure AD B2C ilkelerine özel UI dosyaları sağlamak için kullanılır.

Aşağıdaki mimari diyagram uygulamayı gösterir.

![Jumio-mimari-diyagram ekran görüntüsü](./media/partner-jumio/jumio-architecture-diagram.png)

|Adım | Açıklama |
|:-----| :-----------|
| 1. | Kullanıcı bir oturum açma sayfasına ulaştı. Kullanıcılar yeni bir hesap oluşturmak ve sayfaya bilgi girmek için kaydol ' u seçer. Azure AD B2C Kullanıcı özniteliklerini toplar.
| 2. | Azure AD B2C orta katman API 'sini çağırır ve Kullanıcı özniteliklerine geçirir.
| 3. | Orta katman API 'SI Kullanıcı özniteliklerini toplar ve bunu Jumio API 'sinin tüketebileceği bir biçime dönüştürür. Sonra bunu Jumio 'ye gönderdikten sonra.
| 4. | Jumio, bilgileri kullandıktan ve işleyerek, sonuç orta katman API 'sine döndürülür.
| 5. | Orta katman API 'SI bilgileri işler ve ilgili bilgileri Azure AD B2C 'a geri gönderir.
| 6. | Azure AD B2C, bilgileri ortadaki katman API 'sinden geri alır. Hata yanıtı gösteriyorsa, kullanıcıya bir hata iletisi görüntülenir. Başarılı bir yanıt gösteriyorsa, kullanıcının kimliği doğrulanır ve dizine yazılır.

## <a name="onboard-with-jumio"></a>Jumio ile ekleme

1. Jumıo hesabı oluşturmak için [Jumıo](https://www.jumio.com/contact/) ile iletişim kurun

2. Bir hesap oluşturulduktan sonra API yapılandırmasında bilgiler kullanılır. Aşağıdaki bölümlerde işlem açıklanır.

## <a name="configure-azure-ad-b2c-with-jumio"></a>Jumio ile Azure AD B2C yapılandırma

### <a name="part-1---deploy-the-api"></a>1. Bölüm-API 'YI dağıtma

Belirtilen [API kodunu](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/API/Jumio.Api) bir Azure hizmetine dağıtın. Kod, bu [yönergeleri](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019)Izleyerek Visual Studio 'dan yayımlanabilir.

>[!NOTE]
>Azure AD 'yi gerekli ayarlarla yapılandırmak için dağıtılan hizmetin URL 'sine ihtiyacınız vardır.

### <a name="part-2---deploy-the-client-certificate"></a>Bölüm 2-istemci sertifikasını dağıtma

1. Jumio API çağrısı, bir istemci sertifikası tarafından korunur. Aşağıda bahsedilen PowerShell örnek kodunu kullanarak kendinden imzalı bir sertifika oluşturun:

``` PowerShell
$cert = New-SelfSignedCertificate -Type Custom -Subject "CN=Demo-SigningCertificate" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3") -KeyUsage DigitalSignature -KeyAlgorithm RSA -KeyLength 2048 -NotAfter (Get-Date).AddYears(2) -CertStoreLocation "Cert:\CurrentUser\My"
$cert.Thumbprint
$pwdText = "Your password"
$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText
Export-PfxCertificate -Cert $Cert -FilePath "{your-local-path}\Demo-SigningCertificate.pfx" -Password $pwd.

```

2. Daha sonra sertifika, {} için belirtilen konuma aktarılabilir ``your-local-path`` .

3. Bu [belgede](https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#upload-a-private-certificate)bahsedilen yönergeleri Izleyerek sertifikayı Azure App Service 'e aktarın.

### <a name="part-3---create-a-signingencryption-key"></a>3. kısım-imzalama/şifreleme anahtarı oluşturma

Yalnızca harfler veya rakamlar içeren 64 karakterden büyük uzunlukta rastgele bir dize oluşturun.

Örnek: ``C9CB44D98642A7062A0D39B94B6CDC1E54276F2E7CFFBF44288CEE73C08A8A65``

64 karakterlik uzunluktaki alfasayısal bir değer oluşturmak için aşağıdaki PowerShell betiğini kullanın.

```PowerShell
-join ((0x30..0x39) + ( 0x41..0x5A) + ( 0x61..0x7A) + ( 65..90 ) | Get-Random -Count 64  | % {[char]$_})

```

### <a name="part-4---configure-the-api"></a>Bölüm 4-API 'YI yapılandırma

Uygulama ayarları [Azure 'Daki App Service 'te yapılandırılabilir](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings). Bu yöntemde, ayarlar bir depoya denetlenmeden güvenle yapılandırılabilir. REST API için aşağıdaki ayarları sağlamanız gerekir:

| Uygulama ayarları | Kaynak | Notlar |
| :-------- | :------------| :-----------|
|JumioSettings: AuthUsername | Jumıo hesap yapılandırması |     |
|JumioSettings: AuthPassword | Jumıo hesap yapılandırması |     |
|AppSettings: Signingcertparmak Izi|Otomatik olarak imzalanan sertifikanın parmak izi oluşturuldu|  |
|AppSettings: ıdtokensigningkey| İmzalama anahtarı PowerShell kullanılarak oluşturuldu | |
| AppSettings: ıdtokenencryptionkey |PowerShell kullanılarak oluşturulan şifreleme anahtarı
| AppSettings: ıdtokenıssuer | JWT belirteci için kullanılacak veren (bir GUID değeri tercih edilir) |
| AppSettings: ıdtokenaudience  | JWT belirteci için kullanılacak hedef kitle (bir GUID değeri tercih edilir) |
|AppSettings: BaseRedirectUrl | B2C ilkesinin temel URL 'si | https://{-kiracı-adı}. b2clogin. com/{sizin-uygulama kimliği}|
| WEBSITE_LOAD_CERTIFICATES| Otomatik olarak imzalanan sertifikanın parmak izi oluşturuldu |

### <a name="part-5---deploy-the-ui"></a>5. Bölüm-Kullanıcı arabirimini dağıtma

1. [Depolama hesabınızda bir BLOB depolama kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container)ayarlayın.

2. Kullanıcı arabirimi dosyalarını [Kullanıcı arabirimi klasöründen](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/UI) blob kapsayıcınıza depolayın.

#### <a name="update-ui-files"></a>UI dosyalarını Güncelleştir

1. Kullanıcı arabirimi dosyalarında [ocean_blue](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/UI/ocean_blue) klasöre gidin.

2. Her bir HTML dosyasını açın.

3. {Your-UI-blob-Container-URL} öğesini bulup blob kapsayıcı URL 'niz ile değiştirin.

4. {The-ara-api-URL}} öğesini ara API App Service 'in URL 'siyle bulun ve değiştirin.

>[!NOTE]
> En iyi uygulama olarak, müşterilerin öznitelik koleksiyonu sayfasına onay bildirimi eklemesini öneririz. Kimlik doğrulaması için bilgilerin üçüncü taraf hizmetlere gönderileceğini kullanıcılara bildirin.

### <a name="part-6---configure-the-azure-ad-b2c-policy"></a>Bölüm 6-Azure AD B2C ilkesini yapılandırma

1. Ilkeler klasöründeki [Azure AD B2C ilkesine](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/Policies) gidin.

2. [LocalAccounts başlangıç paketini](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts) indirmek için bu [belgeyi](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications#custom-policy-starter-pack) izleyin

3. Azure AD B2C kiracı için ilkeyi yapılandırın.

>[!NOTE]
>Belirli kiracınızla ilgili olarak belirtilen ilkeleri güncelleştirin.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Azure AD B2C kiracıyı açın ve Ilkeler altında **kimlik deneyimi çerçevesi**' ni seçin.

2. Daha önce oluşturduğunuz **Signupsignın**' ı seçin.

3. **Kullanıcı akışını Çalıştır** ' ı seçin ve ayarları seçin:

   a. **Uygulama**: kayıtlı uygulamayı seçin (örnek JWT)

   b. **Yanıt URL 'si**: **yeniden yönlendirme URL** 'sini seçin

   c. **Kullanıcı akışını Çalıştır**' ı seçin.

4. Kaydolma akışına gidin ve hesap oluşturun

5. Kullanıcı özniteliği oluşturulduktan sonra, akış sırasında jumıo hizmeti çağrılacaktır. Akış tamamlanmamışsa, kullanıcının dizine kaydedilip kaydedilmediğini kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure AD B2C'deki özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Azure AD B2C özel ilkeleri kullanmaya başlama](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
