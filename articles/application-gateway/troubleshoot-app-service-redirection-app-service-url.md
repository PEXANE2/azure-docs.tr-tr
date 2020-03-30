---
title: Uygulama Hizmeti URL'sine yeniden yönlendirme sorunu
titleSuffix: Azure Application Gateway
description: Bu makalede, Azure Uygulama Ağ Geçidi Azure Uygulama Hizmeti ile kullanıldığında yeniden yönlendirme sorununu nasıl giderikarşılacaklarına ilişkin bilgiler verilmektedir
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: 961ed17bcef19b445c2546a557725bb6bd8653cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293531"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Uygulama Ağ Geçidi'ndeki Uygulama Hizmeti sorunlarını giderme

Azure Uygulama Hizmeti, Azure Uygulama Ağ Geçidi ile arka uç hedefi olarak kullanıldığında karşılaşabileceğiniz sorunları nasıl tanılayıp çözeceğinizi öğrenin.

## <a name="overview"></a>Genel Bakış

Bu makalede, aşağıdaki sorunları nasıl gidereceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama hizmeti URL'si, yeniden yönlendirme olduğunda tarayıcıda açıklanır.
> * Uygulama hizmeti ARRAffinity çerez etki alanı, özgün ana bilgisayar yerine uygulama hizmeti ana bilgisayar adı example.azurewebsites.net olarak ayarlanır.

Bir arka uç uygulaması yeniden yönlendirme yanıtı gönderdiğinde, istemciyi arka uç uygulamasında belirtilenden farklı bir URL'ye yönlendirmek isteyebilirsiniz. Bir uygulama hizmeti bir uygulama ağ geçidinin arkasında barındırıldığında ve istemcinin göreli yoluna yeniden yönlendirme yapmasını gerektirdiğinde bunu yapmak isteyebilirsiniz. Bir örnek, contoso.azurewebsites.net/path1'dan contoso.azurewebsites.net/path2'a yönlendirmedir. 

Uygulama hizmeti bir yeniden yönlendirme yanıtı gönderdiğinde, yanıtının konum üstbilgisinde, uygulama ağ geçidinden aldığı istekteki yle aynı ana bilgisayar adını kullanır. Örneğin, istemci, uygulama ağ geçidinden geçmek yerine isteği doğrudan contoso.azurewebsites.net/path2 contoso.com/path2. Uygulama ağ geçidini atlamak istemezsin.

Bu sorun aşağıdaki ana nedenlerden dolayı gerçekleşebilir:

- Uygulama hizmetinizde yeniden yönlendirme yapılandırıldı. Yeniden yönlendirme, isteğe bir çizgi eklemek kadar basit olabilir.
- Yeniden yönlendirmeye neden olan Azure Active Directory kimlik doğrulaması vardır.

