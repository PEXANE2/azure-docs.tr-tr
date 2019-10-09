---
title: SSL sertifikası yükleme ve bağlama-Azure App Service | Microsoft Docs
description: Özel bir SSL sertifikasını, Azure App Service Web uygulamanıza, mobil uygulama arka ucuna veya API uygulamasına nasıl bağlayacağınızı öğrenin.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: ''
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/06/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c5095bc8c274ef0985b00459b0d088371ab24d88
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177038"
---
# <a name="tutorial-upload-and-bind-ssl-certificates-to-azure-app-service"></a>Öğretici: Azure App Service SSL sertifikalarını karşıya yükleme ve bağlama

[Azure App Service](overview.md) , yüksek düzeyde ölçeklenebilir, kendini yayama bir Web barındırma hizmeti sağlar. Bu öğreticide, güvenilir bir sertifika yetkilisinden satın aldığınız sertifikayla App Service özel bir etki alanının güvenliğini sağlama gösterilmektedir. Ayrıca, uygulamanızın ihtiyaç duyacağı özel ve genel sertifikaların nasıl karşıya yükleneceğini de gösterir. İşiniz bittiğinde, uygulamanıza özel DNS etki alanının HTTPS uç noktasında erişebileceksiniz.

![Özel SSL sertifikasına sahip Web uygulaması](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Bu öğreticide, aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltin
> * Özel bir etki alanını sertifikayla güvenli hale getirme
> * Özel bir sertifikayı karşıya yükle
> * Ortak sertifikayı karşıya yükle
> * Sertifikaları yenileme
> * HTTPS 'yi zorla
> * TLS 1.1/1.2 'yi zorla
> * Betiklerle TLS yönetimini otomatikleştirin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Özel BIR DNS adını App Service uygulamanıza eşleyin](app-service-web-tutorial-custom-domain.md) (özel bir etki alanı güvenli hale getirilçalışıyorsa)
- Güvenilir bir sertifika yetkilisinden sertifika alma
- Sertifika isteğini imzalamak için kullandığınız özel anahtara sahip (özel sertifikalar için)

<a name="requirements"></a>

## <a name="prepare-a-private-certificate"></a>Özel bir sertifika hazırlama

Bir etki alanının güvenliğini sağlamak için, sertifikanın aşağıdaki tüm gereksinimleri karşılaması gerekir:

* Sunucu kimlik doğrulaması için yapılandırıldı
* Güvenilen bir sertifika yetkilisi tarafından imzalandı
* Parola korumalı PFX dosyası olarak verildi
* En az 2048 bit uzunluğunda özel anahtar içerir
* Sertifika zincirindeki tüm ara sertifikaları içerir

> [!TIP]
> Özel bir SSL sertifikası almanız gerekiyorsa, Azure portal doğrudan bir tane alabilir ve uygulamanıza aktarabilirsiniz. [App Service sertifikaları öğreticisini](web-sites-purchase-ssl-web-site.md)izleyin.

> [!NOTE]
> **Eliptik Eğri Şifreleme (ECC) sertifikaları** App Service ile çalışabilir, ancak bu makalede kapsanmayan değildir. ECC sertifikaları oluşturmak için tam adımlarda sertifika yetkilinizle birlikte çalışın.

Sertifika sağlayıcınızdan bir sertifika edindikten sonra, bu bölümdeki adımları izleyerek App Service için hazırlayın.

### <a name="merge-intermediate-certificates"></a>Ara sertifikaları birleştirme

Sertifika yetkiliniz sertifika zincirinde birden çok sertifika veriyorsa, sertifikaları sırayla birleştirmeniz gerekir.

Bunu yapmak için, aldığınız her sertifikayı bir metin düzenleyicisinde açın.

Birleştirilmiş sertifika için, _birleştirisertifika. CRT_adlı bir dosya oluşturun. Bir metin düzenleyicisinde, her bir sertifikanın içeriğini bu dosyaya kopyalayın. Sertifikalarınızın sırası sertifika zincirinde, sertifikanıza ve kök sertifikayla biten sırayı izlemelidir. Aşağıdaki örneğe benzer şekilde görünür:

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

### <a name="export-certificate-to-pfx"></a>Sertifikayı PFX 'e aktar

Birleştirilmiş SSL sertifikanızı, Sertifika isteğinizin oluşturulduğu özel anahtarla dışarı aktarın.

OpenSSL kullanarak sertifika isteğinizi oluşturduysanız, bir özel anahtar dosyası oluşturdunuz. Sertifikanızı PFX 'e aktarmak için aşağıdaki komutu çalıştırın. _@No__t-1 özel-anahtar-dosya >_ ve _&lt;birleştirilmiş-sertifika-dosyası >_ yer tutucuları, özel anahtarınıza ve birleştirilmiş sertifika dosyanıza yönelik yollarla değiştirin.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

İstendiğinde, bir dışarı aktarma parolası tanımlayın. Daha sonra App Service SSL sertifikanızı karşıya yüklerken bu parolayı kullanacaksınız.

