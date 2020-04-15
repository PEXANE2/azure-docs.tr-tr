---
title: Özel bir kök CA ile kendi imzalı sertifika oluşturma
titleSuffix: Azure Application Gateway
description: Özel kök CA'ya sahip Azure Uygulama Ağ Geçidi kendi imzalı sertifikayı nasıl oluşturacağınızı öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 07/23/2019
ms.author: victorh
ms.openlocfilehash: 5ceefb076b63df942cfff202946f6b82050bbab9
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311935"
---
# <a name="generate-an-azure-application-gateway-self-signed-certificate-with-a-custom-root-ca"></a>Özel kök CA ile Azure Uygulama Ağ Geçidi kendi imzalı sertifika oluşturun

Application Gateway v2 SKU, arka uç sunucularına izin vermek için Güvenilir Kök Sertifikalarının kullanımını tanıtmaktadır. Bu, v1 SKU'da gerekli olan kimlik doğrulama sertifikalarını kaldırır. *Kök sertifika,* X.509 kodlu bir Base-64 sertifikasıdır.( CER) arka uç sertifika sunucusundan root sertifikası biçimlendirin. Sunucu sertifikasını veren kök sertifika yetkilisini (CA) tanımlar ve sunucu sertifikası TLS/SSL iletişimi için kullanılır.

Uygulama Ağ Geçidi, tanınmış bir CA (örneğin, GoDaddy veya DigiCert) tarafından imzalanmışsa, varsayılan olarak web sitenizin sertifikasına güvenir. Bu durumda kök sertifikayı açıkça yüklemeniz gerekmez. Daha fazla bilgi için, [Uygulama Ağ Geçidi ile TLS sonlandırma ve bitiş uca TLS Genel Bakış](ssl-overview.md)bakın. Ancak, bir geliştirme/test ortamınız varsa ve doğrulanmış ca imzalı bir sertifika satın almak istemiyorsanız, kendi özel CA'nızı oluşturabilir ve onunla kendi imzalı sertifikanızı oluşturabilirsiniz. 

> [!NOTE]
> Kendi imzalı sertifikalar varsayılan olarak güvenilir değildir ve bunlar bakımı zor olabilir. Ayrıca, onlar güçlü olmayabilir eski karma ve şifre suit kullanabilirsiniz. Daha iyi güvenlik için, tanınmış bir sertifika yetkilisi tarafından imzalanmış bir sertifika satın alın.

Bu makalede, nasıl öğreneceksiniz:

- Kendi özel Sertifika Yetkilisioluşturun
- Özel CA'nız tarafından imzalanmış kendi imzalı bir sertifika oluşturun
- Arka uç sunucusunun kimliğini doğrulamak için uygulama ağ geçidine kendi imzalı kök sertifikası yükleme

## <a name="prerequisites"></a>Ön koşullar