Ayrıca, uygulama ağ geçidinin arkasındaki uygulama hizmetlerini kullandığınızda, uygulama ağ geçidiyle ilişkili alan adı (example.com) uygulama hizmetinin alan adından farklıdır (example.azurewebsites.net. Uygulama hizmeti tarafından ayarlanan ARRAffinity çerezinin etki alanı değeri, arzu edilen example.azurewebsites.net etki alanı adını taşır. Example.com orijinal ana bilgisayar adı, çerezdeki etki alanı adı değeri olmalıdır.

## <a name="sample-configuration"></a>Örnek yapılandırma

- HTTP dinleyicisi: Temel veya çok siteli
- Arka uç adres havuzu: Uygulama Hizmeti
- HTTP ayarları: **Arka uç adresinden Hostname'yi seçin** etkin
- Sonda: HTTP Ayarları etkin **den Hostname seçin**

## <a name="cause"></a>Nedeni

Uygulama Hizmeti çok kiracılı bir hizmettir, bu nedenle isteği doğru bitiş noktasına yönlendirmek için istekteki ana bilgisayar üstbilgisini kullanır. Uygulama Hizmetlerinin varsayılan etki alanı adı *.azurewebsites.net (örneğin, contoso.azurewebsites.net), uygulama ağ geçidinin etki alanı adından farklıdır (örneğin, contoso.com). 

İstemciden gelen özgün istek, ana bilgisayar adı olarak uygulama ağ geçidinin etki alanı adı contoso.com vardır. İsteği uygulama hizmetinin arka ucuna yönlendirirken, özgün istekteki ana bilgisayar adını uygulama hizmetinin ana bilgisayar adına değiştirmek için uygulama ağ geçidini yapılandırmanız gerekir. Uygulama ağ geçidinin HTTP ayar yapılandırmasında **Arka Uç Adresinden Hostname Seç** düğmesini kullanın. Sistem durumu sondası yapılandırmasında **Backend HTTP Ayarlarından Hostname'yi seç'i** anahtarını kullanın.



![Uygulama ağ geçidi ana bilgisayar adını değiştirir](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Uygulama hizmeti yeniden yönlendirme yaptığında, aksi yapılmadığı sürece, geçersiz kılınan ana bilgisayar adı contoso.com yerine konum üstbilgisinde geçersiz kılınan ana bilgisayar adını contoso.azurewebsites.net kullanır. Aşağıdaki örnek istek ve yanıt üstbilgilerini denetleyin.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
Önceki örnekte, yanıt üstbilginin yeniden yönlendirme için 301 durum koduna sahip olduğuna dikkat edin. Konum üstbilgide özgün ana bilgisayar adı yerine uygulama `www.contoso.com`hizmetinin ana bilgisayar adı vardır.

## <a name="solution-rewrite-the-location-header"></a>Çözüm: Konum üstbilgisini yeniden yazma

Konum üstbilgisindeki ana bilgisayar adını uygulama ağ geçidinin etki alanı adına ayarlayın. Bunu yapmak için, yanıttaki konum üstbilgisinin azurewebsites.net içerip içermediğini değerlendiren bir durumla yeniden [yazma kuralı](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) oluşturun. Ayrıca, uygulama ağ geçidinin ana bilgisayarının adını almak için konum üstbilgisini yeniden yazmak için bir eylem gerçekleştirmesi gerekir. Daha fazla bilgi için, [konum üstbilgisini yeniden yazma yönergelerine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url)bakın.

> [!NOTE]
> HTTP üstbilgi yeniden yazma desteği yalnızca Uygulama Ağ Geçidi'nin [Standard_v2 ve WAF_v2 SKU'su](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) için kullanılabilir. v1 SKU kullanıyorsanız, [v1'den v2'ye geçiş](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2)yapmanızı öneririz. V2 SKU ile kullanılabilen yeniden yazma ve diğer [gelişmiş yetenekleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) kullanmak istiyorsunuz.

## <a name="alternate-solution-use-a-custom-domain-name"></a>Alternatif çözüm: Özel bir etki alanı adı kullanma

v1 SKU kullanıyorsanız, konum üstbilgisini yeniden yazamazsınız. Bu özellik sadece v2 SKU için kullanılabilir. Yeniden yönlendirme sorununu gidermek için, ana bilgisayar geçersiz kılma yapmak yerine uygulama ağ geçidinin uygulama hizmetine aldığı ana bilgisayar adını da geçirin.

Uygulama hizmeti artık uygulama ağ geçidine değil, kendi ana geçidine işaret eden aynı orijinal ana bilgisayar üstbilgisindeki yeniden yönlendirmeyi (varsa) yapar.

Özel bir etki alanına sahip olmalı ve şu işlemi izlemelisiniz:

- Etki alanını uygulama hizmetinin özel etki alanı listesine kaydedin. Özel etki alanınızda uygulama hizmetinin FQDN'sini gösteren bir CNAME olmalıdır. Daha fazla bilgi için, [Varolan özel bir DNS adını Azure Uygulama Hizmeti'ne eşle'ye](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain)bakın.

    ![Uygulama hizmeti özel etki alanı listesi](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Uygulama hizmetiniz ana bilgisayar adını `www.contoso.com`kabul etmeye hazırdır. DNS'deki CNAME girişinizi, örneğin uygulama ağ geçidinin FQDN'sine geri çekmek için `appgw.eastus.cloudapp.azure.com`değiştirin.

- DNS sorgusu `www.contoso.com` yaptığınızda etki alanınızın uygulama ağ geçidinin FQDN'sine göre çözüldüğünden emin olun.

- Özel sondanızı **Backend HTTP Ayarlarından Ana Bilgisayar Adı Seç'i**devre dışı bırakacak şekilde ayarlayın. Azure portalında, sonda ayarlarındaki onay kutusunu temizleyin. PowerShell'de **Set-AzApplicationGatewayProbeConfig** komutundaki **-PickHostNameFromBackendAyarlar** anahtarını kullanmayın. Sondanın ana bilgisayar adı alanına, uygulama hizmetinizin FQDN,example.azurewebsites.net girin. Uygulama ağ geçidinden gönderilen sonda istekleri bu FQDN'yi ana bilgisayar üstbilgisinde taşır.

  > [!NOTE]
  > Bir sonraki adımiçin, özel sondanızın arka uç HTTP ayarlarınızla ilişkilendirilmediğinden emin olun. HTTP ayarlarınızda bu noktada **Arka Uç Adresinden Hostname Seç** anahtarı hala etkindir.

- Uygulama ağ geçidinizin HTTP **ayarlarını, Arka Uç Adresinden Hostname Seç'i**devre dışı bırakacak şekilde ayarlayın. Azure portalında onay kutusunu temizleyin. PowerShell'de **Set-AzApplicationGatewayBackendHttpSettings** komutundaki **-PickHostNameFromBackendAddress** anahtarını kullanmayın.

- Özel sondayı arka uç HTTP ayarlarıyla ilişkilendirin ve arka uçta sağlıklı olduğunu doğrulayın.

- Uygulama ağ geçidi artık aynı ana `www.contoso.com`bilgisayar adını, uygulama hizmetine iletmelidir. Yeniden yönlendirme aynı ana bilgisayar adı üzerinde olur. Aşağıdaki örnek istek ve yanıt üstbilgilerini denetleyin.

Varolan bir kurulum için PowerShell'i kullanarak önceki adımları uygulamak için aşağıdaki örnek PowerShell komut dosyasını kullanın. Sonda ve HTTP ayarları yapılandırmasında **-PickHostname** anahtarlarını nasıl kullanmadığımıza dikkat edin.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Sonraki adımlar

Önceki adımlar sorunu çözmediyse, bir [destek bileti](https://azure.microsoft.com/support/options/)açın.