Sertifika isteğinizi oluşturmak için IIS veya _CertReq. exe_ ' yi kullandıysanız, sertifikayı yerel makinenize YÜKLEYIP [sertifikayı PFX 'e aktarın](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

Artık App Service sertifikayı karşıya yükleyin.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Özel bir etki alanının güvenliğini sağlama

> [!TIP]
> Özel bir SSL sertifikası almanız gerekiyorsa, Azure portal doğrudan bir tane alabilir ve uygulamanıza bağlayabilirsiniz. [App Service sertifikaları öğreticisini](web-sites-purchase-ssl-web-site.md)izleyin.

[Özel bir etki alanını](app-service-web-tutorial-custom-domain.md) bir üçüncü taraf sertifikasıyla güvenli hale getirmek için, [hazırlanan özel sertifikayı](#prepare-a-private-certificate) karşıya yükleyip özel etki alanına bağlarsınız, ancak App Service işlemi sizin için basitleştirir. Aşağıdaki adımları uygulayın:

Uygulamanızın sol gezinti bölmesinde **özel etki alanları** ' na tıklayın ve ardından korumak istediğiniz etki alanı Için **bağlama Ekle** ' ye tıklayın. Bir etki alanı için **bağlama ekleme** ' yi görmüyorsanız, zaten güvenlidir ve **güvenli** bir SSL durumuna sahip olmalıdır.

![Etki alanına bağlama ekleme](./media/app-service-web-tutorial-custom-ssl/secure-domain-launch.png)

**Sertifikayı karşıya yükle**' ye tıklayın.

**PFX Sertifika dosyasında**PFX dosyanızı seçin. **Sertifika parolası**' nda, pfx dosyasını aktardığınızda oluşturduğunuz parolayı yazın.

Tamamlamak için **Karşıya Yükle**.

![Etki alanı için sertifikayı karşıya yükle](./media/app-service-web-tutorial-custom-ssl/secure-domain-upload.png)

Azure 'un sertifikanızı karşıya yüklemesi için bekleyin ve SSL bağlamaları iletişim kutusunu başlatın.

SSL bağlamaları iletişim kutusunda, karşıya yüklediğiniz sertifikayı ve SSL türünü seçin ve ardından **bağlama Ekle**' ye tıklayın.

> [!NOTE]
> Aşağıdaki SSL türleri desteklenir:
>
> - **[SNI tabanlı SSL](https://en.wikipedia.org/wiki/Server_Name_Indication)** -birden çok SNı tabanlı SSL bağlamaları eklenebilir. Bu seçenek, birden çok SSL sertifikasının aynı IP adresindeki birden fazla etki alanının güvenliğini sağlar. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNı destekler ( [sunucu adı belirtme](https://wikipedia.org/wiki/Server_Name_Indication)daha kapsamlı tarayıcı destek bilgileri bulabilirsiniz).
> - **IP tabanlı SSL** -yalnızca tek bir IP tabanlı SSL bağlaması eklenebilir. Bu seçenek, yalnızca bir SSL sertifikasının ayrılmış bir genel IP adresinin güvenliğini sağlamasına izin verir. Birden çok etki alanının güvenliğini sağlamak için, bunların tümünü aynı SSL sertifikası ile güvenli hale getirin. Bu, SSL bağlama için geleneksel bir seçenektir.

![SSL 'yi etki alanına bağlama](./media/app-service-web-tutorial-custom-ssl/secure-domain-bind.png)

Etki alanının SSL durumu şimdi **güvenli**olarak değiştirilmelidir.

![Etki alanı güvenliği](./media/app-service-web-tutorial-custom-ssl/secure-domain-finished.png)

> [!NOTE]
> **Özel etki alanlarında** **güvenli** bir durum, bir sertifikayla güvenli hale getirildiği anlamına gelir, App Service ancak örneğin, tarayıcının otomatik olarak imzalanıp imzalanmadığını veya dolduğunu (örneğin, tarayıcıların bir hata veya uyarı görüntülemesine da neden olabilir) kontrol etmez.

## <a name="remap-a-record-for-ip-ssl"></a>IP SSL için bir kaydı yeniden eşleyin

Uygulamanızda IP tabanlı SSL kullanmıyorsanız, [özel etki alanınız Için test https](#test)'ye atlayın.

Varsayılan olarak, uygulamanız paylaşılan bir genel IP adresi kullanır. IP tabanlı SSL ile bir sertifika bağladığınızda App Service, uygulamanız için yeni ve ayrılmış bir IP adresi oluşturur.

Uygulamanız için bir A kaydı eşleştirdiyseniz, etki alanı kayıt defterinizi bu yeni, ayrılmış IP adresiyle güncelleştirin.

Uygulamanızın **özel etki alanı** sayfası, yeni ve ayrılmış IP adresi ile güncelleştirilir. [Bu IP adresini kopyalayın](app-service-web-tutorial-custom-domain.md#info)ve ardından [bir kaydı](app-service-web-tutorial-custom-domain.md#map-an-a-record) bu yeni IP adresiyle yeniden eşleyin.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Şimdi yapılacak tüm bunlar, HTTPS 'nin özel etki alanınız için çalıştığından emin olmak içindir. Çeşitli tarayıcılarda, uygulamanızı görçalıştığını görmek için `https://<your.custom.domain>` ' a gidin.

![Azure uygulamasına Portal gezintisi](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Uygulamanız size sertifika doğrulama hataları veriyorsa, muhtemelen kendinden imzalı bir sertifika kullanıyorsunuz demektir.
>
> Böyle bir durum söz konusu değilse, sertifikanızı PFX dosyasına aktardığınızda ara sertifikalara ayrıldınız.

## <a name="renew-certificates"></a>Sertifikaları yenileme

Bağlama IP tabanlı olsa bile, bir bağlamayı sildiğinizde gelen IP adresiniz değişebilir. Bu, özellikle bir IP tabanlı bağlamadaki bir sertifikayı yenilediğinizde önemlidir. Uygulamanızın IP adresindeki bir değişikliği önlemek için aşağıdaki adımları sırasıyla izleyin:

1. Yeni sertifikayı karşıya yükleyin.
2. Yeni sertifikayı, eski olanı silmeden istediğiniz özel etki alanına bağlayın. Bu eylem, eskisini kaldırmak yerine bağlamayı değiştirir.
3. Eski sertifikayı silin. 

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS 'yi zorla

Varsayılan olarak, herhangi biri HTTP kullanarak uygulamanıza erişmeye devam edebilir. Tüm HTTP isteklerini HTTPS bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulama sayfanızda, sol gezinti bölmesinde **SSL ayarları**' nı seçin. Ardından, **yalnızca https**'de **Açık**' ı seçin.

![HTTPS 'yi zorla](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

İşlem tamamlandığında, uygulamanızı işaret eden HTTP URL 'Lerinden birine gidin. Örneğin:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>TLS sürümlerini zorunlu kıl

Uygulamanız, [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)gibi endüstri standartlarına göre önerilen TLS düzeyi olan [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1,2 ' i varsayılan olarak sağlar. Farklı TLS sürümlerini zorlamak için şu adımları izleyin:

Uygulama sayfanızda, sol gezinti bölmesinde **SSL ayarları**' nı seçin. Ardından, **TLS sürümü**' nde istediğiniz en düşük TLS sürümünü seçin. Bu ayar yalnızca gelen çağrıları denetler. 

![TLS 1,1 veya 1,2 ' i zorla](./media/app-service-web-tutorial-custom-ssl/enforce-tls1.2.png)

İşlem tamamlandığında, uygulamanız daha düşük TLS sürümleriyle tüm bağlantıları reddeder.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview)kullanarak BETIKLERLE uygulamanız için SSL bağlamalarını otomatik hale getirebilirsiniz.

### <a name="azure-cli"></a>Azure CLı

Aşağıdaki komut, bir aktarılmış PFX dosyasını karşıya yükler ve parmak izini alır.

```azurecli-interactive
thumbprint=$(az webapp config ssl upload \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --certificate-file <path-to-PFX-file> \
    --certificate-password <PFX-password> \
    --query thumbprint \
    --output tsv)
```

Aşağıdaki komut, önceki komuttan gelen parmak izini kullanarak SNı tabanlı bir SSL bağlaması ekler.

```azurecli-interactive
az webapp config ssl bind \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

Aşağıdaki komut, uygulamayı https kullanmaya zorlar.

```azurecli-interactive
az webapp update \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --https-only true
```

Aşağıdaki komut, 1,2 ' nin en düşük TLS sürümünü zorlar.

```azurecli-interactive
az webapp config set \
    --name <app-name> \
    --resource-group <resource-group-name> \
    --min-tls-version 1.2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki komut, bir içe aktarılmış PFX dosyasını karşıya yükler ve SNı tabanlı bir SSL bağlaması ekler.

```powershell
New-AzWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="use-certificates-in-your-code"></a>Kodunuzda sertifika kullanma

Uygulamanızın uzak kaynaklara bağlanması gerekiyorsa ve uzak kaynak sertifika kimlik doğrulaması gerektiriyorsa, genel veya özel sertifikaları uygulamanıza yükleyebilirsiniz. Bu sertifikaları uygulamanızdaki herhangi bir özel etki alanına bağlamanız gerekmez. Daha fazla bilgi için bkz. [Azure App Service içindeki uygulama kodunuzda BIR SSL sertifikası kullanma](app-service-web-ssl-cert-load.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltin
> * Özel sertifikanızı App Service bağlayın
> * Sertifikaları yenileme
> * HTTPS 'yi zorla
> * TLS 1.1/1.2 'yi zorla
> * Betiklerle TLS yönetimini otomatikleştirin

Azure Content Delivery Network kullanmayı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure App Service Content Delivery Network (CDN) ekleme](../cdn/cdn-add-to-web-app.md)

Daha fazla bilgi için bkz. [Azure App Service içindeki uygulama kodunuzda BIR SSL sertifikası kullanma](app-service-web-ssl-cert-load.md).