- **Windows veya Linux çalıştıran bir bilgisayarda [OpenSSL](https://www.openssl.org/)** 

   Sertifika yönetimi için başka araçlar olsa da, bu öğretici OpenSSL kullanır. OpenSSL'yi Ubuntu gibi birçok Linux dağıtımıyla birlikte bulabilirsiniz.
- **Bir web sunucusu**

   Örneğin, sertifikaları sınamak için Apache, IIS veya NGINX.

- **Bir Uygulama Ağ Geçidi v2 SKU**
   
  Varolan bir uygulama ağ geçidiniz yoksa, [Bkz. Quickstart: Azure Application Gateway ile doğrudan web trafiği - Azure portalı.](quick-create-portal.md)

## <a name="create-a-root-ca-certificate"></a>Kök CA sertifikası oluşturma

OpenSSL'yi kullanarak kök CA sertifikanızı oluşturun.

### <a name="create-the-root-key"></a>Kök anahtarı oluşturma

1. OpenSSL'nin yüklü olduğu bilgisayarınızda oturum açın ve aşağıdaki komutu çalıştırın. Bu, parola korumalı bir anahtar oluşturur.

   ```
   openssl ecparam -out contoso.key -name prime256v1 -genkey
   ```
1. İsteyişte, güçlü bir parola yazın. Örneğin, büyük harf, küçük harf, sayılar ve semboller kullanarak en az dokuz karakter.

### <a name="create-a-root-certificate-and-self-sign-it"></a>Kök Sertifikası oluşturma ve kendi kendine imzalama

1. CSR ve sertifikaoluşturmak için aşağıdaki komutları kullanın.

   ```
   openssl req -new -sha256 -key contoso.key -out contoso.csr

   openssl x509 -req -sha256 -days 365 -in contoso.csr -signkey contoso.key -out contoso.crt
   ```
   Önceki komutlar kök sertifikasını oluşturur. Bunu sunucu sertifikanızı imzalamak için kullanırsınız.

1. İstendiğinde, kök anahtarın parolasını ve Ülke, Devlet, Org, OU ve tam nitelikli alan adı gibi özel CA'nın kuruluş bilgilerini yazın (bu verenin etki alanıdır).

   ![kök sertifikası oluşturma](media/self-signed-certificates/root-cert.png)

## <a name="create-a-server-certificate"></a>Sunucu sertifikası oluşturma

Ardından OpenSSL kullanarak bir sunucu sertifikası oluşturursunuz.

### <a name="create-the-certificates-key"></a>Sertifikanın anahtarını oluşturma

Sunucu sertifikası için anahtarı oluşturmak için aşağıdaki komutu kullanın.

   ```
   openssl ecparam -out fabrikam.key -name prime256v1 -genkey
   ```

### <a name="create-the-csr-certificate-signing-request"></a>CSR oluşturma (Sertifika İmzalama İsteği)

CSR, sertifika talep ederken CA'ya verilen ortak bir anahtardır. CA, bu özel istek için sertifikayı yayınlar.

> [!NOTE]
> Sunucu sertifikasının CN'si (Ortak Ad) verenin etki alanından farklı olmalıdır. Örneğin, bu durumda, veren için CN `www.contoso.com` ve sunucu sertifikasıc's `www.fabrikam.com`CN olduğunu.


1. CSR oluşturmak için aşağıdaki komutu kullanın:

   ```
   openssl req -new -sha256 -key fabrikam.key -out fabrikam.csr
   ```

1. İstendiğinde, kök anahtarın parolasını ve özel CA için kuruluş bilgilerini yazın: Ülke, Eyalet, Org, OU ve tam nitelikli alan adı. Bu web sitesinin etki alanı ve veren farklı olmalıdır.

   ![Sunucu sertifikası](media/self-signed-certificates/server-cert.png)

### <a name="generate-the-certificate-with-the-csr-and-the-key-and-sign-it-with-the-cas-root-key"></a>Sertifikayı CSR ve anahtarla oluşturun ve CA'nın kök tuşuyla imzalayın

1. Sertifikayı oluşturmak için aşağıdaki komutu kullanın:

   ```
   openssl x509 -req -in fabrikam.csr -CA  contoso.crt -CAkey contoso.key -CAcreateserial -out fabrikam.crt -days 365 -sha256
   ```
### <a name="verify-the-newly-created-certificate"></a>Yeni oluşturulan sertifikayı doğrulama

1. CRT dosyasının çıktısını yazdırmak ve içeriğini doğrulamak için aşağıdaki komutu kullanın:

   ```
   openssl x509 -in fabrikam.crt -text -noout
   ```

   ![Sertifika doğrulama](media/self-signed-certificates/verify-cert.png)

1. Dizininizdeki dosyaları doğrulayın ve aşağıdaki dosyalara sahip olduğundan emin olun:

   - contoso.crt
   - contoso.key
   - fabrikam.crt
   - fabrikam.key

## <a name="configure-the-certificate-in-your-web-servers-tls-settings"></a>Sertifikayı web sunucunuzun TLS ayarlarında yapılandırın

Web sunucunuzda, fabrikam.crt ve fabrikam.key dosyalarını kullanarak TLS yapılandırın. Web sunucunuz iki dosya yıyıtamamıyorsa, OpenSSL komutlarını kullanarak bunları tek bir .pem veya .pfx dosyasıyla birleştirebilirsiniz.

### <a name="iis"></a>IIS

Sertifika alma ve IIS'de sunucu sertifikası olarak yükleme ile ilgili talimatlar için [bkz.](https://support.microsoft.com/help/816794/how-to-install-imported-certificates-on-a-web-server-in-windows-server)

TLS bağlama yönergeleri için, [IIS 7'de SSL nasıl ayarlanır.](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis#create-an-ssl-binding-1)

### <a name="apache"></a>Apache

Aşağıdaki yapılandırma, Apache'de [SSL için yapılandırılan](https://cwiki.apache.org/confluence/display/HTTPD/NameBasedSSLVHosts) bir örnek sanal ana bilgisayardır:

```
<VirtualHost www.fabrikam:443>
      DocumentRoot /var/www/fabrikam
      ServerName www.fabrikam.com
      SSLEngine on
      SSLCertificateFile /home/user/fabrikam.crt
      SSLCertificateKeyFile /home/user/fabrikam.key
</VirtualHost>
```

### <a name="nginx"></a>NGINX

Aşağıdaki yapılandırma, TLS yapılandırması ile örnek bir [NGINX sunucu bloğudur:](https://nginx.org/docs/http/configuring_https_servers.html)

![TLS ile NGINX](media/self-signed-certificates/nginx-ssl.png)

## <a name="access-the-server-to-verify-the-configuration"></a>Yapılandırmayı doğrulamak için sunucuya erişin

1. Root sertifikasını makinenizin güvenilir kök deposuna ekleyin. Web sitesine erişdiğinizde, tüm sertifika zincirinin tarayıcıda görüldüğünden emin olun.

   ![Güvenilen kök sertifikalar](media/self-signed-certificates/trusted-root-cert.png)

   > [!NOTE]
   > DNS'nin web sunucusu adını (bu örnekte, www.fabrikam.com) web sunucunuzun IP adresine yönlendirecek şekilde yapılandırıldığı varsayılır. Değilse, adı gidermek için [ana bilgisayar dosyasını](https://answers.microsoft.com/en-us/windows/forum/all/how-to-edit-host-file-in-windows-10/7696f204-2aaf-4111-913b-09d6917f7f3d) da yapabilirsiniz.
1. Web sitenize göz atın ve site ve sertifika bilgilerini doğrulamak için tarayıcınızın adres kutusundaki kilit simgesini tıklatın.

## <a name="verify-the-configuration-with-openssl"></a>OpenSSL ile yapılandırmayı doğrulayın

Veya sertifikayı doğrulamak için OpenSSL'yi kullanabilirsiniz.

```
openssl s_client -connect localhost:443 -servername www.fabrikam.com -showcerts
```

![OpenSSL sertifika doğrulaması](media/self-signed-certificates/openssl-verify.png)

## <a name="upload-the-root-certificate-to-application-gateways-http-settings"></a>Kök sertifikayı Uygulama Ağ Geçidi'nin HTTP Ayarlarına yükleyin

Sertifikayı Application Gateway'e yüklemek için .crt sertifikasını .cer formatında Base-64 kodlu bir sertifikaya dışa aktarmanız gerekir. .crt zaten base-64 kodlanmış biçiminde ortak anahtarı içerdiğinden, dosya uzantısını .crt'den .cer'e yeniden adlandırmamız. 

### <a name="azure-portal"></a>Azure portal

Portaldan güvenilir kök sertifikasını yüklemek için **HTTP Ayarları'nı** seçin ve **HTTPS** protokolünü seçin.

![Portalı kullanarak sertifika ekleme](media/self-signed-certificates/portal-cert.png)

### <a name="azure-powershell"></a>Azure PowerShell

Veya, kök sertifikayı yüklemek için Azure CLI veya Azure PowerShell'i kullanabilirsiniz. Aşağıdaki kod bir Azure PowerShell örneğidir.

> [!NOTE]
> Aşağıdaki örnek uygulama ağ geçidine güvenilir bir kök sertifikası ekler, yeni bir HTTP ayarı oluşturur ve arka uç havuzunun ve dinleyicinin zaten var olduğunu varsayarak yeni bir kural ekler.

```azurepowershell
## Add the trusted root certificate to the Application Gateway

$gw=Get-AzApplicationGateway -Name appgwv2 -ResourceGroupName rgOne

Add-AzApplicationGatewayTrustedRootCertificate `
   -ApplicationGateway $gw `
   -Name CustomCARoot `
   -CertificateFile "C:\Users\surmb\Downloads\contoso.cer"

$trustedroot = Get-AzApplicationGatewayTrustedRootCertificate `
   -Name CustomCARoot `
   -ApplicationGateway $gw

## Get the listener, backend pool and probe

$listener = Get-AzApplicationGatewayHttpListener `
   -Name basichttps `
   -ApplicationGateway $gw

$bepool = Get-AzApplicationGatewayBackendAddressPool `
  -Name testbackendpool `
  -ApplicationGateway $gw

Add-AzApplicationGatewayProbeConfig `
  -ApplicationGateway $gw `
  -Name testprobe `
  -Protocol Https `
  -HostName "www.fabrikam.com" `
  -Path "/" `
  -Interval 15 `
  -Timeout 20 `
  -UnhealthyThreshold 3

$probe = Get-AzApplicationGatewayProbeConfig `
  -Name testprobe `
  -ApplicationGateway $gw

## Add the configuration to the HTTP Setting and don't forget to set the "hostname" field
## to the domain name of the server certificate as this will be set as the SNI header and
## will be used to verify the backend server's certificate. Note that TLS handshake will
## fail otherwise and might lead to backend servers being deemed as Unhealthy by the probes

Add-AzApplicationGatewayBackendHttpSettings `
  -ApplicationGateway $gw `
  -Name testbackend `
  -Port 443 `
  -Protocol Https `
  -Probe $probe `
  -TrustedRootCertificate $trustedroot `
  -CookieBasedAffinity Disabled `
  -RequestTimeout 20 `
  -HostName www.fabrikam.com

## Get the configuration and update the Application Gateway

$backendhttp = Get-AzApplicationGatewayBackendHttpSettings `
  -Name testbackend `
  -ApplicationGateway $gw

Add-AzApplicationGatewayRequestRoutingRule `
  -ApplicationGateway $gw `
  -Name testrule `
  -RuleType Basic `
  -BackendHttpSettings $backendhttp `
  -HttpListener $listener `
  -BackendAddressPool $bepool

Set-AzApplicationGateway -ApplicationGateway $gw 
```

### <a name="verify-the-application-gateway-backend-health"></a>Uygulama ağ geçidi arka uç durumunu doğrulama

1. Sondanın sağlıklı olup olmadığını kontrol etmek için uygulama ağ geçidinizin **Arka Uç Sağlık** görünümünü tıklatın.
1. HTTPS sondası için **Durum'un Sağlıklı** olduğunu görmelisiniz.

![HTTPS sondası](media/self-signed-certificates/https-probe.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi'nde SSL\TLS hakkında daha fazla bilgi edinmek için, [Uygulama Ağ Geçidi ile TLS sonlandırma genel bakış ve uca TLS](ssl-overview.md)bakın.

