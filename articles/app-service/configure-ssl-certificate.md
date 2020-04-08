---
title: TLS/SSL sertifikaları ekleme ve yönetme
description: Ücretsiz sertifika oluşturun, Uygulama Hizmeti sertifikası alın, Key Vault sertifikası alın veya Azure Uygulama Hizmeti'nde bir Uygulama Hizmeti sertifikası satın alın.
tags: buy-ssl-certificates
ms.topic: tutorial
ms.date: 10/25/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 4edf710e575bbb26fb0e247e59ff5c796f16226e
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810598"
---
# <a name="add-a-tlsssl-certificate-in-azure-app-service"></a>Azure Uygulama Hizmetinde TLS/SSL sertifikası ekleme

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu makalede, Özel bir sertifikaveya ortak sertifikanın App Service'e nasıl oluşturulabileceğinizi, yükleyip aktarılabildiğinizi veya içe aktarabileceğinizi gösterir. 

Sertifika, Uygulama Hizmeti uygulamanıza veya [işlev uygulamanıza](https://docs.microsoft.com/azure/azure-functions/)eklendikten sonra, özel [bir DNS adını güvenli hale](configure-ssl-bindings.md) getirebilir veya uygulama [kodunuzda kullanabilirsiniz.](configure-ssl-certificate-in-code.md)

Aşağıdaki tablo, Uygulama Hizmeti'ne sertifika eklemek için sahip olduğunuz seçenekleri listeler:

|Seçenek|Açıklama|
|-|-|
| Ücretsiz Uygulama Hizmeti Yönetilen Sertifika Oluşturma (Önizleme) | `www` [Özel etki alanınızı](app-service-web-tutorial-custom-domain.md) veya Uygulama Hizmeti'ndeki çıplak olmayan etki alanadlarınızı güvenli hale getirmek için gereken kullanımı kolay özel bir sertifika. |
| Uygulama Hizmeti sertifikası satın alma | Azure tarafından yönetilen özel bir sertifika. Otomatik sertifika yönetiminin basitliğini ve yenileme ve ihracat seçeneklerinin esnekliğini birleştirir. |
| Key Vault'tan sertifika alma | [PKCS12 sertifikalarınızı](https://wikipedia.org/wiki/PKCS_12)yönetmek için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) kullanıyorsanız kullanışlıdır. [Bkz. Özel sertifika gereksinimleri.](#private-certificate-requirements) |
| Özel sertifika yükleme | Bir üçüncü taraf sağlayıcıdan özel bir sertifikanız varsa, sertifikayı yükleyebilirsiniz. [Bkz. Özel sertifika gereksinimleri.](#private-certificate-requirements) |
| Herkese açık sertifika yükleme | Ortak sertifikalar özel etki alanlarını güvenli hale getirmek için kullanılmaz, ancak uzak kaynaklara erişmesi gerekirse bunları kodunuza yükleyebilirsiniz. |

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını takip etmek için:

- [Bir Uygulama Hizmeti uygulaması oluşturun.](/azure/app-service/)
- Yalnızca ücretsiz sertifika: Bir alt etki `www.contoso.com`alanını (örneğin, [CNAME kaydı](app-service-web-tutorial-custom-domain.md#map-a-cname-record)yla Uygulama Hizmetine) eşle.

## <a name="private-certificate-requirements"></a>Özel sertifika gereksinimleri

> [!NOTE]
> Azure Web Apps AES256'yı **desteklemez** ve tüm pfx dosyaları TripleDES ile şifrelenmelidir.

[Ücretsiz Uygulama Hizmeti Yönetilen Sertifika](#create-a-free-certificate-preview) veya Uygulama Hizmeti [sertifikası](#import-an-app-service-certificate) zaten Uygulama Hizmeti gereksinimlerini karşılar. Özel bir sertifikayı Uygulama Hizmetine yüklemeyi veya almayı seçerseniz, sertifikanızın aşağıdaki gereksinimleri karşılaması gerekir:

* [Parola korumalı PFX dosyası](https://en.wikipedia.org/w/index.php?title=X.509&section=4#Certificate_filename_extensions) olarak dışa aktarılmaz
* En az 2048 bit uzunluğunda özel anahtar içermesi
* Sertifika zincirindeki tüm ara sertifikaları içermesi

TLS bağlamada özel bir etki alanını güvence altına almak için sertifikanın ek gereksinimleri vardır:

* Sunucu kimlik doğrulaması için [Genişletilmiş Anahtar Kullanımı](https://en.wikipedia.org/w/index.php?title=X.509&section=4#Extensions_informing_a_specific_usage_of_a_certificate) içerir (OID = 1.3.6.1.5.5.7.3.1)
* Güvenilir bir sertifika yetkilisi tarafından imzalanması

> [!NOTE]
> **Eliptik Eğri Şifrelemesi (ECC) sertifikaları**, App Service ile birlikte çalışabilir ancak bu makalenin konusu değildir. ECC sertifikaları oluşturmaya ilişkin tam adımlar için sertifika yetkilinizle birlikte çalışın.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="create-a-free-certificate-preview"></a>Ücretsiz sertifika oluşturma (Önizleme)

Ücretsiz Uygulama Hizmeti Yönetilen Sertifikası, Özel DNS adınızı App Service'te güvence altına almak için anahtar teslim bir çözümdür. App Service tarafından yönetilen ve otomatik olarak yenilenen tamamen işlevsel bir TLS/SSL sertifikasıdır. Ücretsiz sertifika aşağıdaki sınırlamalarla birlikte gelir:

- Joker karakter sertifikalarını desteklemez.
- Çıplak etki alanlarını desteklemez.
- Dışa aktarılamaz.
- DNS A-kayıtlarını desteklemez.

> [!NOTE]
> Ücretsiz sertifika DigiCert tarafından verilir. Bazı üst düzey etki alanları için, değeri olan bir [CAA etki alanı kaydı](https://wikipedia.org/wiki/DNS_Certification_Authority_Authorization) oluşturarak DigiCert'in sertifika veren kuruluş olarak açıkça izin vermeniz gerekir: `0 issue digicert.com`.
> 

Ücretsiz Bir Uygulama Hizmeti Yönetilen Sertifika oluşturmak için:

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarlarını** > özel**anahtar sertifikaları (.pfx)** > **Uygulama Hizmeti Yönetilen Sertifika Oluştur'u**seçin.

![Uygulama Hizmeti'nde ücretsiz sertifika oluşturun](./media/configure-ssl-certificate/create-free-cert.png)

CNAME kaydıyla uygulamanız için düzgün bir şekilde eşlenen çıplak olmayan herhangi bir etki alanı iletişim kutusunda listelenir. Ücretsiz sertifika oluşturmak için özel etki alanını seçin ve **Oluştur'u**seçin. Desteklenen her özel etki alanı için yalnızca bir sertifika oluşturabilirsiniz.

İşlem tamamlandığında, Sertifikayı Özel Anahtar **Sertifikaları** listesinde görürsünüz.

![Tamamlanan ücretsiz sertifika oluşturma](./media/configure-ssl-certificate/create-free-cert-finished.png)

> [!IMPORTANT] 
> Bu sertifikayla özel bir etki alanını güvence altına almak için yine de bir sertifika bağlama oluşturmanız gerekir. [Bağlayıcı oluştur'daki](configure-ssl-bindings.md#create-binding)adımları izleyin.
>

## <a name="import-an-app-service-certificate"></a>Uygulama Hizmet Sertifikası Alma

Azure'dan bir Uygulama Hizmeti Sertifikası satın aldıysanız, Azure aşağıdaki görevleri yönetir:

- GoDaddy'den satın alma işlemini halletmek.
- Sertifikanın etki alanı doğrulamasını gerçekleştirir.
- Sertifikayı Azure [Anahtar Kasası'nda](../key-vault/key-vault-overview.md)tutar.
- Sertifika yenilemeyi yönetir (bkz. [sertifika yıy).](#renew-certificate)
- Sertifikayı App Service uygulamalarında alınan kopyalarla otomatik olarak senkronize edin.

Uygulama Hizmeti sertifikası satın almak için [Başlangıç sertifikası siparişine](#start-certificate-order)gidin.

Zaten çalışan bir Uygulama Hizmeti sertifikanız varsa, şunları yapabilirsiniz:

- [Sertifikayı Uygulama Hizmetine aktarın.](#import-certificate-into-app-service)
- [Sertifikayı](#manage-app-service-certificates)yenileme, yeniden anahtarlama ve dışa aktarma gibi yönetme.

### <a name="start-certificate-order"></a>Sertifika siparişini başlat

Uygulama Hizmeti Sertifikası oluşturma <a href="https://portal.azure.com/#create/Microsoft.SSL" target="_blank">sayfasında</a>bir Uygulama Hizmeti sertifikası siparişi başlatın.

![Uygulama Hizmeti sertifikası satın alımına başlayın](./media/configure-ssl-certificate/purchase-app-service-cert.png)

Sertifikayı yapılandırmanıza yardımcı olmak için aşağıdaki tabloyu kullanın. Tamamladığınızda **Oluştur**’a tıklayın.

| Ayar | Açıklama |
|-|-|
| Adı | Uygulama Hizmeti sertifikanız için dostça bir ad. |
| Çıplak Etki Alanı Host Adı | Kök etki alanını burada belirtin. Verilen sertifika hem kök etki alanını `www` hem *de* alt etki alanını güvence altına alıyor. Verilen sertifikada Ortak Ad alanı kök etki alanını, Özne Alternatif `www` Ad alanı ise etki alanını içerir. Yalnızca herhangi bir alt etki alanını güvence altına almak için, alt `mysubdomain.contoso.com`alan adının tam nitelikli alan adını burada belirtin (örneğin, ).|
| Abonelik | Sertifikayı içeren abonelik. |
| Kaynak grubu | Sertifikayı içerecek kaynak grubu. Örneğin, yeni bir kaynak grubu kullanabilir veya Uygulama Hizmeti uygulamanızla aynı kaynak grubunu seçebilirsiniz. |
| Sertifika SKU | Standart bir sertifika veya [joker karakter sertifikası](https://wikipedia.org/wiki/Wildcard_certificate)olsun, oluşturulacak sertifika türünü belirler. |
| Yasal Şartlar | Yasal koşulları kabul ettiğinizi doğrulamak için tıklayın. Sertifikalar GoDaddy'den alınır. |

### <a name="store-in-azure-key-vault"></a>Azure Anahtar Kasasında Depola

Sertifika satın alma işlemi tamamlandıktan sonra, bu sertifikayı kullanmaya başlamadan önce tamamlamanız gereken birkaç adım daha vardır. 

[Uygulama Hizmeti Sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasındasertifikayı seçin ve ardından Sertifika Yapılandırma Adım**1:Depola'yı** **Certificate Configuration** > tıklatın.

![Uygulama Hizmeti sertifikasının Anahtar Kasa depolama alanını yapılandırın](./media/configure-ssl-certificate/configure-key-vault.png)

[Key Vault,](https://docs.microsoft.com/azure/key-vault/key-vault-overview) bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve sırların korunmasına yardımcı olan bir Azure hizmetidir. Uygulama Hizmeti sertifikaları için tercih edilen depolama alanıdır.

Anahtar **Kasa Durumu** sayfasında, yeni bir kasa oluşturmak veya varolan bir kasa seçmek için **Anahtar Kasa Deposu'nu** tıklatın. Yeni bir kasa oluşturmayı seçerseniz, tonozu yapılandırmanıza yardımcı olmak için aşağıdaki tabloyu kullanın ve Oluştur'u tıklatın. Yeni Key Vault'u, Uygulama Hizmeti uygulamanızla aynı abonelik ve kaynak grubunda oluşturun.

| Ayar | Açıklama |
|-|-|
| Adı | Alfasayısal karakterler ve tireler için oluşan benzersiz bir ad. |
| Kaynak grubu | Öneri olarak, Uygulama Hizmeti sertifikanızla aynı kaynak grubunu seçin. |
| Konum | Uygulama Hizmeti uygulamanızla aynı konumu seçin. |
| Fiyatlandırma katmanı | Daha fazla bilgi için [Azure Key Vault fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/key-vault/)bakın. |
| Erişim ilkeleri| Uygulamaları ve kasa kaynaklarına izin verilen erişimi tanımlar. Daha sonra, Grant çeşitli uygulamalar [anahtar kasasına erişim](../key-vault/key-vault-group-permissions-for-apps.md)adımlarını izleyerek, bunu yapılandırabilirsiniz. |
| Sanal Ağ Erişimi | Belirli Azure sanal ağlarına kasa erişimini kısıtlayın. [Azure Anahtar Kasası Güvenlik Duvarlarını ve Sanal Ağları Yapılandırma](../key-vault/key-vault-network-security.md) adımlarını izleyerek daha sonra yapılandırabilirsiniz |

Kasayı seçtikten sonra Key **Vault Depo sayfasını** kapatın. **Adım 1: Mağaza** seçeneği başarı için yeşil bir onay işareti göstermelidir. Bir sonraki adım için sayfayı açık tutun.

### <a name="verify-domain-ownership"></a>Etki alanı sahipliğini doğrulama

Son adımda kullandığınız aynı **Sertifika Yapılandırma** sayfasından **Adım 2:Doğrula'yı**tıklatın.

![Uygulama Hizmeti sertifikası için etki alanını doğrulama](./media/configure-ssl-certificate/verify-domain.png)

**Uygulama Hizmeti Doğrulamasını**seçin. Etki alanını web uygulamanıza zaten eşlediğiniz için [(Ön koşullara](#prerequisites)bakın), zaten doğrulanmış. Bu adımı tamamlamak için **Doğrula'yı** tıklatın. İleti **Sertifikası Etki Alanı Doğrulanana** kadar **Yenile** düğmesini tıklatın.

> [!NOTE]
> Dört tür etki alanı doğrulama yöntemi desteklenir: 
> 
> - **Uygulama Hizmeti** - Etki alanı zaten aynı abonelikteki bir Uygulama Hizmeti uygulamasına eşlendiğinde en uygun seçenektir. Uygulama Hizmeti uygulamasının etki alanı sahipliğini zaten doğrulamış olmasından yararlanır.
> - **Etki Alanı** - [Azure'dan satın aldığınız](manage-custom-dns-buy-domain.md)bir Uygulama Hizmeti etki alanını doğrulayın. Azure, sizin için doğrulama TXT kaydını otomatik olarak ekler ve işlemi tamamlar.
> - **Posta** - Etki alanı yöneticisine bir e-posta göndererek etki alanını doğrulayın. Seçeneği seçtiğinizde talimatlar sağlanır.
> - **El Kitabı** - Bir HTML sayfası (yalnızca**Standart** sertifika) veya DNS TXT kaydı nı kullanarak etki alanını doğrulayın. Seçeneği seçtiğinizde talimatlar sağlanır.

### <a name="import-certificate-into-app-service"></a>Sertifikayı Uygulama Hizmetine Aktarma

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarlarını** > özel**anahtar sertifikaları (.pfx)** > **İthalat Uygulaması Hizmet Sertifikası'nı**seçin.

![Uygulama Hizmetinde Uygulama Hizmeti Sertifikası Alma](./media/configure-ssl-certificate/import-app-service-cert.png)

Yeni satın aldığınız sertifikayı seçin ve **Tamam'ı**seçin.

İşlem tamamlandığında, Sertifikayı Özel Anahtar **Sertifikaları** listesinde görürsünüz.

![Uygulama Hizmeti sertifikasını alma tamamlandı](./media/configure-ssl-certificate/import-app-service-cert-finished.png)

> [!IMPORTANT] 
> Bu sertifikayla özel bir etki alanını güvence altına almak için yine de bir sertifika bağlama oluşturmanız gerekir. [Bağlayıcı oluştur'daki](configure-ssl-bindings.md#create-binding)adımları izleyin.
>

## <a name="import-a-certificate-from-key-vault"></a>Key Vault'tan sertifika alma

Sertifikalarınızı yönetmek için Azure Key Vault kullanıyorsanız, [gereksinimleri karşılattığı](#private-certificate-requirements)sürece Key Vault'tan App Service'e bir PKCS12 sertifikası içe aktarabilirsiniz.

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarlarını** > özel**anahtar sertifikaları (.pfx)** > **İthalat Anahtar KasaSı Sertifikası'nı**seçin.

![Uygulama Hizmetinde Anahtar Kasa sertifikası alma](./media/configure-ssl-certificate/import-key-vault-cert.png)

Sertifikayı seçmenize yardımcı olmak için aşağıdaki tabloyu kullanın.

| Ayar | Açıklama |
|-|-|
| Abonelik | Anahtar Kasası'nın ait olduğu abonelik. |
| Key Vault | Almak istediğiniz sertifikanın olduğu kasa. |
| Sertifika | Kasadaki PKCS12 sertifikaları listesinden seçim yapılı. Kasadaki tüm PKCS12 sertifikaları parmak izleriyle listelenir, ancak tümü App Service'de desteklenmez. |

İşlem tamamlandığında, Sertifikayı Özel Anahtar **Sertifikaları** listesinde görürsünüz. İçe aktarma işlemi bir hatayla başarısız olursa, sertifika [Uygulama Hizmeti gereksinimlerini](#private-certificate-requirements)karşılamıyor.

![İthalat Anahtar Kasası sertifikası tamamlandı](./media/configure-ssl-certificate/import-app-service-cert-finished.png)

> [!IMPORTANT] 
> Bu sertifikayla özel bir etki alanını güvence altına almak için yine de bir sertifika bağlama oluşturmanız gerekir. [Bağlayıcı oluştur'daki](configure-ssl-bindings.md#create-binding)adımları izleyin.
>

## <a name="upload-a-private-certificate"></a>Özel sertifika yükleme

Sertifika sağlayıcınızdan sertifika aldıktan sonra, Uygulama Hizmeti'ne hazır hale getirmek için bu bölümdeki adımları izleyin.

### <a name="merge-intermediate-certificates"></a>Ara sertifikaları birleştirme

Sertifika yetkiliniz size sertifika zincirinde birden çok sertifika verirse, sertifikaları sırayla birleştirmeniz gerekir.

Bunu yapmak için, aldığınız her sertifikayı bir metin düzenleyicisinde açın.

Birleştirilmiş sertifika için _mergedcertificate.crt_ adlı bir dosya oluşturun. Bir metin düzenleyicisinde her bir sertifikanın içeriğini bu dosyaya kopyalayın. Sertifikalarınızın sırası, sertifikanızla başlayıp kök sertifika ile sona ererek sertifika zincirindeki sırayla aynı olmalıdır. Aşağıdaki örneğe benzer şekilde görünür:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Sertifikayı PFX dosyasına aktarma

Birleştirilmiş TLS/SSL sertifikanızı, sertifika isteğinizin oluşturulduğu özel anahtarla dışa aktarın.

Sertifika isteğinizi OpenSSL kullanarak oluşturduysanız bir özel anahtar dosyası oluşturduğunuz anlamına gelir. Sertifikanızı PFX dosyasına aktarmak için aşağıdaki komutu çalıştırın. Yer tutucuları _ &lt;özel anahtar dosyası>_ ve _ &lt;birleştirilmiş sertifika-dosya>_ özel anahtarınızın ve birleştirilmiş sertifika dosyanızın yollarıyla değiştirin.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Sorulduğunda bir dışarı aktarma parolası tanımlayın. TLS/SSL sertifikanızı daha sonra App Service'e yüklerken bu parolayı kullanırsınız.

Sertifika isteğinizi oluşturmak için IIS veya _Certreq.exe_ kullandıysanız, sertifikayı yerel makinenize yükleyin ve sonra [sertifikayı PFX’e aktarın](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-certificate-to-app-service"></a>Sertifikayı Uygulama Hizmetine Yükleme

Artık sertifikayı Uygulama Hizmeti'ne yüklemeye hazırsınız.

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarlarını** > özel**anahtar sertifikaları (.pfx)** > **Yükleme Sertifikası'nı**seçin.

![Uygulama Hizmetine özel sertifika yükleme](./media/configure-ssl-certificate/upload-private-cert.png)

**PFX Sertifika Dosyası**’nda PFX dosyanızı seçin. **Sertifika parolası** alanına PFX dosyasını dışa aktardığınızda oluşturduğunuz parolayı yazın. Bittiğinde, **Yükle'yi**tıklatın. 

İşlem tamamlandığında, Sertifikayı Özel Anahtar **Sertifikaları** listesinde görürsünüz.

![Yükleme sertifikası tamamlandı](./media/configure-ssl-certificate/create-free-cert-finished.png)

> [!IMPORTANT] 
> Bu sertifikayla özel bir etki alanını güvence altına almak için yine de bir sertifika bağlama oluşturmanız gerekir. [Bağlayıcı oluştur'daki](configure-ssl-bindings.md#create-binding)adımları izleyin.
>

## <a name="upload-a-public-certificate"></a>Herkese açık sertifika yükleme

Genel sertifikalar *.cer* biçiminde desteklenir. 

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarları** > **Genel Sertifikalar (.cer)** > Genel Anahtar Sertifikası**Yükle'yi**tıklatın.

**Ad'** de, sertifika için bir ad yazın. **CER Sertifika dosyasında**CER dosyanızı seçin.

**Karşıya Yükle**'ye tıklayın.

![Uygulama Hizmetinde herkese açık sertifika yükleme](./media/configure-ssl-certificate/upload-public-cert.png)

Sertifika yüklendikten sonra, sertifikanın parmak izini kopyalayın ve [sertifikayı erişilebilir hale getir'e](configure-ssl-certificate-in-code.md#make-the-certificate-accessible)bakın.

## <a name="manage-app-service-certificates"></a>Uygulama Hizmeti sertifikalarını yönetme

Bu bölümde, Uygulama Hizmeti sertifikası içe aktar'da satın aldığınız bir Uygulama [Hizmeti sertifikasını](#import-an-app-service-certificate)nasıl yönetebilirsiniz.

- [Rekey sertifikası](#rekey-certificate)
- [Sertifikayı yenileme](#renew-certificate)
- [Sertifikayı dışarı aktarma](#export-certificate)
- [Sertifikayı silme](#delete-certificate)

### <a name="rekey-certificate"></a>Rekey sertifikası

Sertifikanızın özel anahtarının tehlikeye atIldiğini düşünüyorsanız, sertifikanızı yeniden anahtarlayabilirsiniz. [Uygulama Hizmeti Sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasındaki sertifikayı seçin ve ardından sol gezintiden **Rekey ve Eşitle'yi** seçin.

İşlemi başlatmak için **Rekey'i** tıklatın. Bu işlemin tamamlanması 1-10 dakika sürebilir.

![Uygulama Hizmeti sertifikasını yeniden anahtarlama](./media/configure-ssl-certificate/rekey-app-service-cert.png)

Sertifikanızı yeniden anahtarlama, sertifika yetkilisinden verilen yeni bir sertifikayla sertifikayı teslim alar.

Rekey işlemi tamamlandıktan sonra **Eşitle'yi**tıklatın. Eşitleme işlemi, uygulamanızda herhangi bir kapalı kalma süresine neden olmadan App Hizmeti'ndeki sertifikanın ana bilgisayar adı bağlamalarını otomatik olarak güncelleştirir.

> [!NOTE]
> **Eşitle'yi**tıklatmazsan, Uygulama Hizmeti sertifikanızı 48 saat içinde otomatik olarak eşitler.

### <a name="renew-certificate"></a>Sertifikayı yenileme

Sertifikanızın istediğiniz zaman otomatik olarak yenilenmesini açmak [için, Uygulama Hizmeti Sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasındaki sertifikayı seçin ve ardından sol navigasyonda **Otomatik Yenileme Ayarlarını** tıklatın. Varsayılan olarak, Uygulama Hizmeti Sertifikalarının bir yıllık geçerlilik süresi vardır.

**Devam et'i** seçin ve **Kaydet'e**tıklayın. Otomatik yenileme açıksa, sertifikalar sona ermeden 60 gün önce otomatik olarak yenilenmeye başlayabilir.

![Uygulama Hizmeti sertifikasını otomatik olarak yenile](./media/configure-ssl-certificate/auto-renew-app-service-cert.png)

Bunun yerine sertifikayı el ile yenilemek için **El Ile Yenile'yi**tıklatın. Sertifikanızın süresinin bitiminden 60 gün önce el ile yenilemeisteğiniz.

Yenileme işlemi tamamlandıktan sonra **Eşitle'yi**tıklatın. Eşitleme işlemi, uygulamanızda herhangi bir kapalı kalma süresine neden olmadan App Hizmeti'ndeki sertifikanın ana bilgisayar adı bağlamalarını otomatik olarak güncelleştirir.

> [!NOTE]
> **Eşitle'yi**tıklatmazsan, Uygulama Hizmeti sertifikanızı 48 saat içinde otomatik olarak eşitler.

### <a name="export-certificate"></a>Sertifikayı dışarı aktarma

Uygulama Hizmet Sertifikası [Bir Anahtar Kasa sırrı](../key-vault/about-keys-secrets-and-certificates.md#key-vault-secrets)olduğundan, bir PFX kopyasını dışa aktarabilir ve diğer Azure hizmetleri için veya Azure dışında kullanabilirsiniz.

App Service Certificate'ı PFX dosyası olarak dışa aktarmak için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutları çalıştırın. [Azure CLI'yi yüklediyseniz](https://docs.microsoft.com/cli/azure/install-azure-cli)yerel olarak da çalıştırabilirsiniz. Yer tutucuları, Uygulama Hizmeti sertifikasını oluşturduğunuzda kullandığınız adlarla [değiştirin.](#start-certificate-order)

```azurecli-interactive
secretname=$(az resource show \
    --resource-group <group-name> \
    --resource-type "Microsoft.CertificateRegistration/certificateOrders" \
    --name <app-service-cert-name> \
    --query "properties.certificates.<app-service-cert-name>.keyVaultSecretName" \
    --output tsv)

az keyvault secret download \
    --file appservicecertificate.pfx \
    --vault-name <key-vault-name> \
    --name $secretname \
    --encoding base64
```

İndirilen *appservicecertificate.pfx* dosyası, hem genel hem de özel sertifikaları içeren ham bir PKCS12 dosyasıdır. Her istemi, alma parolası ve PEM geçiş tümceciği için boş bir dize kullanın.

### <a name="delete-certificate"></a>Sertifikayı silme 

Uygulama Hizmeti sertifikasının silinmesi son ve geri alınamaz. Uygulama Hizmeti Sertifikası kaynağının silinmesi, sertifikanın iptal edilmesiyle sonuçlanır. Bu sertifikaile Uygulama Hizmeti'ndeki tüm bağlamalar geçersiz olur. Azure, yanlışlıkla silinmesini önlemek için sertifikayı kilitler. Bir Uygulama Hizmeti sertifikasını silmek için, önce sertifikadaki silme kilidini kaldırmanız gerekir.

Uygulama Hizmeti Sertifikaları sayfasındaki sertifikayı seçin ve ardından sol navigasyonda [Kilitler'i](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) seçin. **Locks**

**Sil**' i sil türüyle sertifikanızdaki kilidi bulun. Sağdaki sedle' yi **seçin.**

![Uygulama Hizmeti sertifikası için kilidi silme](./media/configure-ssl-certificate/delete-lock-app-service-cert.png)

Artık Uygulama Hizmeti sertifikasını silebilirsiniz. Sol gezintiden **Genel Bakış** > **Sil'i**seçin. Onay iletişim kutusunda, sertifika adını yazın ve **Tamam'ı**seçin.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure Uygulama Hizmetinde TLS/SSL bağlama ile özel bir DNS adı güvenliğini sağlama](configure-ssl-bindings.md)
* [HTTPS zorlama](configure-ssl-bindings.md#enforce-https)
* [TLS 1.1/1.2 zorlama](configure-ssl-bindings.md#enforce-tls-versions)
* [Azure Uygulama Hizmeti'nde kodunuzda TLS/SSL sertifikası kullanma](configure-ssl-certificate-in-code.md)
* [SSS : Uygulama Hizmet Sertifikaları](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
